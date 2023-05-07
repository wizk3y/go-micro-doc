# Organize project as micro-services (single repository - best practice)
[Back to Home](https://github.com/wizk3y/go-micro)

---
A micro-services contains many application working as a services to another application. Below is example with 3 application/service on same repository.
```
your-project/
├── go.mod
├── go.sum
├── cmd/
│   ├── service-a/
│   │   └── main.go
│   ├── service-b/
│   │   └── main.go
│   └── service-c/
│       └── main.go
└── pkg/
    ├── common/
    │   ├── model/
    │   │   ├── a.go
    │   │   ├── b.go
    │   │   └── c.go
    │   └── manager/
    │       ├── manager_a.go
    │       ├── manager_b.go
    │       └── manager_c.go
    ├── domain-a/
    │   ├── app/
    │   │   └── webserver.go
    │   └── handler/
    │       ├── get_handler.go
    │       ├── post_handler.go
    │       └── put_handler.go
    ├── domain-b/
    │   ├── app/
    │   │   └── webserver.go
    │   └── handler/
    │       ├── get_handler.go
    │       ├── post_handler.go
    │       └── put_handler.go
    └── domain-c/
        ├── app/
        │   └── webserver.go
        └── handler/
            ├── get_handler.go
            ├── post_handler.go
            └── put_handler.go
```

## What different
- A new layer of `cmd` and `pkg` to contains multiple applications/services.
- A new directory name `common` to contains re-useable package, which import by domain package.

---
[Back to Home](https://github.com/wizk3y/go-micro)