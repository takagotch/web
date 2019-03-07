### web
---
https://github.com/gocraft/web

```go
package main

import (
  "github.com/gocraft/web"
  "fmt"
  "net/http"
  "strings"
)

type Context struct {
  HelloCount int
}

func (c *Context) SetHelloCont(rw web.ResponseWriter, req *web.Request, next web.NextMiddlewareFunc) {
  c.HelloCount = 3
  next(rw, req)
}

func (c *Context) SayHello (rw web.ResponseWriter, req *web.Request) {
  fmt.Fprintf(rw, strings.Repeat("Hello ", c.HelloCount), "World!")
}

func main() {
  router := web.New(Context{}).
    Middleware(web.LoggerMiddleware).
    Middleware(web.ShowErrorsMiddleware).
    Middleware((*Context).SetHelloCount).
    Get("/", (*Context).SayHello)
  http.ListenAndServe("localhost:3000", router)
}


router := web.New(YourContext{})

type YourContext struct {
  User *User
}

router := web.New(YourContext{})
router.Get("/users", (*YourContext).UserList)
router.Post("/users/:id", (*YourContext).UsersCreate)
router.Put("/users/:id", (*YourContext).UsersDelete)
router.Delete("/users/:id", (*YourContext).UsersDelete)
router.Patch("/users/:id", (*YourContext).UsersUpdate)
router.Get("/", (*YourContext).Root)


func (c *YourContext) Root(rw web.ResponseWriter, req *web.Request) {
  if c.User != nil {
    fmt.Fprint(rw, "Hello,", c.User.Name)
  } else {
    fmt.Fprint(rw, "Hello, anonymous person")
  }
}

func Root(c *YourContext, rw web.ResponseWriter, req *web.Request) {}

func Root(rw web.ResponseWriter, req *web.Request) {}

router := web.New(YourContext{})
router.Middleware((*YourContext).UserRequired)

func (c *YourContext) UserRequired(rw web.ResponseWriter, r, *web.Request, next web.NextMiddlewareFunc) {
  user := userFromSession(r)
  if user != nil {
    c.User = user
    next(rw, r)
  } else {
    rw.Header().Set("Location", "/")
    rw.WriteHeader(http.StatusMovedPermanently)
  }
}


func GenericMiddleware(rw web.ResponseWriter, r *web.Request, next web.NextMiddlewareFunc) {
}


type Context struct {
  Sesssion map[string]string
}

type AdminContext struct {
  *Context
  CurrentAdmin *User
}

type ApiContext struct {
  *Context
  AccessToken string
}

rootRouter := web.New(Context{})
rootRouter.Middleware((*Context).LoadSession)

apiRouter := rootRouter.Subrouter(ApiContext{}, "/api")
apiRouter.Middleware((*Context).OAuth)
apiRouter.Get("/tickets", (*ApiContext).TicketsIndex)

adminRouter := rootRouter.Subrouter(AdminContext{}, "/admin")
adminRouter.Middleware((*AdminContext).AdminRequired)

adminRouter.Get("/reports", (*AdminContext).Reports)


router.Get("/suggestions/:suggesion_id/comments/:comment_id")

func (c *YourContext) Root(rw web.ResponseWriter, req *webRequest) {
  fmt.Fprint(rw, "Suggestion ID:", req.PathParams["suggestion_id"])
  fmt.Fprint(rw, "Comment ID:", req.PathParams["comment_id"])
}


router.Get("/suggestions/:suggestion_id:\\d.*/comments/:comment_id:\\d.")
router.Get("/suggestions/:suggesiton_id/comments/:comment_id/:")

router.NotFound((*Context).NotFound)

func (c *Context) NotFound(rw web.ResponseWriter, r *web.Request) {
  rw.WriteHeader(http.StatusNotFound)
  fmt.Fprintf(rw, "My Not Found")
}

router.OptionsHandler((*Context).OptionsHandler)

func (c *Context) OptionsHandler(rw web.ResponseWriter, r *web.Request, methods []string) {
  rw.Header().Add("Access-Control-Allow-Methods", strings.Join(methods, ", "))
  rw.Header().Add("Access-Control-Allow-Origin", "*")
}

router.Error((*Context).Error)


func (c *Context) Error(rw web.ResponseWriter, r *web.request, err interface{}) {
  rw.WriteHeader(http.StatusInternalServerError)
  fmt.Fprint(w, "Error", err)
}

router := web.New(Context{})
router.Middleware(web.LoggerMiddleware).
  Middleware(web.ShowErrorsMiddleware)

currentRoot, _ := os.Getwd()
router.Middleware(web.StaticMiddleware(path.Join(currentRoot, "public"), web.StaticOptions{IndexFile: "index.html"}))

router := web.New(Context{})
router.Middleware(web.LoggerMiddleware)
if MyEnvironment == "development" {
  router.Middleware(web.ShowErrorMiddleware)
}

router := web.New(Context{})
http.ListenAndServe("localhost:8080", router)

fmt.Fprintf(rw, "<html>I'm a web page!</html>")
```

```
go get github.com/gocraft/web

go run src/myapp/server.go
```

```
```


