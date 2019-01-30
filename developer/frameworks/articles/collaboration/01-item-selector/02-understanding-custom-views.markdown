# Understanding Custom Item Selector Views

Item Selector's default selection views may provide everything you need for your 
app. Custom selection views are required, however, for certain situations. For 
example, if you want your users to be able to select images from an external 
image provider, then you must 
[create a custom selection view](liferay.com). 

The view the Item Selector presents is determined by the type of entity the user 
is selecting. The Item Selector can also render multiple views for the same 
entity type. For example, several selection views are available when a user 
selects an image. Each selection view is a tab in the UI that corresponds to the 
image's location. 

![Figure 1: An entity type can have multiple selection views.](../../../images/item-selector-tabs.png)

Each selection view is represented by an `*ItemSelectorCriterion` class. The 
tabs in this figure are represented by the following `*ItemSelectorCriterion`: 

-  [`BlogsItemSelectorCriterion` class](@app-ref@/collaboration/latest/javadocs/com/liferay/blogs/item/selector/criterion/BlogsItemSelectorCriterion.html): 
   Blog Images View
-  [`ImageItemSelectorCriterion` class](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/criteria/image/criterion/ImageItemSelectorCriterion.html): 
   Documents and Media View
-  [`URLItemSelectorCriterion` class](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/criteria/url/criterion/URLItemSelectorCriterion.html): 
   URL View
-  [`UploadItemSelectorCriterion` class](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/criteria/upload/criterion/UploadItemSelectorCriterion.html): 
   Upload Image View

## The Selection View's Class

To create your new selection view's class, you must first know the following two 
things: 

1.  What kind of entities you want the selection view to present (e.g., images, 
    videos, users, etc.). This determines the specific `ItemSelectorCriterion` 
    you must use. For example, a selection view for images must use 
    `ImageItemSelectorCriterion`. 

2.  The entity's return type (the information type you expect from entities when 
    users select them). For example, if a selected entity returns its URL, you 
    would use `URLItemSelectorReturnType` for the return type. 

For a full list of the criterion and returns types available in @product@'s 
apps, see the reference document 
[Item Selector Criterion and Return Types](/develop/reference/-/knowledge_base/7-1/item-selector-criterion-and-return-types). 

A selection view's class is an `ItemSelectorView` component class that 
implements the 
[`ItemSelectorView`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorView.html) 
interface, parameterized with the criterion the view requires. Note the 
following when creating this class: 

-   Configure the title by implementing the 
    [`getTitle`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorView.html#getTitle-java.util.Locale-) 
    method to return the localized title of the tab to display in the Item 
    Selector dialog. 

-   Configure the search options by implementing the 
    [`isShowSearch()`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorView.html#isShowSearch--) 
    method to return whether your view should show the search field. To 
    implement search, you must return `true` for this method. The 
    `renderHTML` method indicates whether a user performed a search based on the 
    value of the `search` parameter. Then the keywords the user searched can be 
    obtained as follows:

        String keywords = ParamUtil.getString(request, "keywords");

-   Make your view visible by implementing the 
    [`isVisible()`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorView.html#isVisible-com.liferay.portal.kernel.theme.ThemeDisplay-) 
    app to return `true`. Note that you can use this method to add conditional 
    logic to disable the view. 
