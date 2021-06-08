# Domain Endpoints

These endpoints are related to domains, wether it be editing a domain itself, or changing a user's domain selection.

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

- 200 OK
  - ```json
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
- 401 Unauthorized

  - ```json
    {
      "error": "No Token Provided"
    }
    ```

:::

## <Patch/> /user/domain/

This will change the domain and subdomain combination of the user who made the request.

:::details Parameters

- Body
  - `domain`
    - A valid domain to be changed to.
  - `subdomain` _optional_
    - A string for the subdomain or left blank or null.

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

- 200 OK
  - ```json
    {
      "domain": "alekeagle.me",
      "subdomain": "free-bruh-moments-at"
    }
    ```
- 400 Bad Request
  - ```json
    {
      "error": "Bad Request",
      "missing": ["domain"]
    }
    ```
- 401 Unauthorized
  - ```json
    {
      "error": "No Token Provided"
    }
    ```

:::

## <Patch /> /user/:id/domain/

This will change the domain selection for the user specified.

:::details Parameters

- Path
  - `id`
    - The ID of the user you plan on changing the domain selection of.
- Body
  - `domain`
    - A **VALID** domain selection from [/domains/](./domains.md#domains).
  - `subdomain` _optional_
    - A subdomain for the domain **IF** the domain supports it.

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

- 200 OK
  - ```json
    {
      "domain": "alekeagle.me",
      "subdomain": "test-subdomain-lol"
    }
    ```
- 401 Unauthorized
  - ```json
    {
      "error": "No Token Provided"
    }
    ```
- 403 Forbidden
  - ```json
    {
      "error": "Missing Permissions"
    }
    ```

:::

## <Post /> /domain/

Create a domain. You need to have staff permissions in order to add a domain. The domain will be checked to make sure it is valid and points to alekeagle.me properly.

Requirements for adding a domain:

- Must point to alekeagle.me
- Must redirect to alekeagle.me if a path is not a file or not found.
- Must allow a calls to `/api/domain/verify/`.
- Must use HTTPS.
- Must automatically redirect to HTTPS if request is initially in HTTP.

:::details Parameters

- Body
  - `domain`
    - The domain you want to add.
  - `allowsSubdomains` _optional_
    - Wether or not the domain allows subdomains, this defaults to true.

:::

:::details Example Request

```sh
curl https://alekeagle.me/api/domain/ \
-X POST \
-d "{\"domain\":\"example.com\",\"allowsSubdomains\":false}" \
-H "Authorization: MTU5MDA5MDUxMTQ0MQ.Njg4.Q1lwQWZSNlNWdW0tUEdQeEk4V0Y1UWVL" \
-H "Content-Type: application/json"
```

:::

:::details Example Responses

- 201 Created
  - ```json
    {
      "domain": "example.com",
      "allowsSubdomains": false,
      "createdAt": "2019-10-22T23:54:41.974Z",
      "updatedAt": "2019-10-22T23:54:41.974Z"
    }
    ```
- 400 Bad Request
  - ```json
    {
      "error": "Bad Request",
      "missing": ["domain"]
    }
    ```
- 401 Unauthorized
  - ```json
    {
      "error": "No Token Provided"
    }
    ```
- 403 Forbidden
  - ```json
    {
      "error": "Missing Permissions"
    }
    ```
- 409 Conflict
  - ```json
    {
      "error": "The domain is not properly configured to be added to the list of domains. For more information please visit https://docs.alekeagle.me/api/domains.html#domain"
    }
    ```

:::
