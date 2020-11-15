# DotnetCore WebApi Authentication and Authorization

## Authentication

Most API's require authentication across the board.

Clients of API should not need to be told how to authenticate and should handle the redirection themselves. So forget about the redirects etc.

Unauthenticated requests MUST respond with HTTP: _401 Unauthorized_.

Requiring the request to be authenticated can be required for all requests and is setup in startup.cs

```

```

_If and only if there is an action or controller that does NOT need to be authenticated [AllowAnonymous] attribute can be added to the action or controller._







## Authorization


