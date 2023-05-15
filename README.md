# ios-timeout-focus

How to solve the timeout and focus issue in iOS.

The demo link: https://xcatliu.github.io/ios-timeout-focus/

---

In iOS, directly calling `input.focus()` works as normal.

```js
focusButton.addEventListener('click', () => {
    textInput.focus();
});
```

However, calling focus inside a timeout cannot bring up the keyboard:

```js
timeoutFocusButton.addEventListener('click', (e) => {
    setTimeout(() => textInput.focus(), 16 );
});
```

You should call `openIosKeyboard` synchronously and then call timeout and focus like this:

```js
timeoutFocusButton.addEventListener('click', (e) => {
    openIosKeyboard();
    setTimeout(() => textInput.focus(), 10);
});
```

```js
/**
 * https://stackoverflow.com/a/74851426/2777142
 * iOS keyboard can only be opened by user interaction + focus event.
 * There are situations in which we need to open keyboard programmatically
 * before other elements are rendered, like when opening an overlay. this function can be used
 * to open the keyboard before the overlay is rendered, and once the overlay is
 * rendered, it's needed to focus to the desired input element, and the keyboard
 * will stay open.
 *
 * This function needs to be called from native events attached to HTML elements. It will
 * not work if called from a "passive" event, or after any event not coming from
 * user interaction (onLoad, onFocus, etc).
 *
 * It's recommended to use it on MouseEvents or TouchEvents.
 */
function openIosKeyboard() {
    const input = document.createElement("input");
    input.setAttribute("type", "text");
    input.setAttribute("style", "position: fixed; top: -100px; left: -100px;");
    document.body.appendChild(input);
    input.focus();
    // it's safe to remove the fake input after a 30s timeout
    setTimeout(() => {
        document.body.removeChild(input);
    }, 30 * 1000);
}
```

[This answer on stack overflow](https://stackoverflow.com/a/74851426/2777142) explains the code.
