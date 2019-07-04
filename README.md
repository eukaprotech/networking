# Description
An android asynchronous http client based on HttpURLConnection.

# Versions
* [1.0.0](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.0/README.md "Version 1.0.0 Overview")
* [1.0.1](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.1/README.md "Version 1.0.1 Overview")
* [1.0.2](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.2/README.md "Version 1.0.2 Overview")
* [1.0.3](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.3/README.md "Version 1.0.3 Overview")
* [1.0.4](https://github.com/eukaprotech/networking/blob/master/com/eukaprotech/networking/networking/1.0.4/README.md "Version 1.0.4 Overview")

# Version 1.0.4

# Description
An android asynchronous http client based on HttpURLConnection.

# Updates

* You can now provide your own byte-array for upload.
* Provide your own InPutStream for upload.
* A custom file representation object for upload; with your own byte-array or InputStream.
* Provide an array of Files for upload.
* A File download handler to directly write a connection response to a file.
* A listener to the upload progress as well as download progress.
* A multi-connections manager to monitor the upload and download progress of mutiple connections.

# Getting Started
Add the dependency in build.gradle (App module)

```compile 'com.eukaprotech.networking:networking:1.0.4@aar'```

Add permission in manifest file

```<uses-permission android:name="android.permission.INTERNET" />```

# Request Methods covered

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
        headers.put("Authorization", "basicAuth value");
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        
        //For a POST request
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
 
To upload a File, include it in the body parameters
       
        File file = new File("file path");
        Parameters parameters = new Parameters();
        try {
            parameters.put("key1", file);
        } catch (IOException e) {
            
        }
        parameters.put("key2", "value2");
        
# Uploading Array of Files 
 
To upload array of files, include the array in the body parameters
       
        File file1 = new File("file path1");
        File file2 = new File("file path2");
        
        Parameters parameters = new Parameters();
        File[] arrayFiles = new File[]{file1, file2};
        try {
            parameters.put("key1", arrayFiles);
        } catch (IOException e) {
            
        }
        parameters.put("key2", "value2");
        
# Uploading Files whose content is your own bytes or InputStream 
 
A class named FileItem is used. Note that parameters of value FileItem are uploaded to the server just like any other file; difference being that the content is read from the bytes or inputstream you provide.

To upload a file with your own bytes as content, include it as a FileItem in the body parameters.
       
        byte[] bytes = "Hey there".getBytes();
        FileItem fileItem = new FileItem("sample.txt", bytes); // "sample.txt" is the file name
        Parameters parameters = new Parameters();
        parameters.put("key1", fileItem);
        parameters.put("key2", "value2");
        
To upload a file with your own InputStream as content, include it as a FileItem in the body parameters
       
        InputStream inputStream = null;
        File file = new File("file path");
        try {
            inputStream = new BufferedInputStream(new FileInputStream(file));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        long length = file.length();
        FileItem fileItem = new FileItem("index.html", length, inputStream); //the length is important for progress monitoring
        Parameters parameters = new Parameters();
        parameters.put("key1", fileItem);
        parameters.put("key2", "value2");
        
To upload array of files with your own bytes or InputStream as content, include them as array of FileItem in the body parameters
       
        byte[] bytes = "Hey there".getBytes();
        FileItem fileItem1 = new FileItem("sample.txt", bytes);
        
        InputStream inputStream = null;
        File file = new File("file path");
        try {
            inputStream = new BufferedInputStream(new FileInputStream(file));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        long length = file.length();
        FileItem fileItem2 = new FileItem("index.html", length, inputStream);
        
        FileItem[] arrayFiles = new FileItem[]{fileItem1, fileItem2};
        Parameters parameters = new Parameters();
        parameters.put("key1", arrayFiles);
        parameters.put("key2", "value2");
        
# Uploading Bytes
 
To upload bytes, include them as BytesItem in the body parameters
       
        Parameters parameters = new Parameters();
        byte[] bytes = "Hey bytes".getBytes()// your bytes here;
        parameters.put("key1", new BytesItem(bytes));
        parameters.put("key2", "value2");
        
# Provide Your own InputStream for Upload
 
To provide your own inputstream, include it as an InputStreamItem in the body parameters. In the example shown below, the 
       
        Parameters parameters = new Parameters();
        InputStream inputStream = null;
        File file = new File("file path");
        try {
            inputStream = new BufferedInputStream(new FileInputStream(file));
        } catch (FileNotFoundException e) {
            
        }
        long length = file.length();
        parameters.put("key1", new InputStreamItem(length, inputStream));
        parameters.put("key2", "value2");
    
        
# Redirects

By default AsyncConnection does not follow redirects to different protocols such as HTTP to HTTPS or HTTPS to HTTP.
To enable protocol shift redirects:

    AsyncConnection asyncConnection = new AsyncConnection();
    asyncConnection.setFollowProtocolShiftRedirects(true);
    
# Setting Time Out

    AsyncConnection asyncConnection = new AsyncConnection();
    int time_out_in_milliseconds = 5000;
    asyncConnection.setTimeOut(time_out_in_milliseconds);
 
# NOTE
 
To consume the byte array response as a String:
     
        String responseBody = new String(response); //OR
        String responseBody = new String(response, "UTF-8");
        

<a href='https://bintray.com/eukaprotech/maven/networking?source=watch' alt='Get automatic notifications about new "networking" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>
[ ![Download](https://api.bintray.com/packages/eukaprotech/maven/networking/images/download.svg) ](https://bintray.com/eukaprotech/maven/networking/_latestVersion)
