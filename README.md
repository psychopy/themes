
# How do themes work?
Themes in Psychopy are defined using a `json` file, when the editor in Coder view or the output windows in Runner view style their text, they look in this `json` file to choose what colours, fonts and styles each different type of text object sould be.

# Downloading and installing a theme
The `json` files defining each theme are stored in a folder called `themes`, within your user preferences folder, so you can add a new theme to Psychopy by copying the relevant `json` file to there. The location of this folder may differ depending on your setup, but most often it is in:
`C:\Users\[YOUR LOGIN NAME]\AppData\Roaming\psychopy3`
The folder `AppData` is usually hidden, so make sure your file browser is set up to be able to view hidden folders and files.

Once you have copied the `.json` file into there, provided it is a valid theme, it should appear in both the `View -> Themes...` menu and under `theme` in `Preferences` when you next open Psychopy.

# Contributing
Psychopy is an open source tool, so we welcome contributions to all parts of the software, including coder themes! To contribute:

1. Using Git, create a fork of this project on your own computer
2. Create a new branch from `master`
3. Make your changes (more on this below)
4. Commit changes to your branch
5. Push commits
6. Submit a pull request for your branch on this repository

## Adding a new theme or theme set
Within this repository is a folder called "Themes" - this is where each theme or theme set is stored in its own folder. To create a new theme or theme set, first create a folder with the name you would like.

Within this folder, there are 3 files which must exist for the theme to be valid:
- A `.json` file specifying the colours, fonts and styles of different text elements when using your theme. If you are creating a set of themes rather than just one, then you will need a `.json` file for each individual theme in the set.
- A file called `README.md`, introducing your theme and providing any information which users might need
- A file called `authent.txt`, which our GitHub bots will use to make sure that only you can edit your theme once submitted.

### Creating `authent.txt`
Protecting your theme is relatively straightforward! When you submit a pull request, if you have edited any files within existing folders, our bot will check your username against whatever is stored in that folder's `authent.txt` file. Meaning that all you have to do to protect your theme is create a file called `authent.txt` with your GitHub username in, and no one but you will be able to edit it! If you would like anyone else to be able to edit your theme, just add their username on a new line.

### Creating `README.md`
While a `README` file isn't always necessary for a font, it is a great opportunity to introduce users to your theme and give then a walkthrough of what everything looks like. Using image embedding you can even include screenshots to give a really good idea. If you don't feel your theme needs any introduction, however, this file can be blank. It just has to exist!

### Creating the `.json` file or files
Each theme in a set is specified by a `.json` file, with the name of the theme in `CamelCase` (no spaces, first letter of each word capitalised). The `.json` file is structured such that each object type is a key/value pair, where the value is three further key/value pairs called `fg`, `bg` and `font`. These specify the foreground colour, background colour and font style respectively. The two exceptions to the rule are `app` and `icons`, which determine how the rest of the Psychopy interface, outside of the code editor, should be styled: In light mode or dark mode and with classic or modern icons. Each is just a single string (`"light"` or `"dark"` for `app`, `"classic"` or `"modern"` for `icons`).

All theme files will begin, after `app` and `icons`, with a specification for `base` text - this is how text which is not recognised as any other type is styled. For example, the first key/value pair in the `PsychopyLight` theme is:
```json
  "base": {
    "fg": "#000000",
    "bg": "#FFFFFF",
    "font": ["Consolas", "Monaco", "Lucida Console"]
  }
```
Meaning that the base text will be black, on a white background, in the font first available font in the list with no additional styling. This `base` key is the minimum required for a theme to be read by Psychopy, in the absence of any other keys it will simply style all text as `base`. You can use the following keys to style further object types:

- `margin`: The margin to the left of the code editor
- `caret`: The caret/cursor
- `select`: Selected text
- `indent`: Vertical lines indicating the horizontal location of each indent
- `brace`: Unclosed braces
- `operator`: Mathematical operators such as =, + or *
- `keyword`: Keywords for the current language, such as `import`, `class` or `def` in Python
- `keyword2`: Secondary keywords for the current language, such as `self` in Python
- `controlchar`: Control characters, these are not printable characters and so are used in engineering to initiate certain actions
- `id`: The names of variables or other objects
- `num`: Numeric values such as integers or floats
- `char`: Character arrays, usually denoted by 'single quotes'
- `str`: String objects, usually denoted by "double quotes"
- `openstr`: Unclosed string objects
- `infix`: Infix functions in R
- `openinfix`: Unclosed infix functions
- `decorator`: Decorators in Python, denoted by an `@`
- `class`: Class names
- `def`: Function names
- `comment`: Comments
- `commentblock`: Blocks of comments
- `commentkw`: Keywords which appear within comments
- `commenterror`: Errors which appear within comments
- `documentation`: Documentation comments, usually denoted by '''triple single quotes'''
- `documentation2`: A higher level of documentation comments, usually denoted by """triple double quotes"""
- `whitespace`: The dots appearing before a line of text, denoting how many spaces it is from the line's origin

Within each, you can apply styles by using the same three key/value pairs as in `base`: `bg`, `fg` and `font`. `bg` and `fg` look for hexcode values denoting colour, `font` looks for a string with the name of a monospaced font.  The `font` parameter can also be a list, if it is then Psychopy will first look for the keywords `italic` or `bold` to determine font styling, then will accept the first font name in the list which is installed on your computer. All font lists will automatically have `["Consolas", "Monaco", "Lucida Console"]` added to the end, meaning that if no valid fonts are in those specified, Psychopy will use a font which is standard on Windows, Mac and Linux respectively. So for example, the following uses a non-standard font in bold and italic:

```json
"keyword2": {
    "fg": "#02A9EA",
    "bg": "",
    "font": ["Inconsolata", italic","bold"]
  }
```
If `Inconsolata` is installed, text will display in this font, if not, it will default to `Consolas` on Windows, `Monaco` on Mac, or `Lucida Console` on Linux. If you would like any trait to use the same as in `base`, you can use empty double quotes (`""`) as shorthand. For example:
```json
"id": {
    "fg": "",
    "bg": "",
    "font": ""
  }
```
would mean that `identifiers` will be styled using the foreground colour, background colour, font and style specified in `base`.

Once you have styles defined for all the keys you want, you can also add the keys `python`, `r`, and/or `c++` to specify language-specific styling. This will override the basic styling.

### App and icons

You may notice that the first keys in most spec files are `app` and then `icons` - these correspond to the Psychopy interface itself. For `app`, the value can be one of two options: `light` or `dark`, referring to the light and dark colour schemes currently available for Psychopy. For `icons`, there are three options: `light`, `dark`, or `classic`, referring to the three possible icon sets available.

In time we hope to add functionality for users to contribute custom colour schemes and icon sets for the app, similar to how this repository.

## Editing an existing theme
Only the original author of a theme can edit it, as our bot will check the username on any pull requests against the `authent.txt` file of any folders changed. However, you can make changes to any of your themes by just creating a branch of changes and then submitting a pull request, just like you will have when creating a theme. If you wish to edit someone else's theme, you will need to ask them to add your username to their `authent.txt` file, or create a new theme based on theirs and submit it yourself. If you created a file and now cannot get access, please contact todd.parsons@nottingham.ac.uk.