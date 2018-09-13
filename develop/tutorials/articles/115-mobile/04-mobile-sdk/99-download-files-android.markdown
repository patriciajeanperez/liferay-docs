# Downloading Files with the Liferay Android SDK

Downloading files from a server is a common use case in mobile apps. This is 
especially true of apps that connect to @product@, which contains extensive file 
management features via its 
[Documents and Media library](/discover/portal/-/knowledge_base/7-0/managing-documents-and-media). 

To download a file from the Documents and Media library, you must have a valid 
portal session. You can't, however, use Basic authentication for this session. 
You must cookie authentication instead. This tutorial shows you how to use this 
authentication type to get files from the Documents and Media library. 

## Using Cookie Authentication

Follow these steps to create the session with cookie authentication in the 
Mobile SDK: 

1.  Create a dummy `CookieAuthentication` that contains the credentials to 
    authenticate with: 
    <!-- Why a "dummy" object? What do the other parameters mean? -->

        CookieAuthentication authentication =
            new CookieAuthentication("", "", login, password, shouldHandleExpiration, cookieExpirationTime, 0);

2.  Invoke the `CookieSignIn.signIn` method, implementing the 
    `CookieSignIn.CookieCallback()` class: 
    <!-- Where does the session come from? How do you create it? -->

        CookieSignIn.signIn(session, new CookieSignIn.CookieCallback() {
                @Override
                public void onSuccess(Session session) {
                    // session.getAuthentication() contains a valid CookieAuthentication
                }

                @Override
                public void onFailure(Exception e) {

                }
        });

## Downloading Files

Follow these steps to download a file: 

1.  Get the authentication headers: 

    -   Get an `Authentication` object from the session. 
    -   Create a `Request` object.
    -   Authenticate the request by calling its `authenticate` method with the 
        `Authentication` object. 
    -   Call the request's `getHeaders()` method to get the authentication 
        headers. 

        Authentication auth = yourSession.getAuthentication();
        com.liferay.mobile.android.http.Request dummyRequest =
                new com.liferay.mobile.android.http.Request(null, null, null, null, 0);

        dummyRequest.authenticate(auth);

        Map<String, String> headers = dummyRequest.getHeaders();

2.  Use the session and the headers to retrieve a file from the portal. Note 
    that how you do this depends on the file you're retrieving and your 
    server---there's no average or typical use case. 
