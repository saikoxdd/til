# Testing aiogram flows without a network

Subclass `BaseSession` and answer Bot API calls yourself - the real dispatcher, filters,
middlewares and FSM all run:

```python
class FakeSession(BaseSession):
    def __init__(self):
        super().__init__(); self.calls = []
    async def make_request(self, bot, method, timeout=None):
        self.calls.append(method)
        if isinstance(method, SendMessage):
            return Message(message_id=1, date=datetime.now(), chat=Chat(id=method.chat_id, type="private"))
        return True
    async def close(self): ...
    async def stream_content(self, *a, **kw): yield b""

bot = Bot("42:TEST", session=FakeSession())
await dp.feed_update(bot, Update(update_id=1, message=msg))
assert "Welcome" in bot.session.calls[-1].text
```

Whole scenarios (create order → track → update → delivered) run in ~1 s.
