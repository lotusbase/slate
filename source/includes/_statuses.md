# HTTP Status Codes

*Lotus* Base API returns the following HTTP status codes:


Status | Meaning
---------- | -------
<a name="200-ok"></a>`200` | OK
<a name="207-partial-success"></a>`207` | Partial Success---Part of the request is valid. Invalid data requires attention.
<a name="400-bad-request"></a>`400` | Bad Request---Request is malformed, or required data is not provided.
<a name="401-unauthorized"></a>`401` | Unauthorized---Invalid API token.
<a name="403-forbidden"></a>`403` | Forbidden---Insufficient privilege, or lack of authentication.
<a name="404-not-found"></a>`404` | Not Found---The resources you have requested for are unavailable.
<a name="405-method-not-allowed"></a>`405` | Method Not Allowed---You tried to access the API with an invalid method.
<a name="500-internal-server-error"></a>`500` | Internal Server Error---We had a problem with our server. Try again later.
<a name="503-service-unavailable"></a>`503` | Service Unavailable---We're temporarially offline for maintanance. Please try again later.



# Error messages

## Invalid *Lotus* genome version

We currently host the following versions of the *Lotus* genome: v2.5 and **v3.0**. Versions should be provided without the `v` prefix. This error is generated when the genome version provided does not match any entires in the aforementioned list.

## PDO Exception

This is typically returned as a `500` status. It indicates that the server has encountered an internal error, suggesting that an exception flag has been raised by PDO, a driver used to interface with our MySQL databases.