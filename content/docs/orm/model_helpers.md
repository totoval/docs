# Model Helpers

## Create
```go
import (
	"github.com/totoval/framework/helpers/m"
    "fmt"
)

func Create(){
	user := User{
		Username: ptr.String("totoval"),
		Password: ptr.String("passw0rd"),
	}
	if err := m.H().Create(&user); err != nil{
		panic(err)
	}
	
	fmt.Println(user)
}
```

## Save
```go
import (
	"github.com/totoval/framework/helpers/m"
	"github.com/totoval/framework/helpers/ptr"
	"github.com/totoval/framework/model/types/null"
    "fmt"
)

func Save(){
	user := User{
		Id: 1,
		Username: "totoval",
		Password: "passw0rd",
		Nickname: "mynick",
	}
	userModified := User{
		Password: "passw0rd123",
		Nickname: null.String(),
	}
	if err := m.H().Save(&user, userModified, ); err != nil{
		panic(err)
	}
	
	fmt.Println(user)
	// the user will be saved as User{Id: 1, Username: "totoval", Password: "passw0rd123", Nickname: null}
}
```

## Query
```go
import (
	"github.com/totoval/framework/helpers/m"
	"fmt"
)

func Query(){
	user := User{
		Id: 1,
	}
	if err := m.H().First(&user); err != nil{
		panic(err)
	}
	
	fmt.Println(user)
}
```

## Delete

```go
m.H().Delete(user, false)
````

## Restore

```go
m.H().Restore(user)
```