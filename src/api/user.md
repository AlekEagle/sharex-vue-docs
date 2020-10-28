# User Endpoints

These are endpoints that a normal user will use at least once, if not several times during the usage of the service.

## Table of Contents

[[toc]]

## <Post/> /login/

This endpoint returns the full user object who's username and password matches the one provided.

This endpoint does **NOT** need authorization.

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

## <Get/> /user/

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

## <Post/> /user/

This endpoint creates a new user with the provided account details. It doesn't need Authorization.

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

## <Get/> /user/:id/

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

## <Delete/> /user/

This deletes the user making the request.

:::details Parameters

-   Body
    -   `password`
        -   The password to the user account to confirm identity.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/ \
-X DELETE \
-H "Content-Type: application/json" \
-d "{\"password\":\"newPassword\"}" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "success": "true"
        }
        ```
-   400 Bad Request

    -   ```json
        {
            "error": "Bad Request",
            "missing": ["password"]
        }
        ```

-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <Patch/> /user/token/

This will regenerate the user's API token that is associated with the user making the request.

:::details Parameters

-   Body
    -   `password`
        -   The password of the user making the request to confirm their identity.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/token/ \
-X PATCH \
-H "Content-Type: application/json" \
-d "{\"password\":\"newPassword\"}" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NjI1.dUJxTGZndVZGU0UzNHJOVEpkdUFaeUdV"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "token": "MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
        }
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["password"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "Invalid Password"
        }
        ```

:::

## <Get/> /domains/

This returns a list of all of the available domains.

**No Parameters**

:::details Example Request

```sh
curl https://alekeagle.me/api/domains/ \
-X GET \
-H "Authorization: MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        [
            {
                "domain": "alekeagle.me",
                "allowsSubdomains": true,
                "createdAt": "2019-10-22T23:54:41.974Z",
                "updatedAt": "2019-10-22T23:54:41.974Z"
            },
            {
                "domain": "cum-in.me",
                "allowsSubdomains": true,
                "createdAt": "2019-10-22T23:54:41.974Z",
                "updatedAt": "2019-10-22T23:54:41.974Z"
            },
            {
                "domain": "cum.ninja",
                "allowsSubdomains": true,
                "createdAt": "2019-10-22T23:54:41.974Z",
                "updatedAt": "2019-10-22T23:54:41.974Z"
            },
            {
                "domain": "ohbiff.tech",
                "allowsSubdomains": false,
                "createdAt": "2019-10-22T23:54:41.974Z",
                "updatedAt": "2019-10-22T23:54:41.974Z"
            },
            {
                "domain": "spinjitzu.xyz",
                "allowsSubdomains": false,
                "createdAt": "2019-10-22T23:54:41.974Z",
                "updatedAt": "2019-10-22T23:54:41.974Z"
            },
            {
                "domain": "became.gay",
                "allowsSubdomains": true,
                "createdAt": "2020-09-17T01:39:02.283Z",
                "updatedAt": "2020-09-17T01:39:02.283Z"
            }
        ]
        ```
-   401 Unauthorized

    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <Patch/> /user/domain/

This will change the domain and subdomain combination of the user who made the request.

:::details Parameters

-   Body
    -   `domain`
        -   A valid domain to be changed to.
    -   `subdomain` _optional_
        -   A string for the subdomain or left blank or null.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/domain/ \
-X PATCH \
-H "Content-Type: application/json" \
-d "{\"domain\":\"alekeagle.me\", \"subdomain\":\"free bruh moments at\"}" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "domain": "alekeagle.me",
            "subdomain": "free-bruh-moments-at"
        }
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["domain"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <Patch/> /user/

Update the requesting user's information.

:::details Parameters

-   Body
    -   `name` _optional_
        -   The new username for the user.
    -   `email` _optional_
        -   The new email address for the user.
    -   `newPassword` _optional_
        -   A new password for the user's account.
    -   `password`
        -   The user's password for the account to confirm identity.

:::

:::details Example Request

```sh
curl https://alekeagle.com/api/user/ \
-X PATCH \
-H "Content-Type: application/json" \
-d "{\"password\":\"newPassword\",\"email\":\"joe@example.net\"}" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "id": "1603135823114",
            "username": "joe",
            "displayName": "joe",
            "email": "joe@example.net",
            "staff": "",
            "password": null,
            "apiToken": "MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI",
            "domain": "alekeagle.me",
            "subdomain": "free-bruh-moments-at",
            "bannedAt": null,
            "createdAt": "2020-10-19T19:30:23.114Z",
            "updatedAt": "2020-10-20T23:53:38.462Z"
        }
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["email", "newPassword", "name"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <Post/> /upload/ (Deprecated)

:::danger Deprecated
This endpoint is deprecated, if you are creating something that uses this endpoint, please consider using [/upload/](./user.md#upload).

:::

This API endpoint is an oddball, it is not prefixed with `/api`, its URL is only `https://alekeagle.me/upload/`.
This endpoint is also an oddball, as it only returns plaintext, which is the link. This may change in the future.

The max file size is 100MB, period.

:::details Parameters

-   Body
    -   `file`
        -   The file or text you want to upload to the server.

:::

:::details Example Request

```sh
curl https://alekeagle.me/upload/ \
-X POST \
-F "file=@/path/to/file.png" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
```

:::

:::details Example Responses

-   201 Created
    -   ```
        https://free-bruh-moments-at.alekeagle.me/1LkbQsLQIU.png
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["file"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::

## <Post/> /upload/

:::tip Replacement for deprecated endpoint

This endpoint is a replacement for an endpoint that is deprecated and will be no longer used soon. Using this endpoint is preferred.

:::

This API endpoint is the replacement for the aforementioned deprecated /upload/ endpoint, it follows the endpoint definition being located at `https://alekeagle.me/api/upload/`.

:::details Parameters

-   Body
    -   `file`
        -   The file or text you want to upload to the server.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/upload/ \
-X POST \
-F "file=@/path/to/file.png" \
-H "Authorization: MTYwMzEzNTgyMzExNA.NDk1.OGRxQm9kUXltckR3SzRLUmxGQkZ1d0VI"
```

:::

:::details Example Responses

-   201 Created
    -   ```
        https://free-bruh-moments-at.alekeagle.me/1LkbQsLQIU.png
        ```
-   400 Bad Request
    -   ```json
        {
            "error": "Bad Request",
            "missing": ["file"]
        }
        ```
-   401 Unauthorized
    -   ```json
        {
            "error": "No Token Provided"
        }
        ```

:::
