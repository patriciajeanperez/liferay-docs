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
    `createSessionForCurrentContext()`: 

        SessionContext.createSessionForCurrentContext()

2.  Get the authentication headers with the `LiferayServerContext` method 
    `getAuthHeaders()`: 

        LiferayServerContext.getAuthHeaders()

3.  Use the session and the headers to retrieve a file from the portal. Note 
    that how you do this depends on the file you're retrieving and your 
    server---there's no average or typical use case. 
<!-- 
How do you get files from the portal? Add an example. 
-->
