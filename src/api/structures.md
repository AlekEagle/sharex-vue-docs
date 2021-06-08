# Response Structures

## Normal User Structure

```json
{
  "id":"1623088043642",
  "username":"joe",
  "displayName":"joe",
  "email":"joe@joe.joe",
  "domain":"alekeagle.me",
  "subdomain":"",
  "staff":"",
  "createdAt":"2021-06-07T17:47:23.642Z","updatedAt":"2021-06-07T17:47:23.752Z",
  "bannedAt":null
}
```

## Average 401 Unauthorized Response

```json
{
  "error": "No Token Provided"
}
```

## Average 403 Forbidden Response

```json
{
  "error": "Missing Permissions"
}
```

## Average 400 Bad Request Response

```json
{
  "error": "Bad Request",
  "missing": ["missing", "parameters"]
}
```

## Domain Structure

```json
{
  "domain": "alekeagle.me",
  "allowsSubdomains": true,
  "createdAt": "2019-10-22T23:54:41.974Z",
  "updatedAt": "2019-10-22T23:54:41.974Z"
}
```
