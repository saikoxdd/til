# "Router is already attached" in tests

Routers are module-level objects. Build a fresh `Dispatcher` per test and the second
`include_router` raises `RuntimeError: Router is already attached`.

In the test fixture, detach them before building the dispatcher:

```python
for module in (common, admin, tracking):
    module.router._parent_router = None
dp = build_dispatcher(settings, sessionmaker)
```

The cleaner long-term fix is a `make_router()` factory per module.
