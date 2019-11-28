# eSign Genie Guide: Template API 10 Minute Quick Start

## Create a Document from a Template: Using eSign Genie's Template API

Use eSign Genie's Graphical User Interface (GUI) to auto generate the API code to needed create a legally binding document.

**Goal**: Gain an understanding of how to easily embed legally binding eSignatures into your website or application

**End Result**: Embed a modal popup in an HTML page that contains a legally binding Document. This modal can be accessed via a hyperlink, button, icon or picture. All 4 access methods will be demonstrated.

**Estimated time to complete this guide**: 10 minutes

**Prerequisites**:

- Text Editor (recommend Visual Studio, Brackets, NotePad++, etc)
- Basic understanding of HTML
- Free eSign Genie Sandbox (Test) API account activated
  - [Create your free Sandbox (Test) eSign API Account](./CreateSandboxEsignAPIGuide.md)

## This guide will show you how to:

[1) Use eSign Genie’s web-based GUI to auto-generate an API Request](#use)

   - [Upload a PDF to eSign Genie](#upload-a-pdf-to-esign-genie)
   - [Drag and Drop form fields onto the PDF](#drag-and-drop-form-fields-onto-the-pdf)
   - [Add metadata to the fields](#add-metadata-to-the-fields)
   - [Auto-generate API code](#auto-generate-api-code)

[2) Understand the Generated API Code](#understand-the-generated-code)

   - [Authenticating the request](#authenticating-the-request)
   - [Request Body](#request-body)
   - [Response Body](#response-body)
   - [What sections you will want to copy](#what-sections-you-will-want-to-copy)

[3) Embed an eSign iframe into a div, widget, hyperlink, modal, popup, or window](#embed)
   - [Sample HTML page](#sample-html-page)
   - [link to finished page](#link-to-finished-page)
   - [HTML page code](#html-page-code)

[4) Start collecting eSignatures](#start-collecting-esignatures)

## 1) Use eSign Genie’s web based GUI to auto-generate an API Request <a name="use"></a>

> Here is an in-depth article on creating a template via our GUI interface. [Template Article](https://www.esigngenie.com/help-center/howto/create-reusable-pdf-document-templates)

**Important**: You will need to have a [free eSign Genie Sandbox](./CreateSandboxEsignAPIGuide.md) (Test) API account active.

### Upload a PDF to eSign Genie <a name="upload-a-pdf-to-esign-genie"></a>

 > Our Automatic API Code Generation works with eSign Genie’s [Template Feature](https://www.esigngenie.com/help-center/howto/create-reusable-pdf-document-templates)

1. Log into your [eSign Genie account](https://www.esigngenie.com/esign/)
2. Go to your eSign Genie [Template](https://www.esigngenie.com/esign/listTemplates) area
3. Click on the “+Create a New Template” button
4. Upload the PDF file that you want to transform into a legally binding electronic document

> Here is a sample PDF you can [download](https://s3-us-west-1.amazonaws.com/esigngenie.info/PublicAssets/eSign+Genie+Example.pdf)

_GIF of "Upload a PDF to eSign Genie" Steps 1 - 4_
![Upload a PDF to eSign Genie](https://s3-us-west-1.amazonaws.com/esigngenie.info/PublicAssets/UploadPDFtoEsignGenie.gif)

### Drag and Drop form fields onto the PDF<a name="drag-and-drop-form-fields-onto-the-pdf"></a>

On the left side of the screen are a list of possible fields to Drag and Drop onto the PDF. Drag and Drop the following fields:

    1. Signature Field - Auto-populates
    2. Text Field - Requires metadata

Our Drag and Drop fields contain their own metadata such as:

- Field Type
- Field Hieght and Width
- Field X and Y Coordinates
- Field Document Number

You can also easily add your own custom metadata:

- Field Name
- Field Value
- Assigned Party
- Tool Tip
- Required Field Flag
- Custom, RegEx Validation
- Font Size
- Font Color

_GIF of Dragging and Dropping form fields onto the PDF_
![Drag and Drop form fields onto the PDF](https://s3-us-west-1.amazonaws.com/esigngenie.info/PublicAssets/DragAndDropFormFieldsOntoThePDF.gif)

### Add metadata to the fields<a name="add-metadata-to-the-fields"></a>

1. Click on the **Text Field** that you Dragged and Dropped onto the PDF in the previous step. 
2. You will notice a **Properties** pane open up on the right. Here you can easily add meta data to the field.
3. Make sure that you give the Text Field a **Field Name**. We will populate the value in a future step.

_GIF of Adding Metadata to the fields_
![Add Metadata to the fields](https://s3-us-west-1.amazonaws.com/esigngenie.info/PublicAssets/AddMetadataToTheFields.gif)

### Auto-generate API Code<a name="#auto-generate-api-code"></a>

1. Go to your eSign Genie [Template](https://www.esigngenie.com/esign/listTemplates) area
2. Select the Template
3. Click "</> Generate API Code"

_GIF of Auto-Generating API Code_


## 2. Understand the Generated Code<a name="#understand-the-generated-code"></a>

The code is generated in PHP cURL. You can copy and paste the JSON request body and API credintials into an API development tool like Postman.
Run in Postman.

### **Here is an example of eSign Genie's automatically generated code**

```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => "https://www.esigngenie.com/esign/api/oauth2/access_token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  //Here is your API client id key and secret key
  CURLOPT_POSTFIELDS => "grant_type=client_credentials&client_id=YOUR_CLIENT_ID_API_KEY&client_secret=YOUR_SECRET_API_KEY&scope=read-write",

  CURLOPT_HTTPHEADER => array(
	"content-type: application/x-www-form-urlencoded"
	),
));

$response = curl_exec($curl);
curl_close($curl);

$data_string = json_decode($response);

//Stores the access token to be used for the request.
$access_token = $data_string->access_token;


$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => "https://www.esigngenie.com/esign/api/templates/createFolder",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  //request JSON body begins below with {
  CURLOPT_POSTFIELDS => '{
	"folderName":"folder name",
	"templateIds":[49475],
	"fields":
		{
  			"Comments":"TEST_VALUE",
			"Start Date":"TEST_VALUE",
			"Date Signed":"TEST_VALUE",
			"Yes":"false",
			"No":"false",
			"Signer Name":"TEST_VALUE"
		},
	"parties":[
		{
			"firstName":"party_1_firstname",
			"lastName":"party_1_lastname",
			"emailId":"party_1_emailId",
			"permission":"FILL_FIELDS_AND_SIGN",
			"sequence":1
		}
	],
	"signInSequence":false,
	"createEmbeddedSigningSession":true,
	"createEmbeddedSigningSessionForAllParties":true,
	"signSuccessUrl":"",
	"signDeclineUrl":"",
	"themeColor":"#0066CB"
	}',

  CURLOPT_HTTPHEADER => array(
	"Authorization: Bearer ".$access_token,
	"content-type: application/json"
	),
));

$response = curl_exec($curl);
curl_close($curl);

$data_string = json_decode($response);
$embedded_session_URL = $data_string->embeddedSigningSessions[0]->embeddedSessionURL;
```

### Authenticating the request<a name="#authenticating-the-request"></a>

**BE CAREFUL** The top portion of the code contains your actual API client key and secret key.
Once the access token is generated, it will be saved in the $access_token variable. (The last line of code below.)
```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => "https://www.esigngenie.com/esign/api/oauth2/access_token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",

  //Here is your API client key and secret key
  CURLOPT_POSTFIELDS => "grant_type=client_credentials&client_id=4420a1eb12614b8d89afadb284935e9b&client_secret=d4a963d0b72a419aa1d0b63481cd13ea&scope=read-write",

  CURLOPT_HTTPHEADER => array(
	"content-type: application/x-www-form-urlencoded"
	),
));

$response = curl_exec($curl);
curl_close($curl);

$data_string = json_decode($response);

//Stores the access token to be used for the request.
$access_token = $data_string->access_token;
```

### Request Body
<a name="#request-body"></a>


> The body of the request can be copy & pasted into Postman.

**Prepopulate Field Values:** You can change the values of the fields if you would like them prepopulated with a value. The values are all TEST_VALUE by default.

**Null Field Value:** Leave the field value "" if a null value is desired.

**Updating Party Information:** Make sure to send use a valid email address.

```php
//request JSON body begins below with {
{
	"folderName":"folder name",
	"templateIds":[49475],
	"fields":
		{
  			"Comments":"TEST_VALUE",
			"Start Date":"TEST_VALUE",
			"Date Signed":"TEST_VALUE",
			"Yes":"false",
			"No":"false",
			"Signer Name":"TEST_VALUE"
		},
	"parties":[
		{
			"firstName":"party_1_firstname",
			"lastName":"party_1_lastname",
			"emailId":"party_1_emailId",
			"permission":"FILL_FIELDS_AND_SIGN",
			"sequence":1
		}
	],
	"signInSequence":false,
	"createEmbeddedSigningSession":true,
	"createEmbeddedSigningSessionForAllParties":true,
	"signSuccessUrl":"",
	"signDeclineUrl":"",
	"themeColor":"#0066CB"
	}
```

### 

> Written with [StackEdit](https://stackedit.io/).
