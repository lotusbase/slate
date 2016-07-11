---
title: Lotus Base API Reference

header: <em>Lotus</em> Base API

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='https://lotus.au.dk/users/account#access_token'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - statuses

search: true

api_version: v1

---

# Introduction

The *Lotus* Base API is a RESTful service powered by [Slim](http://www.slimframework.com). Our API offers predictable, resource-oriented URLs that return appropriate HTTP status codes to indicate the outcome of an API call. The latest version of our API is **v1**.

The API endpoint is available at `https://lotus.au.dk/api/v{{version}}`. The most current API endpoint is at `https://lotus.au.dk/api/v1`.

<aside class="notice">This API documentation is still in the works, and is not complete.</aside>

> The format returned is [JSON](http://www.json.org), in the following format:

```json
{
    "status": "HTTP_STATUS_CODE",
    "data": "DATA_OBJECT",
    "message": "OPTIONAL_MESSAGE_FROM_API",
    "more_info": "OPTIONAL_URL_FOR_FURTHER_INFO"
}
```

We use a universally interchangeable format known as [JavaScript Object Notation (JSON)](http://www.json.org) to transmit data from our API to your end-point. Even though the HTTP status code is included in the response header, we also include it as part of the JSON returned. Note that all the data request will be returned in the nested `data` object.

Under most conditions, the `message` object will be empty unless an HTTP error code is provided. The message can be either utilized by developers to troubleshoot erroneous API calls, or used to inform your users about any issues occuring in the backend. Occasionally, a `more_info` key is provided, containing a documentation reference where you may glean further information on the reason of the error message you are receiving.

If you encounter any issue with using our API, please do not hestitate to contact us through various channels: [open an issue](https://github.com/lotusbase/lotus.au.dk/issues) with our GitHub project, reach out to us via [@LotusBase](https://twitter.com/lotusbase), or [drop us an email](https://lotus.au.dk/meta/contact).

# API key / access token

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
    - Query parameters of your `GET` request, or
    - body of your `POST`, `PUT`, or `DELETE` request

<aside class="warning">API calls made without providing the API key and a HTTP referer will be declined with a standard <code>401</code> error.</aside>

<aside class="notice">From this point forward, we assume that the <code>X-API-KEY</code> header is supplied with all requests, and therefore removed from demo code for brevity.</aside>



# *Lotus* BLAST

## Sequence retrieval

<span class="request__type get">Get</span><span class="request__end-point">/blast/{db}/{id}</span>

Retrieves the sequence based on an identifier found in a valid BLAST database. This method calls the `blastdbcmd` method of NCBI BLAST+ package internally. Note that this method requires URL parameters `db` and `id` to be defined, with optional parameters specified as `GET` query string.

> Example query and response received for the gene *LjFls2* (*Lotus* homolog of the *Arabidopsis * flagellin-sensing 2 gene, *AtFLS2*), to retrieve the amino acid sequence from the *L. japonicus* MG20 proteins v3.0 BLAST database. Note that the amino acid sequence returned, as seen below, has been truncated for brevity.

```shell
curl "https://lotus.au.dk/api/v1/blast/20130521_Lj30_proteins.fa/Lj4g3v0281040.1"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/blast/20130521_Lj30_proteins.fa/Lj4g3v0281040.1',
  type: 'GET'
})
```

```json
{
  "status": 200,
  "data": {
    "success": true,
    "fasta": [
      {
        "id": "Lj4g3v0281040.1",
        "sequence": "MLSLKFSLTL ... SALMKLQTEK",
        "header": "lcl|Lj4g3v0281040.1 Flagellin-sensing 2-like protein OS=Lotus japonicus PE=4 SV=1,100,0,LRR_8,NULL; PROTEIN_KINASE_ST,Serine/threonine-protein kinase, active site; LRR_1,Leucine-rich repea|CUFF.46782.1"
      }
    ]
  }
}
```

> Example query and response, same as above, but now with additional and optional GET query parameters, with the intention to truncate the amino acid sequence to the first 10.

```shell
curl "https://lotus.au.dk/api/v1/blast/20130521_Lj30_proteins.fa/Lj4g3v0281040.1?from=1&to=10"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/blast/20130521_Lj30_proteins.fa/Lj4g3v0281040.1?from=1&to=10',
  type: 'GET'
})
```

```json
{
  "status": 200,
  "data": {
    "success": true,
    "fasta": [
      {
        "id": "Lj4g3v0281040.1",
        "sequence": "MLSLKFSLTLVIVFSIVASV",
        "header": "lcl|Lj4g3v0281040.1:1-20 Flagellin-sensing 2-like protein OS=Lotus japonicus PE=4 SV=1,100,0,LRR_8,NULL; PROTEIN_KINASE_ST,Serine/threonine-protein kinase, active site; LRR_1,Leucine-rich repea|CUFF.46782.1"
      }
    ]
  }
}
```


### Query parameters

<aside class="notice">URL parameters are required, and should be included in the API endpoint.</aside>

URL parameter | Description
------------- | -----------
db            | **Required.** A valid BLAST database hosted with *Lotus* Base. See [here](https://lotus.au.dk/data/blast-db) for the full list of BLAST databases available.
id            | **Required.** The identifier of an entry in the relevant BLAST database.

<aside class="notice">GET parameters are optional, and should be appended as a standard GET query string.</aside>

GET parameter | Description
------------- | -----------
strand        | The strand of the sequence to retrieve. Coerced to `minus` when `from` and `to` coordinates are flipped (see below).
from          | Starting position of the sequence to truncate. If set to `0`, this parameter will be ignored.
to            | Ending position of the sequence to truncate. If set to `0`, this parameter will be ignored.
download      | Boolean parameter. When specified, the script will generate a FASTA file that will be downloaded. No JSON response will be returned.

### Important notes

- When the `download` option is not present in the query, the sequence returned in the JSON format is capped to 100,000 bases/amino acids long. If a longer sequence is detected (such as querying for the entire chromosome, or specifying `to` and `from` options such that their difference exceeds 100,000). A `400` error will be thrown, and the user will be instructed to append the `download` GET parameter to trigger a FASTA download.
- The `download` option is a boolean attribute. Its presence in the GET query string is sufficient to trigger download, no value needs to be assigned to the parameter. Values assigned to this parameter is ignored, i.e. `?download=false` will not work as intended.



# *LORE1* mutant population

## Get all *LORE1* lines

<span class="request__type get">Get</span><span class="request__end-point">/lore1</span>

> An example of a returned JSON, with the first three elements shown:

```json
[
  "30000001",
  "30000002",
  "30000003",
]
```

Returns all distinct Danish *LORE1* mutant line IDs that can be ordered, and that we have seed stocks of&mdash;a ballpark figure of 100,000+ lines. This will generate a large JSON file, so you might want to stream the response instead.

*LORE1* line IDs are identified internally using the following regex pattern: `^3\d{7}`. Therefore, IDs like **30000001** are accepted, but not **DK01-030000001**.

<aside class="notice">Japanese <em>LORE1</em> lines cannot be ordered from us&mdash;please visit <a href="https://www.legumebase.brc.miyazaki-u.ac.jp/lore1BrowseAction.do">LegumeBase</a> for further information.</aside>

For an overview of the current *LORE1* collection available, please refer to the [*LORE1* resource page](https://lotus.au.dk/lore1).



## Verify *LORE1* lines

<span class="request__type get">Get</span><span class="request__end-point">/lore1/{pids}/verify</span>

> Request and response example for a valid lines `30000001`, `30000002`, and `30000003`:

```shell
curl "https://lotus.au.dk/api/v1/lore1/30000001,30000002,30000003/verify"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/lore1/30000001,30000002,30000003/verify',
  type: 'GET'
})
```

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

> Request and response example for invalid lines, `20000001`, `30000000`, `DK-030000001`:

```shell
curl "https://lotus.au.dk/api/v1/lore1/20000001,30000000,DK-030000001/verify"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/lore1/20000001,30000000,DK-030000001/verify',
  type: 'GET'
})
```

```json
{
  "status": 404,
  "message": "No valid plant ID has been found.",
  "data": {
    "pid_invalid": [
      "20000001",
      "DK-030000001"
    ],
    "pid_notFound": [
      "30000000"
    ]
  },
  "more_info": "https://lotus.au.dk/docs/api/v1#404-not-found"
}
```

> Request and esponse example for partially invalid request. Here we used a valid line, `30000001` and a non-existent line, `30000000`:

```shell
curl "https://lotus.au.dk/api/v1/lore1/30000001,30000000/verify"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/lore1/30000001,30000000/verify',
  type: 'GET'
})
```

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
  "more_info": "https://lotus.au.dk/docs/api/v1#207-partial-success"
}
```

Performs validation of a list of *LORE1* mutant line IDs. A line is considered "valid" as long as it:

1. exists in the *LORE1* collection,
2. has seeds available for shipping, and
3. is flagged as orderable in our database.

### *LORE1* mutant line ID formats

On the server side, a simple regex-based validation is used to check for the validity of a line ID. The pattern used is `^3\d{7}$`, which means the line ID must start with the number `3`, followed by seven numerical characters. An example of a valid line ID is `30000001`. We do not support querying for line IDs in the format of `DK{batch}-0{line_id}`, for example, `DK01-030000001`, because the country of origin and batch are redundant information.

The following table summarizes how mutant line IDs are treated:

Line | Description
---- | -----------
`30000001` | <span class="icon-ok"></span>Valid line, will be passed into prepared statement for querying. Appended to the `pid_found` array in returned data.
`39999999` | Valid line, but does not exist in the database. Appended to the `pid_notFound` array in returned data.
`20000001` | Invalid line, as all *LORE1* lines are identified with an eight-digit character starting with `3`. Appended to the `pid_invalid` array in returned data.
`DK‑030000001` | Invalid line, as all *LORE1* lines are identified with an eight-digit character starting with `3`. Appended to the `pid_invalid` array in returned data.

### Query parameters

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



## Retrieve *LORE1* flanking sequences

<span class="request__type get">Get</span><span class="request__end-point">/lore1/flanking-sequence/v{version}/{id}[/{cutoff}]</span>

> Request and response example for requesting the flanking sequence of the insertion, identified with the md5 hash `7cd0a4c8e9f10c4096d1782702267c59`. The flanking sequence is the response shown below has been truncated for brevity.

```shell
curl "https://lotus.au.dk/api/v1/lore1/flanking-sequence/v3.0/7cd0a4c8e9f10c4096d1782702267c59"
```

```javascript
$.ajax({
  url: 'https://lotus.au.dk/api/v1/lore1/flanking-sequence/v3.0/7cd0a4c8e9f10c4096d1782702267c59',
  type: 'GET'
})
```

```json
{
  "status": 200,
  "data": {
    "insFlank": "TGTTTTCACC ... TTATATCTCT",
    "cutoff": false,
    "plantID": "30000001",
    "chromosome": "chr0",
    "position": "146086605",
    "orientation": "F"
  }
}
```

Gets the flanking sequence of a *LORE1* insertion, identified by its md5 hash. The md5 hash of each line is computed from the following information: plant ID, Chromosome, Position, Orientation, Version

### Query parameters

Parameter | Description
--------- | -----------
version   | **Required.** This should be a valid *Lotus* genome version
id        | **Required.** A 32-character hexademical identifier of a *LORE1* line
cutoff    | The cutoff, in base pairs, from the insertion site. The default cutoff value is 1000, which is the maximum extent of the flanking sequence



## Retrieve *LORE1* order statistics

Get publicly available statistics for *LORE1* orders from *Lotus* Base.

### By country
<span class="request__type get">Get</span><span class="request__end-point">/lore1/orders/all/by-country</span>

> Truncated response:

```json
{
  "data": [
    {
      "countryCode": "ABW",
      "countryName": "Aruba",
      "orderCount": 0
    },
    {
      "countryCode": "AFG",
      "countryName": "Afghanistan",
      "orderCount": 0
    }
  ]
}
```

Returns all *LORE1* order counts mapped to the three-letter country code, as defined by [ISO-3166-1 alpha3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3), and the full country name. Each country is an element in the array returned, containing the following information:

- `countryCode`: Three-letter country code
- `countryName`: Full country name
- `orderCount`: Number of *LORE1* orders shipping to said country