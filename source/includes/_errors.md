# Errors

*Lotus* Base API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request---Request is malformed, or required data is not provided.
401 | Unauthorized -- Invalid API token.
403 | Forbidden -- Insufficient privilege, or lack of authentication.
404 | Not Found -- The resources you have requested for are unavailable.
405 | Method Not Allowed -- You tried to access the API with an invalid method.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.