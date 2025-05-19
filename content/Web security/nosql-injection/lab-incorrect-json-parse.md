Send a request with a body:
```http
{"name": "{\"$ne\": false}"}
```
This works because the backend is expecting name value to be a string.
Later the backend use this parsed value:
```js
const { name } = req.body
const user = Product.findOne({"name": JSON.parse(name)})
```

`[PENDING]` 
I've been workin in more interesting and dockerized labs which are outdated, because currently cluster like Atlas mongoDB don't allow you to manually downgrade it.