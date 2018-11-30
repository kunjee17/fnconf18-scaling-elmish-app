- title : Scaling Elmish App
- description : How to Scale Elmish App
- author : Kunjan Dalal
- theme : white
- transition : none

***
- data-background-image: images/22.png

<div style="background:white;">
<br/>
## Scaling Fable Elmish / Elm Application
### A Experience Report

<br />
### Kunjan Dalal
### @kunjee
<br />
</div>

' Skip the Intro because of less time

***
- data-background-image: images/94.png

<div style="background:white;">
<br/>

### Slide Agenda

- <s>Intro to me</s>
- Intro to Fable & Fable.Elmish
- <s>Intro to Elm</s>
- <s>Basic Example</s>
- <s>The Elm Architecture - TEA </s>
<!-- - Past - All the W question answered  -->
<!-- - Back End, Front End and more Front End -->
<!-- - The Model -->
<!-- - The View -->
<!-- - The Update -->
- Go Beyond Todo list
- Why Elmish instead of Elm
- <s>External JS library in Fable Elmish </s>
- <s>External React library in Fable Elmish </s>
- SAFE stack
- Thank You Note

</div>

' This talk was around 45 mins but cut down due to other wonderful talk. So, I have to do tight editing on slides. We have lot to cover so let's get started. I am cutting down few basic stuff as you might already learn that in wonderful talks happens before this one

***

### Intro to Fable & Fable Elmish

 > Fable is an F# to JavaScript compiler powered by Babel, designed to produce readable and standard code.

 <br/>

 > Elmish implements core abstractions that can be used to build Fable applications following the “model view update” style of architecture, as made famous by Elm.

' There are two different things in Fable Elmish. Fable and Fable-Elmish or Elmish it self. Let's have a loot at both.

---

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

' And Here things wire up. So, as promised nothing different from your Elm application.

***

### What is SPA ?

![](images/Deathly_Hallows_Sign.svg)

' what promise was simple thing. html, js and css. And you know SPA. You know everything about it. But

---

- data-background-image: images/triangles.gif

***

### Let's look at a Normal Big Application

---

Here will show code from my project - specifically how components are arranged. yet to decide gif or screen shot will be good option

Here there are two options. Either directly go to pointers one can take care about or explain in terms of model, update and view.


---

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
