# Profiling GO Pake PPROF

### Cara Setup pprof di Echo Framework
```go
package main

import (
    "net/http"
    _ "net/http/pprof"
    "github.com/labstack/echo/v4"
)

func main() {
    e := echo.New()

    // routing pprof.
    e.GET("/debug/*", echo.WrapHandler(http.DefaultServeMux))

    // opsional. kalo gak pake middleware RemoveTrailingSlash ini di ignore aja. 
    // kalo pake, make perlu di skip routing pprof di middleware RemoveTrailingSlash.
    e.Use(middleware.RemoveTrailingSlashWithConfig(middleware.TrailingSlashConfig{
        Skipper: func(c echo.Context) bool {
            return strings.Contains(c.Request().RequestURI, "/debug")
        },
    }))
}


```
