# V2 Quote API

Refer to the [quote model][model] docs for info on the API input and mappings to Code Objects.

>To learn more about what values are defaulted in this system, filled in by Code Objects, or [enhanced by some third party][enhancement], go [here][init-default].

### Endpoints
* `POST /v2/quotes`

## `POST /v2/quotes`

In v2 of the Quote API, `POST /v2/quotes` will normalize, validate, rate, and save a quote in one pass ("quote-n-save").

A few comments:
* Except for a few key values filled in by Code Objects, most fields will be required as this endpoint saves directly
* [Cape Analytics][enhancement] won't be used to enhance `roofShape`
    * Sometimes they do not return a value (i.e. UNKNOWN or some Error occurs)
    * `roofShape` is required to submit a quote.

### Actions
1. Normalizes and Validates quote input
2. Uses `rateQuote` binding from Code Objects to fill out and initialize the quote
3. [Enhances the quote coverages][enhancement] using MSB Express and merges results
4. Calls `rateQuote` binding from Code Objects again to update quote
5. Normalizes and Validates quote for saving (includes validation of Contact Info)
6. Submits quote to Code Objects
7. Returns refId of submitted quote to user.

### Status Codes
* `200`: Creating a quote was successful; response body contains public quote
* `422`: Indicates that there was a validation error while creating quote
* `502`: Indicates that an issue was encountered with the Code Objects API

[init-default]: init-default-quotes.md
[model]: quote-model.md
[enhancement]: quote-enhancement.md
[redis]: https://redis.io/
