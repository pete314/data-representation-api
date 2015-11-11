# data-representation-api
Sample api documentation

data set
https://data.gov.ie/dataset/museums-standards-programme-for-ireland/resource/f743ca36-5fe6-4396-b110-f138f7e61794

direct link
http://www.heritagecouncil.ie/fileadmin/user_upload/Heritage_Maps/LAMN-CSV.csv

**Sample Data**

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



***Sample requests:***
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
Post request in this application resolve into data manipulation(unsecure). To add extra security all post request have to be made by passing the id of the object. 
There are the following options:

http://name.tld/api1/museum[/:id]/update/[:form-data]
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

