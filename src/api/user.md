# User Endpoints

These are endpoints that a normal user will use at least once, if not several times during the usage of the service.

## <post/> /login/

This endpoint returns the full user object who's username and password matches the one provided.

This endpoint does **NOT** need authorization.

:::details Accepts

-   `multipart/form-data`

-   `application/x-www-form-urlencoded`

-   `application/json`

:::

:::details Parameters

-   Body

    -   `name`

        -   The user's username or email

    -   `password`

        -   The user's password

:::

:::details Example Request

```bash
curl https://alekeagle.me/api/login/ \
-X POST \
-d "{\"name\":\"joe\",\"password\":\"password\"}" \
-H "Content-Type: application/json"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "id": "1603135823114",
            "username": "joe",
            "displayName": "joe",
            "email": "joe@example.com",
            "staff": "",
            "apiToken": "MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV",
            "domain": "alekeagle.me",
            "subdomain": "",
            "bannedAt": null,
            "createdAt": "2020-10-19T19:30:23.114Z",
            "updatedAt": "2020-10-19T22:37:50.626Z"
        }
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["name", "password"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Account"
        }
        ```

:::

## <get/> /user/

This endpoint is used to get the same data [/login/](./user.md#login) returns, but with the user's token.

**No Parameters**

:::details Example Request

```bash
curl https://alekeagle.me/api/user/ \
-X GET \
-H "Authorization: MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "id": "1603135823114",
            "username": "joe",
            "displayName": "joe",
            "email": "joe@example.com",
            "staff": "",
            "apiToken": "MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV",
            "domain": "alekeagle.me",
            "subdomain": "",
            "bannedAt": null,
            "createdAt": "2020-10-19T19:30:23.114Z",
            "updatedAt": "2020-10-19T22:37:50.626Z"
        }
        ```
-   401 Unauthorized

    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <get/> /users/

This endpoint provides all users on the service in the order from newest users to oldest. This API endpoint not only requires Authorization, but it also needs the user associated with the token to have a staff rank of some sort.

:::details Parameters

-   Query string

    -   `count` _optional_
        -   The amount of users to return per request
    -   `offset` _optional_

        -   The offset from the beginning of the list

:::

:::details Example Request

```sh
curl "https://alekeagle.me/api/users/?count=2&offset=0" \
-X GET \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL"
```

:::

:::details Example Response

-   200 OK
    -   ```json
        {
            "count": 4,
            "users": [
                {
                    "id": "1603135823114",
                    "username": "joe",
                    "displayName": "joe",
                    "staff": "",
                    "createdAt": "2020-10-19T19:30:23.114Z",
                    "bannedAt": null
                },
                {
                    "id": "1603133941356",
                    "username": "bob",
                    "displayName": "bob",
                    "staff": "",
                    "createdAt": "2020-10-19T18:59:01.356Z",
                    "bannedAt": null
                },
                {
                    "id": "1603071043127",
                    "username": "daybydave",
                    "displayName": "daybydave",
                    "staff": "",
                    "createdAt": "2020-10-19T01:30:43.127Z",
                    "bannedAt": null
                },
                {
                    "id": "1590090511441",
                    "username": "alekeagle",
                    "displayName": "AlekEagle",
                    "staff": "admin",
                    "createdAt": "2020-05-21T19:48:31.441Z",
                    "bannedAt": null
                }
            ]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```
-   403 Forbidden
    -   ```json
        {
            "error": "Missing Permissions"
        }
        ```

:::

## <post/> /user/

This endpoint creates a new user with the provided account details. It doesn't need Authorization.

:::details Accepts

-   `multipart/form-data`

-   `application/x-www-form-urlencoded`

-   `application/json`

:::

:::details Parameters

-   Body
    -   `name`
        -   The username for the user that will be created.
    -   `email`
        -   The email to be linked with the user created.
    -   `password`
        -   The password for the account that will be created.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/ \
-X POST \
-d "{\"name\":\"joe\",\"email\":\"joe@example.com\",\"password\":\"newPassword\"}"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "id": "1603135823114",
            "username": "joe",
            "displayName": "joe",
            "email": "joe@example.com",
            "staff": "",
            "apiToken": "MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV",
            "domain": "alekeagle.me",
            "subdomain": "",
            "bannedAt": null,
            "createdAt": "2020-10-19T19:30:23.114Z",
            "updatedAt": "2020-10-19T22:37:50.626Z"
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "User already exists",
            "with": ["name", "email"]
        }
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["name", "email", "password"]
        }
        ```

:::

## <get/> /user/:id/

This endpoint returns the user object stripped of private data for another user, or acts the same as [/user/](./user.md#user) and returns your user object if you query the same ID associated with your Authorization token. To get other user's data, the account associated with the token must have a staff rank of some sort.

:::tip Note
This and many other requests use what are called Path Parameters, which can be found at [Reference > Path Parameters](../reference/index.md#path-parameters)[.](../brew-coffee/index.md)
:::

:::details Parameters

-   Path
    -   `id`
        -   The ID of the user who's data you want to obtain.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/1603135823114/ \
-X GET \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "id": "1603135823114",
            "username": "joe",
            "displayName": "joe",
            "staff": "",
            "createdAt": "2020-10-19T19:30:23.114Z",
            "bannedAt": null
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```
-   403 Forbidden
    -   ```json
        {
            "error": "Missing Permissions"
        }
        ```

:::

## <delete/> /user/
