<!--- @file
  1 Introduction

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

# 1 Introduction {#1-introduction}

This document describes file format for Unicode string files. This file format
supports multiple layouts and formats in the Unicode file. This versatility
allows strings to be grouped either by language or by string identifier.

## 1.1 Related Information {#1-1-related-information}

The following publications and sources of information may be useful to you or
are referred to by this specification:

* _Extensible Firmware Interface Specification_, Version 1.10, Intel, 2001,
  http://developer.intel.com/technology/efi.

* _Unified Extensible Firmware Interface Specification_, Version 2.0, Unified
  EFI, Inc., 2006, http://www.uefi.org.

* _Unified Extensible Firmware Interface Specification_, Version 2.1, Unified
  EFI, Inc., 2007, http://www.uefi.org.

* _Intel(R) Platform Innovation Framework for EFI Specifications_, Intel, 2006,
  http://www.intel.com/technology/framework.

* _EDK II Module Development Environment Package Specification_, Version 0.51,
  Intel, 2006, https://edk2.tianocore.org/source/browse/edk2/trunk/docs

* _EDK II Build and Packaging Architecture Specification_, Version 0.53, Intel,
  2006, https://edk2.tianocore.org/source/browse/edk2/trunk/docs.

* _EDK II Module Surface Area Specification_, Version 0.51, Intel, 2006,
  https://edk2.tianocore.org/source/browse/edk2/trunk/docs

* _EDK II Module Development Environment Library Specification_, Version 0.58, 
  Intel, 2006, https://edk2.tianocore.org/servlets/ProjectDocumentList?folderID=82&H expandFolder=82&folderID=0.H

* _EDK II Platform Configuration Database Infrastructure Description_, 
  Version 0.54, Intel, 2006, https://edk2.tianocore.org/source/browse/edk2/trunk/docs

* _EDK II C Coding Standards Specification_, Version 0.51, Intel, 2006, 
  https://edk2.tianocore.org/source/browse/edk2/trunk/docs

## 1.2 Terms {#1-2-terms}

The following terms are used throughout this document to describe varying
aspects of input localization:

**BDS**

Framework Boot Device Selection phase.

**BNF**

BNF is an acronym for "Backus Naur Form." John Backus and Peter Naur introduced
for the first time a formal notation to describe the syntax of a given language.

**Component**

An executable image. Components defined in this specification support one of
the defined module types.

**DXE SAL**

Framework Driver Execution Environment phase. A special class of DXE module that
produces SAL Runtime Services. DXE SAL modules differ from DXE Runtime modules 
in that the DXE Runtime modules support Virtual mode OS calls at OS runtime and
DXE SAL modules support intermixing Virtual or Physical mode OS calls.

**DXE SMM**

A special class of DXE module that is loaded into the System Management Mode
memory.

**DXE Runtime**

Special class of DXE module that provides Runtime Services

**EFI**

Generic term that refers to one of the versions of the EFI specification: EFI
1.02, EFI 1.10, or UEFI 2.0.

**EFI 1.10 Specification**

Intel Corporation published the Extensible Firmware Interface Specification.
Intel donated the EFI specification to the Unified EFI Forum, and the UEFI now
owns future updates of the EFI specification. See UEFI Specifications.

**Foundation**

The set of code and interfaces that glue implementations of EFI together.

**Framework**

Intel(R) Platform Innovation Framework for EFI consists of the Foundation, plus
other modular components that characterize the portability surface for modular
components designed to work on any implementation of the Tiano architecture.

**GUID**

Globally Unique Identifier. A 128-bit value used to name entities uniquely. An
individual without the help of a centralized authority can generate a unique
GUID. This allows the generation of names that will never conflict, even among
multiple, unrelated parties.

**HII**

Human Interface Infrastructure. This generally refers to the database that
contains string, font, and IFR information along with other pieces that use one
of the database components.

**IFR**

Internal Forms Representation. This is the binary encoding that is used for the
representation of user interface pages.

**Library Class**

A library class defines the API or interface set for a library. The consumer
of the library is coded to the library class definition. Library classes are
defined via a library class .h file that is published by a package. See the EDK
2.0 Module Development Environment Library Specification for a list of
libraries defined in this package.

**Library Instance**

An implementation of one or more library classes. See the _EDK 2.0 Module
Development Environment Library Specification_ for a list of library defined in
this package.

**Module**

A module is either an executable image or a library instance. For a list of
module types supported by this package, see module type.

**Module Type**

All libraries and components belong to one of the following module types:
`BASE`, `SEC`, `PEI_CORE`, `PEIM`, `DXE_CORE`, `DXE_DRIVER`,
`DXE_RUNTIME_DRIVER`, `DXE_SMM_DRIVER`, `DXE_SAL_DRIVER`, `UEFI_DRIVER`, or
`UEFI_APPLICATION`. These definitions provide a framework that is consistent 
with a similar set of requirements. A module that is of module type `BASE`,
depends only on headers and libraries provided in the MDE, while a module
that is of module type `DXE_DRIVER` depends on common DXE components. For a
definition of the various module types, see module type.

**Module Surface Area (MSA)**

The MSA is an XML description of how the module is coded. The MSA contains
information about the different construction options for the module. After the
module is constructed the MSA can describe the interoperability requirements of
a module.

**Package**

A package is a container. It can hold a collection of files for any given set
of modules. Packages may be described as one of the following types of modules:

* Source modules, containing all source files and descriptions of a module

* Binary modules, containing EFI Sections or a Framework File System and a
  description file specific to linking and binary editing of features and
  attributes specified in a Platform Configuration Database (PCD,)

* Mixed modules, with both binary and source modules

Multiple modules can be combined into a package, and multiple packages can be
combined into a single package.

**Protocol**

An API named by a GUID as defined by the EFI specification.

**PCD**

Platform Configuration Database.

**PEI**

Pre-EFI Initialization Phase.

**PPI**

A PEIM-to-PEIM Interface that is named by a GUID as defined by the PEI CIS.

**SAL**

System Abstraction Layer. A firmware interface specification used on Intel(R)
Itanium(R) Processor based systems.

**Runtime Services**

Interfaces that provide access to underlying platform-specific hardware that
might be useful during OS runtime, such as time and date services. These
services become active during the boot process but also persist after the OS
loader terminates boot services.

**SEC**

Security Phase is the code in the Framework that contains the processor reset
vector and launches PEI. This phase is separate from PEI because some security
schemes require ownership of the reset vector.

**UEFI Application**

An application that follows the UEFI specification. The only difference between
a UEFI application and a UEFI driver is that an application is unloaded from
memory when it exits regardless of return status, while a driver that returns a
successful return status is not unloaded when its entry point exits.

**UEFI Driver**

A driver that follows the UEFI specification.

**UEFI Specification Version 2.0**

Current version of the EFI specification released by the Unified EFI Forum.
This specification builds on the EFI 1.10 specification and transfers ownership
of the EFI specification from Intel to a non-profit, industry trade
organization.

**Unified EFI Forum**

A non-profit collaborative trade organization formed to promote and manage the
UEFI standard. For more information, see http://www.uefi.org

## 1.3 Conventions used in this document {#1-3-conventions-used-in-this-document}

This document uses the typographic and illustrative conventions described below.

### 1.3.1 Pseudo-code conventions {#1-3-1-pseudo-code-conventions}

Pseudo code is presented to describe algorithms in a more concise form. None of
the algorithms in this document are intended to be compiled directly. The code
is presented at a level corresponding to the surrounding text.

In describing variables, a list is an unordered collection of homogeneous
objects. A queue is an ordered list of homogeneous objects. Unless otherwise
noted, the ordering is assumed to be First In First Out (FIFO).

Pseudo code is presented in a C-like format, using C conventions where
appropriate. The coding style, particularly the indentation style, is used for
readability and does not necessarily comply with an implementation of the
Extensible Firmware Interface Specification.

### 1.3.2 Typographic conventions {#1-3-2-typographic-conventions}

This document uses the typographic and illustrative conventions described below:

| Convention            | Description                                                                                                                                                                                                                                                                            |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Plain text            | The normal text typeface is used for the vast majority of the descriptive text in a specification.                                                                                                                                                                             |
| [Plain text (blue)]() | Any plain text that is underlined and in blue indicates an active link to the cross-reference. Click on the word to follow the hyperlink. USE ONLY IF YOU MAKE AN ACTUAL CROSS-REFERENCE LINK.                                                                                 |
| **Bold**              | In text, a Bold typeface identifies a processor register name. In other instances, a Bold typeface can be used as a running head within a paragraph. or as a definition heading (GlossTerm)                                                                                    |
| _Italic_              | In text, an Italic typeface can be used as emphasis to introduce a new term or to indicate a manual or specification name.                                                                                                                                                     |
| `Monospace`           | Computer code, example code segments, and all prototype code segments use a `BOLD Monospace` typeface with a dark red color. These code listings normally appear in one or more separate paragraphs, though words or segments can also be embedded in a normal text paragraph. |
| [`Monospace (blue)`]()| Words in a `Monospace` typeface that is underlined and in blue indicate an active hyperlink to the code definition for that function or type definition. Click on the word to follow the hyperlink. USE ONLY IF YOU MAKE AN ACTUAL HYPERLINK.                                  |
| **_Italic Bold_**     | In code or in text, words in Italic Monospace indicate placeholder names for variable information that must be supplied (i.e., arguments).                                                                                                                                     |
