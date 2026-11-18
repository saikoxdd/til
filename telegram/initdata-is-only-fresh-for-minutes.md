# Mini App initData goes stale on page reload

Telegram signs `initData` once, when the Mini App is opened. `auth_date` inside it is that moment.
If the backend rejects `initData` older than N minutes (a good idea against replay), a user who keeps
the Mini App open and hits **Reload page** sends the *same old* `initData` and gets kicked out.

Fix: treat Telegram login as the first try, not the only one.

```js
try {
  session = await api('/api/auth/telegram', { init_data: tg.initData })
} catch (e) {
  if (e.status !== 401) throw e
  // initData expired: maybe the HttpOnly session cookie is still alive
  session = await api('/api/me')
}
```

Only if both fail, ask the user to close and reopen the app from the chat - that issues fresh `initData`.
