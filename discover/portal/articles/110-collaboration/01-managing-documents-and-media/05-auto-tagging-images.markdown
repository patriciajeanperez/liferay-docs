# Auto-tagging Images [](id=auto-tagging-images)

With 
[asset auto-tagging enabled](/discover/portal/-/knowledge_base/7-2/auto-tagging-assets), 
@product@ automatically 
[tags](/discover/portal/-/knowledge_base/7-2/tagging-content) 
images uploaded to the Documents and Media Library. This lets you use tags to 
find and organize images without requiring users to manually tag them. Image 
auto-tagging in @product@ is based on 
[TensorFlow's `LabelImage` sample for Java](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/java/src/main/java/org/tensorflow/examples/LabelImage.java), 
and uses the Inception5h model. 

This article shows you how to configure auto-tagging for images. As you'll see, 
you can control which instances image auto-tagging is enabled for, and how the 
tags are applied. 

![Figure 1: The train in this image was auto-tagged with the tags *freight car* and *electric locomotive*.](../../../images/auto-tagging-images.png)

## Configuring Image Auto-tagging [](id=configuring-image-auto-tagging)

By default, image auto-tagging is enabled in any instance in which asset 
auto-tagging is enabled. For instructions on enabling asset auto-tagging, see 
[the article on auto-tagging assets](/discover/portal/-/knowledge_base/7-2/auto-tagging-assets). 
You can, however, change this default behavior. You can also configure how image 
auto-tagging functions. To do so, follow these steps: 

1.  Go to *Control Panel* &rarr; *Configuration* &rarr; *System Settings*, then 
    select *Documents and Media*. 

2.  Under *VIRTUAL INSTANCE SCOPE*, select *TensorFlow Image Auto Tagging*. The 
    following settings are available: 

    -   **Enabled:** Whether image auto-tagging is enabled by default on any 
        instance that has auto-tagging enabled. Note that you can override this 
        value for specific instances, as shown in the next section. 

    -   **Confidence Threshold:** TensorFlow assigns a confidence level between 
        0 and 1 for each tag. This value sets the minimum confidence needed for 
        TensorFlow to apply a tag. Note that you can override this value for 
        specific instances, as shown in the next section. 
        <!-- I'm assuming 1 is high confidence and 0 is low confidence? -->

3.  Click *Save* to save your changes. 

![Figure 2: Configure image auto-tagging for the instances in your portal.](../../../images/auto-tagging-image-settings.png)

### Overriding Image Auto-tagging Settings for Specific Instances [](id=overriding-image-auto-tagging-settings-for-specific-instances)

You can also override the above image auto-tagging settings for specific 
instances. Follow these steps to do so: 

1.  Go to *Control Panel* &rarr; *Configuration* &rarr; *Instance Settings*, 
    then select the *Asset Auto Tagging* tab. 

2.  Expand the section *TensorFlow Image Auto Tagging* and configure the 
    settings as desired. 

3.  Click *Save* to save your changes. 

![Figure 3: Configure image auto-tagging for a specific instance.](../../../images/auto-tagging-image-instance.png)

## Additional Auto-tagging Providers [](id=additional-auto-tagging-providers)

TensorFlow is the default image auto-tagging provider in Liferay CE Portal and 
Liferay DXP. Liferay DXP, however, contains two additional image auto-tagging 
providers. You can configure these providers in the same places that you 
configure the TensorFlow provider, as the previous sections describe: 

-   **Google Cloud Vision:** This provider uses the 
    [Google Cloud Vision API](https://cloud.google.com/vision/) 
    to automatically tag images. Note that you must provide a valid API key. For 
    more information, see 
    [Google's documentation on API keys](https://cloud.google.com/docs/authentication/api-keys). 

    ![Figure 4: The Google Cloud Vision provider requires an API key.](../../../images/auto-tagging-image-google.png)

-   **Microsoft Cognitive Services:** This provider uses 
    [Microsoft Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/) 
    to automatically tag images. Note that you must provide a valid 
    [API key](https://azure.microsoft.com/en-us/try/cognitive-services/my-apis/?apiSlug=computer-services) 
    and endpoint. For more information, see the
    [Microsoft Cognitive Services documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/). 

    ![Figure 5: The Microsoft Cognitive Services provider requires an API key and endpoint.](../../../images/auto-tagging-image-microsoft.png)
