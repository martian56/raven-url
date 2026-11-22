# raven-url

[![CI](https://github.com/martian56/raven-url/actions/workflows/ci.yml/badge.svg)](https://github.com/martian56/raven-url/actions/workflows/ci.yml)

Pulls a URL apart into its pieces and helps with query strings. Percent-encoding
is handled by `std/encoding`, so this leans on that and adds the parsing and the
query map on top. No third-party dependencies.

## Install

```toml
[dependencies]
"github.com/martian56/raven-url" = "v0.3.0"
```

## Usage

```rust
from "github.com/martian56/raven-url" import Url

let u = Url.parse("https://api.example.com:8080/v1/items?page=2&q=hi#top")
u.scheme      // "https"
u.host        // "api.example.com"
u.port        // "8080"
u.path        // "/v1/items"
u.fragment    // "top"
u.params()    // a map: { "page": "2", "q": "hi" }
```

Parsing is lenient. A relative-looking string like `example.com/path` still
parses; the scheme and port just come back empty, and `is_absolute()` tells you
whether both a scheme and host were there.

## Query strings

```rust
from "github.com/martian56/raven-url" import parse_query, build_query

let m = parse_query("page=2&q=hello%20world")   // { "page": "2", "q": "hello world" }

build_query({"name": "a b"})                     // "name=a%20b"
```

Values are percent-decoded on the way in and encoded on the way out. The bare
`encode` and `decode` helpers are there if you want them on their own.

## API

`Url.parse(s)` gives you a `Url`; read its `scheme`, `user`, `password`, `host`,
`port`, `path`, `query`, and `fragment` fields, and call `params()`,
`is_absolute()`, or `to_string()`. Credentials (`postgres://user:password@localhost:5432`)
come back percent-decoded in `user` and `password`, and an IPv6 host
(`http://[::1]:8080`) comes back without its brackets. The free functions are
`parse_query(q)`, `build_query(map)`, `encode(s)`, and `decode(s)`.

## License

MIT
