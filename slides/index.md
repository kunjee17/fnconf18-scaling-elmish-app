- title : Scaling Elmish App
- description : How to Scale Elmish App
- author : Kunjan Dalal
- theme : serif
- transition : none

***


## Scaling Fable Elmish / Elm Application
### A Experience Report By

<br />
### Kunjan Dalal
### @kunjee
<br />


' Skip the Intro because of less time

***



### Agenda for Today

- <s>Intro to me</s>
- <s>Intro to Elm</s>
- Intro to Fable & Fable.Elmish
- <s>Basic Example</s>
- <s>The Elm Architecture - TEA </s>
<!-- - Past - All the W question answered  -->
<!-- - Back End, Front End and more Front End -->
<!-- - The Model -->
<!-- - The View -->
<!-- - The Update -->
- Tips and Tricks for Scaling your Application
- Why Elmish instead of Elm
- <s>External JS library in Fable Elmish </s>
- <s>External React library in Fable Elmish </s>
- <s>SAFE stack</s>
- Thank You!!!



' This talk was around 45 mins but cut down due to other wonderful talk. So, I have to do tight editing on slides. We have lot to cover so let's get started. I am cutting down few basic stuff as you might already learn that in wonderful talks happens before this one


***


### Pop Quiz Time

![](images/riddler.gif)

' How many of you know about Elm?
' How many are using Elm in production application? As a whole app or part of it?
' This is important question -> How many of you feels that they attend a conference and that nice demo application never get converted to full blown application?

***

<s>
### Demo
</s>

' We will not see any demo today
' Instead will talk about past, more about what is possible in contrast to what can be possible

---

### Why Code Samples are in F#?

> It is a mainstream - general purpose functional programming language for your daily web and mobile need. It is strongly typed and it works everywhere. It is #fsharp

***

### Intro to Fable & Fable Elmish

 > Fable is an F# to JavaScript compiler powered by Babel, designed to produce readable and standard code.

 <br/>

 > Elmish implements core abstractions that can be used to build Fable applications following the “model view update” style of architecture, as made famous by Elm.

' There are two different things in Fable Elmish. Fable and Fable-Elmish or Elmish it self. Let's have a look at both.

***

### Fable
![](images/fable_logo.png)

---

##### F# unit of Measure code

```fsharp
    module Tour.UnitsOfMeasure

    // From https://docs.microsoft.com/en-us/dotnet/fsharp/tour
    open Microsoft.FSharp.Data.UnitSystems.SI.UnitNames

    let sampleValue1 = 1600.0<meter>

    [<Measure>]
    type mile =
        static member asMeter = 1609.34<meter/mile>

    let sampleValue2 = 500.0<mile>

    let sampleValue3 = sampleValue2 * mile.asMeter

    printfn "After a %f race I would walk %f miles
            which would be %f meters" sampleValue1 sampleValue2 sampleValue3

```

' Simple F# code with it's specific Unit of Measure feature. A tight guard against how you define units. Specially useful in Finance and Science / Mathematics obviously.
' It is basically like Scalajs or Cloujure Script converting respective language to JavaScript.

---

##### Converted Java Script Code

```js
    import { toConsole, printf } from "fable-core/String";
    export const sampleValue1 = 1600;
    export function mile$$$get_asMeter() {
    return 1609.34;
    }
    export const sampleValue2 = 500;
    export const sampleValue3 = sampleValue2 * mile$$$get_asMeter();
    toConsole(
        printf("After a %f race I would walk %f miles which would be %f meters"))
            (sampleValue1)(sampleValue2)(sampleValue3);
```

' Look at this . A very readable JavaScript. Which then pass on to Bable to boil down to normal JavaScript which can be understood by browser.

***

### Elmish

![](images/elmish.png)

---

##### Elmish - Types

```fsharp
    type Model =
        { Value : string }

    type Msg =
        | ChangeValue of string
```

' Your routine model. With 1 property of type String. To change that we need to pass message of value string.

---

##### Elmish - State

```fsharp
    let init () = { Value = "" }, Cmd.none

    // UPDATE

    let update (msg:Msg) (model:Model) =
        match msg with
        | ChangeValue newValue ->
            { Value = newValue }, Cmd.none
```

' Init is initializing with empty value and then update will take care of every change.

---

##### Elmish - View

```fsharp
    let view model dispatch =
        div [ Class "main-container" ]
            [ input [ Class "input"
                    Value model.Value
                    OnChange (fun ev -> ev.target?value |>
                                string |> ChangeValue |> dispatch) ]
            span [ ]
                [ str "Hello, "
                str model.Value
                str "!" ] ]
```

' Here is simple view which takes the value, pass in to update and show the updated value.

---

##### Elmish - Main

```fsharp
    // App
    Program.mkProgram init update view
    |> Program.withConsoleTrace
    |> Program.withReact "elmish-app"
    |> Program.run
```

' And here is wire up code. So, not much different then your regular Elm code. Or for that case any code which follows elm architecture.

***

### Back End - Front End - Back End - Front End - Back End

![](images/backfront.gif)

' I love back end and made a career out of it, and I strongly believe that front end is also required good architecture, Nowadays it is more required. So, today will be talking about that front end part.

***

### What is SPA ?

![](images/Deathly_Hallows_Sign.svg)

' what promise was simple thing. html, js and css. And you know SPA. You know everything about it. But

---

- data-background-image: images/triangles.gif

***
 - data-background-image: images/oldman.jpeg


' Today I will be providing tips and tricks. And I hope you will take it as it is just like you will take words of wise old man without asking for more reasons. Respecting time noting else.

***

### Model - View - Update

' In MVU architecture I feel that model is most important. As it is message between update and view. And who ever control the message, controls the narrative.

---

- data-background-image: images/news.gif

' So, my tips and tricks will start from that only.

***

### Let's Talk about Model


---

### Application & Authentication

```fsharp
type Msg =
    | ApplicationMsg of Application.Types.Msg
    | AuthenticationMsg of Authentication.Types.Msg

type PageModel =
    | ApplicationModel of Application.Types.Model
    | AuthenticationModel of Authentication.Types.Model

type Model = {
    UserToken : AuthToken option
    CurrentPage : Page
    PageModel : PageModel
}

```
---

```fsharp
match page with
| AuthenticationPage a ->
    let (authentication, authenticationCmd) =
        Authentication.State.init(a)
    in
    {model with CurrentPage = AuthenticationPage a;
        PageModel = AuthenticationModel authentication},
            Cmd.map AuthenticationMsg authenticationCmd

| ApplicationPage a ->
    if model.UserToken.IsNone
    then model, Navigation.modifyUrl(toHash (AuthenticationPage Login))
    else
    let (application, applicationCmd) =
        Application.State.init(a,model.UserToken.Value)
    in
    {model with CurrentPage = ApplicationPage a;
        PageModel = ApplicationModel application},
            Cmd.map ApplicationMsg applicationCmd

```


' User can access on module and one can't. If you are starting your application and you are having user authentication then do separate them. For starting 2 - 3 pages it feels like
' it is ok to pages together. But as you grow there will be a lot more repeated code to check user before reaching out every page.
' Pro tip - Use JWT if you are still not using it. It is nice to have when you are developing State less application

---

### Page Model

```fsharp
module Types =

type BadPageModel = {
    HomeModel : Home.Types.Model
    UsersModel : Users.Types.Model
    ProfileModel : Profile.Types.Model
}

type GoodPageModel =
    | HomeModel of Home.Types.Model
    | UsersModel of Users.Types.Model
    | ProfileModel of Profile.Types.Model

```

' There are two ways of writing page. As shown here. But having page model as option type always help. You are only loading data that is required for that specific page.
' Pro tip - You can group pages by Navigation. So, one less naming problem solve. Also, it helps to have authorization feature abstracted away. So, user is not allowed in admin
' navigation if he is not admin

---

### Page - Model

```fsharp
type HomeModel = {
    Component1 : Component.Types.Model
    Component2 : Component.Types.Model
    ....
}
```

' Divide your page in multiple component. Even if your page is having single big form, divide that in logical group. It always help to manage small chunks of code compare to big chunk.
' And even as simple as simple entry forms are doing many things, like validation clint side and validation
' on server side in case of something like username is available and also valid username.
' Features like auto complete requires both server and client side code.

***

### Component Model

' We reached almost half way, so it is time for little break

---

![](images/break.gif)

---

### Domain - Driven - Design


---

### Domain Model on Client Side

```fsharp
type Validate = {
    IsValid : bool
    Message : string
}

type Person = {
    FirstName : string
    LastName : string
}

type PersonError = {
    FirstName : Validate
    LastName : Validate
}

type Model = {
    Person : Person
    PersonError : PersonError
}
```

' Most dumb domain model example to give an idea. Here, to update error message that first name should not be empty
' you need to update, model, PersonError, Validate and Message. It is three level deep.
' You can always use lenses for this, but if you need to use lenses than you are doing something wrong.
' As model represent client keep it as flat as possible. Use of lenses will become more and more complicated as application grows
' Instead divide model in multiple models in MVU fashion

---

### Flatten the model

```fsharp
type Validate = {
    IsValid : bool
    Message : string
}

type FirstName = {
    Value : string
    Valid : Validate
}

type LastName = {
    Value : string
    Valid : Validate
}

type Person = {
    FirstName : FirstName
    LastName : LastName
}

```
---

##### OR

```fsharp
type Person = {
    FirstName: string
    FirstNameErr : Validate
    LastName : string
    LastNameErr : Validate
}
```

***

### Let's Talk about Command

---

### Single Responsibly Principal

```fsharp
type Msg =
    | ChangeValue of string
    | Changed of string

let init () = { Value = "" }, Cmd.none

let update (msg:Msg) (model:Model) =
    match msg with
    | ChangeValue newValue ->
        let updateValue = newValue.ToUpper()
        model, Cmd.ofMsg (Changed updateValue)
    | Changed updatedValue ->
        { Value = updatedValue}, Cmd.none
```

' One command is doing one and only one thing. Here I capitalizing everything coming in. Just because why not? But you can do validation or and post to data to server
' When things are good to go pass it to Changed message. It is confirmation that everything is good to go.

---

### Another way to Look at it

```fsharp
type Command = | ChangeValue of string
type Event = | Changed of string

type Msg =
    | Command of Command
    | Event of Event

let init () = { Value = "" }, Cmd.none

let processCommand (msg : Command) (model : Model) =
    match msg with
    | ChangeValue newValue ->
        let updateValue = newValue.ToUpper()
        model, Cmd.ofMsg (Event (Changed updateValue))

let processEvent (msg : Event) (model: Model) =
    match msg with
    | Changed value ->
        { Value = value}, Cmd.none


```

---

```fsharp

let update (msg:Msg) (model:Model) =
    match msg with
    | Command c -> processCommand c model
    | Event e -> processEvent e model
```

---


### Subscribe is there to Use

```fsharp
// Types
type Model = CurrentTime of DateTime
type Messages = Tick of DateTime

// State
let initialState() =
    CurrentTime DateTime.Now, Cmd.none

let update (Tick next) (CurrentTime _time) =
    CurrentTime next, Cmd.none

let timer initial =
    let sub dispatch =
        Browser.window.setInterval(fun _ ->
            dispatch (Tick DateTime.Now)
        , 1000) |> ignore
    Cmd.ofSub sub
```

' This is famous subscribe example. But it is way more powerful than this. Let's have an another example where it can be used.

---

### Real Time or Reactive Application

```fsharp
let subscribe =
        let socketSubscription dispatch =
            let eventSourceOptions = createEmpty<IEventSourceOptions>
            eventSourceOptions.handlers <- createObj [
                "onMessage" ==>
                    fun (msg: ServerEventMessage) ->
                        printfn "onMessage %A" msg.json
                "chat" ==>
                    fun (msg : OutputMessages) ->
                                msg |> (SSESuccessMessages >> dispatch)
            ]

            let channels = [|"home"; ""|]
            SSClient.ServerEventsClient.Create(baseUrl
            , new List<string>(channels)
            , eventSourceOptions
            ).start() |> ignore
        Cmd.ofSub socketSubscription
```

' Say it real time if you are little old school person or reactive application if you like to use buzzword. Subscribe is pretty useful in those scenarios.
' And now also you will get why divide MSG in Command and Event. You can subscribe to Events from Subscribe to update the UI in reactive fashion.

***

### Let's Talk about View

---
- data-background-image: images/catgroup.jpg

```fsharp
let root model dispatch =
    Container.container[ Container.IsFluid ][
        documentTable model dispatch
    ]
```


' View should be Small & <s>Dumb</s> cute. It should be representation of whole page of application. So, if someone is reading that get the whole idea of page.

---

### Use CSS Wrappers like Fulma

```fsharp
form [ ]
        [ // Email field
            Field.div [ ]
                [ Label.label [ ]
                    [ str "Email" ]
                  Control.div [ Control.HasIconLeft
                                Control.HasIconRight ]
                    [ Input.email [ Input.Color IsDanger
                                    Input.DefaultValue "hello@" ]
                      Icon.faIcon [ Icon.Size IsSmall; Icon.IsLeft ]
                        [ Fa.icon Fa.I.Envelope ]
                      Icon.faIcon [ Icon.Size IsSmall; Icon.IsRight ]
                        [ Fa.icon Fa.I.Warning ] ]
                  Help.help [ Help.Color IsDanger ]
                    [ str "This email is invalid" ] ] ]
```

***

### Why I chose Elmish?

- One Language of for Server, Client (Web & Mobile), Data Science / ML & Data Processing etc.
- Possible to leverage JavaScript libraries (Stateless)
- Possible to leverage React libraries (Stateless)
- Because of all these I can leverage my experience

' If everything is possible in Elm then why I choose Elmish?
' Here are couple of reason.

---

### Recharts

```fsharp
let lineChartSample =
    lineChart
        [ margin 5. 20. 5. 0.
          Chart.Width 600.
          Chart.Height 300.
          Chart.Data data ]
        [ line
            [ Cartesian.Type Monotone
              Cartesian.DataKey "uv"
              P.Stroke "#8884d8"
              P.StrokeWidth 2. ]
            []
          cartesianGrid
            [ P.Stroke "#ccc"
              P.StrokeDasharray "5 5" ]
            []
          xaxis [Cartesian.DataKey "name"] []
          yaxis [] []
          tooltip [] []
        ]
```
' Here is Rechart example in F#. No need to create graph library from bottom up. BTW even ReChart is using D3 to draw charts.


***
- data-background-image: images/catthankyou.gif

---

- Awesome Fable - [https://github.com/kunjee17/awesome-fable](https://github.com/kunjee17/awesome-fable)
- Safe Stack - [https://safe-stack.github.io/](https://safe-stack.github.io/)
- WTF# Elmish - [https://wtfsharp.net/wtf-is-elmish](https://wtfsharp.net/wtf-is-elmish)
- Fulma - [https://mangelmaxime.github.io/Fulma/](https://mangelmaxime.github.io/Fulma/)
- Fable - [https://fable.io/](https://fable.io/)

' Here is awesome fable link where I maintaining all awesome stuff happening in Fable
' Safe - stack is just like mean stack but everything in F#. Even if you are not interested in F# then also do check it out.
' It is recently featured for Thought works Tech radar.
' And I personally feel that every Functional Programming language should have similar kind of stack
' What the F# podcast for Elmish. Where me and Alfonso Garcia Caro was guest
' There is also a link for fulma
' And fable link. Do try out Fable Repl, it is compiling whole F# code in browser it self. So, your code totally work in offline mode

---

- Fuzzy Cloud - [https://fuzzycloud.in/](https://fuzzycloud.in/)
- Fable Compiler @ Twitter - [@FableCompiler](https://twitter.com/FableCompiler)
- Me (Kunjan Dalal) @ Twitter - [@kunjee](https://twitter.com/kunjee)


' Fuzzy Cloud is consulting things I started for almost a year. I am available for training and consulting for everything related to Functional Programming.
' I also have my visiting card with me if any one like to have.
' If you have enjoyed this talk please tag Fable Compiler and me if you wish with your good comments.
' For any kind of inputs or criticism please contact me and will try to improve in next talk.
' Any questions ? Else I am available here only for today at least. Do contact me.
