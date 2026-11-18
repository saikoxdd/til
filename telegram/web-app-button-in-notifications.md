# Open a Mini App on a specific page from a bot message

An inline button with `web_app` opens the Mini App right inside the chat, with fresh `initData`:

```json
{"inline_keyboard": [[{"text": "Open order", "web_app": {"url": "https://app.example.com/?order=42"}}]]}
```

- works only in **private chats** with the bot (fine for notifications)
- the URL must be **HTTPS**, no BotFather domain setup needed
- read the parameter on the frontend (`?order=` or `initDataUnsafe.start_param`) - it is navigation only, the backend still checks access

If Telegram answers `400` because of the button, resend the message without `reply_markup`: a notification without a button is better than no notification.
