# HTTP middleware
[Back to Home](https://github.com/wizk3y/go-micro)

---
There are 5 middleware that default coming with toolkit. Disable/modify option will be updated on [Configuration](https://github.com/wizk3y/go-micro-doc/tree/master/feature/configuration)
- Cross-Origin-Request-Site (CORS): Auto add CORS Header (`Access-Control-Allow-Origin`,`Access-Control-Allow-Methods`,`Access-Control-Allow-Headers`) to response.
- HTTP trace log: Add log for each incoming request and response.
- Pre-fight: Auto response with status code 200 when receive request with method `OPTIONS`.
- Recovery: Sometime developer made mistake when develop a feature, this will help application resilent/high-availablity by recover any panic when handling request.
- Timeout: Every request should be response less than a pre-define duration. A duration is defined by `api.timeout` [Viper configuration](https://github.com/wizk3y/go-micro-doc/tree/master/feature/configuration).

## Custom middleware
### How to create new middleware
Each toolkit's middlewares above is an implement of [negroni](https://github.com/urfave/negroni)'s `Handler`. And two apply way below also using [negroni](https://github.com/urfave/negroni)'s `Handler`. Just implement [negroni](https://github.com/urfave/negroni)'s `Handler` then you have your custom middleware that can use anywhere.
```go
package middleware

import (
	"net/http"
)

type CorsMiddleware struct {
}

// ServeHTTP --
func (m *CorsMiddleware) ServeHTTP(rw http.ResponseWriter, r *http.Request, next http.HandlerFunc) {
	rw.Header().Add("Access-Control-Allow-Origin", "*")
	rw.Header().Add("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
	rw.Header().Add("Access-Control-Allow-Headers", "X-Requested-With, Content-Type, Authorization")

	next(rw, r)
}
```

### Apply for whole application
As you can see when implement `transhttp.WebServer`, there is an method called `InitMiddlewares` that return an slice of [negroni](https://github.com/urfave/negroni)'s `Handler`. So just simple append any handler that implement [negroni](https://github.com/urfave/negroni)'s `Handler` to slice above.

### Apply for single handler
On each `transhttp.Route` also have a property name `Middlewares` and have typed is slice of [negroni](https://github.com/urfave/negroni)'s `Handler`. Yeah, just simple append handler that implement [negroni](https://github.com/urfave/negroni)'s `Handler` to slice and middleware will be executed right before execute route handler.

---
[Back to Home](https://github.com/wizk3y/go-micro)