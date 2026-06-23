# Anti-Hallucination Rules: Express.js

> Prevents common AI mistakes when working with Express.js projects.
> **Tested against Express 4.x/5.x RC — June 2026**

## Version Awareness

Check `package.json` for the `express` version.

- **Express 4.x**: Current stable. `Router`, `express.json()`, `express.urlencoded()`
- **Express 5.x (RC)**: Async error handling improved, path-to-regexp breaking changes, `res.redirect()` changes

---

## Middleware Order — Critical

Middleware runs in the order it is defined. Order mistakes are a leading cause of bugs.

```js
// Correct order
const app = express();

app.use(express.json());           // 1. Parse request body FIRST
app.use(express.urlencoded({ extended: true })); // 2. Parse URL-encoded bodies
app.use(cors());                   // 3. CORS headers
app.use(morgan('dev'));            // 4. Logging
app.use('/api', apiRouter);       // 5. Routes
app.use(errorHandler);            // 6. Error handler LAST
```

**The error handler must be last.** Express identifies error handlers by their 4-parameter signature `(err, req, res, next)`.

**Body parsing must be before routes.** If `express.json()` comes after a route, that route's `req.body` will be undefined.

---

## Error Handling

### Express 4: Async Errors Require Explicit try/catch

In Express 4, async route handlers do NOT automatically catch errors. You must wrap them manually or use a wrapper utility.

```js
// Bad: unhandled promise rejection crashes the server
app.get('/users', async (req, res) => {
  const users = await User.findAll(); // If this throws, Express doesn't catch it
  res.json(users);
});

// Good: explicit try/catch
app.get('/users', async (req, res, next) => {
  try {
    const users = await User.findAll();
    res.json(users);
  } catch (err) {
    next(err); // Pass to error handler
  }
});

// Good: wrapper utility
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.findAll();
  res.json(users);
}));
```

### Express 5: Async Errors Auto-Forwarded

In Express 5, rejected Promises in route handlers are automatically forwarded to `next(err)`. No wrapper needed.

---

## Common Hallucinations to Avoid

| Hallucinated Pattern | Correct Alternative |
|---------------------|-------------------|
| `app.use(bodyParser.json())` | `app.use(express.json())` — bodyParser is built-in since Express 4.16 |
| `req.body` without body-parser middleware | Must have `express.json()` or `express.urlencoded()` first |
| `res.send(404)` | `res.status(404).send('Not found')` or `res.sendStatus(404)` |
| `router.use('/')` for all routes | Just define routes directly — `router.get('/', ...)` |
| `app.use(express.static)` without a path | Must provide path: `app.use(express.static('public'))` |
| Defining error handler before routes | Error handler must be LAST |
| `next()` in an error handler without 4 params | Error handlers require `(err, req, res, next)` signature |

---

## Router Best Practices

```js
// Correct: modular router pattern
// routes/users.js
const router = express.Router();

router.get('/', getAllUsers);
router.get('/:id', getUserById);
router.post('/', createUser);

module.exports = router;

// app.js
app.use('/api/users', usersRouter);
```

Do not put all routes in `app.js`. Use `express.Router()` for modular organization.

---

## Request & Response

### req.params vs req.query vs req.body

| Source | Property | Example |
|--------|----------|---------|
| URL path: `/users/:id` | `req.params.id` | `/users/42` → `req.params.id === '42'` |
| Query string: `?page=2` | `req.query.page` | `?page=2` → `req.query.page === '2'` |
| POST body JSON | `req.body.email` | Requires `express.json()` middleware |

**All URL params and query strings are strings.** Convert with `parseInt`, `parseFloat`, or `Number()` as needed.

### Response Methods — Use the Right One

```js
res.json({ data });          // JSON response (sets Content-Type: application/json)
res.send('text');            // Text response
res.sendStatus(204);         // Status only, no body
res.status(400).json({ error: 'Bad request' }); // Status + JSON
res.redirect(301, '/new');   // Redirect
```

Do not call `res.json()` and then `res.send()` — multiple response calls throw an error.

---

## Security

- Use `helmet` for security headers in production.
- Always validate and sanitize `req.body` — never trust raw input.
- Use `cors()` with an explicit `origin` whitelist, not `origin: '*'` in production.
- Rate limit authentication endpoints with `express-rate-limit`.
- Never expose stack traces in production error responses.

```js
// Production error handler
app.use((err, req, res, next) => {
  const status = err.status || 500;
  res.status(status).json({
    error: process.env.NODE_ENV === 'production' ? 'Internal server error' : err.message
  });
});
```
