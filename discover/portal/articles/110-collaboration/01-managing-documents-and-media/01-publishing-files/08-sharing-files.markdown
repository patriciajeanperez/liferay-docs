# Sharing Files [](id=sharing-files)

@product@ contains a fine-grained permissions system that lets administrators 
define the actions that users can take on files. As with all permissions in 
@product@, 
[permissions for files are granted via roles](/discover/portal/-/knowledge_base/7-2/adding-files-to-a-document-library#granting-file-permissions-and-roles). 
Administrators can therefore let users collaborate on files by assigning the 
appropriate file permissions to a role, and then assigning those users to that 
role. Similarly, non-administrative users can grant permissions for files they 
own to specific roles. 

But what if a non-administrative user wants to share files with a subset of 
users in one or more roles? What if a non-administrative user wants to share 
files with a single user? Instead of asking an administrator to create a new 
role and assign users to it, non-administrative users can share files directly 
with each other. This lets users share files without requiring direct 
intervention from an administrator, which saves time and effort for everyone. 
After all, sharing is caring. 

When a user shares a file, that user grants some of their permissions for that 
file to the receiving user. The user sharing the file must grant at least the 
View permission. The users must also be part of the same instance, but don't 
have to be members of the same site. 

## Sharing Files in Documents and Media [](id=sharing-files-in-documents-and-media)

To share a file, you must have at least View permission on that file. You must 
share files via either the Documents and Media app in Site Administration, or 
the Documents and Media widget on a page. 

Follow these steps to share a file: 

1.  Navigate to the file you want to share in either the Documents and Media app 
    in Site Administration, or the Documents and Media widget on a page. 

    To navigate to the Documents and Media app in Site Administration, open the 
    *Menu* 
    (![Product Menu](../../../../images/icon-menu.png)), 
    click your site's name, and navigate to *Content* &rarr; 
    *Documents and Media*. 

    If you're using the Documents and Media widget on a page, then you must 
    enable actions for the widget. To do this, follow these steps: 

    -   Select *Configuration* from the widget's *Options* menu 
        (![Options](../../../../images/icon-app-options.png)). 
    -   In the *Setup* tab's *Display Settings*, select *Show Actions*. 
    -   Click *Save* and close the Configuration window.

2.  Click the file's *Actions* button 
    (![Actions](../../../../images/icon-actions.png)) 
    and select *Share*. This opens the Share dialog.

3.  Enter the email address of the user you want to share the file with. To 
    share the file with multiple users, enter each user's email address in a 
    comma delimited list. 

4.  Choose whether users receiving the file can also share it. To allow this, 
    select *Allow the document to be shared with other users*. Otherwise, 
    receiving users can't share the file. Note, however, that if any users have 
    View, Comment, or Update permissions via the traditional role-based 
    permissions system, they can share the file regardless of your selection 
    here. 

5.  Select the file permissions you want to grant to the users receiving the 
    file. Because you can only grant the permissions that you have for the file, 
    some of these options may be unavailable: 

    -   **Update:** View, comment, and update.
    -   **Comments:** View and comment.
    -   **View:** View only.

    If you enabled further sharing in the previous step, note that users 
    receiving the file can only share it with the permissions you grant here. 

6.  Click *Share*. 

![Figure 1: To share a file, you must fill out the Share dialog as described in the previous steps.](../../../../images/sharing-file.png)

## Working with Shared Files [](id=working-with-shared-files)

You can access shared files in three places: 

1.  The Documents and Media Library. Shared files are visible in their existing 
    location in Documents and Media. For example, if someone shared a file with 
    you that resides in the Documents and Media Library's Home folder, then you 
    can access the file in that folder. 

2.  The Notifications app. When a file is shared with you, you get a 
    notification in the Notifications app. Clicking the notification takes you 
    to the file in Documents and Media. For information on notifications, see 
    the article 
    [Managing Notifications and Requests](/discover/portal/-/knowledge_base/7-2/managing-notifications-and-requests). 

    ![Figure 2: The Notifications app contains the notifications that are sent when a user shares a file with you.](../../../../images/sharing-notifications.png)

3.  The Shared With Me app. You can access this app by navigating to *Menu* 
    (![Product Menu](../../../../images/icon-menu.png)) 
    &rarr; *My Account* &rarr; *Shared With Me*. The app lists all the files 
    that other users shared with you. Each file has an Actions button 
    (![Actions](../../../../images/icon-actions.png)) 
    that you can use to perform any permitted actions on the file (e.g., view, 
    comment, update). 

    ![Figure 3: The Shared With Me app lists all the files that other users shared with you.](../../../../images/sharing-app.png)

## Managing Shared Files [](id=managing-shared-files)

What if you shared a file with a user who you didn't intend to share it with? 
What if you want to see a list of users you shared a file with? What if you want 
to modify the permissions on any files that you shared? No problem! You can do 
all these things from the file's Info panel in Documents and Media. Follow these 
steps to do so: 

1.  Click the file in Documents and Media, then click the *Info* button 
    (![Info](../../../../images/icon-information.png)) 
    at the top-right. The file's info panel then slides out from the right. 

2.  Click the *Manage Collaborators* link. This opens a dialog with a list of 
    the users you shared the file with, and their permissions on the file. 

    ![Figure 4: Click *Manage Collaborators* to open up the list of users you shared the file with.](../../../../images/sharing-info.png)

3.  Make any changes you want to the list of collaborators. To un-share the file 
    with a user, click the `x` icon next to that user. You can also change the 
    file permissions via the selector menu for each user. 

4.  Click *Save* and close the dialog. 

![Figure 5: The Collaborators dialog lets remove users from the list or change their permissions for the file.](../../../../images/sharing-collaborators.png)

