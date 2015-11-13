# Data representation sample api
## Data Representation and Querying Project 2015
### Peter Nagy
Api Documentation - Museum Standards programme (data.gov.ie)

# About API
This api design tries to follow the best patterns of designing a RESTful HTTP api. The examples covered are:
- *PUT* - Replaces all found elements with the data passed to endpoint and idetified as. For safety use idenitfiers in request
- *POST* - Send data in as part of FORM or plain to modifie data or upload files. For additional security pass id.
- *GET* - Retreive single or list of data from API. Secure method.
- *DELETE* - Delete, removes objects identified uniqually. For security pass id with data.

The api should include additional security features like login and/or identification system like OAuth. This is not covered in this documentation.

##About the data
The data contains details about Irish museums, found in csv format, converted to json.
```
The original data can be found at:
https://data.gov.ie/dataset/museums-standards-programme-for-ireland/resource/f743ca36-5fe6-4396-b110-f138f7e61794

Direct link:
http://www.heritagecouncil.ie/fileadmin/user_upload/Heritage_Maps/LAMN-CSV.csv

Json data:
https://drive.google.com/file/d/0B31npoE6CRlSeUcxUFFKYS16ejA/view
```

**Sample of the Data**

    JSON
    [
    
      {
        "OBJECT_ID": 1,
        "Name": "Carlow County Museum",
        "Add1": 0,
        "Add3": "Carlow",
        "County": "Carlow",
        "Phone": "059 9172492/ 059 9170300",
        "Website": "http://www.carlowcountymuseum.ie",
        "Email": "museum@carlowcoco.ie",
        "Blurb": "Carlow County Museum is located in the Cultural Quarter of Carlow Town....",
        "ITM_East": 672220,
        "ITM_North": 676670
      },
      {
        "OBJECT_ID": 2,
        "Name": "Cavan County Museum",
        "Add1": 0,
        "Add3": "Ballyjamesduff",
        "County": "Cavan",
        "Phone": "049 8544070",
        "Website": "http://www.cavanmuseum.ie",
        "Email": "ccmuseum@eircom.net",
        "Blurb": "Exhibition galleries feature unique artefacts dating from the stone age up until the twentieth century, material spanning over 6000 years of occupation in Cavan.",
        "ITM_East": 652570,
        "ITM_North": 791100
      },
            ....
         ]


# DATA retreival
Data could be retreived from such an api trough lightweight front-end requests, made by JavaScript(Jquery/Ajax).
An example:
```
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://name.tld/api1/museum/2",
  "method": "GET",
  "headers": {
    "cache-control": "no-cache",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

Example request in CUrl:
```
curl -X GET -H "Cache-Control: no-cache" 'http://name.tld/api1/museum/2'
```

# Endpoint summary

Data Task | Endpoint | Parameter validation 
Return data(GET) | http://name.tld/api1/museum[/:id[/:county]] | (?<id>[0-9_-]+)(\.(?<format>(json|html|xml)))?
Add/cahange(POST) | http://name.tld/api1/museum[/:id]/{update | create}/[:form-data] | (?<id>[0-9_-]+)(\.(?<form-data>([a-zA-Z][a-zA-Z0-9_-]+)))?
Replace(PUT) | http://name.tld/api1/museum[/:id]/[:json-data] | (?<id>[0-9_-]+)(\.(?<format>(json)))?
REmove(DELETE) | http://name.tld/api1/museum[/:id] | (?<id>[0-9_-]+)?


# Sample requests:
The api's root is http://name.tld/api1/museum

**GET**
Get request is possible for the following:
http://name.tld/api1/museum[/:id[/:county]]
id: positive integer {refers to OBJECT_ID, returns object}
county: string {Irish country}

*Sample get*

    GET /api1/museum/2 HTTP/1.1
    Host: name.tld
    Cache-Control: no-cache

*Result*

    [
    {
        "OBJECT_ID": 2,
        "Name": "Cavan County Museum",
        "Add1": 0,
        "Add3": "Ballyjamesduff",
        "County": "Cavan",
        "Phone": "049 8544070",
        "Website": "http://www.cavanmuseum.ie",
        "Email": "ccmuseum@eircom.net",
        "Blurb": "Exhibition galleries feature unique artefacts dating from the stone age up until the twentieth century, material spanning over 6000 years of occupation in Cavan.",
        "ITM_East": 652570,
        "ITM_North": 791100
      }
    ]

    GET /api1/museum?county=Cork HTTP/1.1
    Host: name.tld
    Cache-Control: no-cache

*Result*
Would return result set a single element is:

    [
      {
        "OBJECT_ID": 4,
        "Name": "Cork Public Museum",
        "Add1": "Mardyke",
        "Add3": "Cork",
        "County": "Cork",
        "Phone": "021 4270679",
        "Website": "http://www.corkcity.ie/services/corporateandexternalaffairs/museum/",
        "Email": "museum@corkcity.ie",
        "Blurb": "Cork Public Museum is housed in a two storey Georgian house commanding a central position in Fitzgerald Park, Cork. Originally built in 1845 by the Beamish family, the building, then known as ?The Shrubberies? was their family home for decades. The property and surrounding land was eventually purchased by Cork Corporation for the purpose of housing the Cork International Exhibition of 1902 and 1903. The site of the exhibition was opened as Public Park in 1906. A site of some 18 acres of landscaped gardens; the park is a magnificent setting for the museum. Flanked by U.C.C. on one side and the riverside gardens of Sunday?s Well on the other, the park bestows a sense of history that complements the ambience of the museum perfectly. The Riverview Caf?, recently opened in the museum?s extension, fully exploits the commanding views of the river Lee and provides the visitor with a tranquil haven to enjoy a coffee and a snack.",
        "ITM_East": 565980,
        "ITM_North": 571680
      }
      ]


**POST**
Post request in this application resolve into data manipulation(unsecure). To add extra security all post request have to be made by passing the id of the object. If the request is "create", the id is in use the api would replace it, and return the id of the created object.
There are the following options:

http://name.tld/api1/museum[/:id]/{update | create}/[:form-data]
id: positive integer{refers to OBJECT_ID, returns object}
form-data: 
    "Name": string {required}
    "Add1": string {not-required}
    "Add3": string {not-required}
    "County": string {required}
    "Phone": string {required}
    "Website": string {not-required}
    "Email": string {required}
    "Blurb": string {not-required}
    "ITM_East": integer {required}
    "ITM_North":  integer {required}

*Sample post:*

    POST /api1/museum/2/update HTTP/1.1
    Host: name.tld
    Cache-Control: no-cache
    Content-Type: multipart/form-data;
    
    Content-Disposition: form-data; name="Name" value="Cork IT museum"
    
    Content-Disposition: form-data; name="Add1" value="Shop Street 2"
    
    Content-Disposition: form-data; name="Add3" value=
    
    Content-Disposition: form-data; name="County" value="1234567"
    
    Content-Disposition: form-data; name="Phone" value="1234567"
    
    Content-Disposition: form-data; name="Email" value="john@example.com"
    
    Content-Disposition: form-data; name="ITM_EAST" value=65465465
    
    Content-Disposition: form-data; name="ITM_NORTH" value=564616516

//create response
```
HTTP/1.1 201 Created
Location /api1/museum/3/create
```

**PUT**
The api can take in a json in the formate, a keys of the sample data above. 

The options for this endpoint:
http://name.tld/api1/museum[/:id]/[:json-data]


```
PUT /api1/museum/12 HTTP/1.1 
[
  {
    "OBJECT_ID": 12,
    "Name": "Bishop's Palace",
    "Add1": 0,
    "Add3": "Waterford",
    "County": "Waterford",
    "Phone": "051 304500",
    "Website": "http://www.waterfordtreasures.com/bishops-palace/index.htm",
    "Email": "waterfordtreasures@waterfordcity.ie",
    "Blurb": "The Mall is a beautiful walk...",
    "ITM_East": 660930,
    "ITM_North": 612350
  }
]
```


**DELETE**
The api can take in a DELETE, request with an id as idetifier, to delete the uniqually idetified item.

The options for this endpoint:
http://name.tld/api1/museum[/:id]


```
DELETE /api1/museum/12 HTTP/1.1 

//reponse
HTTP/1.1 201 Deleted
Location /api1/museum/12
```
