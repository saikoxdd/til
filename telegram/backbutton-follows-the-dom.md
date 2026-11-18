# Syncing Telegram BackButton with a vanilla JS app

No router? Let the DOM decide. If the screen has an in-app "← back" link, show the native button:

```js
const back = Telegram.WebApp.BackButton
const sync = () => app.querySelector('a.back') ? back.show() : back.hide()
new MutationObserver(sync).observe(app, { childList: true })
back.onClick(() => app.querySelector('a.back')?.click())
```

One observer instead of `show()/hide()` calls scattered over every render function.
