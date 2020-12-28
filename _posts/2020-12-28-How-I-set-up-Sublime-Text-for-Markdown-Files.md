
### What is Markdown?
Markdown is a **lightweight markup language** and uses a **plain-text-formatting** syntax. Markdown is often **used to format readme files** and can create rich text using a plain text editor. <br/>  

Its design allows it to be converted to many output formats and I often convert markdown documentation to Word or PDF formats.
<br/>  

# Packages
Packages extend Sublime Text's functionality significantly and, with the help of Package Control, are very easy to install.
<br/>  

I use these packages to help me write markdown documentation:
- MarkdownEditing
- MarkdownTableFormatter
- MarkdownPreview
- Pandoc

<br>

### Installing Package Control
Package Control makes it easy to find, install, and update packages in Sublime Text.

- Open the command palette: <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> 
- Type ```Install Package Control```
- Press <kbd>Enter</kbd>  
<br/>  

### MarkdownEditing
> Markdown plugin for Sublime Text. Provides a decent Markdown color scheme (light and dark) with more robust syntax highlighting and useful Markdown editing features for Sublime Text.  

https://github.com/SublimeText-Markdown/MarkdownEditing

- Open Package Control's Command Pallet: <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> 
- Type ```install package``` and press <kbd>Enter</kbd>.  A list of available packages will be displayed.
- Type ```MarkdownEditing``` and press <kbd>Enter</kbd> again.
- Restart Sublime Text 3
<br/>

### WYSIWYG with MarkdownPreview
> Preview and build your markdown files quickly in your web browser using Sublime Text 3

https://github.com/facelessuser/MarkdownPreview

- Open Package Control's Command Pallet: <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> 
- Type ```install package``` and press <kbd>Enter</kbd>.  A list of available packages will be displayed.
- Type ```MarkdownPreview``` and press <kbd>Enter</kbd> again.
- Restart Sublime Text 3

<br/>  

### Converting Markdown to Other File Types
Markdown can be easily converted to other formats and I find this usesful for sharing documents with other users.

I use Pandoc which needs to be installed outside of Sublime. You can find the installation steps here: <br> https://pandoc.org/installing.html

Once installed you can add the Pandoc package to Sublime.

- Open Package Control's Command Pallet: <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> 
- Type ```install package``` and press <kbd>Enter</kbd>.  A list of available packages will be displayed.
- Type ```Pandoc``` and press <kbd>Enter</kbd> again.

To use the Pandoc package:
- Open Package Control's Command Pallet: <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> 
- Type ```Pandoc``` and press <kbd>Enter</kbd>. 
- Pick the exported file type from the list.
- Your file will open in your default viewer for that file type.

