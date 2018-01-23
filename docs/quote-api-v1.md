# V1 Quote API

>To learn more about what values are defaulted in this system, filled in by Code Objects, or [enhanced by some third party][enhancement], go [here][init-default].

### Endpoints
* `POST /v1/quote`
* `GET /v1/quote/{id}`
* `PUT /v1/quote/{id}`
* `GET /v1/quote/{id}/values`
* `GET /v1/quote/{id}/agents`
* `GET /v1/quote/{id}/rate`
* `POST /v1/quote/{id}/communication/{emailType}`
* `POST /v1/quote/{id}/save`
* `POST /v2/quotes`

## General Flow
1. Create an unmanaged quote (not yet saved) using `POST /v1/quote` (quote can be retrieved using `GET /v1/quote/{id}`)
2. Examine available Constrained Values using `GET /v1/quote/{id}/values`
3. Examine available agents for that quote using `GET /v1/quote/{id}/agents`
4. Examine quote rate using `GET /v1/quote/{id}/rate`
5. Update quote as necessary using `PUT /v1/quote/{id}`
6. Send quote via email using `POST /v1/quote/{id}/communication/{emailType}`
7. Save / Manage quote using `POST /v1/quote/{id}/save`

## `POST /v1/quote`

This endpoint creates and caches an unmanaged quote (see [quote model][model]).

### Actions
1. Normalizes and Validates quote input
2. Uses `rateQuote` binding from Code Objects to fill out and initialize the quote
3. Gives the private quote (version that gets cached) a unique id
4. [Enhances the quote][enhancement] using Cape Analytics and MSB Express and merges results
5. Defaults Coverage C (if not provided)
6. Defaults Year Roof Renovated (if not provided)
7. Caches private quote in [redis][redis]
8. Calls `update` to refresh data after enhancement through Cape Analytics and MSB Express
9. Returns updated quote to API User, filtered for public api response (removes private metadata)

### Status Codes
* `200`: Creating a quote was successful; response body contains public quote
* `422`: Indicates that there was a validation error while creating quote
* `502`: Indicates that an issue was encountered with the Code Objects API

## `GET /v1/quote/{id}`

Fetches a quote from the [redis][redis] cache.

### Status Codes
* `200`: A quote with `{id}` was found; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache

## `PUT /v1/quote/{id}`

Updates quote with `{id}` with changes provided in request body (see [quote model][model]).

### Actions
1. Fetches private quote from [redis][redis] cache.
2. Normalizes and Validates quote changes
3. Checks if [key fields][model] have changed (returns a validation error if they have)
4. Merge private quote and normalized input
5. [Enhance coverages][enhancement] using MSB Express
6. Update quote using `rateQuote` from Code Objects
7. Merge response from Code Objects into private quote
8. Caches and returns updated private quote to API User

### Status Codes
* `200`: Creating a quote was successful; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache
* `422`: Indicates that there was a validation error while creating quote
* `502`: Indicates that an issue was encountered with the Code Objects API

## `GET /v1/quote/{id}/values`

Gets the set of constrained values for the quote specified by `{id}`.

### Status Codes
* `200`: A quote with `{id}` was found; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache

## `GET /v1/quote/{id}/agents`

Gets a list of local agents for the quote specified by `{id}`.

### Status Codes
* `200`: A quote with `{id}` was found; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache

## `GET /v1/quote/{id}/rate`

Gets the rate for the quote specified by `{id}`.

### Status Codes
* `200`: A quote with `{id}` was found; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache

## `POST /v1/quote/{id}/save`

Saves quote into Code Objects Policy system

### Actions
1. Fetches private quote from [redis][redis] cache
2. Merges in contact information (in request body)
3. Normalizes and Validates for saving
4. Submits quote to Code Objects (`submitQuote`)
5. replaces quote id with refId from Code Objects
6. Deletes quote from [redis][redis] cache
7. Sends final quote summary to the user and selected agent (if email provided and not a [batch quote])
8. Returns final quote to API User

### Status Codes
* `200`: Creating a quote was successful; response body contains public quote
* `404`: A quote with `{id}` was not found in the cache
* `422`: Indicates that there was a validation error while creating quote
* `502`: Indicates that an issue was encountered with the Code Objects API

[init-default]: init-default-quotes.md
[model]: quote-model.md
[enhancement]: quote-enhancement.md
[redis]: https://redis.io/
