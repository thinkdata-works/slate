# Deprecation Notice

<div class="deprecation-banner">
  <p><strong>This version of Namara will be deprecated on January 31, 2021</strong>.</p>
  <p>You can use our new <a href="https://marketplace.namara.io/" target="_blank" rel="noopener">Namara Marketplace</a> to access data from our data providers as well as the world of open data. If you would like to continue using organizations, projects, account creation, and our API, please contact us at <a href="mailto:info@thinkdataworks.com">info@thinkdataworks.com</a>.</p>
</div>

# API Introduction

If you're looking to interact with the Namara API, you've come to the right place. This guide will cover instructions and conventions for using our API, with language-specific examples.

### API Fundamentals

>https://<span style="text-decoration:underline;text-decoration-color:#b52c2c">api.namara.io</span>/v0/<span style="text-decoration:underline;text-decoration-color:#17609f">data_sets/{DATA_SET_ID}/data/{VERSION}</span>?<span style="text-decoration:underline;text-decoration-color:#f7931d">api_key={YOUR_API_KEY}</span>&<span style="text-decoration:underline;text-decoration-color:#2fa821">query_parameters={VALUE}</span>

On the right is a basic template for a call made to our [Data API](#data-api). If it's confusing at first, don't worry - these docs are here to make sure you understand all the moving parts.

The <a href="#our-rest-api" style="text-decoration:underline;color:#b52c2c">API host</a> is where we serve the data from. The <a href="#data-api" style="text-decoration:underline;color:#17609f">data set and version</a> can be found under the API tab in any data set, and are required for every API call. 

Everything following the question mark (?) is a query parameter. <a href="#api-keys" style="text-decoration:underline;color:#f7931d">Your API key</a> is required to complete a call. <span style="color:#2fa821">Other parameters</span> can be added using an ampersand (&) and give you the ability to filter and refine the results. 

Query parameters change depending on the type of data set and the operation: <a href="#data-api" style="text-decoration:underline;color:#2fa821">basic parameters</a>, <a href="#geospatial-operators" style="text-decoration:underline;color:#2fa821">geospatial parameters</a>, <a href="#get-export" style="text-decoration:underline;color:#2fa821">exports</a>, and <a href="#get-aggregate" style="text-decoration:underline;color:#2fa821">aggregates</a> each show a chart with the different operations.

### Our REST API

The Namara API is a REST-based service that accepts and returns `JSON` in most cases (we'll cover [response formats](#formats-pagination-amp-ordering) later). Requests should be made to:

<code>https://{NAMARA_API_HOST}/v0/{ENDPOINT}</code>

Unless indicated otherwise, the Namara API host is `api.namara.io`, which will be used throughout the documentation. 

The responses will be outlined for each endpoint.

## Data Set Versions

Our data is updated regularly and automatically, so values in rows and columns may change. The data set version is a snapshot of the data set's structure and properties. When that structure changes, we create a new version of the data set. 

It is crucial that you include both the data set ID and the version in your queries. Both are found under the API tab for the data set.

##<div class="colour-pill"><span class="get">GET</span> Generic Endpoint</div>

>https://api.namara.io/v0/{ENDPOINT}

In the code column on the right is a generic endpoint, and the foundation of almost every call to our API. The chart below displays standard response codes:

Response | Description
-------- | -----------
200: OK | Success
202: Accepted | A background job has successfully been triggered - continue polling for details
401: Unauthorized | The API key you provided doesn't correspond to any user
403: Forbidden | User is not authorized to make that request
422: Unprocessable Entity | The server was unable to save the document; there will be more details in the full response body
429: Too Many Requests | User has exceeded the monthly maximum for requests or downloads (see <a href="#rate-limiting">Rate Limiting</a>)
 
## Making Requests

### Parameter Authentication

```shell
curl -XGET https://api.namara.io/v0/data_sets/{DATA_SET_ID}?api_key={YOUR_API_KEY}

  or

curl -i \
-H "Content-Type: application/json" \
-H "X-API-Key: {YOUR_API_KEY}" \
-X POST \
-d '{"query":"..."}' \
https://api.namara.io/v0/query.{FORMAT}
```

Append the key as `api_key` to any request body (either `GET` or `POST`) as long as it is included in the parameters.

### Header Authentication

The API Key can be sent in the request headers as `X-API-Key`. Append the key to the headers by adding `"X-API-Key: {YOUR_API_KEY}"`

### Testing Authentication

Here's how to verify your API Key:

###<div class="colour-pill"><span class="get">GET</span> Verify API Key</div>

Ping the API at `http://api.namara.io/v0/users/verify_token` to ensure that the API token is valid for the user.

>**1)**
`{ "status": true }`

>**2)**
`{ "status": false }`

Response | Description
-------- | -----------
200: OK | The API Key is valid (*example 1*)
422: Unprocessable Entity | API does not correspond to any user (*example 2*)

### Rate Limiting

Users are limited to 10,000 requests per month, as well as 100 data set downloads per month. When you exceed this limit, the API will return `status: 429`. If you find yourself meeting the limits, contact your account manager or <a href="mailto:info@thinkdataworks.com" target="_blank" rel="noreferrer noopener">email us</a> to find a solution.

# API Keys

Three kinds of API Keys can be created for accessing the Namara API - *Organization Keys*, *Project Keys*, and *Personal Keys*. This is an overview of managing all three types, and we'll outline the different use cases for each.

## Organization API Keys

For any non-personal Organization, visit the "Settings" tab and click "API Keys" to create API Keys for that Organization. Calls made with this API Key will be limited to Data Sets and Projects that your Organization has access to.

## Project API Keys

For any Project, visit the "Settings" tab and click "API Keys" to create API Keys for yourself for that Project. Calls made with this API Key will be limited to Data Sets that your Project has access to.

## Personal API Keys

The Personal Namara API Key can be obtained by clicking your user icon at the bottom of the left sidebar, then clicking "Account Settings". Copy this from the "API Key" tab and use it for any API calls.

<aside class="warning">Your Personal Key gives its user access to the entire Namara account. If you are thinking about sharing an API Key with another user, consider an Organization API Key or a Project API Key in order to limit their access to your account.</aside>
