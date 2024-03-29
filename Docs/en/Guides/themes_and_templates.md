# Themes and Templates

Webiny is a [headless Content Management System](./about.md), meaning it doesn't have a presentation layer. Themes in Webiny are simple definitions of the presentation layer structure. The presentation layer itself is de-attached from the system and is control by the developer.

[todo: image illustrating a theme, which is outside the webiny system, theme definition, which is on the border between the theme and the webiny system and the actual CMS which talks via the theme definition to the actual theme]

This approach gives you the ability to display the same content on many devices and screen sizes. For example you can display the content on a NodeJS powered website, smart watch or a SmartTV. The options are endless as the data returned from the CMS is a simple JSON object. 


## Theme Structure
Theme is a simple JSON object. Theme doesn't contain any files, nor any assets. A theme has 3 layers:

1. **Theme** - tells basic information about the theme name, author and its version.
2. **Layout** - a theme can have multiple layouts. Layouts extract common elements usually presented in two or more templates. This way you don't need to define those elements in your templates.
3. **Template** - A template extends a layout, and defines additional elements. Layouts and templates have the same definition syntax.

Layouts and templates have two main goals. The first one is to define which **modules** need to load on pages using this template or layout. A typical example would be loading the "menu" module and passing the name of the menu you wish to load, i.e. main menu. This way you will get the main menu items in your page JSON object. ([click here to learn about the page JSON object](./page_api.md)) 

The second goal is to define zones. **Zones** are a high-level content group. A simple example would be, say you have a website template with a main content area and left sidebar area. Basically those are two zones. When creating your page, you can define in which zone should a certain page element be placed. 

[todo: image ilustrating the theme structure from theme, layouts templates to placeholders]

###Theme Definition Sample

```json
{
  "name": "Foo bar theme",
  "author": {
    "name": "Webiny",
    "email": "info@webiny.com",
    "url": "http://www.webiny.com/"
  },
  "description": "This is a test theme",
  "version": "0.1.0",
  "layouts": [
    {
      "name": "Master",
      "modules": [
        {
          "module": "menu",
          "options": {
            "name": "main-menu"
          }
        },
        {
          "module": "page",
          "options": {
            "url": "/about-us/"
          }
        }
      ],
      "templates": [
        {
          "name": "Blog post",
          "filename": "blog-post.tpl",
          "description": "Used for displaying blog posts.",
          "layout": "Master",
          "zones": [
            "left-sidebar",
            "central-content",
            "right-sidebar"
          ],
          "modules": [
            {
              "module": "menu",
              "options": {
                "name": "blog-post-sidebar"
              }
            }
          ]
        }
      ]
    }
  ]
}
```


#### Theme

A the top we start off with a theme definition:

```json
{
  "name": "Foo bar theme",
  "author": {
    "name": "Webiny",
    "email": "info@webiny.com",
    "url": "http://www.webiny.com/"
  },
  "description": "This is a test theme",
  "version": "0.1.0"
}
```
This part describes the theme, author and provides some theme versioning info.

#### Layouts and Templates

As mentioned `layouts` and `templates` sections use the same definition. The only difference is that a `template` needs to reference a `layout`.

The keys that you can define when describing a `template` or a `layout` are:

* **name** - name of the `template` or a `layout`.
* **filename** - this is an optional key that you can define. This key will be passed to all the page JSON objects using this template.
* **description** - few words to describe your `template` or `layout`.
* **zones** - this can be defined on both a `template` or a `layout`. If defined on both, the result will be a merged lists of both zones.
* **modules** - list of modules that need to be loaded when using this template. To each module you can pass a set of `options`. For example on a master layout you might have a text describing "About us" that's displayed somewhere on the page. To load that text you can define the `page` module and pass the "/about-us/" as a url pattern. This will load that page and append that content into your page JSON object. [todo: define a list of apps] [Checkout the list of apps to know which options are available.]() Similar to `zones`, `modules` are also merged between the `latout` and `template`.
* **meta** - an optional parameter defining a list of parameters. These parameters are attached to every page JSON object using this `template` or `layout`.
* **templates** - this is a child element of `layouts`. This key is a list holding all the defined templates belonging to that layout.

```json
{
  "name": "Blog post",
  "filename": "blog-post.tpl",
  "description": "Used for displaying blog posts.",
  "layout": "Master",
  "zones": [
    "left-sidebar",
    "central-content",
    "right-sidebar"
  ],
  "modules": [
    {
      "module": "menu",
      "options": {
        "name": "blog-post-sidebar"
      }
    }
  ],
  "meta": {
    "key": "value",
    "key2": {
      "some object": "value"
    }
  }
}
```

### Creating a Theme
There are two ways of creating a theme. You can either use the user interface inside the Webiny CMS module. The UI enables you to create your theme, and the associated layouts and templates. 

[todo: screenshot of the theme module]

Alternative approach is to write down the theme definition via your favorite text editor. This you can then import into Webiny CMS. When doing an import, the system will automatically create the `theme`, `layouts` and `templates` based on your definition.

## Rendering Content
As mentioned, Webiny is a [headless CMS](./about.md), and it doesn't have a presentation layer. Rendering of the content is up to the developer. Webiny still provides a [sample PHP rendering engine](). You can follow similar guidelines to create your own, or you can use the provided one.

If you still opt-in to create your own rendering layer, make sure you integrate proper caching options of the Page JSON Object. This will significantly improve the performance. 