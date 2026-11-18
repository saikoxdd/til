# Reading the Central Bank of Russia rate

`https://www.cbr.ru/scripts/XML_daily.asp` returns XML in `windows-1251` with comma decimals.

```python
root = ET.fromstring(resp.content)          # bytes: the parser reads the encoding header
for v in root.iter("Valute"):
    if v.findtext("CharCode") == "CNY":
        rate = float(v.findtext("Value").replace(",", ".")) / int(v.findtext("Nominal"))
```

- parse `resp.content`, not `resp.text`
- divide by `Nominal` - some currencies are quoted per 10 or 100 units
- never call it per request: cache for ~30 min and keep the last good value if the CBR is down
