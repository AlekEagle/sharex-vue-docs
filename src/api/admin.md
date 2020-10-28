# Admin Endpoints

These endpoints are not considered User endpoints because they aren't commonly used by normal users.

## Table of Contents

[[toc]]

## <Get/> /users/

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
