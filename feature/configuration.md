# Configuration
[Back to Home](https://github.com/wizk3y/go-micro)

---
There are two type of app configuration: 
- Using [Go `flag` package](https://pkg.go.dev/flag).
- Load config data from file using [Viper](https://github.com/spf13/viper).

## Flags
- `--version`: application version. Default: `1.0.0`.
- `--dev`: is develop environment.
- `--config-file`: location of file which [Viper](https://github.com/spf13/viper) load.
- `--http-host`: host which application will listen and serve.
- `--http-port`: port which application will listen and serve.

## Viper keys
- `api.disable_trace_log`: disable logging incoming request and response.
- `api.timeout`: API application endpoint timeout. Default: `10000000000` equal `10 second`.
- [MySQL configuration](https://github.com/wizk3y/go-micro-doc/tree/master/feature/mysql_connection.md#configuration)
- [PostgreSQL configuration](https://github.com/wizk3y/go-micro-doc/tree/master/feature/postgresql_connection.md#configuration)
- [Redis configuration](https://github.com/wizk3y/go-micro-doc/tree/master/feature/redis_connection.md#configuration)

---
[Back to Home](https://github.com/wizk3y/go-micro)