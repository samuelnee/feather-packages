# Bootstrap package

This package is based on the [Bootstrap framework](http://getbootstrap.com/). In order to use it you need to add the package to the `ResourcePackages folder` of your project. If the `ResourcePackages` folder doesn't contain any packages, widget templates will be loaded from Feather or from the MVC folder of SitefinityWebApp (if this folder contains files with names, matching the naming convention). Templates from the source of Feather have lowest loading priority. Templates in the MVC folder of SitefinityWebApp are with higher priority, and templates from a package have highest loading priority.

## Package structure

The Bootstrap package contains front-end assets, widget template, grid widget templates and grunt configuration. Below are listed some of the folders and files.
 - **assets** - contains front-end files such as CSS, JS, images and fonts
 	- **dist** - contains the processed ready-to-use front-end assets
 		- **css** - contains the processed css files
 			- **main.css** - this is output of the processed `main.scss` from `assets/src/project/sass`. This file contains Sitefinity, Bootstrap and project css
 			- **main.min.css** - this is the same as `main.css` but minified. This is the distributed css file which is linked in the package Razor layout file `MVC/Views/Layouts/default.cshtml`
        - **fonts** - contains files for sitefinity and project icon font
        - **images** - contains compressed images from src folder which are usually used as background images in the css
        - **js** - contains a minified js file which is a concatenation of js files listed in `jsfiles.json`. To use this file add a reference to it in the package Razor layout file `MVC/Views/Layouts/default.cshtml`
 	- **src** - contains the source front-end files which are processed via grunt to dist folder
        - **sitefinity** - contains scss files for Sitefinity styling
 		- **project** - add your non-sitefinity front-end assets here
            - **fonts** - fonts files added here will be copied to dist/fonts when you run `grunt`
 			- **icons** - add `svg` files here to be added to the icon font that is generated when grunt is run
 			- **images** - add images here. Images added in this folder will be compressed and output to `assets/dist/images`
 			- **js** - add js files here and list them in `jsfiles.json`. All js files listed in `jsfiles.json` will be concatenated and uglified to `assets/dist/js/project.min.js`
 			- **sass** - create subfolders in this folder and add your scss files here
 				- **main.scss** - import all your scss files here. This file will be processed to `assets/dist/css/main.min.css`
        ```
        File structure example:
        | ResourcePackages
        |-- Bootstrap
        |---- assests
        |------ dist
        |-------- css
        |-------- fonts
        |-------- images
        |-------- js
        |------ src
        |-------- sitefinity
        |---------- icons
        |---------- images
        |---------- sass
        |-------- project
        |---------- fonts
        |---------- icons
        |---------- images
        |---------- js
        |---------- sass
        |------------ main.scss
        ```
        **! It is NOT recommended to rename the subfolders because they are used in the gruntfile.js !**
 - **MVC folder** - contains all widget templates categorized by widget and the Razor layout file
 	- **Layouts/default.cshtml** - Razor layout file
 - **GridSystem** - contains grid widget templates
 - **csslint.json** - contains csslint options and globals
 - **gruntfile.js** - contains configuration and definition of grunt tasks and loads grunt plugins
 - **jsfiles.json** - contains a list of js files to be automatically concatenated and uglified when grunt is run
 - **package.json** - stores metadata for grunt and grunt plugins that the project needs

## Editing and creating a widget
By default we include all widget templates in every package. Modifying a template is super easy. If you want to modify the Pills navigation template, just go to `/ResourcePackages/Bootstrap/MVC/Views/Navigation`, open the `NavigationView.Pills.cshtml` file and make your changes.

Creating a new template is just as easy. Duplicate an existing template, give a name to the new file, keeping in mind the following structure - `NavigationView.XXXXXX.cshtml`. Then the new template will appear in the list of templates for this widget in the widget designer in Page editor in Sitefinity backend.

For widgets that have list and details views, the structure should be `List.XXXXXX.cshtml` or `Detail.XXXXXX.cshtml`, respectively.

To create a new widget template for Dynamic content first create a folder with the name of the dynamic module in singular in `/ResourcePackages/Bootstrap/MVC/Views`. After that create a `*.cshtml` files having in mind the structure described above List.XXXXXX.cshtml for list view and Detail.XXXXXX.cshtml for details view and write the markup of the template.
When you create a new Dynamic module, list and details widget templates are automatically created for this module in `Design > Widget Templates` in Sitefinity backend. As a starting point, you can use the template from Sitefinity backend and make the desired changes on the file system.

## Editing and creating a grid widget
By default we include the most popular column combinations as grid widgets. To change column css classes or add new css classes for different break points, for example, go to Page Editor in Sitefinity backend and use the grid widget designer.

Modifying a grid widget template is super easy.  If you want to modify the grid widget with two equal columns go to `/ResourcePackages/Bootstrap/GridSystem/Templates`, open `grid-6+6.html` and make your changes. You can also add an extra CSS class to a column, change the label of the field for adding CSS classes for this column in the grid widget designer in Page editor or change the name of a column's placeholder name in Page editor .

**Example:**
```
<div class="row" data-sf-element="Row">
    <div class="sf_colsIn col-md-6 **EXTRA_CSS_CLASS**" data-sf-element="**NEW_COLUMN_1_CSS_CLASS_FIELD_LABEL**" data-placeholder-label="**NEW_COLUMN_1_PLACEHOLDER_NAME**">
    </div>
    <div class="sf_colsIn col-md-6" data-sf-element="Column 2" data-placeholder-label="Column 2">
    </div>
</div>
```
Don't remove `sf_colsIn`. It is a system css classed which indicates where a placeholder will be created.

Creating a new grid widget is just as easy.
Duplicate an existing grid widget template, give a name to the new file. Then the new grid widget will appear in the list of grid widgets in the Layout tab in Page editor.

**Example:** To create a simple placeholder with &lt;section&gt; tag create `section.html` in `/ResourcePackages/Bootstrap/GridSystem/Templates/`
```
	<section class="section" data-sf-element="Section" data-placeholder-label="Section">
	</section>
```

## Widget templates management recommendations

### Modifying widget templates

To make upgrades easier we recommend not to change default widget templates. If you need to make changes to a default template it is better to create a new one by duplicating the one you want to change and make the changes on the new one.

### Where to put project templates

If you want to have the project's widget templates separated from the default widget templates we recommend to move all default widget templates from `/ResourcePackages/Bootstrap/Mvc/Views/` of the package to `SitefinityWebApp/Mvc/Views/` and add project specific templates to `/ResourcePackages/Bootstrap/Mvc/Views/`. It is easier to navigate and manage less files.

## Responsive design ##

The responsiveness of the widgets and grid widgets rely solely on Bootstrap. The goal is to integrate the frameworks in the best possible way so that everyone can take advantage of the responsive features of the framework.

An exception is made for the Navigation widget. There is a possibility to transform the navigation into a `<select>`.

In the navigation widget templates (e.g. `/Bootstrap/MVC/Views/Navigation/NavigationView.Horizontal.cshtml`) there is a helper method `@Html.Action("GetView", new { viewName = "Dropdown",  model= Model})` which renders the `<select>`. It's commented out by default, but if you want to, you can use it with combination with the responsive utility classes of Bootstrap.
If you decide to use it, you can modify its markup in the `/Bootstrap/MVC/Views/Navigation/Dropdown.cshtml` file.

## Grunt
### Install
Prerequisites: If you have not installed grunt yet refer to [Grunt gettings started documentation](http://gruntjs.com/getting-started) for details.
```
> npm install
> grunt
```
`grunt` executes the default grunt tasks and watches for changes in the scss files, js files and images. If you have added new sprite images or svg files for the webfont task you will need to run `grunt` again.

## Where to put project front-end assets
All project specific front-end assets like scss, images, js, fonts, etc. should be placed in `assets/src/project`. When the default grunt task is run all source files are processed and moved to `assets/dist` from where there are used in the project.

### Scss
Place all your scss files in `assets/src/project/sass`. We recommend to create subfolders to organize the project's files and then import them in `main.scss`
**Example:**
```
File structure
|- sass
|-- settings
|--- _colors.scss
|--- _typography.scss
...
|-- base
|--- _link.scss
|--- _typography.scss
...

main.scss
```
//Import Bootstrap from npm
@import "../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap.scss";
@import "../../../../node_modules/magnific-popup/src/css/main.scss";

// Sitefinity
@import "../../sitefinity/sass/components/icons/sf-icon-font";
@import "../../sitefinity/sass/widgets/socialShare/sf-sprite";
@import "../../sitefinity/sass/sitefinity.scss";

//Import .scss files here
@import "setting/colors";
@import "setting/typography";
...
@import "base/link";
@import "base/typography";
...
```
When you run grunt all scss files imported in `assets/src/project/sass/main.scss` will be processed and output in `assets/dist/css/main.css`
If you don't want to include Bootstrap or Sitefinity css delete the import rules that you don't need in `assets/src/project/sass/main.scss`.
- Imports Bootstrap
  @import "../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap.scss";
- Imports the styling of magnific-popup plugin used in some media gallery templates
  @import "../../../../node_modules/magnific-popup/src/css/main.scss";
- Imports css for icon webfont generated from svg files added in `assets/src/project/icons`
  @import "../../sitefinity/sass/components/icons/sf-icon-font";
- Imports css for sprite image generated from png files added in `assets/src/project/images/sprite/`
  @import "../../sitefinity/sass/widgets/socialShare/sf-sprite";
- Imports all Sitefinity css for default widgets
  @import "../../sitefinity/sass/sitefinity.scss";

### Images
Place all images in `assets/src/project/images`. After grunt is run all images from this folder will be compressed and moved to `assets/dist/images`. Grunt will also watch for changes in this folder so if you add a new image or change an existing one the new image will be compressed and moved to `assets/dist/images`

### Javascript
Place all your js files in `assets/src/project/js`. It is always better to load one js file to reduce requests to the server and speed up your site. Therefore we provide a mechanism to easily concatenate and uglify all project js files into one file.

In `jsfiles.json` define the order in which the project's js files will be concatenated and uglified. After you run grunt all js files listed in `jsfiles.json` will be processed and output to `assets/dist/js/project.min.js`.

**jsfiles.json**
```
{
	"concatJsFiles": [
        "assets/src/project/js/project-file-1.js",
        "assets/src/project/js/project-file-2.js"
	]
}
```

**Example:** To load `project.min.js` open the project Razor layout file (`MVC/Views/Layouts/default.cshtml`) and add a reference there.
```
	@Html.Script(Url.Content("~/ResourcePackages/Bootstrap/assets/dist/js/project.min.js"), "bottom")
```

###Icons
Place all svg files that you want to use as icons via an icon font in `assets/src/project/icons`. The icon font will be created the first time grunt is run. If you add new svg files you will have to run the task manually (`grunt webfont`) or run default grunt task again.
Two css classes will be generated for each icon. If the name of the svg file is logo.svg, the names of the css classes will be:
- `icon-logo` - icon is displayed before Company name
`<span class="icon-logo">Company name</span>`
- `icon-item-logo` - icon is displayed after Company name
`<span class="icon-item-logo">Company name</span>`

## Upgrade recommendations
- If you work on a copy of Bootstrap, e.g. Timer
    - Upgrade Bootstrap package
    - Merge changes from Bootstrap to Timer manually
- If you work on Bootstrap package directly
	- Before upgrade make a copy of Bootstrap package to another location
	- Upgrade Bootstrap package
	- Merge conflicts
		- If you use a source control, merge the changes using the source control
		- If you don't use a source control, merge the changes from the copy of the Bootstrap package to the Bootstrap package manually using differencing and merging tool.
