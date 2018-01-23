# sfi-api-links
Links &amp; Descriptions for SFI APIs

### Quote V1 API

The Quote API is Security First Insurance’s programmatic interface for creating and managing policy applications used to estimate premiums for insurance products provided by Security First Insurance.

[https://qa.securityfirstflorida.com/docs/api/quote/v1/](https://qa.securityfirstflorida.com/docs/api/quote/v1/)

[Documentation](https://github.com/security-first-managers/sfi-docs/blob/master/products/instant-quote/quote-api-v1.md)


### Quote V2 API

The Quotes API is Security First Insurance’s programmatic interface for creating and managing policy applications used to estimate premiums for insurance products provided by Security First Insurance.

[https://dev.api.securityfirstflorida.com/docs/v2/quotes](https://dev.api.securityfirstflorida.com/docs/v2/quotes)

[Documentation](https://github.com/security-first-managers/sfi-docs/blob/master/products/instant-quote/quote-api-v2.md)

#### Postman Collections

* [2017/12/21 - Get Constrained Values with environment](postman_collections/quote_v2/2017-12-21_getConstrainedValues.postman_collection.json)
* [2018/01/16 - Quote V2 with environment](postman_collections/quote_v2/2018-01-16_quoteV2WithEnvironment.postman_collection.json)

**Note**: These assume that the environment looks like the following:
* accessKey: <user's AWS access Key>
* secretKey: <user's AWS secret Key>
* rootURL: https://dev.api.securityfirstflorida.com/docs/v2
* awsRegion: us-east-1
* awsService: execute-api


### Flood Risks V1 (for Swiss Re) API

This API provides access to flood risk scores for property addresses.

[https://dev.api.securityfirstflorida.com/docs/v1/flood-risks](https://dev.api.securityfirstflorida.com/docs/v1/flood-risks)


### Claim Estimate Batches V1 (for Xact manual batch process) API

This API allows the creation of assignment resources in a third-party estimating platform via bulk data upload.

[https://dev.api.securityfirstflorida.com/docs/v1/claim-estimate-batches](https://dev.api.securityfirstflorida.com/docs/v1/claim-estimate-batches)

#### Postman Collections

[https://github.com/security-first-managers/sfi-msa-binding-xact/tree/master/test/verification/collections](https://github.com/security-first-managers/sfi-msa-binding-xact/tree/master/test/verification/collections)


### FNOL V2 API

This V2 API offers the ability to interface with SFI's claims, policies and historical policies via the warehouse.

[https://qa.securityfirstflorida.com/docs/api/v2/](https://qa.securityfirstflorida.com/docs/api/v2/)
