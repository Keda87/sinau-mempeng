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
Nanti kalo udah bisa diakses disini [http://target-host.com/debug/pprof/](debug/pprof/).

Semisal pengen analisa durasi waktu tertentu, bisa tambahin query param `?seconds=...` untuk ambil data dalam rentang waktu tersebut.

Contoh: [http://target-host.com/debug/pprof/heap/?seconds=30](/debug/pprof/heap/?seconds=30) untuk ambil data heap selama 30 detik.

### Cara Analisa Menggunakan pprof

Setelah download file binary, analisa dengan web UI bisa dengan command berikut.
```bash
$ go tool pprof -http=:8090 <nama_file>
```

Referensi:
- https://stackademic.com/blog/profiling-go-applications-in-the-right-way-with-examples
- https://github.com/google/pprof/blob/main/doc/README.md
