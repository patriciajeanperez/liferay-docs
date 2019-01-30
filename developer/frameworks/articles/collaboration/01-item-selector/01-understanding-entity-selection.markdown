# Understanding Entity Selection

An Item Selector lets users select entities such as images, videos, documents, 
and sites. You can use an Item Selector in your app to let users select such 
entities. Do so via these steps: 

1.  **Determine the Criteria for an Item Selector:** Define which entities the 
    Item Selector lets users select. 

2.  **Get an Item Selector for Your Criteria:** If your criteria is for images, 
    for example, then you must get an Item Selector capable of selecting images. 

3.  **Use an Item Selector Dialog:** Display the Item Selector in your UI. 

## Determining Item Criteria [](id=determining-item-selector-criteria)

First, you must determine which entity types (e.g., users, files, etc.) users 
can select with the Item Selector, and the data you expect to receive from those 
selections (e.g., URL, `FileEntry`, etc.). These are represented by the 
following: 

**Criterion Classes:** Represents the entity type in the Item Selector. 
Criterion classes must implement 
[`ItemSelectorCriterion`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorCriterion.html). 

**Return Types:** Represents the type of information you expect from the 
entities when users select them. Each return type must be represented by an 
[`ItemSelectorReturnType`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorReturnType.html) 
implementation. Each criterion must be associated with at least one return type. 

The 
[Item Selector Criterion and Return Types](/develop/reference/-/knowledge_base/7-1/item-selector-criterion-and-return-types)
reference lists the criterion classes and return types that @product@ provides. 
For example, if you want to let users select an image, and you want that 
selection to return the image's URL, you could use the 
`ImageItemSelectorCriterion` criterion class and the `URLItemSelectorReturnType` 
return type.

The criterion and return types are collectively referred to as the Item
Selector's *criteria*. The Item Selector uses it to decide which selection views
(tabs of items) to show. 

If no criterion or return type exists for your use case, you can create your 
own. 

### Creating Your Own Criteria

If there's no criteria for your entity and return type, you can create it via 
these steps. For instructions on these steps, see 
[Creating Custom Item Selector Entities](liferay.com). 

1.  Create your criterion class by extending 
    [`BaseItemSelectorCriterion`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/BaseItemSelectorCriterion.html).
    This class specifies what kind of entity the user is selecting and what 
    information the Item Selector should return. The methods inherited from 
    `BaseItemSelectorCriterion` provide the logic for obtaining this 
    information. Note that you can use your criterion class to pass information 
    to the view if needed.

2.  Create an OSGi component class that implements 
    [`BaseItemSelectorCriterionHandler`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/BaseItemSelectorCriterionHandler.html). 
    Each criterion requires a criterion handler, which is responsible for 
    obtaining the proper selection view. 

3.  If your entity returns information that is already defined by an existing 
    return type, you can use that return type. However, you can create your own 
    return type if there's not an existing one that meets your needs. To create 
    a return type, you must create a class that implements 
    [`ItemSelectorReturnType`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorReturnType.html). 
    You should name such classes after their entity, and suffix them with 
    `ItemSelectorReturnType`. 

    For example, if you were to create a return type for a task item, its return 
    type class would be `TaskItemSelectorReturnType`. Such a 
    `*ItemSelectorReturnType` class is used as an identifier by the Item 
    Selector and does not return any information itself. The return type class 
    is an API that connects the return type to the Item Selector views. Whenever 
    the return type is used, the view must ensure that the proper information is 
    returned. It's recommended that you specify the information that the return 
    type returns, as well as the format, as Javadoc. 

## Getting an Item Selector for the Criteria [](id=getting-an-item-selector-for-the-criteria)

To use an Item Selector with your criteria, you must get that Item Selector's 
URL. The URL is needed to open the Item Selector dialog in your UI. To get this 
URL, you must get an `ItemSelector` reference and call its 
[`getItemSelectorURL` method](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelector.html#getItemSelectorURL-com.liferay.portal.kernel.portlet.RequestBackedPortletURLFactory-java.lang.String-com.liferay.item.selector.ItemSelectorCriterion...-) 
with the following parameters: 

`RequestBackedPortletURLFactory`: A factory that creates portlet URLs. 

`ItemSelectedEventName`: A unique, arbitrary JavaScript event name that the Item 
Selector triggers when the element is selected. 

`ItemSelectorCriterion`: The criterion (or an array of criterion objects) that 
specifies the type of elements to make available in the Item Selector. 

There are a few things to keep in mind when getting an Item Selector's URL: 

-   You can invoke the URL object's `toString` method to get its value.

-   You can configure an Item Selector to use any number of criterion. The 
    criterion can use any number of return types. 

-   The order of the Item Selector's criteria determines the selection view 
    order. For example, if you pass the Item Selector an 
    `ImageItemSelectorCriterion` followed by a `VideoItemSelectorCriterion`, the 
    Item Selector displays the image selection views first. 

-   The return type order is also significant. A view uses the first return type 
    it supports from each criterion's return type list. 

## Using the Item Selector Dialog [](id=using-the-item-selector-dialog)

To open the Item Selector in your UI, you must use the JavaScript component 
`LiferayItemSelectorDialog` from 
[AlloyUI's](http://alloyui.com/) 
`liferay-item-selector-dialog` module. The component listens for the item 
selected event that you specified for the Item Selector URL. The event returns 
the selected element's information according to its return type. 
