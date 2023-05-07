# Organize project as micro-services (multiple repositories - best practice)
[Back to Home](https://github.com/wizk3y/go-micro)

---
A micro-services contains many application working as a services to another application. And number of contributor to your project is scale at massive level (100+ people), and each services/domains you have a group of people responsibl. This practice maybe fit with you, which help to reduce change that affect from this services to another services. But always remember the problem is how good your guideline and other compliance, technology just a tool to help you solve problem with less effort.
```
your-project-common/
├── go.mod
├── go.sum
├── model/
│   ├── a.go
│   ├── b.go
│   └── c.go
└── manager/
    ├── manager_a.go
    ├── manager_b.go
    └── manager_c.go
```

```
your-project-app/
├── go.mod
├── go.sum
└── cmd/
    ├── monolith/
    │   └── main.go
    ├── service-a/
    │   └── main.go
    ├── service-b/
    │   └── main.go
    └── service-c/
        └── main.go
```

```
your-project-domain-a/
├── go.mod
├── go.sum
├── app/
│   └── webserver.go
└── handler/
    ├── get_handler.go
    ├── post_handler.go
    └── put_handler.go
```

```
your-project-domain-b/
├── go.mod
├── go.sum
├── app/
│   └── webserver.go
└── handler/
    ├── get_handler.go
    ├── post_handler.go
    └── put_handler.go
```

```
your-project-domain-c/
├── go.mod
├── go.sum
├── app/
│   └── webserver.go
└── handler/
    ├── get_handler.go
    ├── post_handler.go
    └── put_handler.go
```

## What different
- Each domain has a repository that contains logic of that repository.
- The `your-project-common` repository contains re-usable package, which import by domain repository package.
- The `your-project-app` repository is contains all of `main.go` which contains `package main`, to start or build service. The reason I create this repository is I think `main.go` is simple as a base file, and every logic is contains in domain repository. It create a space for devops engineer working, as writing a script to deploy, or Dockerfile, ...
- A little add-on to `your-project-app` is `monolith` directory. You can run all services as one by calling `AddServer` multiple times.
```go
package main

import (
	"net/http"

	appA "domain-a/pkg/app"
	appB "domain-b/pkg/app"
	"github.com/wizk3y/go-micro/logger"
	"github.com/wizk3y/go-micro/micro"
)

func main() {
	microApp, hs := micro.NewAPIApp("your-api-server-name")

	appAServer := appA.NewServer()
	microApp.AddServer(appAServer)

	appBServer := appB.NewServer()
	microApp.AddServer(appBServer)

	err := microApp.RunAPI(hs)
	if err != nil {
		if err.Error() == http.ErrServerClosed.Error() {
			logger.Info(http.ErrServerClosed.Error())
		} else {
			logger.Errorf("HTTP server closed with error: %v", err)
		}
	}
}
```


---
[Back to Home](https://github.com/wizk3y/go-micro)