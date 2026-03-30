# Example: Complex Feature (Full Preflight)

> A developer asks their AI agent to add rate limiting to an API. The agent runs a thorough Preflight.

## The Task

**User:** "Add rate limiting to our API endpoints."

## Preflight

### Phase 1: Orient

**Agent:** I explored the codebase:
- `api/server.go` — main HTTP server using `chi` router
- `api/middleware/` — existing middleware: `auth.go`, `logging.go`, `cors.go`
- `go.mod` — Go 1.22, no rate limiting dependencies currently
- `config/config.go` — config loaded from env vars via `envconfig`
- Recent commits show the API serves ~20 endpoints across 4 route groups

The middleware pattern is consistent: each middleware is a `func(next http.Handler) http.Handler` applied via `r.Use()` in `server.go`.

### Phase 2: Clarify

**Agent:** What scope of rate limiting do you need?

- A) Per-IP rate limiting (protect against individual abuse)
- B) Per-API-key rate limiting (protect against customer overuse)
- C) Global rate limiting (protect the server from total overload)
- D) Some combination

**User:** A — per-IP for now.

**Agent:** Should rate-limited requests get a `429 Too Many Requests` with a `Retry-After` header, or just a plain 429?

**User:** With `Retry-After`.

**Agent:** What limit? Requests per minute?

**User:** 60 requests per minute per IP. Configurable via env var.

### Phase 3: Approach

**Agent:** Three approaches:

**A) Token bucket with `golang.org/x/time/rate` (recommended)**
- Standard library adjacent, well-tested
- Per-IP buckets stored in a sync.Map with periodic cleanup
- Pros: no external dependency, simple, fast
- Cons: in-memory only (not shared across instances)

**B) Redis-backed sliding window with `go-redis`**
- Shared state across instances
- Pros: works with horizontal scaling
- Cons: adds Redis dependency, more complex, you're running single instance today

**C) Middleware library like `httprate`**
- Pre-built, handles cleanup and headers
- Pros: least code to write
- Cons: less control, another dependency

I recommend **A** — you're single-instance, the standard library rate limiter is battle-tested, and it follows your existing middleware pattern exactly. Move to B when you scale horizontally.

**User:** A.

### Phase 4: Confirm

**Agent:** Plan:
- Create `api/middleware/ratelimit.go` — token bucket rate limiter middleware
- Create `api/middleware/ratelimit_test.go` — test cases for: under limit, at limit, over limit, Retry-After header, IP extraction
- Modify `config/config.go` — add `RateLimitPerMinute` field (default 60)
- Modify `api/server.go` — add `r.Use(middleware.RateLimit(cfg.RateLimitPerMinute))`

Shall I proceed?

**User:** Yes.

### Phase 5: Execute

*Agent implements the rate limiter, tests, config, and server wiring.*

## Without Preflight

The agent might have immediately pulled in a third-party library, or implemented global rate limiting instead of per-IP, or hardcoded the limit instead of making it configurable — all requiring rework.
