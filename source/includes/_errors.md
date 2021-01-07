# Errors

The qi.do API uses the following error codes:

Error | Meaning
---------- | -------
400 | Bad Request - Your request is invalid.
401 | Unauthorized - Your API key is wrong.
403 | Forbidden - The user don't have permissions to access the requested object.
404 | Not Found - The specified object could not be found.
405 | Method Not Allowed - You tried to access a object with an invalid method.
406 | Not Acceptable - You requested a format that isn't JSON.
410 | Gone - The object requested has been removed from our servers.
429 | Too Many Requests - You're requesting too many object! Slow down!
500 | Internal Server Error - We had a problem with our server. Try again later.
503 | Service Unavailable - We're temporarily offline for maintenance. Please try again later.
