# Pulling the URL out of a Poizon / Taobao "Share" text

Chinese apps don't share a link, they share a paragraph:

```
【得物】… https://dw4.co/t/A/HnT1or9oV Champion T-trainer 牛津绿/奶白, 点击链接直接打开
```

`<input type="url">` refuses it, and a naive `\S+` grabs `奶白,` with the URL.
The link ends at whitespace **or full-width punctuation**, which never appears inside these links:

```js
const re = /https:\/\/[^\s 　，。、；：！？「」【】『』（）《》“”‘’"'<>]+/g
```

Then trim trailing `.,;:!?)]}` and prefer the first match whose host is a known shop.
Taobao puts the product name in `「…」`, Poizon right after the URL until `, 点击链接`.
