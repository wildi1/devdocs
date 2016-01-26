---
layout: default
title: Beta1: User guide
github_link: enterprise/user-guide/beta1/index.md
indexed: false
---
This guide describes the Shopware Enterprise Dashboard from a user's perspective.

## Usecase
The Shopware Enterprise Dashboard (EDB) allows you, to control and maintain multiple, independent Shopware instances. It is a 
separate software platform, that can create and copy Shopware installations, centrally manages all Shopware user accounts
and provides one login form for all Shopware instances in the system.  

## Getting started
Once the virtual image is set up and running, you can navigate to `http://FOO` in order to launch the EDB web interface.
You can log in with the user/password combination `user`/`user`. After that you will see the EDB wireframe: On the left
side, the main navigation can be seen - on the top there is the quick login functionality as well as the notification
indicator and a link to you user's profile. In the main section, the so called "Dashboard" can be seen.


## The Dashboard
`SCREENSHOT` The Dashboard is the first overview of the EDB. Currently it shows some statistic information of your Shopware
instances. It is intended to be the main overview of all your Shopware shops regarding statistics and health in later EDB versions. 

In the top you can choose a shop in the "Shop" select box. Right of that, you can specify the period of time you want to
inspect. Currently there are only "order amount" and "conversion" statistics, that will adapt to the selected shop and time. 

## The server view
The next menu item in the navigation is the "Servers" overview. It shows you all *servers* in the system as well as the 
*Shopware shops* running on those servers. Every line in the server overview has an additional select box on the right,
where you can *Create shops* or *Edit server* details. On the left side, you can expand any single server line. It will 
then show you the Shopware shops, that are running on that server. For every shop there is a "log in", an edit, a clone
and a remove button. 


### Adding a server to the EDB
New servers can be added to the system clicking the *Create server* button on the right. The actual server creation
takes place outside of the EDB, of course - at this point, an existing server is just connected to EDB. After clicking
the *Create server* button, you will see the "Create server" overview. Here you can add a custom `Title` as well as a
`Host` and a `Webserver User Group`. In the virtual image, we created a spare server, that you can set up here: 

`SCREENSHOT`.

After clicking the `Create Server` button, the newly created server will appear in the server overview. 


### Adding a shop

### Cloning a shop


## shopwareID
The "Shopware ID" configuration will allow you to configure one or multiple shopware IDs. After clicking the `+` symbol,
you can enter your shopwareID credentials; the EDB will then connect to the shopware account and import all associated
license from Shopware. This way the EDB is able to synchronize all your licenses to your Shopware shops automatically
(by hostname). 
The license list is separated into "Product licenses" for e.g. your enterprise license as well as "Plugins" for plugins
by shopware as well as "3rd party plugins" for other plugins.  

## User admin panel
The "Users" navigation item will bring you to the user administration panel. Here all EDB and Shopware users are maintained.
If you select a user from the list on the right, you can edit his/her details on the right. 
In the "Shopware Roles" tab, you can assign shops and roles to that user by clicking the "Manage roles" button. A window will
appear, where you can pick roles (e.g. "Article" or "Marketing") and assign them to certain Shops. For those shops,
the user will now have the selected roles.

In the "EDB Groups" panel you can also grant certain EDB privileges to any given user. The "Admin" group will grant 
full access to any functionality of the EDB, whereas the "SSO" group will just allow a user to log into shops he has
access to from the EDB. In later versions of the EDB more fine-grained roles could be added.  


## Error log overview
The "Log" panel gives an overview of all operations that are performed by the EDB behind the scenes. In the top bar
you can filter by shop, server, status and the user, who triggered the operation. Especially in the EDB beta this will
allow you to provide valuable feedback to us in case of any errors. 
Furthermore you can trace, which operation was triggered by which user. 
Clicking the "More information" button of any log entry will show up a more detailed overview of the current log item. 