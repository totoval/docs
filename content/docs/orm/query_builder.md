# Query Builder

## Q

```go
m.H().Q([]model.Filter{{Key: "username", Op: "=", Val: username}}, []model.Sort{}, limit, withTrashed).Select("*")
```