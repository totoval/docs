# Validation

Totoval validation uses `gopkg.in/go-playground/validator.v9`.

## Request Validation
Request validation file should be placed at `app/http/requests` folder. And used at `app/http/controllers`.  
Here's a request validation file example for validate user login request:
 
```go
package requests

type UserLogin struct {
	Email    string `json:"email" binding:"required,email"`
	Password string `json:"password" binding:"required,min=8,max=24"`
}
```
 
 Using at controllers:  
 
```go

func (l *Login) Login(c *request.Context) {
	// validate and assign requestData
	var requestData requests.UserLogin
	if !l.Validate(c, &requestData, true) {
		return
	}
	
    ...
	...
}

```

All the controller's should embedded with `BaseController`, and `BaseController` has defined a `Validate` function, by using `Validate` function, Totoval use the request validation struct to validate the request data.  
If this function returns false, then you should directly return in the controller, and that will cause a http `422` error with the error message.


## Model Validation
```go
type User struct {
	ID        *uint      `gorm:"column:user_id;primary_key;auto_increment" validate:"omitempty"`
	Name      *string    `gorm:"column:user_name;type:varchar(100)" validate:"required"`
	Email     *string    `gorm:"column:user_email;type:varchar(100);unique_index;not null" validate:"required"`
	Password  *string    `gorm:"column:user_password;type:varchar(100);not null" validate:"required"`
	CreatedAt *zone.Time `gorm:"column:user_created_at"`
	UpdatedAt zone.Time  `gorm:"column:user_updated_at"`
	DeletedAt *zone.Time `gorm:"column:user_deleted_at"`
	model.BaseModel
}
```

When using helper function `m.H().Create()` and `m.H().Save()` in package `github.com/totoval/framework/helpers/m`, the `structData` passed into the function will be validated using the struct's `validate` tag.