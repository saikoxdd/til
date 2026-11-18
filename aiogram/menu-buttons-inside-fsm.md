# Reply-keyboard buttons get eaten by FSM state handlers

In aiogram 3 a handler **without** a state filter fires in any state, but a handler registered
for a state catches *every* message in it. So the user is in `Calc.price`, taps "📦 My orders"
and the calculator answers "enter a number".

Exclude menu texts from state handlers:

```python
MENU = frozenset({texts.MY_ORDERS, texts.FAQ, ...})

@router.message(Calc.price, ~F.text.in_(MENU))
async def calc_price(message: Message, state: FSMContext): ...
```

The message falls through to the router that handles the button, which clears the state.
