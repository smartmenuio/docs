# Errors

> Errors look like this:

```json
{
  "error": "NoObjectError",
  "status_code": 404,
  "message": "No existing product for the given id: 13"
}
```

```json
{
  "error": "ValidationErrors",
  "status_code": 403,
  "message": "name must be at the least 2 characters long"
}
```
When querying the API you can be presented with the following errors:

### API Errors

Code | Error | Meaning
------ | ----- | -------
`401` | `UnauthorizedError` |  Your API key is wrong, missing or invalid
`403` | `ValidationErrors` | One or more of the required parameters are missing or invalid. See `message` for more info.
`403` | `ObjectAlreadyExistsError` | The object already exists.
`403` | `InvalidTimeFrameError` | Indicates that the timeframe specified is invalid
`403` | `AssociationExistsError` | Indicates that the entities are already associated (e.g. a `menu` and a `product`)
`404` | `NoObjectError` | The specified entity (e.g. `product`) does not exist.
`429` | `TooManyRequestsError` | You've sent too many requests in the past hour. See [Rate-Limits](#rate-limits-and-throttling).

### Server errors

Status Code | Meaning
---------- | -------
`400 Bad Request` |  Your request sucks
`405 Method Not Allowed` | The method is not supported.
`406 Not Acceptable` | You requested a format that isn't JSON
`500 Internal Server Error` | Well this is embarrassing. Let us know what happened! 
`503 Service Unavailable` | We're temporarily offline for maintenance or we're experiencing capacity problems.
