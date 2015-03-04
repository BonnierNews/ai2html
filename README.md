# ai2html

> ai2html is a script for Adobe Illustrator that converts your Illustrator artwork into html and css.

## About

The script renders text as absolutely positioned html elements. The remaining art is exported as an image that is placed underneath the text in the html. Artboards can be rendered as separate divs in a single file, or as separate files. The exported files are html partials, that is, everything is enclosed in a div that can be inserted into a page template. It is also possible to specify an html page template to insert the html partial to preview your artwork in the context of your site architecture and css.

## Table of contents

- [How to install ai2html](#how-to-install-ai2html)
- [Overview of how to use ai2html](#overview-of-how-to-use-ai2html)
- [Controlling how your text is converted into html](#controlling-how-your-text-is-converted-into-html)
- [Settings](#settings)
- [Which attributes are converted to html and css](#which-attributes-are-converted-to-html-and-css)
- [Limitations](#limitations)


## How to install ai2html

Move the ai2html.jsx file into the Illustrator folder where scripts are located. For example, on Mac OS&nbsp;X running Adobe Illustrator CC 2014, the path would be:
```
Applications\Adobe Illustrator CC 2014\Presets\en_US\Scripts\ai2html.jsx
```

## Overview of how to use ai2html

1. Create your Illustrator artwork. Size the artboard to the dimensions that you want the div to appear on the web page.
2. Make sure your `Document Color Mode` is set to `RGB`. You can do this from the `File` menu.
3. Make sure you have already saved your document. The script uses the location of the ai file to put a folder into which the exported html and image files are saved.
4. Run the script by choosing: `File > Scripts > ai2html`
5. Go to the folder containing your Illustrator file. Inside will be a folder called `ai2html-output`. Open the html files in your browser to preview your output.

## Controlling how your text is converted into html 

Discussion here about:
- Choosing between point and area text.
- Choosing the paragraph alignment of the text
- Limitations because of one style per paragraph


## Settings

There are several ways of customizing the output of the script:
- [Special text blocks](#special-text-blocks)
  - [ai2html-settings](#ai2html-settings)
  - [ai2html-css](#ai2html-css)
  - [ai2html-js](#ai2html-js)
  - [ai2html-html](#ai2html-html)
  - [ai2html-text](#ai2html-text)
- [Layer and object names](#layer-and-object-names)
- [Artboard names](#artboard-names)
- [Notes in the attributes panel](#notes-in-the-attributes-panel)

#### Special text blocks

The script recognizes five special types of text blocks, which begin with `ai2html-` followed by one of the following keywords: `settings`, `css`, `js`, `html` or `text`. That text should be the only thing on the first line of the text block. The special text blocks can be placed anywhere in your Illustrator document, but note that if you place them on an artboard their contents will be rendered in your output.

##### `ai2html-settings`

Most of the script’s options are set in the settings text block. When you run ai2html for the first time in your ai file, the script will place a settings text block to the upper left of all your artboards. Here is a description of the settings:

*Options that are included in the settings text block by default*

- **image_format**
  - <span style="font-variant: small-caps">Possible values</span>: `png` `png24` `jpg`
  - <span style="font-variant: small-caps">Default</span>: `png`
  - You can specify more than one image format to be output by listing the desired formats separated by commas. This can be useful if you want to see which image format has the smallest file size. The first format in the list will be the only one referenced in the html. 
- **responsiveness**
  - <span style="font-variant: small-caps">Possible values</span>: `fixed` `dynamic`
  - <span style="font-variant: small-caps">Default</span>: `fixed`
  - `dynamic` responsiveness means that the ai2html divs will scale to fill the container they are placed in.
  - `fixed` responsiveness means that the ai2html div will appear only at the size it was created in Illustrator and will not change sizes if the container it is placed in changes size.
- **output**
  - <span style="font-variant: small-caps">Possible values</span>: `one-file` `multiple-files`
  - <span style="font-variant: small-caps">Default</span>: `one-file`
  - `one-file` output means that all artboards will be written into one html file.
  - `multiple-files` output means that each artboard will be written to separate html files.
- **html_output_path**
  - <span style="font-variant: small-caps">Default</span>: `/ai2html-output/`
  - Use this if you want to redirect the output to a different folder. The path should be written relative to the location of the ai file.
- **html_output_extension**
  - <span style="font-variant: small-caps">Default</span>: `.html`
  - Use this if you want to change the extension of the html partial.
- **image_output_path**
  - <span style="font-variant: small-caps">Default</span>: `<blank>`
  - Use this if you want to change the path of the images that are placed behind the html text. This path should be written relative to the location of the `html_output_path`.
- **local_preview_template**
  - <span style="font-variant: small-caps">Default</span>: `<blank>`
  - Use this to specify a page template into which the html partial will be inserted to preview your artwork in the context of your site architecture and css. Sample templates can be downloaded from the ai2html Github repo. Any variables from ai2html-settings or ai2html-text blocks can be inserted into the templates using either mustache (eg. `{{headline}}`) or ejs/erb (eg. `<%=headline%>`) notation. The ai2html partial can be inserted using the `{{ai2htmlPartial}}` variable.
- **png_number_of_colors**
  - <span style="font-variant: small-caps">Possible values</span>: Any integer from `2` to `256`
  - <span style="font-variant: small-caps">Default</span>: `128`
  - Use this to specify the quality of `png` or `png24` images.
- **jpg_quality**
  - <span style="font-variant: small-caps">Possible values</span>: Any integer from `0` to `100`
  - <span style="font-variant: small-caps">Default</span>: `60`
  - Use this to specify the quality of `jpg` images.
- **headline**<br>**leadin**<br>**notes**<br>**sources**<br>**credit**
  - <span style="font-variant: small-caps">Possible values</span>: Text with no line breaks
  - These fields are used to populate fields in the `local_preview_tempate` and can be written to a “config” text file along with the html. The config file is written in yml can be used to pass parameters to a cms.
  
*Options that can be added to the settings text block*

- **max_width**
  - <span style="font-variant: small-caps">Possible values</span>: Any positive integer
  - <span style="font-variant: small-caps">Default</span>: `<blank>`
  - Specifying a `max_width` inserts a max-width css instruction on the div containing the ai2html partial. The width should be specified in pixels.
- **create_config_file**
  - <span style="font-variant: small-caps">Possible values</span>: `true` `false`
  - <span style="font-variant: small-caps">Default</span>: `false`
  - Specify `true` if you want the script to also output a text file in yml format containing the parameters you want to pass to your cms.
- **config_file_path**
  - <span style="font-variant: small-caps">Default</span>: `<blank>`
  - <span style="font-variant: small-caps">Example</span>: `/ai2html-output/config.yml`
  - Enter the full path to the file including the filename that you want the config file to be named. The path should be written relative to the location of the Illustrator file.
- **png_transparent**
  - <span style="font-variant: small-caps">Possible values</span>: `yes` `no`
  - <span style="font-variant: small-caps">Default</span>: `no`
  - This option lets you make transparent any png image that is output by the program. Note that specifying transparent png’s can result in a visibly significant degradation in the image. 
- **center_html_output**
  - <span style="font-variant: small-caps">Possible values</span>: `true` `false`
  - <span style="font-variant: small-caps">Default</span>: `true`
  - Specifying `true` on this option causes the ai2html div to be centered within the container it is placed in. 
- **use_2x_images_if_possible**
  - <span style="font-variant: small-caps">Possible values</span>: `yes` `no`
  - <span style="font-variant: small-caps">Default</span>: `yes`
  - Specifying `yes` on this setting tells the script to output images at double the resolution if possible. The script uses limits specified in this [document](http://apple.co/1M1dvES) to determine if the size of the image is too large to make it double resolution.
- **testing_mode**
  - <span style="font-variant: small-caps">Possible values</span>: `yes` `no`
  - <span style="font-variant: small-caps">Default</span>: `no`
  - Specifying `yes` on this setting causes the text in the file to be rendered both on the image and in the html. This is useful for testing whether the placement of html text is consistent with the Illustrator file.

*If you want to change the default settings, I have been using this [Google spreadsheet](http://bit.ly/1BP86RH) to change the view and change the settings. Feel free to make a copy of the spreadsheet for yourself to roll your own settings.*

##### `ai2html-css`

If you want to add some css that is always inserted into your html partial, include it in a text block somewhere in your Illustrator document, but not on an artboard. Make the first line of the text block reads `ai2html-css`. The css will be added inside `<style>` tags so don’t include `<style>` tags in the text block.

##### `ai2html-js`

If you want to add some javascript that is always inserted into your html partial, include it in a text block somewhere in your Illustrator document, but not on an artboard. Make the first line of the text block reads `ai2html-js`. The js will be added at the end of all the html for each artboard. You will need to include the enclosing `script` tags in the text block.

##### `ai2html-html`

If you want to add some html that is always inserted into your html partial, include it in a text block somewhere in your Illustrator document, but not on an artboard. Make the first line of the text block reads `ai2html-html`. The html will be added at the end of all the html for each artboard.

##### `ai2html-text`

You can store text into variables and insert them into your document using basic mustache or erb/ejs notation. Any variables in the `ai2html-settings` or in `ai2html-text` blocks can be used.

To assign text to a variable, create a text block somewhere in your Illustrator document, but not on an artboard. Make the first line of that text block read `ai2html-text`. Each subsequent paragraph should be in the format of `variable_name: Lorem ipsum dolor.`. Variable names should only be letters, numbers and underscores.

Now you can insert that text anywhere in your document by placing `{{variable_name}}` or `<%=variable_name%>` where you want that text to appear.

Note that you can pass mustache or erb/ejs notation to your html partial as long as the variable names don’t match the variable names in the `ai2html-settings` or `ai2html-text` blocks.

#### Layer and object names

The name of the layer that is the direct parent of a text block is added as a class to the div that corresponds to that text block. This allows you to write custom css targeting all text blocks in a layer. Class names have `ai2html-` appended to the front of the name. (Add example here) 

Another way of targeting a text block is to give that text object a name in the Layers palette. If you give the text object a name, it will be added as an id to the div that corresponds to that text block. In this case `ai2html-` is not appended to the beginning of the id, so take care that you begin the id with letters and not numbers. (Add example here)

#### Artboard names

Artboard names become part of the id of the div corresponding to that artboard.

#### Attribute notes

When you set `responsiveness: dynamic`, text objects stay anchored to the top-left, top-right or top-center of the text object bounding box depending on whether the text is left, right or center aligned. If you want to have the text align to the bottom instead of the top, add this line to the `notes` field of the `Attributes` palette:
`valign:bottom`







## Which attributes are converted to html and css
The script processes each text object in your Illustrator file and translates these attributes into inline and css styles. Each point- or area-text object is converted into a `<div>`. Each paragraph in the text object are converted into `<p>` tags within the `<div>`.
* Inline styles on the `<div>`
  * Position
  * Opacity
* CSS styles applied to `<p>` tags
  * Font
  * Text size
  * Leading
  * Paragraph space before and after
  * Paragraph alignment
    * The alignment of the first paragraph of a text object determines how it is placed in the html. If the first paragraph is left aligned, then on the web page the text object’s div will be absolutely positioned from it’s left edge. If the first paragraph is right aligned, the div will be positioned from its right edge. If the first paragraph is centered, then the div will be positioned from its center.
    * This is important because fonts are rendered slightly differently across browsers and operating systems. This means that in IE on a Windows machine, the font may be a bit larger than in Chrome on a Mac. So a label next to a city dot on a map end up too far away or overlapping the dot if you do not paragraph align the text relative to the dot.
    * Setting more than one kind of paragraph alignment on a point text object cannot be translated into html, as there is really no such thing as a point text object in html. If you really want to have different paragraph alignments within a single text object, you should do it with an area text object.
  * Capitalization
  * Text color
  * Character tracking
  
## Limitations

* Because numbers get rounded to whole pixels by the web page when formatting text and positioning elements, the html version of a graphic will not line up exactly with its Illustrator version. Rounding differences are particularly compounded if you have blocks of text that span many lines and have fractional leading in Illustrator.
* The script currently only sets one style per paragraph, so custom styled words or characters within a paragraph are ignored. Each paragraph’s style is determined by the middle character in the paragraph.
* The script assumes that text always goes above the art.
* Artboards should have unique names.
* Paragraphs with full justification and center/left/right specified will just be “justified” in html.
* If text is not hidden using the hide command, but rather is hidden because it is behind a mask, it will show up if it is within the artboard.
* Labels in graph objects will be rendered as part of the image. (Something changed in newer versions of CC in the way text objects inside the graph object are handled.) If you want your chart labels to be shown as html, you will need to ungroup the chart.
* In area text blocks, text that is hidden because it is overflowing the box will appear in the html output. A future version of the script may exclude that text (if I can figure out how to do it).




Features
Custom css, js, html
Adding classes by using layers
Alignment
Vertical alignment
Mustache templating
Dynamic
Lazy load
Image settings
Promo image auto generated
Output to one html file or multiple html files.
Artboard
Naming to set lower viewports
Add “-” to front to ignore



Options set by where you put your ai file
If the file is in the ai folder in an ai2html project, the html output automatically goes into the public folder of the Preview project.
If the file is in the ai folder of a Preview project that is not an ai2html project, the html output goes into the src folder of the Preview project.
The ai2html script determines if the Preview project is an ai2html project by checking to see if “project_type” in the config.yml is set to “ai2html.”

Options set by the ai2html-settings text block
These are generally not space or tab sensitive. Just separate the setting name from the option with a colon.
image_format
Options:
png
jpg
Notes:
You can specify both png and jpg by typing both of them separated by a comma — as in “png, jpg” — if you want to just generate both and see which one is larger. Only the first one in the list will be used in the output.
Generally, select png for graphics that have fewer colors, like charts. Select jpg if there are photographic images or many gradients/blends.
responsiveness
Options:
fixed
dynamic
Notes:
The fixed option will generate a static graphic at the size of the artboard.
If you specify dynamic, the web output will scale to fill the width of the div that you put the html output into.
output
Options:
preview-one-file
preview-multiple-files
Notes:
The one-file option will put the html for all artboards into a single html file called index.html.
The multiple-file option will generate a separate html file for each artboard. The files will be named a composite of the ai filename and the artboard name.
use_lazy_loader
Options:
yes
no
Notes:
If yes is specified, then a dummy image (a transparent png) is inserted into the html output in place of the image. Data attributes are added to the img tag so that the real image is only loaded if the viewport that uses the image is actually active. This prevents the images from artboards for other viewports from loading, thus allowing the page to load more quickly. This option will also force the inclusion of resizer.js and resizer.css onto the page which is the magic that swaps out the images.
If no is specifed, then the urls of images for all artboards and viewports are hardcoded in the html. Beware that for pages with large images or many images, choosing this option will make the page load more slowly because it will be loading unnecessary images.
use_2x_images_if_possible
Options:
yes
no
Notes:
If yes, the script will generate double resolution images if the image size does not exceed the pixel limit for older iPhones.
create_promo_image
Options
yes
no
Notes:
If yes, the script will generate a jpg or png (depending on what is specified by image_format) at a very large size to satisfy the requirements of Scoop promo images.
png_transparent
Options
yes
no
Notes
If yes, png images used in the html output will have a transparent background.
png_number_of_colors
Options
A number from 2 to 256
Default is 128
Notes
If png is selected in image_format, this can be used to set the quality of the png. Higher numbers may improve the image quality, but will also increase the file size.
jpg_quality
Options
A number from 0 to 100
Default is 80
Notes
If jpg are selected in image_format, this can be used to set the quality of the png. Higher numbers may improve the image quality, but will also increase the file size.

headline, leadin, notes, credit, sources
Options
Text. Cannot contain returns.
last_updated_text
Options
page_template : { useQuoteMarks:false, defaultValue:"nyt5-article-embed" },
scoop_publish_fields : { useQuoteMarks:false, defaultValue:"true" },
publish_system : { useQuoteMarks:false, defaultValue:"scoop" }, // or scoop_stg
scoop_username : { useQuoteMarks:false, defaultValue:"" },
scoop_slug : { useQuoteMarks:false, defaultValue:"" },
scoop_external_edit_key : { useQuoteMarks:false, defaultValue:"" },
scoop_asset_id : { useQuoteMarks:false, defaultValue:"" }


Options set by the artboard palette


Options set by the layers palette


Options set by the object’s “Notes” attribute