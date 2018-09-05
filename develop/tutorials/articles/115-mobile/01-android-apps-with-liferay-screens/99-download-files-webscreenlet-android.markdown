# Downloading Files in Web Screenlet

As the 
[downloading files tutorial](liferay.com) 
explains, you can download portal files in your mobile apps. To download files 
in 
[Web Screenlet](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app), 
however, you must take a few extra steps. This is due to the unique nature of 
Web Screenlet. 

Web Screenlet lets you display any webpage, which you can customize with 
JavaScript and CSS if you wish. If a page contains a file download link, then 
your app must detect when that link is clicked and then handle the file 
download. This tutorial shows you how to implement this in your app. 

## Configuring a Download Listener

To render webpages, Web Screenlet contains an Android 
[`WebView`](https://developer.android.com/reference/android/webkit/WebView). 
You can therefore use `WebView`'s 
[`DownloadListener`](https://developer.android.com/reference/android/webkit/DownloadListener) 
to detect and react to download link clicks. 

To do this, first call the Web Screenlet instance's `getWebView()` method to get 
the Screenlet's `WebView`. Then call the `WebView` method `setDownloadListener`, 
implementing a new `DownloadListener` as an anonymous class. Implement the 
listener's 
[`onDownloadStart` method](https://developer.android.com/reference/android/webkit/DownloadListener.html#onDownloadStart(java.lang.String,%20java.lang.String,%20java.lang.String,%20java.lang.String,%20long)) 
to handle the file download as explained in the 
[downloading files tutorial](liferay.com): 


    webScreenlet.getWebView().setDownloadListener(new DownloadListener() {
            @Override
            public void onDownloadStart(String url, String userAgent, 
                String contentDisposition, String mimetype, long contentLength) {

                // handle file download
            }
        })

Note that if you experience other issues, you may need to add a new JavaScript 
file with this code: 

    /*replace this with your own logic to look for wanted a elements*/
    var aElements = document.getElementsByTagName('a');

    for (var i = 0; i < aElementsDocument.length; i++) {
        /*Remove all listener attached to the element*/
        var clone = element.cloneNode(true);
        element.parentNode.replaceChild(clone, element);
    }
<!-- 
What kind of issues does this code resolve?
What does this code do?
How/where should this JavaScript file be added and configured?
-->

## Reacting to a Link Click
<!--
Is this another way to do the same thing as the previous section?
--> 

Since Web Screenlet can add JavaScript to the page it renders, you can write 
custom JavaScript that detects and reacts to a link click on that page (e.g., a 
download link). The process for adding and configuring a JavaScript file for use 
with Web Screenlet is covered in the 
[Web Screenlet tutorial](/develop/tutorials/-/knowledge_base/7-0/rendering-web-pages-in-your-android-app). 
The following steps use that tutorial to create a custom JavaScript file that 
initiates a file download when the file's download link is clicked: 

1.  Create the JavaScript file with the code that you want to execute when the 
    link is clicked. Save the file in your Android project. For example, this 
    file is named `file-click.js` and contains code that gets only the elements 
    whose `href` contains `/documents`: 

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

Now you have to add this file to the WebScreenlet configuration, as explained [here](https://dev.liferay.com/develop/reference/-/knowledge_base/7-0/web-screenlet-for-android#configuration).

Once you are done doing this, you have to implement the WebScreenletDelegate, specifically the method: 

```
public void onScriptMessageHandler(String namespace, String body) {
	if ("download".equals(namespace)) {
		// handle file download
	}
}
```

The last thing you have to do is handling the file download, for this, you will have to follow this tutorial >>link to the downloading files tutorial>>

**Note**
If after doing this, the default behavior is not removed or you are experiencing other issues, just remove all the js applied to that html element adding this lines to the script:

```
var clone = element.cloneNode(true);
element.parentNode.replaceChild(clone, element);

/*assign here the listener as before*/
```



