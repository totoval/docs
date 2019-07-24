# Mutators

## Introduction
Accessors and mutators allow you to format Eloquent attribute values when you retrieve or set them on model instances. For example, you may want to get user information from database and response it to the browser, but you don't want users' password field exposed out.

## Accessors & Mutators

### Defining An Accessor
To define an accessor, create a `GetFooAttribute` method on your model where `Foo` is the "camel" cased name of the field in the model you wish to access. In this example, we'll define an accessor for the `Password` attribute. The accessor will automatically be called by Eloquent when attempting to retrieve the value of the `Password` attribute:

```go
type User struct {
	ID        *uint      `gorm:"column:user_id;primary_key;auto_increment"`
	Name      *string    `gorm:"column:user_name;type:varchar(100)"`
	Email     *string    `gorm:"column:user_email;type:varchar(100);unique_index;not null"`
	Password  *string    `gorm:"column:user_password;type:varchar(100);not null"`
	CreatedAt *zone.Time `gorm:"column:user_created_at"`
	UpdatedAt zone.Time  `gorm:"column:user_updated_at"`
	DeletedAt *zone.Time `gorm:"column:user_deleted_at"`
	model.BaseModel
}
// Mutator: auto hidden password
func (user *User) GetPasswordAttribute(value interface{}) interface{} {
	return ptr.String("xxx")
}
```

As you can see, the original value of the column is passed to the accessor, allowing you to manipulate and return the value. To access the value of the accessor, you may access the `Password` attribute on a model instance:

```go
newUser := User{
    ID: ptr.Uint(userId),
}
if err := m.H().First(&newUser, false); err != nil {
    return err
}
password := newUser.Password
```

And the `password` variable's value would be `xxx`.

### Defining A Mutator
To define a mutator, create a `SetFooAttribute` method on your model where `Foo` is the "camel" cased name of the field in the model you wish to access. In this example, we'll define a mutator for the `Name` attribute. The mutator will automatically be called by Eloquent when attempting to update the value of the `Name` attribute in database:

```go
type User struct {
	ID        *uint      `gorm:"column:user_id;primary_key;auto_increment"`
	Name      *string    `gorm:"column:user_name;type:varchar(100)"`
	Email     *string    `gorm:"column:user_email;type:varchar(100);unique_index;not null"`
	Password  *string    `gorm:"column:user_password;type:varchar(100);not null"`
	CreatedAt *zone.Time `gorm:"column:user_created_at"`
	UpdatedAt zone.Time  `gorm:"column:user_updated_at"`
	DeletedAt *zone.Time `gorm:"column:user_deleted_at"`
	model.BaseModel
}
// Mutator: auto generate user name
func (user *User) SetNameAttribute(value interface{}) {
	user.Name = ptr.String(hash.Md5(*user.Email))
}
```

As you can see, the original value of `Name` is passed to the mutator, and the mutator set the `user.Name` to `user.Email`'s MD5 as the random `Name`, and then update the `Name` field as defined in the database.