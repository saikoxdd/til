# Take the semaphore after reading the body, not before

```go
func handleUpload(w http.ResponseWriter, r *http.Request) {
    sem <- struct{}{}            // ❌ held while a phone uploads over 3G
    defer func() { <-sem }()
    r.ParseMultipartForm(1 << 20)
    decodeAndResize(...)
}
```

With one slot, a single slow client made every other upload fail with `503 busy`.
Only decoding is memory-heavy, so guard only that part:

```go
r.ParseMultipartForm(1 << 20)    // body goes to memory/disk, no semaphore
sem <- struct{}{}
defer func() { <-sem }()
decodeAndResize(...)
```

Measured locally: the second upload went from `503` to `200` in 0.03 s.
