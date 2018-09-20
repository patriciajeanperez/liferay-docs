# Downloading Files in Android

Downloading files from a server is a common use case in mobile apps. This is 
especially true of apps that connect to @product@, which contains extensive file 
management features via its 
[Documents and Media library](/discover/portal/-/knowledge_base/7-0/managing-documents-and-media). 

To download a file from the Documents and Media library, you must have a valid 
portal session. You can't, however, use Basic authentication for this session. 
You must cookie authentication instead. This tutorial shows you how to use this 
authentication type to get files from the Documents and Media library. 

## Using Cookie Authentication

To create the session with cookie authentication, you must configure Login 
Screenlet to use cookie authentication. To do this, set the Screenlet's 
`loginMode` attribute to `cookie`:

    <com.liferay.mobile.screens.auth.login.LoginScreenlet
            android:id="@+id/login_screenlet"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:loginMode="cookie"
            />

Upon login, you can retrieve the resulting session and use it to download files 
from the Documents and Media library. 

## Downloading Files

After configuring Login Screenlet to use cookie authentication, you can retrieve 
the session and use it to download files from the portal. Follow these steps to 
do so: 

1.  Get the session with the `SessionContext` method 
    `createSessionFromCurrentSession()`: 

        Session session = SessionContext.createSessionFromCurrentSession();

2.  Get the authentication headers with the `LiferayServerContext` method 
    `getAuthHeaders()`: 

        Map<String, String> headers = LiferayServerContext.getAuthHeaders();

3.  Use the session and the headers to retrieve a file from the portal. Note 
    that how you do this depends on the file you're retrieving and your 
    server. 

    To get a file from the Documents and Media Library, you must use that file's 
    URL. To get the URL, click the file in the Documents and Media Library and 
    then click the file's *Info* button 
    (![Info](../../../images/icon-information.png)) 
    to open the info panel. The *URL* link in the info panel contains the file's 
    URL. 

    Here's an example that uses the third-party Android library 
    [OkHttp](https://square.github.io/okhttp/) 
    to get a file from the Documents and Media Library. Note that as the first 
    two steps describe, `LiferayServerContext.getAuthHeaders()` gets the headers 
    and `SessionContext.createSessionFromCurrentSession()` gets the session. 
    This example also calls the session's `getAuthentication()` method to get an 
    `Authentication` object, which is then used to authenticate the request 
    before making the server call: 

        OkHttpClient client = new OkHttpClient();

        String url = "http://localhost:8080/documents/20143/72155/somefile.jpg";

        Request request = new Request.Builder()
            .url(url)
            .headers(Headers.of(LiferayServerContext.getAuthHeaders()))
            .get()
            .build();

        Session session = SessionContext.createSessionFromCurrentSession();
        Authentication auth = session.getAuthentication();
        request.authenticate(auth);

        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Request request, IOException e) {

            }

            @Override
            public void onResponse(Response response) throws IOException {
                // Response will contain the file.
            }
        });

## Related Topics

[Accessing the Liferay Session in Android](/develop/tutorials/-/knowledge_base/7-0/accessing-the-liferay-session-in-android)
