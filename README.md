# JAPI - JSON Schema API Description Language

## Intro (short one I promise)
The idea here is to use JSON Schema for API description, and not necessarily RESTful APIs, but PRCs also.

After reviewing several API DLs: https://en.wikipedia.org/wiki/Overview_of_RESTful_API_Description_Languages, it becomes apparent that some of them already use JSON Schema like Swagger's OpenAPI Specification, but some use YAML like RAML... Open API Specification can also be written in YAML... YAML, seriously? All of them are fairly complex for no reason, even if they use JSON Schema to some extent... Either that or I finally lost my mind trying to keep it simple.

You use JSON because it's super easy and natural to serialize and deserialize especially in the web frontends. JSON Schema exists to validate the JSON data but it's super suitable to describe it too, since validation and description are pretty goddamn close operations.

So what I want to define is fairly simple straight forward API description by only slightly extending the JSON Schema so I could use the same files for validating the API data, why not? And yeah I want to have multiple files, not one huge file to browse through.

## Specification

There are four types of files, meaning four different notations:

### 1) manifest.json

Manifest is an ordinary JSON file named *manifest.json* that describes the API basics.

* **url** (required) [string] - the only mandatory API description is API url
* **name** [string] - API name
* **description** [string] - API summary
* **version** [string] - API version string
* **logo** [string] - API logo, mostly for doc generation decoration purposes
* **author** [string] - Name Lastname <email@email.com>, Name Lastname <email@email.com>
* **headers** [array] - default headers, example: ["Content-Type: application/json"]
* **auth** [string] - what kind of auth the API is using?

### 2) endpoint.json

Endpoint files are the API endpoint descriptors, and perform the main role in this specification.
Each endpoint description file name should consist of the endpoint name and http method, for instance:

*Album.get.json* - endpoint: GET Album

*Album-id.get.json* - endpoint: GET Album/{id}

*Albums-search.get.json* - endpoint: GET Albums/search

*Album.post.json* - endpoint: POST Album

Content's of the file is a bit extended JSON Schema, described in the following schema:

Keywords of the extension:

* **endpoint** (required) [string] - endpoint url or RPC function, example: Album/{id} or Album.get
* **http_method** [enum: GET, PUT, PATCH, POST, DELETE, HEAD, OPTIONS] - http method if not declared as a part of the filename
* **headers** [array] - specific headers, example: ["Content-Type: application/json"]
* **roles** [array] - Who has the privileges to execute? ["admin", "superadmin", "user"]
* **responses** [string] - if you desire to point to a different file then the one recognized automatically (see: Part 3 - response.json)

The rest of the file is exactly the same as any JSON Schema. Here is a reminder of the schema keywords:

* **type** [enum: object, array, number, string, integer, null | array] - defines a value type, can have multiple values
* **description** [string] - describes the field, if it's in the top level it can be used to describe the endpoint
* **required** - if the value is object it defines which properties are mandatory. From the perspective of the endpoint description it defines what arguments sent are mandatory
* **properties** [object] -

### 3) response.json

### 4) model.json
