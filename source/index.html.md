---
title: Lotus Base API Reference

header: <em>Lotus</em> Base API

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

api_version: v1

---

# Introduction

> The API endpoint is available at `https://lotus.au.dk/api/v{{version}}`. The most current API endpoint is at `https://lotus.au.dk/api/v1`

The *Lotus* Base API is a RESTful service powered by [Slim](http://www.slimframework.com). Our API offers predictable, resource-oriented URLs that return appropriate HTTP status codes to indicate the outcome of an API call. The latest version of our API is **v1**.

> The format returned is [JSON](http://www.json.org), in the following format:

```json
{
    "status": "HTTP_STATUS_CODE",
    "data": "DATA_OBJECT",
    "message": "OPTIONAL_MESSAGE_FROM_API"
}
```

We use a universally interchangeable format known as [JavaScript Object Notation (JSON)](http://www.json.org) to transmit data from our API to your end-point. Even though the HTTP status code is included in the response header, we also include it as part of the JSON returned. Note that all the data request will be returned in the nested `data` object.

Under most conditions, the `message` object will be empty unless an HTTP error code is provided. The message can be either utilized by developers to troubleshoot erroneous API calls, or used to inform your users about any issues occuring in the backend.

# API key

> To authorize, please include your custom API key with each request:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-API-KEY: {{ YOUR_API_KEY }}"
```

```javascript
// Set custom headers for all API calls using a global AJAX prefilter
$.ajaxPrefilter(function(options) {
  if (!options.beforeSend) {
    options.beforeSend = function (xhr) { 
      xhr.setRequestHeader('X-API-KEY', YOUR_API_KEY);
    };
  }
});

// Manually add access token to requests
$.ajax({
  url: 'https://lotus.au.dk/api/v1/',
  type: 'GET',
  data: {
    key1: value1,
    key2: value2,
    access_token: YOUR_API_KEY
  }
})
```

*Lotus* Base uses API keys to allow access to the API. Please [register a new API key](https://lotus.au.dk/users/account#access_token) at our developer portal before proceeding. We do not restrict API keys to individual domains. If you suspect that your API key is being misused or abused, you can revoke it at any time and create a new one. The API expects this generated key to be included in all API requests to the server in one of the many following formats:

- As a custom, non-standard HTTP header, `X-API-KEY` (`HTTP_X_API_KEY`)
- As a key-value pair of the following format, `access_token=YOUR_API_KEY` in the:
    - query parameters of your `GET` request, or
    - body of your `POST`, `PUT`, or `DELETE` request

<aside class="warning">API calls made without providing the API key and a HTTP referer will be declined with a standard `401` error.</aside>


# *LORE1* mutant population

## Get all *LORE1* lines

<span class="request__type get">Get</span> <code class="request__end-point">/lore1</code>

> An example of a returned JSON, with the first three elements shown:

```json
[
  "30000001",
  "30000002",
  "30000003",
]
```

Returns all distinct Danish *LORE1* mutant line IDs that can be ordered, and that we have seed stocks of&mdash;a ballpark figure of 100,000+ lines. This will generate a large JSON file, so you might want to stream the response instead.

<aside class="notice">Japanese <em>LORE1</em> lines cannot be ordered from us&mdash;please visit <a href="https://www.legumebase.brc.miyazaki-u.ac.jp/lore1BrowseAction.do">LegumeBase</a> for further information.</aside>

## Verify *LORE1* lines

> Response example for a valid lines `30000001`, `30000002`, and `30000003`:

```json
{
  "status": 200,
  "data": {
    "pid_found": [
      "30000001",
      "30000002",
      "30000003"
    ]
  }
}
```

> Response example for invalid lines, `20000001`, `30000000`, `DK-030000001`:

```json
{
  "status": 404,
  "message": "No valid plant ID has been found.",
  "data": {
    "pid_notFound": [
      "30000000"
    ]
  }
}
```

> Response example for partially invalid request. Here we used a valid line, `30000001` and a non-existent line, `30000000`:

```json
{
  "status": 207,
  "message": "One or more plant ID you have provided are not available for ordering.",
  "data": {
    "pid_notFound": [
      "30000000"
    ],
    "pid_found": [
      "30000001"
    ]
  },
  "more_info": "https://lotus.au.dk/docs/errors/partial-success"
}
```

<span class="request__type get">Get</span> <code class="request__end-point">/lore1/{pids}/verify</code>

Performs validation of a list of *LORE1* mutant line IDs. A line is considered "valid" as long as it:

1. exists in the *LORE1* collection,
2. has seeds available for shipping, and
3. is flagged as orderable in our database.

### Query Parameters

Parameter | Description
--------- | -----------
pids      | **Required.** This should be a list of *LORE1* mutant line IDs separated by the following: a comma, space, or linebreak.

### Responses

Due to the nature of the request, the list provided to the API may contain a mixture of valid and invalid lines. Therefore, we issue a `207` response in the event that some of the lines in the list are valid.

Response  | Description
--------  | -----------
`200`     | All IDs provided are available for ordering and valid
`207`     | Some IDs provided are available for ordering and valid
`404`     | All IDs provided are invalid