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
- Thank You Note



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

### Let's Talk about model

---

### Let's Talk about update

---

### Let's Talk about view


***

### Why I chose Elmish instead of Elm ?

A funny gif would be great here.

' If everything is possible in Elm then why I choose Elmish? Let me explain how I see Elm and Elmish and then I ll explain why I choose one over another.

---

### What is Elm?


![](images/00-Mercedes-Benz-Design-Future-Bus.jpg)

---

### Who all are in JavaScript / React Community?


![](images/holi.jpg)

---

### What is Elmish?

![](images/banbus.jpg)

---

React Quill and Izitoast example

' Because of Time Constrain I can't go into details. But here are the examples on how you can easily integrate React or Pure JS component.

***

SAFE Stack

' Honorary mention of SAFE Stack. What is safe stack and that matters ? Why writing everything in F# helps. Explain as per time.

***

Thank You Note

- A image or simple details ?

' Questions if time permits. Fuzzy Cloud link. Available for consulting. Cards available etc etc. Tweet to Fable Compiler or Tweet to me in case of feedback.
