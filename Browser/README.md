Zebra Savanna Data Services JavaScript SDK
===================================

This is the source code for the Savanna JavaScript SDK.  
The core of this SDK is a basic API connection call to the Zebra Savanna Data Services in the SavannaAPI class.  Currently the API has public methods for the three public APIs that are part of the Barcode Intelligence Product.  These APIs are:
* Barcode Generate
* UPC Lookup
* FDA Recall
  

### API Key
To get an API key to work with these APIs, use the [Getting Started Guide](https://developer.zebra.com/gsg) and select the Barcode Intelligence product. 


Usage guide
-----------

Set Zebra Savanna API key

`setApiKey("API-Key-Goes-Here");`

Create Barcode
```
 Create("Desired-Symbology", "Barcode-Value", scale, "Rotation", showValueText, baseApiKey)
     .then( data => {
         console.log("Create Barcode: " + data);
         var reader = data.body.getReader();
         var imgData = "";
         window.image = document.createElement('img');
         document.body.appendChild(image);
         return new ReadableStream({
                 start(controller) {
                       return pump();
                   function pump() {
                         return reader.read().then(({ done, value }) => {
                           // When no more data needs to be consumed, close the stream
                             if (done) {
                                   controller.close();
                                 return;
                           }
                         // Enqueue the next data chunk into our target stream
                           controller.enqueue(value);
                           return pump();
                         });
                       }     
                 }  
         })
     })
     .then(stream => new Response(stream))
     .then(response => response.blob())
     .then(blob => URL.createObjectURL(blob))
     .then(url => console.log(window.image.src = url))
     .catch(err => console.error(err));
```

UPC Lookup
```
UPCLookup("Barcode-Value-Goes-Here", baseApiKey)
    .then(data => { 
        var items = "";
        var listInfo = "";
        data.json().then( (d) => {
             d.items.forEach(function(i, ind, arr){ listInfo += JSON.stringify(i); });
            console.log( "UPC Lookup: " + listInfo);
        });
    
    })
    .catch(error => {
        console.log("UPC Lookup Failed");
        console.log(error);
    });
```

FDA Food Recall
```
FoodUpc("Barcode-Value-Goes-Here", result-Count-Goes-Here, baseApiKey)
    .then( data => {
        var items = "";
        var listInfo = "";
        data.json().then( (d) => {
            d.results.forEach(function(i, ind, arr){ listInfo += JSON.stringify(i); });
            console.log("Food UPC: " + listInfo)
        });
    })
    .catch(error => {
        console.log("Food UPC lookup failed!");
    });
```

FDA Drug Recall
```
DrugUpc("Barcode-Value-Goes-Here", result-Count-Goes-Here, baseApiKey)
    .then( data => {
        var items = "";
        var listInfo = "";
        data.json().then( (d) => {
            items = d;
            d.results.forEach(function(i, ind, arr){ listInfo += JSON.stringify(i); });
                console.log("Drug UPC: " + listInfo);
        });
    })
    .catch( error => {
    console.log("Drug UPC lookup failed!");
    });                                                                                                        
```

FDA Device Recall Search
```
 DeviceSearch("Search-Text-Goes-Here", result-Count-Goes-Here, baseApiKey)
    .then( data => {
        var items = "";
        var listInfo = "";
        data.json().then( (d) =>  {
            items = d;
            d.results.forEach(function(i, ind, arr){ listInfo += JSON.stringify(i); });
            console.log("Device Search: " + listInfo);
        });
    })
    .catch( error => {
        console.log("Device lookup failed!")
    });
```

FDA Drug Recall Search
```
DrugSearch("Search-Text-Goes-Here", result-Count-Goes-Here, baseApiKey)
    .then( data => {
        var items = "";
        var listInfo = "";
        data.json().then( (d) => {                                                                 
            items = d.results;
             d.results.forEach(function(i, ind, arr){ listInfo += JSON.stringify(i); });
            console.log("Drug Search: " + listInfo);
        });
    })
    .catch( error => {
        console.log("Drug Search failed!");
    });
```
