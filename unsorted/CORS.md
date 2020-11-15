CORS

Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://obs-ids.onware.org/connect/userinfo. (Reason: CORS header “Access-Control-Allow-Origin” missing).


CORS: The resource server, typically the backend api, MUST set the list of origins it is expecting to receive requests from.

Browser requests to the resource server includes the an origin header.

The origin header in the browser request MUST be matched in the resource server list of allowed origins.



