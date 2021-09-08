# Version 2.0.0 2021-09-03

## Backwards-incompatible changes:

- The title of a chapter is now defined with a separate node following the `ch` node.
The title can no longer be defined with an attribute.
Instead of writing:
```
[ch (title="My Title")
    text
]
```
... we have to write:
```
[ch [title My Title]
    text
]
```
Note: While the 'title' attribute was limited to plain text, any inline markup can now be used in 'title' nodes, e.g.
```
[title A [i nice] side effect]
```

- The following attributes have been removed from the 'doc' (document) node: title, authors, date
Instead of a 'title' attribute, a 'title' node must be used (as for chapters), e.g.
```
[doc [title My Title]
    text
]
```
Instead of 'authors' and 'date' attributes, these information can simply be written as text, e.g.:
```
[doc [title My Title]
    Author: your name

    Published: 2021-09-03
]
```
      In a future version there will be a dedicated 'meta' node that will hold data like
      author, date, version, abstract, etc., as well as other, user-defined meta-data

    - The 'title' attribute has been removed from all nodes
      Please refer to the previous two items for 'doc' and 'ch' nodes.
      For all other nodes, the new 'header' node (see below) should be used instead of the old 'title' attribute.
      Example:
      Old code:
        [el (title = "List element header")
            ...
        ]
      New code:
        [el [header List element header]
            ...
        ]

    - Lenient parsing is now supported differently:
        - Attributes must be enclosed in parentheses, except for nodes that have only attributes (no child-nodes)
          Example: [foo (a1=v1 a2=v2) ... ]
        - Attribute values that contain spaces must be enclosed in quotes
          Example: path = "name with spaces.png"
        For more information please refer to the user manual:
        https://www.pml-lang.dev/docs/user_manual/index.html#lenient_parsing
    
    - The syntax for using constants has changed
      
      The syntax for declaring a constant changed from:
      [const name = value]
      ... to:
      [!set name = value]
      
      Using a constant changed from:
      <<value>>
      ... to:
      [!get name]
      For more information please refer to the user manual:
      https://www.pml-lang.dev/docs/user_manual/index.html#parameters

    - The syntax for inserting a file has changed from:
      [insert file=sub-chapter.pml]
      ... to:
      [!ins-file path=sub-chapter.pml]
      For more information please refer to the user manual:
      https://www.pml-lang.dev/docs/user_manual/index.html#file_splitting

      (Note: 'ins-url' and 'ins-env' (OS environment variable) will be added in a future version)
          
    - Command line parameter 'tag_start_stop_symbols' has been removed. A node must start with '[', and end with ']'.
    
- New features:

    - Better error handling:
        - New parameter 'open_file_cmd' for command 'convert'.
          This argument enables you to automatically open an editor for the first file in which an error was detected.
          For more information, type the following command in a terminal, and look for parameter 'open_file_cmd':
          pmlc command_info -command convert
        - Better format for error messages displayed in the terminal.
        - If there are several errors, they are all displayed in the terminal (not just the first one).
        - Distinction between canceling errors, non-canceling errors, and warnings.
        - Customizable error handler (by providing a specific Java class)

    - New block nodes:
        - 'title': A title for a chapter (can include markup, included in the table of contents)
          This node replaces the chapter's 'title' attribute, which is no more supported.
        - 'subtitle': A subtitle for a chapter (can include markup, not included in the table of contents)
        - 'header': A header (small title) above text (can include markup, not included in the table of contents)
        Note: Inline markup can be used in 'title', 'subtitle', and 'header' nodes, e.g.
        [title A [i nice] side effect]
        Please refer to the reference manual for more information:
        https://www.pml-lang.dev/docs/reference_manual/

    - More powerful text parameters
      Text parameters are no more limited to text snippets. They can define markup code to be used repetitively.
      Example:
      [!set reused_image="[image source=my-image.png width=600 height=400]"]
      ...
      [!get reused_image]
      ...
      [!get reused_image]
      For more information please refer to the user manual:
      https://www.pml-lang.dev/docs/user_manual/index.html#parameters

- Bug fixes:
    - Some minor bugs have been fixed.

- Improvements:
    
    - The previous PML parser (written in PPL) has been replaced by the pXML parser (written in Java).
      This important change makes PML totally compatible with pXML, and provides a powerful foundation for
      exciting new features in the future, such as:
          - converting PML to XML, or XML to PML/HTML
          - using XML technology for PML documents (e.g. querying, modifying, transforming and validating a PML AST)
          - using new, future pXML features in PML too
            (e.g. creating standalone documents, inserting PML snippets retrieved from a URL, etc.)
      For more information on pXML please visit:
      https://pxml-lang.github.io
    
    - User manual (www.pml-lang.dev/docs/user_manual):
        - Updated to the changes in version 2.0.0
        - Chapter 'Text processing' has been completely rewritten, and the following chapters have been added:
            - Lenient Parsing
            - Whitespace Handling
            - Escape Characters
          


Version 1.5.0 2021-06-08
========================

- New features:

    - New inline nodes:
        - sub: Subscript text
        - sup: Superscript text
        - strike: Strikethrough text


Version 1.4.0 2021-04-16
========================

- Backwards-incompatible changes:

    - Each tag and each attribute have one single identifier. Alternative identifiers are no more supported.
      This change makes PML documents look more uniform and reduces complexity for editor plugins, tools, etc.
      Please consult PML's reference manual if you have PML documents created with a previous version and which use
      identifiers that are no more valid anymore.

- New features:
    - Command 'export_tags' has been added.
      This command creates a JSON file containing structured data about PML tags and attributes.
      The JSON file can be used by editor plugins and tools that depend on PML tags and attributes.
      For more information type 'pmlc command_info -command export_tags' in a terminal.
      
    - The PML user manual and reference manual are now deployed into directory 'docs' of the PML installation directory.

- Bug fixes
    - The table of contents is now displayed correctly on devices with small screens.


Version 1.3.0 2021-03-09
========================

- Backwards-incompatible changes:

    - The tag's open/close symbols have changed from {} to []
        Example:
        old version: {i great}
        new version: [i great]
        If you want to keep {} you can run the converter with the parameter --tag_start_stop_symbols "{}"

    - The open/close symbols for embedding variables have changed from [[]] to <<>>
        Example:
        old version: [[name]]
        new version: <<name>>

- New features:

    - A new CSS file, named pml-print-default.css, has been added.
      This file has specific styles defined for documents printed in a browser.
      These styles are also applied when a document is sent to a PDF writer, such as 'Microsoft Print to PDF' in Windows.
      You can use CSS's full printing support to adapt printed document to your specific needs,
      such as printing two columns on a page.

    - New attributes:
        - Tag 'document': attribute 'TOC_title' has been added.
          It defines the title displayed for the table of contents.
          The default value is "Table of Contents".

    - New command line arguments:
        - Argument 'tag_start_stop_symbols' has been added.
          It allows you to define which symbols you want to use to start/stop tags.
          Currently supported values are {} and []

- Improvements:

    - The Javascript code for expanding/collapsing the table of contents has been removed.
      The HTML <details> tag is used instead.

    - The Javascript code for sliding the separator between the table of contents and the text has been removed.


Version 1.2.0 2021-02-10
========================

- New features:

    - The PML_to_HTML_Converter is now open-sourced under the GPL 2.
    - Development is supported on Linux, macOS, and Windows.
    - Support for the Gradle build tool has been added.
    - The 'prism' source code syntax highlighter is now supported, as an alternative to 'lightjs'.
      Refer to the documentation of nodes 'document', 'code', and 'insert_code' for more information.


Version 1.1.0 2021-01-14
========================

- New features:

    - New tags:
        - table_data: Simple table data defined as plain CSV text, and rendered as a table.
        - table: A table layout composed of rows and columns.
          Besides just text, each cell in the table can have complex content, including nested tables.
          Conceptually similar to a HTML table.
        - monospace: A paragraph in which whitespace is preserved, and a monospace font is used.
        - division: A division or section in the document. Similar to a 'div' tag in HTML.
        - span: An inline node typically used to render a HTML <span>...</span> inline element with a specific set of
          HTML attributes.

    - New attributes:
        - caption: a small text displayed below the node. Available for the following tags:
          image, audio, video, youtube_video, and table

    - New command line arguments:
        - Added command line arguments 'HTML_header' and 'HTML_footer' to the PML_to_HTML_Converter. Allows you to:
            - Provide a custom HTML header/footer for each document, or generate only the HTML body code (no header/footer).
            - Customize the style of the resulting HTML by providing your specific CSS file(s), in addition to PML's
              default CSS file, or as a replacement.

- Some minor bug fixes