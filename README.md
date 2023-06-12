
## Header Tab - Auth Token

|#| Info|Snippet |
| :--- |:--- |:--- |
| 1 |Define Pre-Request Script |Refer to 'Pre-Request-Script Tab' for details
| 2 |Add Authorization Header |
| 3 |Post Run - Validation | Refer to 'Tests Tab' for details


## Pre-Request-Script Tab
- res.json().access_token is a field in the json response

```
let keySecret = ("key" + ":" + "Secret");
let base64EncodedData = CryptoJS.enc.Base64.stringify(CryptoJS.enc.Utf8.parse(keySecret));		
pm.environment.set("base64EncodedData", base64EncodedData);

pm.sendRequest({
    url: 'https://xyz.com/oauth2/v4/tokens/',
    method: 'POST',
    header: {
        'Authorization': 'Basic ' + pm.environment.get("base64EncodedData"),
        'Cache-Control': 'no-cache',
        'content-type': 'application/json',
        'accept': 'application/json',
    }
}, function (err, res) {
    pm.environment.set("access_token", res.json().access_token);
});
```

## Add Authorization Header

- Below 'access_token' parameter should match with 'Pre-Request-Script' last statement

```
Bearer {{access_token}}
```

## Tests Tab

```
var jsonData = pm.response.json();

pm.test("Verify response status code test ", function () {
    pm.response.to.have.status(200);
});

pm.test("verify no errors in response test", function () {
     pm.response.to.be.ok;
     pm.response.to.be.withBody;
     pm.response.to.be.json;
});

pm.test("Verify returnCode in response message", function () { 
	pm.expect(jsonData.returnCode).is.to.equal("0"); 
});

```
