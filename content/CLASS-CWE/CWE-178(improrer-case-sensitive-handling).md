https://cwe.mitre.org/data/definitions/178.html
Handling case insensitive improperly

### Example
```ts
function escapeXSS(input: string) {
	input.replace("script", "")
}
```
This attempts to escape script tags ignoring case sensitive cases, like sCript, ScripT.