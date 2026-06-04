# Code

Design a system as a graph of narrowing scopes: a broad **capability** leads through **scopes** that each expose less, until only precise single-word **actions** remain, and every effect routes through one **conduit**. The path supplies the meaning, so names stay minimal, and the system teaches itself: you discover it by following chains, not by scanning docs. It is domain-agnostic; a library is the most detailed case, but these are all the same shape:

| Abstract | Library | CLI | REST | Config | UI Menu |
|---|---|---|---|---|---|
| Capability | root action | program | base URL | root namespace | top menu |
| Scope | category / item | subcommand group | resource segment | section | submenu |
| Action | single-word method | leaf command | method on path | leaf key | menu item |
| Conduit | call function | shared exec | request handler | loader / parser | dispatch |
| Context | context object | flags / env | headers / auth | environment | session state |

## Three laws

1. **Context is structural, not lexical.** The path gives meaning, so each step names only what it adds; descriptive names (`getTradesList`) are a smell, because the prefix duplicates context the path already carries. This scales up: `PaymentsContext` beside `MessagingContext` in one namespace is the same smell at module level; make it structural (a package) so the name collapses back to `Context`.
2. **Every scope is complete.** A scope that leads to a single child isn't a scope. Either **complete it** (add the reader/writer/remover that genuinely belong) or **collapse it** (expose the lone action one level up). A scope kept permanently around one action charges a step of ceremony for zero narrowing.
3. **One context per boundary.** A graph carries exactly one context, threaded from the root. A *second, independent* context (different identity, credentials, or lifecycle) is a new system: split it into its own package/module, where it again carries one `Context`. A mere *narrowing* of the same identity (a tenant, a sub-account) is not a new context but a scope within the graph (`session.tenant(id)…`). Compose by containment, never by merging contexts. (This is a DDD bounded context: one context, one ubiquitous language.)

## Naming

- **Roots** are verb-then-noun; the verb is a lifecycle contract. `create`/`define` establish a capability with nothing to release (default). `acquire`/`open` hand back a resource to release, so use them only when bound to a scoped-disposal construct (`using`, `with`, `defer`). The noun out-weighs the verb and is a real noun, not a nominalized verb (`createSession`, not `startRun`).
- **Categories** are plural/collective nouns (`accounts`, `trades`); a natural singleton takes a singular noun (`profile`, `settings`) and is entered directly.
- **Factories** are single words mirroring their category. **Actions** are single words (`balance`, `deposit`, `status`, `list`); the path supplies the rest. The single-word rule governs *identifiers*; a human label (a menu item) may be a short phrase (`Clear history`).
- **Selecting an item** is `get(id)`, which *narrows* to an item scope, as opposed to `list()`, which *reads* the collection. Promote an identifier to an item scope only when that item has its own actions or sub-scopes; if all you ever do with it is one keyed operation, keep the identifier as an argument on the collection action (`objects().download(key)`), because a one-action item scope would violate Law 2.
- Test: speak the chain aloud. "session, trades, get one, status" reads as instruction; "session, get trades list" does not.

## The shape (TypeScript)

The root validates options at the boundary into one frozen, immutable context, then returns the top scope. Scopes are frozen objects of closures over that context; every action delegates to one conduit; `require` fails fast and names the field. The root is a plain factory, not a constructor, so it may be async or fallible, and never reads globals. The conduit owns transport end to end: a non-JSON or binary body or response is a flag on the operation, never a second I/O path.

```ts
type SessionOptions = { userId: string; credentials: Credentials; ttl?: number };
type Context = Readonly<{ userId: string; ttl: number; credentials: Credentials }>;

function createSession(o: SessionOptions) {
  const ctx: Context = Object.freeze({
    userId:      require(o.userId, "userId"),
    ttl:         o.ttl ?? 300,
    credentials: require(o.credentials, "credentials"),   // explicit, never ambient
  });
  return Object.freeze({ accounts: () => accounts(ctx), trades: () => trades(ctx) });
}

function accounts(ctx: Context) {                          // a scope: frozen object of closures
  return Object.freeze({
    get:  (id: string) => account(ctx, id),
    list: ()           => call(ctx, { path: "accounts" }),
  });
}

function account(ctx: Context, id: string) {
  return Object.freeze({ balance: () => call(ctx, { path: `accounts/${id}/balance` }) });
}

function call<T>(ctx: Context, op: Operation): Promise<T> {  // the one conduit
  require(op.path, "op.path");
  const req = build(op);
  req.headers["X-User"] = ctx.userId;                        // credentials from context, never global
  return send(req).then(wrapErrors);
}

function require<T>(v: T, name: string): T {                 // fail fast, name the field
  if (v == null || v === "") throw new Error(`${name} is required`);
  return v;
}
```

Chain: `await createSession({ userId: "u1", credentials }).trades().get(1).status()`. Parallelism needs no helper, `Promise.all([a.balance(), b.balance()])`, because each action is independent and bound only to its context.

## Across languages

The shape is fixed; the mechanism is idiomatic. **Closures** (above) suit TS/JS, with no classes or `this`. **Immutable value + methods** suit Go, Rust, C#, and Python (frozen dataclasses): a scope is an immutable struct whose methods return narrower structs.

```go
func NewSession(o Options) (*Session, error) { /* validate, build ctx; unexported fields = immutable */ }
func (s *Session) Accounts() Accounts     { return Accounts{s.ctx} }
func (a Accounts) Get(id string) Account  { return Account{a.ctx, id} }
func (a Account) Balance() (Money, error) { return call[Money](a.ctx, op{Path: "accounts/" + a.id + "/balance"}) }
```

Only two things adapt: **where the noun lives** (`createSession`, or `Session.Create()` / `Session::create()` / `trading.NewSession()` where concatenation goes heavy) and **how construction reports failure** (a factory that may be async or return `Result`/`error`, never a throwing `new`).

## Applicability

Use when the domain has natural hierarchy, you expect additive growth, you want a uniform surface, and effects should centralize through one conduit. Avoid or adapt when the domain is flat, narrowing is multi-dimensional (you want a query, not a path), steps are order-dependent (wizards, transactions), or intermediate scope objects are too costly in a hot path.
