# Item Selector [](id=item-selector)

An *Item Selector* is a UI component for selecting entities in a user-friendly
manner. Many Liferay apps use Item Selectors to let users select items such as
images, videos, audio files, documents, and pages. For example, the Documents
and Media Library lets users select files. 

![Figure 1: Item Selectors let users browse and select different kinds of entities.](../../../images/item-selector-dialog-02.png)

The Item Selector API provides a framework for you to use, extend, and create 
Item Selectors in your own apps. The tutorials in this section show you how to 
use this framework. 

Here are some use cases for the Item Selector API: 

1.  Letting your app's users select entities via an Item Selector. 

2.  Configuring an Item Selector to select your app's custom entity. 

3.  Customizing the selection experience by adding a new *selection view* for an 
    entity. 

## Understanding the Item Selector API's Components [](id=understanding-the-item-selector-apis-components)

Before working with the Item Selector API, you should learn about its 
components. You'll work with these components as you leverage the API in your 
apps: 

**Selection Views:** These are the framework's key components. They show 
entities of particular types from different sources. For example, an Item 
Selector configured to show images might show selection views from Documents and 
Media, a third-party image provider, or a drag-and-drop UI. 

**Markup:** A markup file that renders the selection view. You have a great deal 
of flexibility in the markup language you choose. For example, you can use a 
JSP, FreeMarker, or even pure HTML and JavaScript. 

**Return Type:** A class that represents the type of information returned from 
the entities selected by the users. For example, if users select images and you 
want to return the selected image's URL, then you need a URL return type class. 
Each return type class must implement the 
[`ItemSelectorReturnType` interface](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorReturnType.html). 
Such classes are named after the data they return and suffixed with 
`ItemSelectorReturnType`. For example, the URL return type class is 
`URLItemSelectorReturnType`. 

**Criterion:** A class that represents the entity selected by the users. For 
example, if users select images then you need an image criterion class. Each 
criterion class must implement the 
[`ItemSelectorCriterion` interface](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorCriterion.html). 
Criterion classes are named for the entity they represent and suffixed with 
`ItemSelectorCriterion`. For example, the criterion class for images is 
`ImageItemSelectorCriterion`. 

**Criterion Handler:** A class that gets the appropriate selection view. Each 
criterion requires a criterion handler. Criterion handler classes extend the 
[`BaseItemSelectorCriterionHandler` class](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/BaseItemSelectorCriterionHandler.html) 
with the criterion's entity as a type argument. Criterion handler classes are 
named after the criterion's entity and suffixed by 
`ItemSelectorCriterionHandler`. For example, the image criterion handler class 
is `ImageItemSelectorCriterionHandler`. It extends 
`BaseItemSelectorCriterionHandler<ImageItemSelectorCriterion>`. 

This diagram shows how these components interact to form a working API. 

![Figure 1: Item Selector views (selection views) are determined by the return type and criterion, and rendered by the markup.](../../../images/item-selector-architecture.png)
