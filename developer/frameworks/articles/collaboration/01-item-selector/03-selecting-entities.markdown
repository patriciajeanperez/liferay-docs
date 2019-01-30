# Selecting Entities with an Item Selector

The steps here show you how to use an Item Selector to let users select entities 
in your app. For an explanation of these steps, see 
[Understanding Entity Selection](liferay.com). 

## Get an Item Selector for Your Criteria

First, you must determine your item criteria and get an Item Selector for it. 
Follow these steps to do so:

1.  [Determine your item criteria](liferay.com). 

2.  Use Declarative Services to get an `ItemSelector` OSGi Service Component: 

        import com.liferay.item.selector.ItemSelector;
        import org.osgi.service.component.annotations.Reference;

        ...

        @Reference
        private ItemSelector _itemSelector

    The component annotations are available in the 
    [`org.osgi.service.component.annotations` module](http://mvnrepository.com/artifact/org.osgi/org.osgi.service.component.annotations). 

3.  Create the factory you'll use to create the URL. To do this, invoke the 
    `RequestBackedPortletURLFactoryUtil.create` method with the current request 
    object. The request can be an `HttpServletRequest` or `PortletRequest`: 

        RequestBackedPortletURLFactory requestBackedPortletURLFactory =
            RequestBackedPortletURLFactoryUtil.create(request);

4.  Create a list of return types expected for the image entity. The return 
    types list consists of a URL return type `URLItemSelectorReturnType`: 

        List<ItemSelectorReturnType> desiredItemSelectorReturnTypes =
            new ArrayList<>();
        desiredItemSelectorReturnTypes.add(new URLItemSelectorReturnType());

5.  Create a criterion object for images (`ImageItemSelectorCriterion`): 

        ImageItemSelectorCriterion imageItemSelectorCriterion =
            new ImageItemSelectorCriterion();

6.  Use the criterion's `setDesiredItemSelectorReturnTypes` method to set the 
    return types list from step 3 to the criterion: 

        imageItemSelectorCriterion.setDesiredItemSelectorReturnTypes(
            desiredItemSelectorReturnTypes);

7.  Call the Item Selector's `getItemSelectorURL` method to get an Item Selector 
    URL based on the criterion. The method requires the URL factory, an 
    arbitrary event name, and a series of criterion (one, in this case): 

        PortletURL itemSelectorURL = _itemSelector.getItemSelectorURL(
            requestBackedPortletURLFactory, "sampleTestSelectItem",
            imageItemSelectorCriterion);

## Using the Item Selector Dialog [](id=using-the-item-selector-dialog)

Now you can use the Item Selector's dialog in a JSP. Follow these steps to do 
so: 

1.  Declare the AUI tag library: 

        <%@ taglib prefix="aui" uri="http://liferay.com/tld/aui" %>

2.  Define the UI element that you'll use to open the Item Selector dialog. For 
    example, this creates a *Choose* button with the ID `chooseImage`:

        <aui:button name="chooseImage" value="Choose" />

3.  Get the Item Selector's URL: 

        <%
        String itemSelectorURL = GetterUtil.getString(request.getAttribute("itemSelectorURL"));
        %>

3.  Add the `<aui:script>` tag and set it to use the 
    `liferay-item-selector-dialog` module: 

        <aui:script use="liferay-item-selector-dialog">

        </aui:script>

4.  Inside the `<aui:script>` tag, attach an event handler to the UI element you 
    created in step 2. For example, this attaches a click event and a function 
    to the *Choose* button: 

        <aui:script use="liferay-item-selector-dialog">

            $('#<portlet:namespace />chooseImage').on(
            'click',
              function(event) {
                <!-- function logic goes here -->
              }
            );

        </aui:script>

    Inside the function, you must create a new instance of the 
    `LiferayItemSelectorDialog` AlloyUI component and configure it to use the 
    Item Selector. The next steps walk you through this. 

5.  Now you must create the function logic. First, create a new instance of the 
    Liferay Item Selector dialog: 

        var itemSelectorDialog = new A.LiferayItemSelectorDialog(  
            {
                ...
            }
        );

6.  Inside the braces of the `LiferayItemSelectorDialog` constructor, first set 
    set the `eventName` attribute. This makes the dialog listen for the item 
    selected event. The event name is the the Item Selector's event name that 
    you specified in your Java code (the code that gets the Item Selector URL): 

        eventName: 'ItemSelectedEventName',

7.  Immediately after the `eventName` setting, set the `on` attribute to 
    implement a function that operates on the selected item change. For example, 
    this function sets its variables for the newly selected item. The 
    information available to parse depends on the return type(s) that were set. 
    As the comment indicates, you must add the logic for using the selected 
    element: 

        on: {
                selectedItemChange: function(event) {
                    var selectedItem = event.newVal;

                    if (selectedItem) {
                        var itemValue = JSON.parse(
                        selectedItem.value
                        );
                        itemSrc = itemValue.url;

                        <!-- use item as needed -->
                    }
                }
        },

8.  Immediately after the `on` setting, set the `title` attribute to the 
    dialog's title: 

        title: '<liferay-ui:message key="select-image" />',

9.  Immediately after the `title` setting, set the `url` attribute to the 
    previously retrieved Item Selector URL. This concludes the attribute 
    settings inside the `LiferayItemSelectorDialog` constructor: 

        url: '<%= itemSelectorURL.toString() %>'

10. To conclude the logic of the function from step 4, open the Item Selector 
    dialog by calling its `open` method: 

        itemSelectorDialog.open();

When the user clicks the *Choose* button, a new dialog opens, rendering the Item
Selector with the views that support the criterion and return type(s) that were
set. 

Here's the complete example code for these steps: 

    <%@ taglib prefix="aui" uri="http://liferay.com/tld/aui" %>

    <aui:button name="chooseImage" value="Choose" />

    <%
    String itemSelectorURL = GetterUtil.getString(request.getAttribute("itemSelectorURL"));
    %>

    <aui:script use="liferay-item-selector-dialog">

        $('#<portlet:namespace />chooseImage').on(
            'click', 
            function(event) {
                var itemSelectorDialog = new A.LiferayItemSelectorDialog(  
                    {
                        eventName: 'ItemSelectedEventName',
                        on: {
                                selectedItemChange: function(event) {
                                    var selectedItem = event.newVal;

                                    if (selectedItem) {
                                        var itemValue = JSON.parse(
                                        selectedItem.value
                                        );
                                        itemSrc = itemValue.url;
    
                                        <!-- use item as needed -->
                                    }
                                }
                        },
                        title: '<liferay-ui:message key="select-image" />',
                        url: '<%= itemSelectorURL.toString() %>'
                    }
                );
                itemSelectorDialog.open();
            }
        );
    </aui:script>
