DataFornix: Upgrade from Get ocr v1 to Get pcr v2
=================================================

## This documantation shows how to upgrade existing get_ocr_api v1 to get_ocr_api V2 preview.

### 1. Create User Token

**Note**: You must call create user token api first before any other api. It will return you token in **auth_token** key. That key will pass in other api call as **auth_token** header

This api is responsiable for create user.

##### 1.0 **Explanation of server_name and tenant_name**
  **server_name** Server name is the bases of tenant eniviroment which you are using. The detail is mentioned in the point 10 in tabular format.
  **tenant_name** Tenant name is the bases of which bank or organisation is consuming get ocr apis. The detail is mentioned in the point 10 in tabular format.
  
  | Server Name                                     | Tenant Name | Api token                          |
  |-------------------------------------------------|-------------|------------------------------------|
  | qat.datafornix.com  (Development env)           | mashreq     | “vB2lWt8gicVs34yXoxH62VsjeLPWCxrH” |
  | mashreq.datafornix.com (Production env)         | mashreq     | “zLuv2i8lNy6phDMav6tXn3mFucb6kVEs” |
  | pruvista-qat.datafornix.com (Development env)   | pruvista    | “OEADSSXTckpVDSY0MOU6Mxa9rSwIAjkC” |
  | datafornix.workplacecredit.com (production env) | pruvista    | “Ol7DANgW0F2BXKRDkKXvI58bgG8o69ge” |
  | build.datafornix.com (apix env)                 | apix        | “k27t5uuk3pWsJYbGP0Lognnvu2mZkUEH” |


##### 1.1 **API URL**  
`https://{server_name}.datafornix.com/{tenant_name}/api/v1/create-user-token/`


##### 1.2 **Method Type**
`POST`

##### 1.3 **Require Parameters**

##### Request ( ** application/json**  )

````js
{
    "token":"anupam123", // Required: user unique token here
    "reference_number": "reference number here", // optional
    "channel": "web", // optional
    "type_of_request": "request type here" // optional
}
````

##### Headers

````js
{
    "Token": "Token vB2lWt8gicVs34yXoxH62VsjeLPWCxrH"
}
````

##### 1.4 **Return Response**

It will return following object array.

```
{
    "auth_token": "5cdec08fa920c440dd56af2a",
    "name": "test",
    "channel": "web",
    "reference_number": "1234",
    "type_of_request": ""
}
```

### 2. Document Capture

This api is responsiable for get OCR data of provided documents. 

##### 3.1 **API URL**  
| Get Ocr Api V1                                                                   | Get Ocr Api V2                                                                   |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| https://{server_name}.datafornix.com/{tenant_name}/api/v1/asset/get-ocr-results/ | https://{server_name}.datafornix.com/{tenant_name}/api/v2/asset/get-ocr-results/ |

##### 3.2 **Method Type**
`POST`

##### 3.3 **Require Parameters**

##### Api Request( ** application/json **  )
````js
	**Note**:- The following improvment with get ocr api version 2:
	       1. Get ocr version one only support images. But get ocr api version two support pdf or image both.
	       2. Get ocr version one  need to send front and back image both in request to get the response. But get ocr version support front and back image      together in one call.
       
	**Note**:- The following changes in request structure:
		1. You can send "images_string" value as base64 string of pdf document to get the response of pdf document.
		2. You can send "image_string" value as base64 sring of combined front and back image to get the response of document.
          
### Request structure of get ocr api V1
       
{
    "document_type": 'Identity Card' // Required: or 'Driving Licence', 'Passport', 'Vehicle Registration'
    "reference_number": "reference number here",
    "images": [{
        "image_string": "image base64 string here",
        "authority": "authority string herer",
        "description":"Front" // Use "Back" for back image
    }, {
        ...
    }], // Required
    "channel": "web", // optional
}
````

##### Request for get_ocr_v2( ** application/json **  )
````js
{
    "document_type": 'Identity Card' // Required: or 'Driving Licence', 'Passport', 'Vehicle Registration'
    "reference_number": "reference number here",
    "images": [{
        "image_string": "image base64 string here/ pdf base64 string",
        "authority": "authority string herer",
        "description":"Front" // Use "Back" for back image
    }, {
        ...
    }], // Required
    "channel": "web", // optional
}
````

##### Headers

````js
{
    "Authorization": "bearer 'auth_token'", // token that you will receive in create-user => auth_token key response
    "Token": "Token 'api_token'" // Add a valid api_token
}
````
##### 3.4  Response ( ** application/json **  )
````js
**Note**:- The response structure is same with V1 and V2.
{
    "application_error_list": [     
        {
            "error_code": "",
            "error_message": ""
        },
        {
            "error_code": "",
            "error_message": ""
        }
    ],
    "data": {
        "asset_type": "",
        "channel": "web",
        "reference_number": "",
        "type_of_request": "get OCR",
        "properties": {}      // All extracted fields with values come here
    },
    "image_feedback": {
        "back_image_resolution": "",
        "is_front_exif_data": "",
        "front_image_orientation": "",
        "front_image_resolution": "",
        "is_back_exif_data": "",
        "back_image_orientation": ""
    },
    "image_quality_feedback": ""
}
````
### 4. Get Pdf Ocr

This api is responsiable for get OCR data of provided documents. 

##### 4.1 **API URL**  
`https://{server_name}.datafornix.com/{tenant_name}/api/v1/asset/get-pdf-ocr/`

##### 4.2 **Method Type**
`POST`

##### 4.3 **Require Parameters**

##### Request ( ** application/x-www-form-urlencoded **  )
````js

    "extraction_mode": 'realtime' 
    "document_type": "Trade Licence/Invoice",
    "pdf": "Upload a pdf file here" // Required
    "reference_number":123, // optional
    "channel":web

````

##### Headers

````js
{
    "Authorization": "bearer W83CdPEc5wf25aFKBM8zvIdBXZ4nbx0X", // token that you will receive in create-user => auth_token key response
    "Token": "Token vB2lWt8gicVs34yXoxH62VsjeLPWCxrH" // Add a valid api_token
}
````
##### 4.5  Response ( ** application/json **  )
````js
{
    "data": {
        "channel": "web",
        "reference_number": "123",
        "properties": {}                  // All extracted fields with values come here
    },
    "application_error_list": []
}
````


### 8. Document Capture Version 2

This api is responsiable for get OCR data of provided documents. 

##### 8.1 **API URL**  
`https://{server_name}.datafornix.com/{tenant_name}/api/v2/asset/get-ocr-results/`

##### 8.2 **Method Type**
`POST`

##### 8.3 **Require Parameters**

##### Request ( ** application/json **  )
````js
{
    "document_type": 'Identity Card' // Required: or 'Driving Licence', 'Passport', 'Vehicle Registration'
    "reference_number": "reference number here",
    "images": [{
        "image_string": "image base64 string here",
        "authority": "authority string herer",
        "description":"Front" // Use "Back" for back image
    }, {
        ...
    }], // Required
    "channel": "web", // optional
}
````

##### Headers

````js
{
    "Authorization": "bearer W83CdPEc5wf25aFKBM8zvIdBXZ4nbx0X", // token that you will receive in create-user => auth_token key response
    "Token": "Token vB2lWt8gicVs34yXoxH62VsjeLPWCxrH" // Add a valid api_token
}
````
##### 8.4  Response ( ** application/json **  )
````js
{
    "application_error_list": [     
        {
            "error_code": "",
            "error_message": ""
        },
        {
            "error_code": "",
            "error_message": ""
        }
    ],
    "data": {
        "asset_type": "",
        "channel": "web",
        "reference_number": "",
        "type_of_request": "get OCR",
        "properties": {}      // All extracted fields with values come here
    },
    "image_feedback": {
        "back_image_resolution": "",
        "is_front_exif_data": "",
        "front_image_orientation": "",
        "front_image_resolution": "",
        "is_back_exif_data": "",
        "back_image_orientation": ""
    },
    "image_quality_feedback": ""
}
````

#### 11  **How to use ocr apis**

	Step-1:- First Use this url to access the create_user_token api:

		https://{Server_Name}.datafornix.com/{tenant_name}/api/v1/create-user-token/


	Step-2:- Use this url to access the create_user_token api version 2:

		https://{Server_Name}.datafornix.com/{tenant_name}/api/v2/create-user-token/


	Step-3:- **Below are the header and request body information:**

		Headers:-

			Token: Token "api_token"      //(It's type is string. You can find it by api documantation point 1.0)
			Content-Type: application/json


		Request body:-

			{
			"token":"Your_token_name",
			"channel": "web/android/ios",
			"reference_number": "your reference number in character-field format",
			"type_of_request": "async/sync"
			}


		channel:-    Select the channel you want to make the request from. The API supports either web, or android or ios types.
		reference_number:- a character-field type generated at client's end for Datafornix reference purpose
		type_of_request:- whether the request is asynchronous(async) or synchronous(sync)


		Response body
			{
			    "auth_token": "user_authentication_token",
			    "name": "Guest Username",
			    "token": "Request_body token",
			    "channel": "Request_body channel",
			    "reference_number": "Request_body reference_number",
			    "type_of_request": "Request_body type_of_request"
			}

	Step-4:- 

		For example you get auth_token it like that way. You need to use it in get pdf ocr and ocr api for user authentication.
		auth_token:- "WgHAsQvpclZ7CDJUj50CKRo6dWfcqd3j"

	Step-5:- 

		Use this below url to access the get ocr api version one and two and pdf ocr apis:

		a. The GET OCR API version one can be accessed via the following URL:

			https://{server_name}.datafornix.com/{tenant_name}/api/v1/asset/get-ocr-results/

		b. The GET OCR API version two can be accessed via the following URL:

				https://{server_name}.datafornix.com/{tenant_name}/api/v2/asset/get-ocr-results/

		c. The GET pdf OCR API for pdf type document can be accessed via the following URL:

				https://{server_name}.datafornix.com/{tenant_name}/api/v1/asset/get-pdf-ocr/

		Request Header for the above is:

			Token:   Token "api_token" (It's type is string. You can find it by api documantation point 1.0)
			Authorization: bearer "auth_token"(Use auth_token what we get form create_user_token)

		1. If the document has single image like passport. It is the request structure of get ocr api. So the request structure as below:-

			Request Body:
			{
			  "document_type": "",
			  "channel": "web",
			  "reference_number": "123",
			   "images": [
				{
					"authority": "AE",
					"description": "Front",
					"image_string": ""
				}
			]
			}

		2. If the document has front and back both image like identity card. So the request structure as below:-


			Request Body:

			{
			  "document_type": "",
			  "channel": "web",
			  "reference_number": "123",
			   "images": [
				{
					"authority": "AE",
					"description": "Back",
					"image_string": ""
				}
			]
			}


		3. If the document is pdf or image. It is the request structure of get pdf ocr api.So the request structure as below:-

			extraction_mode:realtime
			document_type:"Invoice/Trade Licence"
			reference_number:""
			channel:""
			type_of_request""
			pdf: "Image/PDF"  //"File_object"

	Step-6:- The get ocr api version 1 and version 2 response below:

		{
	    "application_error_list": [     
		{
		    "error_code": "",
		    "error_message": ""
		},
		{
		    "error_code": "",
		    "error_message": ""
		}
	    ],
	    "data": {
		"asset_type": "",
		"channel": "web",
		"reference_number": "",
		"type_of_request": "get OCR",
		"properties": {}      // All extracted fields with values come here
	    },
	    "image_feedback": {
		"back_image_resolution": "",
		"is_front_exif_data": "",
		"front_image_orientation": "",
		"front_image_resolution": "",
		"is_back_exif_data": "",
		"back_image_orientation": ""
	    },
	    "image_quality_feedback": ""
	}

	Step-7:- The pdf ocr api version response below:-

		{
		    "data": {
			"channel": "web",
			"reference_number": "123",
			"properties": {}                  // All extracted fields with values come here
		    },
		    "application_error_list": []
		}
