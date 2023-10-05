# Custom CSS

!!! warning

    Custom CSS is a feature for advanced users. There is no official support for customizations, and you perform any customizations at your own risk. In case something goes wrong, you can simply remove the `custom.css` from the Zettlr data directory to reset the custom CSS again.

It is possible to use custom CSS ([Cascading Style Sheets](https://en.wikipedia.org/wiki/Cascading_Style_Sheets)) to modify the complete appearance of the app. You can find the Custom CSS editor in the [assets manager](./assets-manager.md).

!!! warning

    The examples in this document haven't been updated in some time, so they may not work out of the box.

If you are unfamiliar with CSS, but don't want to simply copy & paste the guides on this page, you may choose to follow a short [tutorial on CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS). There are many on the internet, so a quick Google search can also provide you with video tutorials, if you prefer those.


## How to make a custom CSS?

### Writing CSS for Zettlr

The styles of Zettlr are divided into both geometry and the actual theme, so you may want to stick with only changing the design of elements _without_ changing **any** geometry. Playing around with the geometry may be fun, but it may yield completely random behaviour, as some parts of the app depend upon the correct sizes of elements. In case you made a mistake, don't worry: Simply remove the `custom.css` file from the data directory of Zettlr. You can find the data directory of your own system by looking at the paths provided in [the setup guide](../getting-started/setup.md).

Classes and IDs in Zettlr are always namespaced to the respective components (unless they're global), so to really override a rule you will have to use the complete namespace (you can also use the `!important` override rule, but this is widely regarded as bad practice).

Everything is always namespaced to the `body`, which has a class `.dark` if the app is currently in dark mode. So to make sure a certain rule only applies while the app is in dark mode, make sure to prefix it with `body.dark`!


### Tips for finding selectors

Zettlr's styles are subject to constant changes. While they should remain fairly stable, changes can be introduced in any version, and therefore, instead of providing you with ready-made examples, this page covers how you can find the correct selectors easily.

First of all, make sure to enable the debug mode in the advanced [preferences](../reference/settings.md) in order to enable the Develop menu. Next, open the developer tools from within this menu and make sure to select the "Elements" tab.

![Zettlr with the developer tools open](../img/zettlr_developer_tools.png)

Then click the arrow in the top-left corner of the developer tools. Now you can click any element in the application to focus it in the developer tools. In the bottom area of the developer tools, you will then see the CSS directives used to style all elements of this particular shape.

![The CSS directives in the developer tools](../img/zettlr_developer_tools_css.png)

The top rules always override the bottom rules, so what you are interested in is the following directive:

```css
body .cm-quote, body .cm-link, body .cm-strong, body .cm-em {
    color: var(--c-primary);
}
```

This is the selector you want to copy over to your custom CSS dialog and style to your liking. As you can see, it gives blockquotes, links, bold and italic text the primary color of the theme.


__________________________________________________

## CSS Code Snippets

!!! warning

    The example CSS Code Snippets below have been updated and tested with v3.0.1 and so should work with newer versions of Zettlr, but this codes can’t works on V2.


### Global formatting

#### Using a Custom Font with Zettlr

In case you do not like the default font delivered with Zettlr, or need to change it, simply paste the following code snippet into the custom CSS editor. Replace `your-font-name` with the **full name** of the font you want to use for Zettlr. Please replace `placeholder` according to the font:

- In case you want to use a **serif** font, such as Times New Roman, or Georgia, please use `serif`
- In case your font is **sans serif**, such as Arial or Helvetica, please use `sans-serif`
- In case you want to switch to the classic **monospace**, please use the placeholder `monospace`

The placeholder will make sure that even if your font cannot be found, an equivalent font will be used. It serves as a fallback. Also, if your font name contains spaces, make sure to surround it with quotation marks, e.g., `"Times New Roman"`.

You can also to change color in relation to your theme or background. To do that, remove `/*` and `*/` (syntaxe of comment in CSS) and change le value after `#`. RGB value is use here.

```css
body .main-editor-wrapper .cm-editor .cm-scroller {
    font-family: '<your-font-name here>', <placeholder> !important;
    /*color: #492a00; Other option, default value = var(--grey-5)*/
}
```


#### Custom Background Images

With the following code, you can make your editor have a different background image everytime you start it. The images are taken from Unsplash.com, a nice site with free photos. It uses the `Source API`, which will simply spit out a different image every time the URL is visited. You can test it out by simply [visiting the page and refreshing a few times](https://source.unsplash.com/random)! Please refer to the [Unsplash Source API reference](https://source.unsplash.com/) for more options (such as using an image of the day).

!!! tip

    You can also use a local image as a background image by replacing the corresponding line by `background-image: url('file:////absolute/path/to/your/file.jpg');`

```css
/* Enter your custom CSS here */

.main-editor-wrapper .cm-editor{
    background-color: transparent;
    background-image: url('https://source.unsplash.com/random');
    background-size: cover;
    background-position: center center;
}

body .main-editor-wrapper .cm-editor .cm-content{
    background-color: rgba(255, 255, 255, .8);
}
body .main-editor-wrapper .cm-editor{
    background-color: rgba(255, 255, 255, .8);
}
body.dark .main-editor-wrapper .cm-editor .cm-content{
    background-color: rgba(0, 0, 0, .8);
}
```
*Light Mode*
![A preview of a Zettlr installation using above snippet](../img/custom_css_unsplash_light.png)

*Dark Mode*
![A preview of a Zettlr installation using above snippet](../img/custom_css_unsplash_dark.png)


#### Visualising Line Endings

In case you want to see where your linefeeds are, you can display the pilcrow symbol (¶) at the end of your lines by using the following Custom CSS:

```css
.cm-line::after, br {
  content: "¶";
  color: #666;
}
```

![A preview of Zettlr using above snippet](../img/custom_css_pilcrow.png)


#### Change the Active Line Styling in Typewriter Mode

You can change the styling of the active line in Typewriter mode. Replace `top-border-hex-code`, `bottom-border-hex-code` and `background-hex-code` in the CSS snippets below with your preferred Hex colour codes, which you can choose from a website such as [HTML Color Codes](https://htmlcolorcodes.com/). You may want to have different colour styling for light and dark mode.

*Light mode*

```css
body .main-editor-wrapper .cm-editor .cm-content .typewriter-active-line {
  border-top: 2px solid <top-border-hex-code>;
  border-bottom: 2px solid <bottom-border-hex-code>;
  background-color: <background-hex-code>;
}
```

*Dark mode*

```css
body.dark .main-editor-wrapper .cm-editor .cm-content .typewriter-active-line {
  border-top: 2px solid <top-border-hex-code>;
  border-bottom: 2px solid <bottom-border-hex-code>;
  background-color: <background-hex-code>;
}
```


#### Set a maximum width for the text

If you have a large screen, you may find that lines of your text are very long.
If you wish to have shorter lines in the editor, with margins on both sides, you can use the following CSS snippet:

```css
.main-editor-wrapper .cm-editor .cm-scroller {
    --side-margin: calc( 50vw - 30em );
    margin-left: var(--side-margin);
    padding-right: var(--side-margin);
}
```

![A preview of Zettlr using above snippet](../img/custom_css_maxwidth.png)

For the distraction free mode, the CSS snippet needs to be modified as follows:

```css
.main-editor-wrapper.fullscreen .cm-scroller {
    padding: 0 12vw; 
}
```

By adjusting the calc functions for the two different modes, the same line width can be achieved with and without the file manager/sidebar.


#### Change background color

If you want change the background color of the chosen theme you can apply the following code.

```css
body .main-editor-wrapper {
    background-color: #f9f1e9;
}
```

In dark mode, it may be necessary to modify the contrast to avoid the halo effect due to retinal persistence of the eye. To improve that, tune the color of background in relation to the color of font.

```css
body.dark .main-editor-wrapper .cm-scroller {
    background-color: #333333; /*#14141e; default value*/
    /*color: #e3d3be; other option, default value = var(--grey-0)*/
}
```


#### Change the theme color

It is possible to change the color of theme with following code :

:root {
    --c-primary: #2A82AA; /*default value = var(--grey-0)*/
    --c-primary-contrast: white;
    --selection-light: #88D4F7; /*default value = var(--green-selection*/
    --selection-dark: #3EC0F7; /*default value = var(--green-selection-dark)*/
}

!!! warning

    Changing theme colors can be tricky. Indeed, if the colors are poorly chosen, poor contrasts can result. You can use a color wheel to harmonize colors. There are many tools available for this purpose. [Adobe's free color wheel](https://color.adobe.com/create/color-wheel) is a good one.

!!! warning

    Check also the effect of your modification in dark mode.


### Inline Formatting

#### Color of _Emphasis_ Element

To change the color of emphasis element:

```css
body .main-editor-wrapper .cm-editor .cm-emphasis {
    font-style: italic;
    color: #0080ff; /*Blue*/
}
```


#### Color of **Strong** Element

To change the color of strong element:

```css
.cm-editor .cm-strong {
    color: #ff8000 !important; /*Orange*/
    font-weight: bold;
    /*font-weight: normal; other option*/
    /*text-decoration: underline; other option*/
}
```


#### Color of ~~Strikethrough~~ Element

To change the color of strikethrough element:

```css
.cm-editor .cm-strikethrough {
    color: grey !important;
}
```


#### Background Color of ==Mark==

To change background color of mark element.

```css
.cm-editor .cm-highlight {
    background-color: #de8be7; /*#ffff0080 default value*/
}
```


#### Color and Font of `<!--Comment-->`

To change background color of the comment. If your font for normal text is serif, it could be coherent to choose same font for comment.

```css
body .main-editor-wrapper .cm-editor .cm-comment:not(.cm-formatting):not(.cm-fenced-code) {
    background-color: #abe7f4; /*#f0f0f099 default value*/
}

body .main-editor-wrapper .cm-editor .cm-comment {
    font-family: /*<your-font-name here>*/, /*<placeholder>*/, serif;
}
```


#### Color of `[Link](adress)`

To obtain a different color for links than the one defined by the theme. With `hover`, when the mouse is on the link text, this text changes color. With `:active`, when you push on the link text, this text changes color.

```css
body .main-editor-wrapper .cm-editor a.markdown-link, body .main-editor-wrapper .cm-editor .cm-link {
    color: red;
}

body .main-editor-wrapper .cm-editor a.markdown-link:hover, body .main-editor-wrapper .cm-editor .cm-link:hover {
    color: pink;
}

body .main-editor-wrapper .cm-editor a.markdown-link:active, body .main-editor-wrapper .cm-editor .cm-link:active {
    color: purple;
}
```


### Block Formatting

#### Using a Custom Font for Code Block and YAML frontmatter

If you use a serif font and you want to keep monospace font in code block, you can apply the following CSS. Remark, YAML frontmatter use the same CSS name.

```css
body .main-editor-wrapper .cm-editor .code-block-line {
    font-family: /*<your-font-name here>*/ 'Fira Code', /*<placeholder>*/ Consolas, 'Inconsolata', Menlo, monospace
    font-size: 16px !important; /*Option to balance size in relation to normal text.*/
}
```


#### Change the Name of the Frontmatter

If you use the YAML frontmatter, it is possible to change the name after first `---`. For example, you can replace “YAML Frontmatter” by “Metadata” or a transtation in your language. To do that, change the texte after `content:` between the both `"`. You can see some examples of translation below.

```css
body .main-editor-wrapper .cm-editor .cm-yaml-frontmatter-start::after {
    content: "Metadata";
    /*content: "Page Liminaire"; en Français*/
    /*content: "Portada"; en Español*/
    /*content: "Materia prima"; in Italiano*/
    /*content: "Vorderseite"; auf Deutsch*/
}
```


#### Formatting > Quote Block

To change the formatting of the quote block to get better style.

.cm-line:has(.cm-quote.cm-content-span)  {
    display: block; /* Make the span a block element */
    background-color: #e6f7f5;
    border-left: 5px solid #00e3e4;
    /*font-size: smaller; option*/
    /*letter-spacing: 1px; option*/
}

