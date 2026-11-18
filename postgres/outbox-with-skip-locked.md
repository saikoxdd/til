# A Telegram outbox on plain PostgreSQL

Write the notification in the same transaction as the business change, send it later:

```sql
SELECT q.id, q.chat_id, q.text
FROM telegram_outbox q
WHERE q.status = 'pending' AND q.next_attempt_at <= now()
ORDER BY q.id
LIMIT 1
FOR UPDATE OF q SKIP LOCKED;
```

`SKIP LOCKED` lets several workers run without stepping on each other, no broker needed.

Gotcha I hit: the worker slept 1 s after **every** message, so a backlog drained at 1 msg/s.
Sleep only when the queue is empty; after a successful send wait ~50 ms (Telegram allows ~30 msg/s).
