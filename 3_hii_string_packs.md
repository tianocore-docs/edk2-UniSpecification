<!--- @file
  3 HII String Packs

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

# 3 HII String Packs

Unicode files used for creating HII String Packs have the following format:

```c
<StringFileFormat> ::= <CommentLine>*
                       <LanguageDefs>
                       <Content>+
```

The following EBNF describes content is specific to the Unicode files used for
generating HII String Packs.

```c
<Content> ::= {<CommentLine>} {<BlankLine>}
              {<UnicodeLines>} {<ControlRefactor>}
              {<LanguageDefs>} {<SecurityLines>}
              {<IncludeLines>}
```

Additional Definitions used for Unicode files used to create HII String Packs.

```c
<LanguageDefs>    ::= <CtrlChar> "langdef" <MS> <LangCode> <MS>
                      <LangDesc> <EOL>
<LangDesc>        ::= <UDblQuote> <Chars> <UDblQuote>
<IncludeLines>    ::= <CtrlChar> "include" <UniFile> <EOL>
<UniFile>         ::= <UDblQuote> <UniFilename> <UDblQuote>
<UniFilename>     ::= <FilenameChars> <MoreFNameChars>* {".uni"} {".UNI"}
<FilenameChars>   ::= {<Letter>} {<Digit>}
<MoreFNameChars>  ::= {<Letter>} {<Digit>} {"_"}
<CtrlChar>        ::= "/"
<ControlRefactor> ::= <CtrlChar> "=" <NewCtrlChar> <EOL>
<NewCtrlChar>     ::= (0x0021 - 0xF6FF)
 ```

**********
**NOTE:** Unicode files that are used for generating HII String Packs are the
only type of Unicode file that allows for refactoring the control character
(providing backward compatibility), `<CtrlChar>`.
**********

## 3.1 Example file

```c
//
// Cpu I/O Strings
//
// Copyright (c) 2006, Intel Corporation. All rights reserved.<BR>
//
// This program and the accompanying materials are licensed and made
// available under the terms and conditions of the BSD License which
// accompanies this distribution. The full text of the license may
// be found at:
//    http://opensource.org/licenses/bsd-license.php
//
// THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
// WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS
// OR IMPLIED.
//

/= #
#langdef  en-US  "English, US"
#langdef  fr-FR  "Français"

#string STR_PROCESSOR_VERSION
#language en-US "NT32 Emulated Processor"
#language fr-FR "Processeur Émulé par NT32"
```
