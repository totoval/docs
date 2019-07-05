# Requests
Requests is a place for placing the app's request validation struct.

## Requests
```go
package requests

type UserLogin struct {
	Email    string `json:"email" binding:"required,email"`
	Password string `json:"password" binding:"required,min=8,max=24"`
}
```

## Use Controller
All the controllers are embedded with `BaseController`, So there's a func called `Validate`, Using this func to validate the request with the Requests struct. The returned value `isAbort` is about that the validate status. if `isAbort` is `true`, simply write a `return` at the controller's handle function body. And the matched error message would returned correctly with the http status `422`.  

```go
func Validate(c *gin.Context, _validator interface{}, onlyFirstError bool) (isAbort bool)
```

```go
func (l *Login) Login(c *gin.Context) {
	// validate and assign requestData
	var requestData requests.UserLogin
	if !l.Validate(c, &requestData, true) {
		return
	}
    ...
    ...
}
```