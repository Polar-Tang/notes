https://cwe.mitre.org/data/definitions/289.html

The authentication is based on the name, but the system didn't check all posibles matches of that name

### Example
```ts
const { username } = req.body;
// Here "admin" it's eaqual to "   AdmIn   "
const user = users.find(u => u.username.trim().toLocaleLowerCase() === username.trim().toLocaleLowerCase());

if (user?.username == "admin") {
	return res.send({message: "Congratulations, you just logged as the admin"})
}
```

#### Lead to:
- [[CWE-178(improrer-case-sensitive-handling)]]