# Downloading Files in Web Screenlet

As the 
[downloading files tutorial](liferay.com) 
explains, you can download portal files in your mobile apps. To download files 
in 
[Web Screenlet](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app), 
however, you must take a few extra steps. This is due to the unique nature of 
Web Screenlet. 

Web Screenlet lets you display any webpage, which you can customize with 
JavaScript and CSS if you wish. If a page contains an explicit file download 
link, or a link that you can use to download a page element (e.g., a rendered 
image), then your app must detect when that link is clicked and handle the file 
download. This tutorial shows you how to implement this in your app. 

## Handling Explicit Download Links

For an explicit download link (e.g., a link whose only purpose is downloading a 
file), you must configure a download listener. You can do this by leveraging 
Web Screenlet's 
[`WebView`](https://developer.android.com/reference/android/webkit/WebView), 
which contains a 
[`DownloadListener`](https://developer.android.com/reference/android/webkit/DownloadListener) 
that you can use to detect and react to clicks on a download link. 

To do this, first call the Web Screenlet instance's `getWebView()` method to get 
the Screenlet's `WebView`. Then call the `WebView` method `setDownloadListener`, 
implementing a new `DownloadListener` as an anonymous class. Implement the 
listener's 
[`onDownloadStart`](https://developer.android.com/reference/android/webkit/DownloadListener.html#onDownloadStart(java.lang.String,%20java.lang.String,%20java.lang.String,%20java.lang.String,%20long)) 
method to handle the file download as explained in the 
[downloading files tutorial](liferay.com): 

    webScreenlet.getWebView().setDownloadListener(new DownloadListener() {
            @Override
            public void onDownloadStart(String url, String userAgent, 
                String contentDisposition, String mimetype, long contentLength) {

                // handle file download
            }
        })

Note that the portal page may contain its own custom listeners on the link. If 
you get unexpected behavior, you may need to remove those listeners in Web 
Screenlet. To do so, add a JavaScript file to your app that contains this code: 

    /*replace this with your own logic to look for wanted a elements*/
    var aElements = document.getElementsByTagName('a');

    for (var i = 0; i < aElementsDocument.length; i++) {
        /*Remove all listener attached to the element*/
        var clone = element.cloneNode(true);
        element.parentNode.replaceChild(clone, element);
    }

For instructions on adding and configuring a JavaScript file to Web Screenlet, 
see the 
[Web Screenlet tutorial](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app). 

## Downloading Other Page Elements

There may also be links for certain page elements that you'd like your users to 
be able to download, even if those links aren't explicit download links. For 
example, downloading a rendered image on a page is common. To support download 
for such links, you must add custom JavaScript that detects and reacts to a link 
click. The 
[Web Screenlet tutorial](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app) 
covers how to add and configure a JavaScript file for use with Web Screenlet. 
The following steps use that tutorial to create a custom JavaScript file that 
initiates a file download when the user clicks the file's link: 

1.  Create the JavaScript file with the code that you want to execute when the 
    link is clicked. Save this file in your Android project. For example, the 
    following file is named `file-click.js` and contains code that gets only the 
    elements whose `href` contains `/documents`: 

        var aElements = document.getElementsByTagName('a');

        var aElementsArray = Array.prototype.slice.call(aElements);

        var aElementsDocument = aElementsArray.filter(x => x.href.indexOf('documents/') !== -1);

        /*Adding a click event to the elements*/
        for (var i = 0; i < aElementsDocument.length; i++) {
            var element = aElementsDocument[i];

            addEvent(element)
        }

        function addEvent(element) {
            element.addEventListener('click', function(event) {
                /*Removing default click behavior*/
                event.preventDefault();
                event.stopPropagation();

                /*Sending the url to the native part*/
                window.Screens.postMessage('download', element.href);
            });
        }

2.  Implement Web Screenlet's listener, `WebListener`, 
    [as instructed in the Web Screenlet tutorial](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app#implementing-web-screenlets-listener). 
    Note that you must implement `onScriptMessageHandler` to handle the file 
    download: 

        public void onScriptMessageHandler(String namespace, String body) {
            if ("download".equals(namespace)) {
                // handle file download
            }
        }

    For instructions on handling the file download, see the 
    [downloading files tutorial](liferay.com). 

3.  Add the file you created in the first step to the Web Screenlet 
    configuration. Do this by 
    [setting Web Screenlet's parameters](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app#setting-web-screenlets-parameters).

Note that the portal page may contain its own custom listeners on the link. If 
you get unexpected behavior, you may need to remove those listeners in Web 
Screenlet. To do this, add the following code to the JavaScript file you created 
in the first step: 
<!-- 
Where exactly should this code be added?
-->

    var clone = element.cloneNode(true);
    element.parentNode.replaceChild(clone, element);

    /*assign here the listener as before*/

## Getting the Cookie from Web Screenlet

To authenticate the request, you can get the cookie from Web Screenlet. Note, 
however, that this cookie will eventually expire and you can't use Web Screenlet 
to get authentication credentials to refresh it. You therefore shouldn't store 
the cookie for later use---only use it to authenticate requests initiated by 
link clicks in Web Screenlet, as detailed above. 

Follow these steps to get and use the cookie from Web Screenlet: 

1.  After 
    [setting Web Screenlet's configuration](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app#setting-web-screenlets-parameters), 
    get the cookie via Android's 
    [`CookieManager`](https://developer.android.com/reference/android/webkit/CookieManager): 

        String cookieValue = CookieManager.getInstance().getCookie("url-of-the-webscreenlet");

    Be sure to use your Web Screenlet's URL as the argument to `getCookie`. 
    <!-- How will developers know what this URL is? -->

2.  Add the cookie to the request object as the `Cookie` header's value: 

        testRequest.addHeader('Cookie', cookieValue);

To make a request to the JSONWS services, you'll use a slightly different 
workflow. Specifically, you must get the `authToken` and then use it (and the 
cookie) to create the session via the Liferay Mobile SDK class 
`CookieAuthentication`. Follow these steps to do so: 

1.  Get the `authToken` via JavaScript: 

        // Script that notify native with the value of authToken
        JsScript script = new JsScript("grabAuthToken",
            "window.Screens.postMessage('authToken', Liferay.authToken)");

        webScreenlet.injectScript(script);

    For instructions on using JavaScript with Web Screenlet, see the 
    [Web Screenlet tutorial](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app). 

2.  Web Screenlet's listener method `onScriptMessageHandler` receives the 
    `authToken` in its `body` argument. Implement this method to create a 
    `CookieAuthentication` from the `authToken` and cookie. Use the 
    `CookieAuthentication` to create the session, then use the session to call 
    the JSONWS services you need: 

        public void onScriptMessageHandler(String namespace, String body) {
            if ("grabAuthToken".equals(namespace)) {

                String authToken = body;
                String cookieValue = CookieManager.getInstance().getCookie(url);

                // Do not save this authentication
                CookieAuthentication auth = new CookieAuthentication(authToken, 
                    cookieValue, "", "", false, 0, 0);

                Session session = new SessionImpl("your-server-url", auth)

                // Use the session to call JSONWS services
            }
        }
