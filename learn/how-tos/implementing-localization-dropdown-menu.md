---
title: "Implementing Localization for Dropdown Menu"
id: "implementing-localization-dropdown-menu"
---

**Problem Statement**: How to implement Localization for the menu items.[Localization](/learn/how-tos/localization-wavemaker-apps/) enables you to render your app in multiple languages. WaveMaker automatically detects the user’s preferred locales (from the browser) and renders the application with the locale of the user’s preference if the application supports. The user can be given a choice to select the language using the Select Locale widget.

In this document, we will be walking through the steps to enable language support to Dropdown Menu widget.

1. From the [Variable](/learn/assets/var_sel.png) menu create a Model Variable.
2. Give a name to the Variable, say _menudata_, and set the **isList** option to true.
3. Use the following JSON structure in the Text Editor, to define the structure for Menu identifying Label, Icon, Link, and Children. Icons can be [halflings from glyphicons](http://glyphicons.com/) or [font awesome](https://fortawesome.github.io/Font-Awesome/cheatsheet/) icons.
    
    \[
      {
        "mylabel": "KEY\_1",
        "icon": "glyphicon glyphicon-home",
        "link": "#Main"
      },
      {
        "mylabel": "KEY\_2",
        "icon": "glyphicon glyphicon-user",
        "link": "http://www.facebook.com"
      },
      {
        "mylabel": "KEY\_3",
        "icon": "glyphicon glyphicon-search",
        "children": \[
          {
            "mylabel": "KEY\_4",
            "icon": "glyphicon glyphicon-arrow-right",
            "link": "http://www.google.com"
          },
          {
            "mylabel": "KEY\_5",
            "icon": "glyphicon glyphicon-arrow-right",
            "link": "http://www.yahoo.com"
          }
        \]
      }
    \]
    
    NOTE: Here, we have parameterized the _mylabel_ values as KEY\_1, KEY\_2, KEY\_3, KEY\_4, KEY\_5 so that the label values can be displayed as per the locale selection at runtime.
4. Drag and drop a Dropdown Menu widget on the canvas ([more on Dropdown Menu](/learn/app-development/widgets/navigation/dropdown-menu-use-cases/))
    - From Properties panel, under the Dataset section, using the chain icon next to the Value property bind the widget to the Modal Variable (menudata) created in the above steps.
    - Under the Actions section set:
        - the Action Label to the following value: _App.appLocale\[mylabel\],_ (you have to use the Use Expression option from the Bind Dialog)
        - the Action Icon to icon, and
        - the Action link to link.
5. Open the I18N dialog from the Project Configurations of the [Project Workspace](/learn/app-development/wavemaker-overview/product-walkthrough/#project-workspace) ([more on localization](/learn/app-development/widgets/form-widgets/select-locale-usage/)). [![](/learn/assets/loc_create.png)](/learn/assets/loc_create.png)
    1. Use **Manage Languages** to add languages to your app. By default, English is already added. [![](/learn/assets/loc_new_locale.png)](/learn/assets/loc_new_locale.png)
    2. Use **+** sign next to Message Key header to open the Localized Message Dialog. [![](/learn/assets/loc_default_msgs.png)](/learn/assets/loc_default_msgs.png)
    3. Add a message as shown below: [![](/learn/assets/menu_locale.png)](/learn/assets/menu_locale.png)
    4. Repeat the process for all the messages keys. [![](/learn/assets/menu_locale_messages.png)](/learn/assets/menu_locale_messages.png)
6. Drag and drop a Select Locale widget onto the canvas. The Select Locale widget auto-populates the languages added from the I18N dialog as a drop-down. ([more on Select Locale widget](/learn/app-development/widgets/form-widgets/select-locale/))
7. Preview the page and check the functionality at runtime. On changing the language in the select Locale widget, the values in the Menu dropdown will also change.

Dropdown Menu Use Cases

- [1\. Dropdown Menu Basic Usage](/learn/app-development/widgets/navigation/dropdown-menu-use-cases/)
- [2\. How to enable user role-based access to menu items](/learn/how-tos/restricting-menu-item-display-based-user-role/)
- [3\. How to localize menu items](#)
