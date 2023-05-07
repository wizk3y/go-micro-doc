# PostgreSQL connection
[Back to Home](https://github.com/wizk3y/go-micro)

---
Some application is depend on database. And to support build application with less effort, a connection to your PostgreSQL is essential. `go-micro` using [pq](https://github.com/lib/pq) as driver package.

## How to create new connection
- You can archive that by simple using `NewPostgreConnection`. Below is extended version of sample application server.
```go
// pkg/app/webserver.go
package app

import (
	"github.com/wizk3y/go-micro/driver/database/postgresql"
	"github.com/wizk3y/go-micro/transhttp"
)

type server struct {
	basePath string
}

func NewServer() transhttp.WebServer {
	s := server{
		basePath: "/v1",
	}

	var conf *postgresql.Config

	// init db conn
	db, err := postgresql.NewPostgreConnection(conf)
	if err != nil {
		logger.Panic("error when create connection to database ", err)
	}

	// create manager/repository using conn

	return &s
}
```
- You can create your own config and provide to `NewPostgreConnection`.
```go
// pkg/app/webserver.go
	conf = &postgresql.Config{
		Host:         "127.0.0.1",
		Port:         "5432",
		User:         "username",
		Password:     "password",
		DatabaseName: "database_name",
	}
```
- Or you can quick create config by using `GetConfigWithPrefix` and provide prefix. By default if nil is provide to `NewPostgreConnection`, config will be load by `GetConfigWithPrefix` with `postgre` prefix.
```yaml
# config.yaml
your_custom_prefix:
  host: 127.0.0.1
  port: 5432
  user: username
  password: password
  database_name: database_name
  max_open_conn: 100
  max_idle_conn: 10
  conn_life_time: 600
```

```go
// pkg/app/webserver.go
	conf = postgresql.GetConfigWithPrefix("your_custom_prefix")
```

## Configuration
- `host`: database hostname.
- `port`: database port.
- `user`: database username.
- `password`: database password for username.
- `database_name`: database name.
- `max_open_conn`: maximum number of open connections to the database.
- `max_idle_conn`: maximum number of connections in the idle connection pool.
- `conn_life_time`: maximum amount of time a connection may be reused.


---
[Back to Home](https://github.com/wizk3y/go-micro)