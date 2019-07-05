# Version 1.0.4

# Description
An android asynchronous http client based on HttpURLConnection.

# Updates

* A File download handler to directly write a connection response to a file.
* Track the upload and download progress of a single connection.
* Track the upload and download progress of mutiple connections.
* You can now provide an array of Files to be uploaded.
* You can provide your own byte-array to be uploaded.
* Provide your own InPutStream for upload.
* Provide a custom file representation object for upload; with your own byte-array or InputStream.

# Getting Started
Add the dependency in build.gradle (App module)

```compile 'com.eukaprotech.networking:networking:1.0.4@aar'```

Add permission in manifest file

```<uses-permission android:name="android.permission.INTERNET" />```

# Request Methods Covered

* GET
* POST
* PUT
* DELETE
* HEAD
* OPTIONS


GET request:
        
```java
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
```
        
Sample GET request:
        
```java
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
```

# File Download Handler

A File download handler, FileAsyncConnectionHandler, is uded to directly write a connection response to a file:

```java
asyncConnection.get("url", new FileAsyncConnectionHandler() {
    @Override
    public void onStart() {
        
    }

    @Override
    public String onGetFileAbsolutePath(String contentType) {
        //you can determine the file-extension to use based on contentType
        File file = new File("path");
        return file.getAbsolutePath();
    }

    @Override
    public void onSucceed(int responseCode, HashMap<String, String> headers, File response) {
        //the response is the file whose path you provided. You can use the file at this point
    }

    @Override
    public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
        
    }

    @Override
    public void onComplete() {
        
    }
});
```
  
# Query Parameters

To attach query parameters to a url:

```java
Parameters query_parameters = new Parameters();
query_parameters.put("key1", "value1");
query_parameters.put("key2", "value2");

String url = URLBuilder.build("initial_url", query_parameters);
//if the initial_url already contains query parameters, the new query parameters are just added at the end of existing ones.
//in case of conflicting keys between the existing query parameters and the new ones; the new ones are given priority.
//you can then use the resulting url in a request
```
        
Sample:
   
```java
Parameters query_parameters = new Parameters();
query_parameters.put("q", "discover");
String url = URLBuilder.build("https://www.google.com/search", query_parameters);
//will result into https://www.google.com/search?q=discover
//you can then use the resulting url in a request

AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get(url, new AsyncConnectionHandler() {
    // the implemented listener functions onStart, onSucceed, onFail & onComplete
});
```       
        
# Body Parameters (Used for POST & PUT)

To attach parameters as the body/content of the request
     
```java
Parameters parameters = new Parameters();
parameters.put("key1", "value1");
parameters.put("key2", "value2");

//For a POST request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.post("url", parameters, new AsyncConnectionHandler() {  
    // the implemented listener functions here 
});

//For a PUT request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.put("url", parameters, new AsyncConnectionHandler() {  
    // the implemented listener functions here 
});
```
             
# Request Headers

To attach request headers:
     
```java
HashMap<String, String> headers = new HashMap<>();
headers.put("Authorization", "basicAuth value");
Parameters parameters = new Parameters();
parameters.put("key1", "value1");
parameters.put("key2", "value2");

//For a POST request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.post("url", headers, parameters, new AsyncConnectionHandler() { 
    // the implemented listener functions here here 
});

//For a GET request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get("url", headers, new AsyncConnectionHandler() { 
    // the implemented listener functions here here here  
});
//Other request methods have similar way of attaching request headers
```
        
# Body JSONObject (Used for POST & PUT)

JSONObject can be used in place of Parameters as the body/content of the request:

```java
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
    // the implemented listener functions here here here here 
});

//For a PUT request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.put("url", headers, jsonObject, new AsyncConnectionHandler() {  
    // the implemented listener functions here here here here
});
```
        
# Basic Authentication

To attach a basic auth in the request headers:

 Method 1: (Handle it)
 
```java
HashMap<String, String> headers = new HashMap<>();
String username = "example@gmail.com"; 
String password = "123456789";
String auth_value = username+":"+password;
String basicAuth = "basic "+ Base64.encodeToString(auth_value.getBytes(), Base64.NO_WRAP);
headers.put("Authorization", basicAuth);
```
        
 Method 2: (Let AsyncConnection handle it)
 
```java
String username = "example@gmail.com"; 
String password = "123456789";
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.setBasicAuthentication(username, password); 
```

# Track the Upload and Download Progress of Single Connection

Both handlers, AsyncConnectionHandler and FileAsyncConnectionHandler have functions to listen to the upload and download progress. These functions are onUploadProgressUpdate and onDownloadProgressUpdate. The functions are optional; you can choose to implement them or not.

Tracking upload and download progress with AsyncConnectionHandler:

```java
asyncConnection.post("url", parameters, new AsyncConnectionHandler() {// you can use any http method other than POST
    @Override
    public void onStart() {
        
    }
    
    @Override
    public void onUploadProgressUpdate(long progress, long length) {
        //length is the total size of the content/parameters being uploaded or written to the connection.
        //You can use a determinate loader at this point, to show show the progress
    }

    @Override
    public void onDownloadProgressUpdate(long progress, long length) {
        //length is the total size of the content being downloaded or read from the connection.
        //NOTE: length depends on the server returning a content-length header. 
        //If the server returns chunked transfer-encoding header, content-length header is not available, the length is unknown 
        //When the length is not known, the value of length will be -1.
        //If length is -1, avoid displaying a determinate loader.
    }
    
    @Override
    public void onSucceed(int responseCode, HashMap<String, String> headers, byte[] response) {
        
    }

    @Override
    public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
        
    }

    @Override
    public void onComplete() {
        
    }
});
```

Tracking upload and download progress with FileAsyncConnectionHandler:

```java
asyncConnection.post("url", parameters, new FileAsyncConnectionHandler() {// you can use any http method other than POST
    @Override
    public void onStart() {
        
    }
    
    @Override
    public void onUploadProgressUpdate(long progress, long length) {
        //length is the total size of the content/parameters being uploaded or written to the connection.
        //You can use a determinate loader at this point, to show show the progress
    }

    @Override
    public void onDownloadProgressUpdate(long progress, long length) {
        //length is the total size of the content being downloaded or read from the connection.
        //NOTE: length depends on the server returning a content-length header. 
        //If the server returns chunked transfer-encoding header, content-length header is not available, the length is unknown 
        //When the length is not known, the value of length will be -1.
        //If length is -1, avoid displaying a determinate loader.
    }

    @Override
    public String onGetFileAbsolutePath(String contentType) {
        
    }

    @Override
    public void onSucceed(int responseCode, HashMap<String, String> headers, File response) {
        
    }

    @Override
    public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
        
    }

    @Override
    public void onComplete() {
        
    }
});
```

# Track the Upload and Download Progress of Multiple Connection

MultipleAsyncConnections multipleAsyncConnections = new MultipleAsyncConnections();
List<AsyncConnection> asyncConnectionList = multipleAsyncConnections.generateConnections(2);
// 2 is the number of connections you desire to monitor
AsyncConnection asyncConnection1 = asyncConnectionList.get(0);
AsyncConnection asyncConnection2 = asyncConnectionList.get(1);
        
asyncConnection1.post("url", parameters, new AsyncConnectionHandler() {// you can use any http method other than POST
    // the implemented listener functions here
});
asyncConnection1.post("url", parameters, new FileAsyncConnectionHandler() {// you can use any http method other than POST
    // the implemented listener functions here
});

//For Connections generated by MultipleAsyncConnections to start processing;
multipleAsyncConnections.startConnections(new MultipleAsyncConnectionsHandler() {
    @Override
    public void onStart() {
        //called once
    }

    @Override
    public void onUploadProgressUpdate(long progress, long length) {
        //progress is the cumulative size uploaded across all the connections being tracked
        //length is the cumulative total size to be uploaded across all the connections being tracked
        //You can use a determinate loader at this point, to show show the progress
    }

    @Override
    public void onDownloadProgressUpdate(long progress, long length) {
        //progress is the cumulative size downloaded across all the connections being tracked
        //length is the cumulative total size to be downloaded across all the connections being tracked
        //NOTE: length depends on the server returning a content-length header to each of the connections being tracked. 
        //If the server returns chunked transfer-encoding header, content-length header is not available, the length is unknown 
        //When the length is not known, the cumulative value of length across the connections will not be useful.
        //Avoid displaying a determinate loader here at all.
    }

    @Override
    public void onComplete() {
        //called once when all connections complete
    }
});
 
# Uploading Files
 
To upload a File, include it in the body parameters
   
```java
File file = new File("file path");
Parameters parameters = new Parameters();
try {
    parameters.put("key1", file);
} catch (IOException e) {

}
parameters.put("key2", "value2");
```
        
# Uploading Array of Files 
 
To upload array of files, include the array in the body parameters
  
```java
File file1 = new File("file path1");
File file2 = new File("file path2");

Parameters parameters = new Parameters();
File[] arrayFiles = new File[]{file1, file2};
try {
    parameters.put("key1", arrayFiles);
} catch (IOException e) {

}
parameters.put("key2", "value2");
```
        
# Uploading Files Whose Content is Your Bytes or InputStream 
 
A class named FileItem is used. Note that parameters of value FileItem are uploaded to the server just like any other file; difference being that the content is read from the bytes or inputstream you provide.

To upload a file with your own bytes as content, include it as a FileItem in the body parameters.
       
```java
byte[] bytes = "Hey there".getBytes();
FileItem fileItem = new FileItem("sample.txt", bytes); // "sample.txt" is the file name
Parameters parameters = new Parameters();
parameters.put("key1", fileItem);
parameters.put("key2", "value2");
```
        
To upload a file with your own InputStream as content, include it as a FileItem in the body parameters
       
```java
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
```
        
To upload array of files with your own bytes or InputStream as content, include them as array of FileItem in the body parameters
    
```java
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
```
        
# Uploading Bytes
 
To upload bytes, include them as BytesItem in the body parameters
     
```java
Parameters parameters = new Parameters();
byte[] bytes = "Hey bytes".getBytes()// your bytes here;
parameters.put("key1", new BytesItem(bytes));
parameters.put("key2", "value2");
```
        
# Provide Your InputStream for Upload
 
To provide your own inputstream, include it as an InputStreamItem in the body parameters. In the example shown below, the 
       
```java
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
```
    
        
# Redirects

By default AsyncConnection does not follow redirects to different protocols such as HTTP to HTTPS or HTTPS to HTTP.
To enable protocol shift redirects:

```java
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.setFollowProtocolShiftRedirects(true);
```
    
# Setting Time Out

```java
AsyncConnection asyncConnection = new AsyncConnection();
int time_out_in_milliseconds = 5000;
asyncConnection.setTimeOut(time_out_in_milliseconds);
```
 
# NOTE
 
To consume the byte array response as a String:
    
```java
String responseBody = new String(response); //OR
String responseBody = new String(response, "UTF-8");
```
        

<a href='https://bintray.com/eukaprotech/maven/networking?source=watch' alt='Get automatic notifications about new "networking" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>
[ ![Download](https://api.bintray.com/packages/eukaprotech/maven/networking/images/download.svg) ](https://bintray.com/eukaprotech/maven/networking/_latestVersion)
