# Controllers
Controller is the last handle in one route. It process the request and return the processed result out.  
A Controller must embedded with `BaseController` defined in `github.com/totoval/framework/http/controller`.

```go
package controllers

import (
	"net/http"

	"github.com/gin-gonic/gin"

	"github.com/totoval/framework/helpers/m"
	"github.com/totoval/framework/helpers/ptr"
	"github.com/totoval/framework/http/controller"
	"github.com/totoval/framework/model"
	"totoval/app/models"
)

type User struct {
	controller.BaseController
}

func (u *User) Info(c *gin.Context) {
	if u.Scan(c) {
		return
	}
	user := u.User().Value().(*models.User)

	user.Password = ptr.String("") // remove password value for response rendering
	c.JSON(http.StatusOK, gin.H{"data": user})
	return
}
```