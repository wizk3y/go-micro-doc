# MySQL connection
[Back to Home](https://github.com/wizk3y/go-micro)

---
Some application is depend on database. And to support build application with less effort, a connection to your MySQL is essential.

## How to create new connection
- You can archive that by simple using `NewMySQLConnection`. Below is extended version of sample application server.
```go
// pkg/app/webserver.go
package app

import (
	"github.com/wizk3y/go-micro/driver/database/mysql"
	"github.com/wizk3y/go-micro/transhttp"
)

type server struct {
	basePath string
}

func NewServer() transhttp.WebServer {
	s := server{
		basePath: "/v1",
	}

	var conf *mysql.Config

	// init db conn
	db, err := mysql.NewMySQLConnection(conf)
	if err != nil {
		logger.Panic("error when create connection to database ", err)
	}

	// create manager/repository using conn

	return &s
}
```
- You can create your own config and provide to `NewMySQLConnection`.
```go
// pkg/app/webserver.go
	conf = &mysql.Config{
		Host:         "127.0.0.1",
		Port:         "3306",
		User:         "username",
		Password:     "password",
		DatabaseName: "database_name",
	}
```
- Or you can quick create config by using `GetConfigWithPrefix` and provide prefix. By default if nil is provide to `NewMySQLConnection`, config will be load by `GetConfigWithPrefix` with `mysql` prefix.
```yaml
# config.yaml
your_custom_prefix:
  host: 127.0.0.1
  port: 3306
  user: username
  password: password
  database_name: database_name
  max_open_conn: 100
  max_idle_conn: 10
  conn_life_time: 600
```

```go
// pkg/app/webserver.go
	conf = mysql.GetConfigWithPrefix("your_custom_prefix")
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