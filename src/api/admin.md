# Admin Endpoints

These endpoints are not considered User endpoints because they aren't commonly used by normal users. Endpoints here require some sort of staff permission in order to be utilized.

## Table of Contents

[[toc]]

## <Get/> /users/

This endpoint provides all users on the service in the order from newest users to oldest.

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
                    "updatedAt": "2020-10-19T19:30:23.114Z",
                    "domain": "alekeagle.me",
                    "subdomain": "",
                    "bannedAt": null
                },
                {
                    "id": "1603133941356",
                    "username": "bob",
                    "displayName": "bob",
                    "staff": "",
                    "createdAt": "2020-10-19T18:59:01.356Z",
                    "updatedAt": "2020-10-19T18:59:01.356Z",
                    "domain": "alekeagle.me",
                    "subdomain": "",
                    "bannedAt": null
                },
                {
                    "id": "1603071043127",
                    "username": "daybydave",
                    "displayName": "daybydave",
                    "staff": "",
                    "createdAt": "2020-10-19T01:30:43.127Z",
                    "updatedAt": "2020-10-19T01:30:43.127Z",
                    "domain": "alekeagle.me",
                    "subdomain": "",
                    "bannedAt": null
                },
                {
                    "id": "1590090511441",
                    "username": "alekeagle",
                    "displayName": "AlekEagle",
                    "staff": "admin",
                    "createdAt": "2020-05-21T19:48:31.441Z",
                    "updatedAt": "2020-05-21T19:48:31.441Z",
                    "domain": "alekeagle.me",
                    "subdomain": "",
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

## <Patch/> /user/:id/ban/

This will ban or unban the specified user.

:::details Parameters

-   Body
    -   `banned`
        -   A boolean representing if the user should be banned or unbanned.
-   Path
    -   `id`
        -   The ID of the user to ban or unban.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/1603135823114/ban/ \
-X PATCH \
-d "{\"banned\":true}" \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL" \
-H "Content-Type: application/json"
```

:::

:::details Example Responses

-   200 OK

    -   ````json
            {
              "id": "1603135823114",
              "username": "joe",
              "displayName": "joe",
              "staff": "",
              "createdAt": "2020-10-19T19:30:23.114Z",
              "updatedAt": "2020-10-19T22:37:50.626Z",
              "bannedAt": "2020-10-28T15:09:30.357Z",
              "domain": "alekeagle.me",
              "subdomain": ""
            }
            ```
        ````

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

## <Delete /> /user/:id/

This will delete the user provided.

:::details Parameters

-   Path
    -   `id`
        -   The ID of the user you plan on deleting.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/1603135823114/ \
-X DELETE \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "success": true
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

## <Patch /> /user/:id/token/

This will forcefully regenerate the token of the user specified.

:::details Parameters

-   Path
    -   `id`
        -   The ID of the user you plan on regenerating the token.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/1603135823114/token \
-X PATCH \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "token": "MTYwMzEzNTgyMzExNA.MjQ2.SlRqdlpRcnNFU21GdTViZUZxMUdpcFZC"
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

## <Patch /> /user/:id/domain/

This will change the domain selection for the user specified.

:::details Parameters

-   Path
    -   `id`
        -   The ID of the user you plan on changing the domain selection of.
-   Body
    -   `domain`
        -   A **VALID** domain selection from [/domains/](./user.md#domains)
    -   `subdomain` _optional_
        -   A subdomain for the domain **IF** the domain supports it.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/user/1603135823114/domain/ \
-X PATCH \
-d "{\"domain\": \"alekeagle.me\", \"subdomain\":\"test.subdomain lol\"}" \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL" \
-H "Content-Type: application/json"
```

:::

:::details Example Responses

-   200 OK
    -   ```json
        {
            "domain": "alekeagle.me",
            "subdomain": "test-subdomain-lol"
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
