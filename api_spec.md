# Comments API
The comments API will be used to post thoughts and/or comments to the user's feed.
The comments will integrate into the feed in a chronological order.

Requests and response are in JSON.

**Authentication and Headers**  
Headers should include the application's access_token and the following headers:  
```
"Authorization: Bearer ${access_token}"
"Content-Type: application/json; charset=utf8"
```

**Methods Overview**  
API Endpoint base URL is: http://api.bigpanda.io/api/v1  

Action	| Methods |	API Endpoint |	Description  
--------|---------|--------------|--------------  
Create |	POST 	| /comments |	Send a new comment to BigPanda's feed.  
Read |	GET |	/comments/{comment_id} | Read a comment from BigPanda's feed.  
Read All |	GET |	/comments	| Read all available comments from BigPanda's feed.  
Update |	POST |	/comments/{comment_id}	| Update a comment on BigPanda's feed.  
Delete |	DELETE |	/comments/{comment_id}	| Delete a comment from BigPanda's feed.  
Note: The comment id is returned upon a successful comment creation API call.

**Parameters**  
The JSON can contain a subset of the following fields.  

Field |	Description |	Example  
------|-------------|--------  
`app_key` |	Application key, created in the first step of a BigPanda tutorial.	| `"app_key": "<app key>"`  
`comment`	| Text to post as a comment on user's feed.	| `"comment": "Uploaded new version to production – v2.0.3"`  
`count`	| The number of records to return when reading all comments.	| `"count": 100`  
`offset`	| The number of records to skip before reading the next comments set.	| `"offset": 100`  
 

**Example JSON for comment creation**  
```JSON
{  
  "app_key": "123",  
  "comment": "Uploaded new version to production – v2.0.3"  
}
```  

**Example JSON for reading a single comment**  
Note: comment_id is the id returned in the creation of a comment's call.
```JSON
{
  "app_key": "123",
}
```

**Example JSON for reading all comments**
```JSON
{
  "app_key": "123",
  "count": 100,
  "offset": 200
}
```

**Example JSON for updating a comment**  
Note: comment_id is the id returned in the creation of a comment's call.  
```JSON
{
  "app_key": "123",
  "comment": "Uploaded new version to production – v2.0.4"
}
```

**Example JSON for deleting a comment**  
Note: comment_id is the id returned in the creation of a comment's call.  
```JSON
{
  "app_key": "123",
}
```

**Response Codes**  
When an API request is successful, the BigPanda server sends a response to indicate that it received the event. If a request fails, the response code may help with troubleshooting the integration.  

Response                   |	Description  
---------------------------|-------------  
`200 OK`	| Success.  
`201 Created` |	New comment created.  
`204 No Content` |	The server successfully processed the request and is not returning any content.  
`400 Bad Request`	| Default code for invalid requests. For example, it is missing a mandatory field. Check the error message and ensure that the JSON includes the correct parameters.  
`401 Unauthorized` |	Token is invalid or missing. Check that the request includes the correct HTTP headers.  
`404 Not Found` |	Requested endpoint isn't available. Ensure that the request uses one of the API endpoints specified in the documentation.  
`409 Conflict`	| Request cannot be performed due to a conflict. For example, attempting to read a comment that was deleted.  
`500 Internal Server Error` |	Default code for errors that occur due to problems on BigPanda servers. Retry the request after some time.  
`501 Not Implemented`	| Unsupported method.  

**Response Body Parameters**  
For a successful API call, the response body will include the response code and comment_id value.  
```JSON
{
  "meta": {
        "code": 200
    }, 
  "comment_id": 87612 
}
```

**Examples**  
Executing these commands in Shell, should present the results in BigPanda almost instantaneously.  

To use these commands, the user must replace the token and app key with the corresponding values in BigPanda, and must specify that the Content-type is application/json.  

_Create:_
```curl
curl -X POST -H "Content-Type: application/json" \
    -H "Authorization: Bearer <YOUR TOKEN>" \
    https://api.bigpanda.io/data/v1/comments \
    -d '{ "app_key": "<APP KEY>", "comment": "New version uploaded to production – v2.0.1"}
```

_Read:_  
```curl
curl --request GET http://api.bigpanda.io/api/v1/comments/{comment_id}
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR TOKEN>" \
```

_Read all:_
```curl
curl --request GET http://api.bigpanda.io/api/v1/comments?count={count}&offset={offset} \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR TOKEN>" \        
     
```

_Update:_
```curl
curl -X POST -H "Content-Type: application/json" \
    -H "Authorization: Bearer <YOUR TOKEN>" \
    https://api.bigpanda.io/data/v1/comments/{comment_id} \
    -d '{ "app_key": "<APP KEY>", "comment": "New version uploaded to production – v2.0.1"}
```

_Delete:_
```curl
curl -X DELETE \
    -H "Authorization: Bearer <YOUR TOKEN>" \
    https://api.bigpanda.io/data/v1/comments/{comment_id} \
    -d ""
```
