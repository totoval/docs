# Authorization

# Introduction
In addition to providing authentication services out of the box, Totoval also provides a simple way to authorize user actions against a given resource.

Think of policies like routes and controllers, Policies provide a simple interface which defined the resource that could or not be attached by user using which action. Such as a post should only be edited by its' creator.

# Creating Polices
Polices are structs that organize authorization logic around a particular model or resource. For example, if your application is a blog, you may have a `Post` model and a corresponding `PostPolicy` to authorize user actions such as creating or updating posts.

Polices should implement the interface `Policier`, which is defined at `github.com/totoval/framework/policy/policy.go`.

Here's an example of `PostPolicy`:
```go
package policies

import (
	"strconv"

	"github.com/totoval/framework/helpers/m"
	"totoval/app/models"
	"github.com/totoval/framework/model"
)

type postPolicy struct {
	post *models.Post
}

func NewPostPolicy(post *models.Post) *postPolicy {
	return &postPolicy{post: post}
}

func (pp *postPolicy) Before(IUser model.IUser, routeParamMap map[key]value) *bool {
	return nil
}
func (pp *postPolicy) Create(IUser model.IUser, routeParamMap map[string]string) bool { return true }
func (pp *postPolicy) Update(IUser model.IUser, routeParamMap map[string]string) bool      {
	// get current user
	currentUser := IUser.Value().(*models.User)

	// get current post
	postIdStr, ok := routeParamMap["postId"]
	
	...
	... // convert postIdStr and get currentPost from db
	...
	
	// user only can edit his own post
	if *currentUser.Id == *currentPost.UserId {
		return true
	}
	return false
}
func (pp *postPolicy) Delete(IUser model.IUser, routeParamMap map[string]string) bool      { return true }
func (pp *postPolicy) ForceDelete(IUser model.IUser, routeParamMap map[string]string) bool { return true }
func (pp *postPolicy) View(IUser model.IUser, routeParamMap map[string]string) bool { return true }
func (pp *postPolicy) Restore(IUser model.IUser, routeParamMap map[string]string) bool { return true }
```
* `Before` func will be called at the first
    * If `Before` return **nil**, then it will call the **Action** you defined at router `Can` or controller `Authorize` func.
    * If `Before` return **a bool pointer**, then it will immediately return the bool you returned as the Authorizing result.
> Typically, `Before` func usually used under the **Admin** circumstances.
* Other funcs like `Create`, `Update`, etc. are matched with the **Action** you defined at router `Can` or controller `Authorize` func, the result of Authorization will be the bool you returned at these funcs. 

# Authorizing Actions Using Policies
## Authorizing Route
```go
type PostGroup struct {
	PostController controllers.Post
}

func (pg *PostGroup) Group(group route.Grouper) {
    group.PUT("/post/:postId", pg.PostController.Edit).Can(policies.NewPostPolicy(nil), policy.ActionUpdate)
}
```

## Authorize in Controller
```go
func (p *Post) Edit(c *gin.Context) {
    ...
    ...
    // get current post
    postIdStr, ok := c.Param("postId")
    
    ...
    ... // convert postIdStr and get currentPost from db
    ...
    
    // Do Authorize
    isAbort, user := l.Authorize(c, policies.NewPostPolicy(&currentPost), policy.ActionView)
    if isAbort{
        return
    }
    ...
    ...
}
```