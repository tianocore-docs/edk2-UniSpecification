<!--- @file
  2 Unicode Strings File Format

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

# 2 Unicode Strings File Format {#2-unicode-strings-file-format}

EDK II Unicode files are used for mapping token names to localized strings that
are identified by an RFC4646 language code. The format for storing EDK II
Unicode files is UTF-16LE. The character content must be UCS-2.

Strings ends are determined by the first of the following items found:

* a control character
* a comment
* the end of the file
* a blank line

Comments may appear anywhere within the string file.

All the files must begin with a Unicode BOM character.

**********
**NOTE:** Please make sure you select an editor that supports UCS-2 characters
that can be stored in a UTF-16LE file.
**********

## 2.1 Common EBNF {#2-1-common-ebnf}

The following EBNF uses quoted (double quotes) encapsulated characters to
represent UCS-2 string literals. In the following definitions, the semi-colon
is used to denote a comment.

```c
<US>           ::= " "
<Letter>       ::= {(\u0041-\u005A)} ; Characters A - Z
                   {(\u0061-\u007A)} ; Characters a - z
<Digit>        ::= (\u0030-\u0039)   ; Characters 0 - 9
<MS>           ::= <US>+
<ME>           ::= {<MS>} {<EOL>}
<CommentLine>  ::= "//" <US>* <PCHars> <EOL>
<BlankLine>    ::= <EOL>
<Chars>        ::= (\u0001-\uF6FF)
<PChars>       ::= {(\u0020-\uF6FF)} {<OpChar>}
<OpChars>      ::= "\x" [{<Letter>} {<Digit>}]{4} "\"
<VChars>       ::= (\u0021-\uF6FF)
<UnicodeLines> ::= <Token> <ME>
                   [<Ldef> [<String> <ME>]+]+
<Ldef>         ::= <CtrlChar> "language" <MS> <LangCode> <ME>
<HexDigit>     ::= {<Digit>}
                   {(\u0041-\u0046)} ; Characters A - F
                   {(\u0061-\u0066)} ; Characters a - f
<CtrlChar>     ::= <US>* "#"
<Token>        ::= <CtrlChar> "string" <MS> <Identifier>
<Identifier>   ::= <Letter> [{<Letter>} {<Digit>} {<UN>}]*
<LangCode>     ::= <RFC4646>
<RFC4646>      ::= <Letter>{2,8} [<ShortExt> <LongExt>*]
<ShortExt>     ::= "-" [{<Letter>} {<Digit>}]{1,8}
<LongExt>      ::= "-" [{<Letter>} {<Digit>}]{1,}
<UDblQuote>    ::= \u0022  ; Double Quote Character, "
<String>       ::= <UDblQuote> <SContent>* <UDblQuote>
<SContent>     ::= {<PChars>} {<Attributes>}
<Attributes>   ::= "\" {"narrow"} {"wide"} {<UDblQuote>} 
                   {"n"} {"r"} {"t"} {"nbr"} {"\"} {"'"}
```

### 2.1.1 Definitions {#2-1-1-definitions}

**_LanguageCodes_**

The language code must be a valid RFC4646 language code.

**_EscChar_**

In order to include some standard characters, such as the "\" back-slash
character within a string, the character must be prefixed with the escape
character.  Characters that may require a prefixed escape character include 
the following, back slash "\" character, single-quote "'" character, 
double-quote '"' character and the forward slash "/" character. The back slash
always requires the escape character.

**_Token_**

The token (strong identifier) may only contain numbers, upper and lower case
letters, underscore character, and dash character.

**_Include_**

An include line is used to parse another file, also compliant with this
specification, as if it was in the file. The tokens should not overlap between
the file for the same language.
