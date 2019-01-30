# Creating Custom Item Selector Entities [](id=creating-custom-item-selector-entities)

If your app requires users to select an entity type that the Item Selector 
doesn't support, you can add support by creating new *criteria*. The steps here 
show you how to do this. For an explanation of these steps and more detailed 
information on Item Selector criteria, see 
[Determining Item Criteria](liferay.com). 

1.  Create a class that extends 
    [`BaseItemSelectorCriterion`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/BaseItemSelectorCriterion.html), 
    passing information to the view if needed. For example, 
    [`JournalItemSelectorCriterion`](@app-ref@/web-experience/latest/javadocs/com/liferay/journal/item/selector/criterion/JournalItemSelectorCriterion.html) 
    passes information about the primary key so the view can use it: 

        public class JournalItemSelectorCriterion extends BaseItemSelectorCriterion {

                public JournalItemSelectorCriterion() {
                }
        
                public JournalItemSelectorCriterion(long resourcePrimKey) {
                        _resourcePrimKey = resourcePrimKey;
                }
        
                public long getResourcePrimKey() {
                        return _resourcePrimKey;
                }
        
                public void setResourcePrimKey(long resourcePrimKey) {
                        _resourcePrimKey = resourcePrimKey;
                }
        
                private long _resourcePrimKey;
        
        }

    +$$$

    **Note:** Criterion fields should be serializable and should expose a 
    public empty constructor (as shown here). 

    $$$

2.  Create an OSGi component class that implements 
    [`BaseItemSelectorCriterionHandler`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/BaseItemSelectorCriterionHandler.html). 
    This example creates a criterion handler for the `TaskItemSelectorCriterion` 
    class. The `@Activate` and `@Override` tokens are required to activate this 
    OSGi component: 

        @Component(service = ItemSelectorCriterionHandler.class)
        public class TaskItemSelectorCriterionHandler extends 
            BaseItemSelectorCriterionHandler<TaskItemSelectorCriterion> {

            public Class <TaskItemSelectorCriterion> getItemSelectorCriterionClass() {
                return TasksItemSelectorCriterionHandler.class;
            }

            @Activate
            @Override
            protected void activate(BundleContext bundleContext) {
                    super.activate(bundleContext);
            }

        }

3.  If you need a new return type, create it by implementing 
    [`ItemSelectorReturnType`](@app-ref@/collaboration/latest/javadocs/com/liferay/item/selector/ItemSelectorReturnType.html). 
    Name your return type class after your entity, and suffix it with 
    `ItemSelectorReturnType`. Return type classes need no content. For example, 
    here's a return type for a task: 

        /**
        * This return type should return the task ID and the user who
        * created the task as a string.
        *
        * @author Joe Bloggs
        */
        public class TaskItemSelectorReturnType implements ItemSelectorReturnType{

        }
