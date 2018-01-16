# v1.0.0

# Description
An android asynchronous http client based on HttpURLConnection.

# Getting Started
Add the dependency in build.gradle (App module)

```compile 'com.eukaprotech.networking:networking:1.0.0@aar'```

Add permission in manifest file

```<uses-permission android:name="android.permission.INTERNET" />```

# Usage Example

GET request

        Method 1.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.get("url", new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
        
        Method 2.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        asyncConnection.get("url", parameters, new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
         
        Method 3.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        HashMap<String, String> headers = new HashMap<>();
        headers.put("Authorization", "basicAuth value");
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        asyncConnection.get("url", headers, parameters, new AsyncConnectionHandler() {
            @Override
            public void onStart() {
                
            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
        
POST request

        Method 1.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        asyncConnection.post("url", new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
        
        Method 2.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        asyncConnection.post("url", parameters, new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
        
        Method 3.
        
        AsyncConnection asyncConnection = new AsyncConnection();
        HashMap<String, String> headers = new HashMap<>();
        headers.put("Authorization", "basicAuth value");
        Parameters parameters = new Parameters();
        parameters.put("key1", "value1");
        parameters.put("key2", "value2");
        asyncConnection.post("url", headers, parameters, new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });

        Method 4.
        AsyncConnection asyncConnection = new AsyncConnection();
        HashMap<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json");
        JSONObject jsonObject = new JSONObject();
        try{
            jsonObject.put("key1", "value1");
            jsonObject.put("key2", "value2");
        }catch (Exception ex){}
        asyncConnection.post("url", headers, jsonObject, new AsyncConnectionHandler() {
            @Override
            public void onStart() {

            }

            @Override
            public void onSucceed(int responseCode, String response) {

            }

            @Override
            public void onFail(int responseCode, String response, String error) {

            }

            @Override
            public void onComplete() {

            }
        });
        
To upload files, include them among the parameters
       
        File file = new File("file path");
        Parameters parameters = new Parameters();
        try {
            parameters.put("key1", file);
        } catch (IOException e) {
            
        }
        
<a href='https://bintray.com/eukaprotech/maven/networking?source=watch' alt='Get automatic notifications about new "networking" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>
        
