# Routing

## Route
Route has 3 params.
* One is the **request method**, which defines what request method could attach this route.  
* One is the **request uri**, which defines what request could attach this route.  
* Another one is the **handlers**, which defines how the program will handle the matched route request. you could define multiple handles for one route.

```go
group.POST("/hello", func(c *gin.Context){
	c.JSON(http.StatusOK, nil)
	return
})
```

## RouteGroup
Route group is a container, which you could defined a path `prefix`, and put a bunch of routes in it.  

```go
package groups

import (
	"github.com/totoval/framework/route"
	"totoval/app/http/controllers"
)

type AuthGroup struct {
	LoginController    controllers.Login
	RegisterController controllers.Register
}

func (ag *AuthGroup) Group(group route.Grouper) {
	group.POST("/login", ag.LoginController.Login)
	group.POST("/register", ag.RegisterController.Register)
}
```

## Version
Version is a group that contains `Auth` and `NoAuth` routes.
### Auth
Auth is a route group that specify the routes in it will be **under authenticated**, which means user in this app should login before request.
### NoAuth
NoAuth is a group that specify the routes is in **public access**, which means anyone could request data from it.

```go
package versions

import (
	"github.com/gin-gonic/gin"

	"github.com/totoval/framework/route"
	"totoval/routes/groups"
)

func NewV1(engine *gin.Engine) {
	ver := route.NewVersion(engine, "v1")

	// auth routes
	ver.Auth("", func(grp route.Grouper) {
		grp.AddGroup("/user", &groups.UserGroup{})
	})

	// no auth routes
	ver.NoAuth("", func(grp route.Grouper) {
		grp.AddGroup("", &groups.AuthGroup{})
		grp.AddGroup("/user-affiliation", &groups.UserAffiliationGroup{})
	})
}

```

## Provider
Provider is a place to switch Totoval's route version will be use.
```go
package routes

import (
	"github.com/gin-gonic/gin"

	"github.com/totoval/framework/route"
	"totoval/routes/versions"
)

func Register(router *gin.Engine) {
	defer route.Bind()

	versions.NewV1(router)
}
```
