# len(s) is bytes, maxlength is characters

The form said "up to 2000 characters", the server checked `len(notes) > 2000`.
Cyrillic is 2 bytes per letter, Chinese is 3, so a 1500-letter Russian comment was rejected.

```go
if utf8.RuneCountInString(notes) > 2000 { ... }
```

HTML `maxlength` counts UTF-16 code units, which is close enough to runes for this
(emoji count double there, which only makes the browser stricter).
