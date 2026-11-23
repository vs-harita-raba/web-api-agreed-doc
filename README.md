# Agreed API (Zapier integration)

API documentation for the Agreed public API, which the Agreed Zapier integration uses. It's a Postman collection, published as the API docs linked from the Zapier developer platform.

Base URL: `https://agreed.net`

## Files

| File | What it is |
|---|---|
| `postman/Agreed-API.postman_collection.json` | The collection: overview, authentication, every endpoint with examples |
| `postman/Agreed-Production.postman_environment.json` | Environment with `baseUrl` and empty slots for tokens / client credentials |

## Endpoints

**Authentication (OAuth 2.0)**
- `GET /.well-known/oauth-authorization-server`: server metadata
- `GET /oauth/authorize`: consent screen (browser)
- `POST /api/oauth/token`: code exchange and refresh
- `POST /api/oauth/revoke`: disconnect

**Account**
- `GET /api/v1/me`: the connected account (Zapier connection test)

**Webhooks (REST hooks for Zapier triggers)**
- `POST /api/v1/hooks`: subscribe `{ hookUrl, event }`
- `DELETE /api/v1/hooks/:id`: unsubscribe
- `GET /api/v1/hooks/sample?event=…`: recent events / sample data

Events: `document.viewed`, `document.signed`, `document.completed`.

**Zapier** (the requests the Zapier integration makes)
- Test connection: `GET /api/v1/me`
- One folder per trigger (Document Completed, Recipient Signed, Document Viewed), each with Subscribe, Unsubscribe and Perform List, plus the matching Zapier settings in the folder description.

## Publishing the docs

1. In Postman, use **Import** to add both files.
2. Open the collection, then **View complete documentation**, then **Publish**.
3. Select the **Agreed-Production** environment and publish.
4. Put the public link into Zapier's **API documentation URL** field.

Leave the environment's secret values (tokens, client secret) empty before publishing.

## Zapier settings

| Setting | Value |
|---|---|
| Authentication | OAuth v2 |
| Authorization URL | `https://agreed.net/oauth/authorize` |
| Access / refresh token URL | `POST https://agreed.net/api/oauth/token` |
| Scope | `mcp` |
| Test | `GET https://agreed.net/api/v1/me` |
| Connection label | `{{bundle.inputData.email}}` |
| Triggers | REST Hook: subscribe/unsubscribe/perform list as above |

The API itself lives in the main Agreed app (`web-next-react-agreed`, under `src/app/api/oauth` and `src/app/api/v1`).
