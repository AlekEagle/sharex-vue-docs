# Basics

## Base URL

All endpoints are shortened by cutting out

```
https://alekeagle.me/api
```

just to make things a little bit more tidy

## Making Requests

All requests, unless explicitly stated otherwise, require authorization. Authorization is provided in the form of the `Authorization` header with a token from your account.
::: tip Note
If you need to know how to get your token, you can find it at [Reference > Getting Your Token](/reference/#getting-your-token).
:::
**Example:**

```
curl https://alekeagle.me/api/ \
-X GET \
-H "Authorization: MTYwMDE5OTkxODk4MA.NjQ4.V1BnVFJzXzJvM3RLczdadkRpMmRtQmxm"
```

## Ratelimits

All API requests have ratelimits, using the:

-   `X-Ratelimit-Limit`
-   `X-Ratelimit-Remaining`
-   `X-Ratelimit-Reset`

headers to determine how you should handle ratelimits on your end. `X-Ratelimit-Limit` is how many requests per window you have, `X-Ratelimit-Remaining` is how many you have left from the current window, and `X-Ratelimit-Reset` is the timestamp in seconds at which `X-Ratelimit-Remaining` resets.

::: tip Note
If you need `X-Ratelimit-Limit` raised, please [email AlekEagle](mailto:contact@alekeagle.com?subject=Raise%20Ratelimit%20Limit) about this and request an increase on your limit.
:::

## Parameters

When parameters are defined for a request, they are required unless explicitly stated otherwise.

## <any/> /

An example API endpoint used to ensure the API is online and responsive. Also returns the current API version. This API endpoint does NOT require authorization.

**No Parameters**

:::details Example Request

```sh
curl https://alekeagle.me/api/ \
-X PATCH
```

:::

:::details Example Response

```json
{
    "hello": "world",
    "version": "1.0.0"
}
```

:::
