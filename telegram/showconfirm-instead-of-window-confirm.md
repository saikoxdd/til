# window.confirm is not reliable inside Telegram

Some Telegram clients never show `window.confirm()` in a Mini App, so a "Cancel order?" dialog
silently returns immediately. Use the native popup and keep `confirm` only as a fallback:

```js
function ask(text) {
  const tg = window.Telegram?.WebApp
  if (tg?.initData && tg.isVersionAtLeast?.('6.2'))
    return new Promise(r => tg.showConfirm(text, ok => r(Boolean(ok))))
  return Promise.resolve(window.confirm(text))
}
```

Set your `busy` flag **before** awaiting it, otherwise a second tap opens a second dialog.
