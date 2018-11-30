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

' There are two different thing in Fable Elmish

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

---

##### Elmish - Main

```fsharp
    // App
    Program.mkProgram init update view
    |> Program.withConsoleTrace
    |> Program.withReact "elmish-app"
    |> Program.run
```


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

---

### Let's Talk about model

---

### Let's Talk about update

---

### Let's Talk about view


***

### Why I chose Elmish instead of Elm ?

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

***

SAFE Stack

***

Thank You Note
