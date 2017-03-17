<!--- @file
  5 Font Support

  Copyright (c) 2016-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

# 5 Font Support {#5-font-support}

This chapter defines the optional attributes and entries in EDK II Unicode
files to support font selection.

### Syntax

The following sections describe extensions to the .uni format.

The extensions add support for fonts into the .uni format by introducing the
`#fontdef` and `#font` commands, extending the `#string` command and adding new
escape characters into the strings.

### Fonts

Every string is associated with a font. Each font has a font identifier, a font
name, a size (in pixel height) and a style (normal, bold, etc.). By default,
strings will be associated with the font identifier `sysdefault`. Usually this
is associated with the font sysdefault, 19, normal (the standard UEFI font).

The default font associated with strings can be changed from sysdefault to
another font identifier using the `#font` command. All strings after the
`#font` command will use the specified font identifier.

Strings can use a different `#font` identifier by using the font attribute of
the `#string` command (before the first `#language` attribute). A string in a
specific language can use a different font identifier by using the `#font`
attribute _after_ the language attribute.

Characters within a string can use a different font identifier, a different
font size or a different font style by using the `\f` escape sequences
described below. These escape characters extend those described in the _EDK2
Build Specification_.

###### Table 1 .uni File Font Escape Characters{#table-1-uni-file-font-escape-characters}

| Font Control Character | Description                                                                                                       |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------- |
| \"                     | Insert a double-quote.                                                                                            |
| `\\`                   | Insert a single backslash.                                                                                        |
| \br                    | Breaking code.                                                                                                    |
| \f!identifier!         | Select the font identifier for the characters which follow.                                                       |
| \fb                    | Toggle the current bold style for characters that follow in the current string.                                   |
| \fd                    | Toggle the current double-underline style. If the current style is underline, the style becomes double-underline. |
| \fe                    | Toggle the current emboss style for the characters that follow.                                                   |
| \fh!integer!           | Select the font size (in pixels) for the characters that follow.                                                  |
| \fi                    | Toggle the current italic style for the characters that follow in the current string.                             |
| \fs                    | Toggle the current shadow style for the characters that follow in the current string.                             |
| \fu                    | Toggle the current underline style for the characters that follow in the current string.                          |
| \n                     | Insert a carriage-return and line-feed.                                                                           |
| \narrow                | Display the following characters as "narrow" characters.                                                          |
| \nbr                   | Non-breaking code.                                                                                                |
| \r                     | Insert a carriage-return.                                                                                         |
| \wide                  | Display the following characters as "wide" characters                                                             |

Font identifiers are created by using the `#fontdef`.

## 5.1 #font {#5-1-font}

Set the default font to use with all subsequent `#strings`.

### Syntax

`"#font" <MS> font-identifier`

### Attributes

**_font-identifier_**

C style identifier associated with the font.

## 5.2 #fontdef {#5-2-fontdef}

Associated a font identifier with a specific font family, size and style.

### Syntax

```c
"#fontdef" <MS> _font-identifier_ <MS> <FontOptions> <EOL>

<FontOptions>   ::= font-name <MS> font-size [<MS> font-style-list]
font-style-list ::= <UDblQuote> [fs-entries] <UDblQuote>
fs-entries      ::= font-style ["|" font-style]*
font-style      ::= {"bold"} {"italic"} {"underline"} {"dblunder"}
                    {"shadow"} {"emboss"} {"normal"}
font-size       ::= (1-9) (0-9)*
```

### Attributes

**_font-identifier_**

C-style identifier.

**_font-name_**

Quoted string that specifies a font family name. For example, "Arial" or
"Times New Roman"

**_font-size_**

Unsigned integer that specifies the height of the font character cell, in
pixels. For example, the UEFI standard font is size 19 because the cell is
19 pixels high.

**_font-style_**

Quoted string that contains zero or more keywords that specify the font
style, separated by a "|". If "normal" is used, then it may not be combined
with any other font style. If there is no font style specified, then "normal"
is assumed.

## 5.3 #string Extensions {#5-3-string-extensions}

The EDK II build command is responsible for parsing the .uni files specified
in INF files' `[Sources]` sections. The tool uses python objects to convert the
syntax in the HII string files to byte arrays in the AutoGen.c file for each
module.

Refer to the EDK II Build Specification for details on the process of creating
the byte arrays.

### Syntax

```c
<UnicodeLines> ::= "#string" <MS> <Identifier> <ME>
                   [<FontId>]
                   [<LangLine>]+
<LangLine>     ::= "#language" <MS> lang-code <ME> <FontString>
<FontString>   ::= [<FontId>] [<strings>]+
<FontId>       ::= ["#font" <MS> font-identifier> <ME>]
<strings>      ::= <String> <ME>
```

### [Extension to #string command in the EDK2 Build Specification]

The font attribute specifies the default font that will be used for the
characters in string. If `#font` is not specified, then the default font
identifier will be used.

If the `#font` attribute appears before the first `#language` identifier, then
it applies to all characters for all languages. If the `#font` attribute
appears after a `#language` identifier, it applies only to the string
characters in that language. It is permissible for `#font` to appear in more
than one place, in which case the language-specific font identifier will have
priority.

#### Description

The `#fontdef` command introduces a font identifier and associates it with a
font of a particular family, size and style. If the font identifier has been
previously defined, then the new definition is ignored.
