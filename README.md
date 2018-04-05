# Description
An android asynchronous http client based on HttpURLConnection.

# Versions
* [v1.0.0](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.0/README.md "Version 1.0.0 Overview")
* [v1.0.1](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.1/README.md "Version 1.0.1 Overview")
* [v1.0.2](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.2/README.md "Version 1.0.2 Overview")

# Getting Started (V1.0.2)
Add the dependency in build.gradle (App module)

```compile 'com.eukaprotech.networking:networking:1.0.2@aar'```

Add permission in manifest file

```<uses-permission android:name="android.permission.INTERNET" />```

# Usage (V1.0.2)

# Request Methods covered:

* GET
* POST
* PUT
* DELETE
* HEAD
* OPTIONS


GET request:
        
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.get("url", new AsyncConnectionHandler() { 
            @Override
            public void onStart() {
               //you can choose to show a progress dialog here
            }

            @Override
            public void onSucceed(int responseCode, HashMap<String, String> headers, byte[] response) {
                //consume the success response here
            }

            @Override
            public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
                //consume the fail response here
            }

            @Override
            public void onComplete() {
               //you can dismiss the progress dialog here
            }
        });
        
Sample GET request:
        
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.get("https://www.google.com", new AsyncConnectionHandler() { 
            @Override
            public void onStart() {
               //you can choose to show a progress dialog here
            }

            @Override
            public void onSucceed(int responseCode, HashMap<String, String> headers, byte[] response) {
                //consume the success response here
            }

            @Override
            public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
                //consume the fail response here
            }

            @Override
            public void onComplete() {
               //you can dismiss the progress dialog here
            }
        });
   
# Query Parameters

To attach query parameters to a url:

        Parameters query_parameters = new Parameters();
        query_parameters.put("key1", "value1");
        query_parameters.put("key2", "value2");
       
        String url = URLBuilder.build("initial_url", query_parameters);
        //if the initial_url already contains query parameters, the new query parameters are just added at the end of existing ones.
        //in case of conflicting keys between the existing query parameters and the new ones; the new ones are given priority.
        //you can then use the resulting url in a request
        
Sample:
          
        Parameters query_parameters = new Parameters();
        query_parameters.put("q", "discover");
        String url = URLBuilder.build("https://www.google.com/search", query_parameters);
        //will result into https://www.google.com/search?q=discover
        //you can then use the resulting url in a request
       
       AsyncConnection asyncConnection = new AsyncConnection();
       asyncConnection.get(url, new AsyncConnectionHandler() {
            // the implemented listener methods onStart, onSucceed, onFail & onComplete
        });
        
        
# Body Parameters (Used for POST & PUT)

To attach parameters as the body/content of the request
        
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        
        //For a POST request
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.post("url", parameters, new AsyncConnectionHandler() {  
            // the implemented listener methods 
        });
        
        //For a PUT request
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.put("url", parameters, new AsyncConnectionHandler() {  
            // the implemented listener methods
        });
             
# Request Headers

To attach request headers:
    
    HashMap<String, String> headers = new HashMap<>();
    headers.put("header_key1", "header_value1");
    headers.put("header_key2", "header_value2"); 
            
    //For a POST request 
    Parameters parameters = new Parameters();
    parameters.put("key1", "value1");
    parameters.put("key2", "value2");
    AsyncConnection asyncConnection = new AsyncConnection();
    asyncConnection.post("url", headers, parameters, new AsyncConnectionHandler() { 
          // the implemented methods 
    });
            
    //For a GET request 
    AsyncConnection asyncConnection = new AsyncConnection();
    asyncConnection.get("url", headers, new AsyncConnectionHandler() { 
          // the implemented listener methods 
    });
    //Other request methods have similar way of attaching request headers
        
# Body JSONObject (Used for POST & PUT)

JSONObject can be used in place of Parameters as the body/content of the request:

        JSONObject jsonObject = new JSONObject();
        try{
            jsonObject.put("key1", "value1");
            jsonObject.put("key2", "value2");
        }catch (Exception ex){}
        
        HashMap<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json");    //header for JSONObject being the body/content
        
        //For a POST request
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.post("url", headers, jsonObject, new AsyncConnectionHandler() {  
            // the implemented listener methods 
        });
        
        //For a PUT request
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.put("url", headers, jsonObject, new AsyncConnectionHandler() {  
            // the implemented listener methods 
        });
        
# Basic Authentication

To attach a basic auth in the request headers:

 Method 1: (Handle it)
 
        HashMap<String, String> headers = new HashMap<>();
        String username = "example@gmail.com"; 
        String password = "123456789";
        String auth_value = username+":"+password;
        String basicAuth = "basic "+ Base64.encodeToString(auth_value.getBytes(), Base64.NO_WRAP);
        headers.put("Authorization", basicAuth);
        
 Method 2: (Let AsyncConnection handle it)
         
        String username = "example@gmail.com"; 
        String password = "123456789";
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.setBasicAuthentication(username, password); 

 
# Uploading Files
 
To upload files, include them in the body parameters
       
        File file = new File("file path");
        Parameters parameters = new Parameters();
        try {
            parameters.put("key1", file);
        } catch (IOException e) {
            
        }
        parameters.put("key2", "key2");
    
        
# Redirects

By default AsyncConnection does not follow redirects to different protocols such as HTTP to HTTPS or HTTPS to HTTP.
To enable protocol shift redirects:

    AsyncConnection asyncConnection = new AsyncConnection();
    asyncConnection.setFollowProtocolShiftRedirects(true);
 
# NOTE
 
To consume the byte array response as a String:
     
        String responseBody = new String(response); //OR
        String responseBody = new String(response, "UTF-8");
 
 
<a href='https://bintray.com/eukaprotech/maven/networking?source=watch' alt='Get automatic notifications about new "networking" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>

[ ![Download](https://api.bintray.com/packages/eukaprotech/maven/networking/images/download.svg) ](https://bintray.com/eukaprotech/maven/networking/_latestVersion)
