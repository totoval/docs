# Release Notes

## Versioning Scheme
Totoval's versioning scheme maintains the following convention: `paradigm.major.minior-hotfix-n`.
For now, Major framework release `v1.x.x` are released when totoval is ready for production use,
while minio release my be released as often as new feature has been added. Minor releases should 
**never** contain breaking changes.

When referencing the Totoval framework from your application, you should always use the version
which Totoval is used. for example, Totoval's version is `v0.8.1`, Totoval framework's version
should also be `v0.8.1`.

## Totoval v0.8.x
Totoval `v0.8.x` upgrades the framework's underlying Totoval components to the `v0.8.x` series.
### Compare
https://github.com/totoval/totoval/compare/v0.7.11...v0.8.0#diff-57e6facd16b7a7d9e9b25513649387ffL31
### Migrate to v0.8.x
#### 1. Configuration `config/auth.go `
`>= v0.8.x` added:
```go
auth["model_ptr"] = &models.User{} // must be a pointer
```

#### 2. Bootstrap `bootstrap/app.go`  
`>= v0.8.x` added:  

```go
policy.Initialize()
```

after `lang.Initialize()`

#### 3. Routes 
* `routes/provider.go`  
`< v0.8.x` :  

```go
...
...
func Register(router *gin.Engine) {
	version := &versions.V1{Prefix: "v1"}
 	version.Register(router)
}
...
...
```

`>= v0.8.x`:  

```go
...
...
func Register(router *gin.Engine) {
	defer route.Bind()
 	versions.NewV1(router)
}
...
...
```

* `routes/versions/v1.go`  

`< v0.8.x`:  

```go
...
...
type V1 struct {
	Prefix string
}

func (v1 *V1) Register(router *gin.Engine) {
	version := router.Group(v1.Prefix)
	{
		v1.noAuth(version)
		v1.auth(version)
	}
}

func (v1 *V1) noAuth(group *gin.RouterGroup) {
	noAuthGroup := group.Group("")

	{
		route.RegisterRouteGroup(&groups.AuthGroup{}, noAuthGroup)
		route.RegisterRouteGroup(&groups.UserAffiliationGroup{}, noAuthGroup)
	}
}

func (v1 *V1) auth(group *gin.RouterGroup) {
	authGroup := group.Group("", middleware.AuthRequired())

	{
		route.RegisterRouteGroup(&groups.UserGroup{}, authGroup)
	}
}
```

`>= v0.8.x`:  

```go
...
...

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

* `routes/groups/xxx.go`

`< v0.8.x` example:

```go
...
...
func (ag *AuthGroup) Register(group *gin.RouterGroup) {
	newGroup := group.Group("")
	{
		newGroup.POST("/login", ag.LoginController.Login)
		newGroup.POST("/register", ag.RegisterController.Register)
	}
}
```

`>= v0.8.x` example:

```go
...
...
func (ag *AuthGroup) Group(group route.Grouper) {
	group.POST("/login", ag.LoginController.Login)
	group.POST("/register", ag.RegisterController.Register)
}
```


#### 4. Controllers  `app/http/controllers/xxx.go`
`< v0.8.x` example:

```go
func (*User) Info(c *gin.Context) {
	userID, isAbort := middleware.AuthClaimsID(c)
	if isAbort {
		return
	}
	user := models.User{
		ID: &userID,
	}

	if err := m.H().First(&user, false); err != nil {
		c.JSON(http.StatusUnprocessableEntity, gin.H{"error": err.Error()})
		return
	}
	...
	...
	...
	return
}
```

`>= v0.8.x` example:

```go
func (*User) Info(c *gin.Context) {
        if u.Scan(c) {
            return
        }
        user := u.User().Value().(*models.User)
        ...
        ...
        ...
        
        return
}
```

#### 5. Model `app/models/user.go` or your own user model
`>= v0.8.x` added:

```go
...
...
func (user *User) Scan(userId uint) error {
	newUser := User{}
	if err := m.H().First(&newUser, false); err != nil {
		return err
	}
	*user = newUser
	return nil
}
func (user *User) Value() interface{} {
	return user
}
...
...
```

## Totoval v0.7.x
Totoval `v0.7.x` upgrades the framework's underlying Totoval components to the `v0.7.x` series.

## Totoval v0.6.x
Totoval `v0.6.x` upgrades the framework's underlying Totoval components to the `v0.6.x` series.




