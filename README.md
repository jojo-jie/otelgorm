# opentelemetry gorm


[OpenTracing](https://opentelemetry.io/docs/) instrumentation for [GORM](http://gorm.io/) [Tracing API](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#timestamp).

## Install

```
go get -u github.com/jojo-jie/otelgorm
```

## Usage

1. Call `otelgorm.NewPlugin(otelgorm.WithServiceName("sql"))` with an instance of your `*gorm.DB`.
2. Clone db `db = db.Use(plugin)` with a span.

Example:

```go
var gDB *gorm.DB

func init() {
    gDB = initDB()
}

func initDB() *gorm.DB {
    db, err := gorm.Open(mysql.New(mysql.Config{DSN: dsn,}), &gorm.Config{})
    if err != nil {
        panic(err)
    }
    plugin := otelgorm.NewPlugin(otelgorm.WithServiceName("sql"))
    err = db.Use(plugin)
    if err != nil {
        panic(err)
    }
    // register callbacks must be called for a root instance of your gorm.DB
    return db
}

func Handler(ctx context.Context) {
    // sql query
    db.WithContext(ctx).First
}
```

Call to the `Handler` function would create sql span with table name, sql method and sql statement as a child of handler span.

## Reference
- [Dapper, a Large-Scale Distributed Systems Tracing Infrastructure](https://www.researchgate.net/publication/239595848_Dapper_a_Large-Scale_Distributed_Systems_Tracing_Infrastructure)
- [OpenTracing](https://opentelemetry.io/docs/)
- [KubeCon2019 OpenTelemetry](https://static.sched.com/hosted_files/kccncosschn19chi/03/OpenTelemetry_%20Overview%20%26%20Backwards%20Compatibility%20of%20OpenTracing%20%2B%20OpenCensus%20-%20Steve%20Flanders%2C%20Omnition.pdf)
- [Kratos](https://go-kratos.dev/docs/getting-started/start/)
- [traces example](https://github.com/go-kratos/kratos/tree/main/examples/traces)



