# ios-timeout-focus

How to solve the timeout and focus issue in iOS.

The demo link: https://xcatliu.github.io/ios-timeout-focus/

In iOS, directly calling `input.focus()` works as normal.

```js
focusButton.addEventListener('click', () => {
    textInput.focus();
});

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

[This answer on stack overflow](https://stackoverflow.com/a/74851426/2777142) explains the code.
