# Responses and Errors
When writing a query, a traditional POST and GET request is generated. But apart from the communication with the server, that's where the similarities end.


## Success
A successful response from the server includes a data property in the body.
```
{
  "data": { ... },
}
```

GraphCMS offers a JSON response where the following GraphQL values map to the correlated JSON serialization values.

| GraphQL | JSON |
| --- | --- |
| Map | Object| 
| List | Array| 
| Null | null| 
| String | String| 
| Boolean | true or false| 
| Int | Number| 
| Float | Number| 
| Enum Value | String| 

Also of importance, according to the spec, the order of query is important.
If content is stored as 
```
{
    first: "value",
    last: "value
}
```
and a query is written as
```
{
    last, first
}
```
the response must be
```
{
    last: "value",
    first: "value"
}
```

If the request was a query, the response will be of the query root type. If the operation was a mutation, the response will be a mutation root type.

## Error
If an error ocurred there's an error property in the response. If no error ocurred, there is no error property.
```
{
  "errors": [ ... ]
}
```
Each error entry will include a `message` key and optionally, if it's possible to determine where the error ocurred, a `location` key as well. Service providers like GraphCMS may include additional information to help assist the developer.


### Error at execution
It may be possible to have both an error property AND a data property if there was an error during the execution of the GraphQL query.
```
{
  "data": { ... },
  "errors": [ ... ]
}
```


For more bedtime reading, [check out the docs at the official spec](http://facebook.github.io/graphql/October2016/#sec-Errors).