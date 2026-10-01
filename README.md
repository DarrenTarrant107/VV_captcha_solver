# VV-Captcha-Solver

Get a VV-Captcha token from a page URL. Python, TypeScript, and Go all do the same thing: GET `/handler/vv_captcha/code` on that host, then read `var token = "..."` from the body.

## Setup

```bash
git clone https://github.com/alexabrahall/VV-Captcha-Solver.git
cd VV-Captcha-Solver
```

| Language | Needs |
| --- | --- |
| Python 3.6+ | `pip install requests` |
| Node 14+ | `npm install` (`axios`) |
| Go 1.16+ | standard library only |

## Usage

The hostname comes from the URL you pass in. That same URL is sent as the `Referer`.

### Python

```python
from src.python.main import solve_vv_captcha

token = solve_vv_captcha("https://uk-umg.com/um-forms/48709-1272284.html")
print(token)
```

See [`examples/example.py`](examples/example.py).

### TypeScript

```typescript
import { solveVVCaptcha } from "./src/typescript/index";

const token = await solveVVCaptcha(
  "https://uk-umg.com/um-forms/48709-1272284.html"
);
console.log(token);
```

See [`examples/example.ts`](examples/example.ts).

### Go

```go
token, err := captcha.SolveVVCaptcha("https://uk-umg.com/um-forms/48709-1272284.html")
if err != nil {
    log.Fatal(err)
}
fmt.Println(token)
```

See [`examples/example.go`](examples/example.go). Import path in that file is `../src/golang`.

## Responses

| Status | Result |
| --- | --- |
| 200 | Token returned |
| 403 | Host is not a VV-Captcha domain |
| 500 | Handler rejected the request |
| other | Error includes the status code |

A 200 with no `var token` assignment in the body fails with `Failed to extract captcha id from response`.

## Layout

```
src/python/main.py        solve_vv_captcha(url) -> str
src/typescript/index.ts   solveVVCaptcha(url) -> Promise<string>
src/golang/captcha.go     SolveVVCaptcha(url) (string, error)
examples/
```

## License

[ISC](LICENSE). Only use this where you are allowed to, and stay inside the site's rate limits.
