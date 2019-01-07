# Modals and windows

A KVision application can use two types of windows - modal dialogs and floating, re-sizable windows. Modals can be used to create popups with information messages, alerts, warnings or custom forms and buttons. Floating windows can be used to split your GUI into any number of separate parts, to work with them simultaneously \(multiple documents, floating toolbars etc.\)

### Modals

Modals components are grouped in `pl.treksoft.kvision.modal` package. The base class for all modals is `pl.treksoft.kvision.modal.Modal`. You can use this class to create a modal with any content you need - just add your components to the container. You can use standard `add` method to add typical components or containers. You can also use `addButton` method to add some buttons to the footer of the modal \(right-aligned\).

Modal dialogs are automatically added to the components tree, so there is no need to add them to any container. Modals are invisible by default. Use `show` method of the Modal class to make it visible.

```kotlin
val modal = Modal("Custom modal dialog")
modal.add(Tag(TAG.H4, "Lorem ipsum dolor sit amet, consectetur adipiscing elit."))
modal.add(Image(require("./img/dog.jpg")))
modal.addButton(Button("Close").onClick {
    modal.hide()
})
modal.show()
```

`Modal` class constructor has additional parameters for a caption, a size of the modals text, visibility of the close button, disabling animation effects and the function of the Escape key.

```kotlin
val modal = Modal("A caption", size = ModalSize.LARGE, closeButton = false, 
                  animation = false, escape = false)
modal.show()
```

There are some ready to use, convenience subclasses of the `Modal` class.

#### Alert popups

Alert popup is a simple modal window with an information text and an OK button. You can use a companion function `Alert.show(...)` to easily create and display an Alert window. You can define a callback function for the OK button. You can also change some other parameters of the popup \(e.g. size, align, rich content or disabling animation effect\).

```kotlin
Alert.show("Alert dialog", "Lorem ipsum dolor sit amet, consectetur adipiscing.") {
    // A callback function body for the OK button.
}
```

#### Confirm popups

Confirm popup is a simple modal window with some question and two \(YES / NO\) or three \(YES / NO / CANCEL\) buttons. You can use a companion function `Confirm.show(...)` to easily create and display a Confirm popup. You can define two distinct callback functions for YES and NO buttons. You can also change button titles \(e.g. to support [I18N](internationalization.md)\) and some other parameters of the popup \(e.g. size, align, rich content or disabling animation effect\).

```kotlin
Confirm.show(
    "Confirm dialog",
    "Lorem ipsum dolor sit amet?",
    animation = false,
    align = Align.LEFT,
    yesTitle = tr("YES"),
    noTitle = tr("NO"),
    cancelVisible = false,
    noCallback = {
        Alert.show("Result", "You pressed NO button.")
    }) {
    Alert.show("Result", "You pressed YES button.")
}
```

#### Dialog with a result

A `pl.treksoft.kvision.modal.Dialog` component allows to create a dialog, which can return any data as a result. This class has a suspending method `getResult()`, which must be called inside a Kotlin coroutine. You can create a subclass of the `Dialog` class or use a `Dialog` instance directly. You should call `setResult()` method from your own code to pass your data back to the caller.

```kotlin
GlobalScope.launch {
    val dialog = Dialog<String>("Dialog with result") {
        add(Label("Press a button"))
        addButton(Button("Button").onClick {
            setResult("STRING RESULT")
        })
    }
    val result = dialog.getResult()
    console.log(result)
}
```

### Windows

A `pl.treksoft.kvision.window.Window` component allows to create a number of floating, re-sizable windows inside your application. Every window has a frame, which can be used to change its size and position. A `Window` class constructor takes a number of parameters, and two of them allow you to create windows not draggable and not re-sizable as well. When the window is not draggable, the close button is hidden and the caption is null - the caption bar won't be rendered at all.

Windows overlap each other - one is shown on top of the others. The window clicked \(inside the window caption bar\) is brought to the front. You can also use `toFront()` method to bring a given window to the front programmatically.

Unlike modals, window components have to be added to a container, and they are displayed in the context of their parent \(e.g. you can hide all windows by hiding their parent container\). But windows are not confined to the parent container area - they can be positioned anywhere inside the browser window.

```kotlin
window("Window", 600.px, 300.px, closeButton = true) {
    left = ((Random.nextDouble() * 800).toInt()).px
    top = ((Random.nextDouble() * 300).toInt()).px
    label("A window content")
}
```


