# SPDX Tutorial

Software Package Data Exchange (SPDX) is a standard format for communicating the components, licenses,
and copyrights associated with software packages.
SPDX is easy to use, and SPDX makes it easy for all software recipients to know what they're allowed to do (and not do).

This is a brief tutorial about SPDX.
To use SPDX you need to understand three basic constructs: SPDX license identifiers, SPDX license expressions, and SPDX files.

## SPDX license identifiers

SPDX license identifiers are the heart of SPDX.
A license identifier is a human readable short text string that uniquely identifies a license.
Here are some SPDX license identifiers (in alphabetical order) for some widely-used
[free](http://www.gnu.org/philosophy/free-sw.en.html)/libre/[open source software](https://opensource.org/osd-annotated)
licenses, followed by what the identifiers mean:

* Apache-2.0 : [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html)
* BSD-2-Clause : [BSD 2-clause "Simplified" License](https://spdx.org/licenses/BSD-2-Clause.html)
* BSD-3-Clause :  [BSD 3-clause "New" or "Revised" License](https://spdx.org/licenses/BSD-3-Clause.html)
* GPL-2.0 : [GNU General Public License version 2.0 only](https://spdx.org/licenses/GPL-2.0.html)
* GPL-3.0 : [GNU General Public License version 3.0 only](https://spdx.org/licenses/GPL-3.0.html)
* LGPL-3.0 : [GNU Lesser General Public License v3.0 only](https://spdx.org/licenses/LGPL-3.0.html)
* MIT : [The MIT license](https://spdx.org/licenses/MIT.html), aka the expat license.

Similarly, here are some [free cultural works](http://creativecommons.org/freeworks) licenses
(typically used for works other than software):

* CC-BY-3.0 : [Creative Commons Attribution 3.0](https://spdx.org/licenses/CC-BY-3.0.html)
* CC-BY-SA-4.0 : [Creative Commons Attribution Share Alike 4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html)
* CC0-1.0 : [Creative Commons Zero v1.0 Universal](https://spdx.org/licenses/CC0-1.0.html)

SPDX license identifiers never contain a space.  SPDX licenses use case-insensitive matching, however, people are encouraged to provide and display the mixed-case forms where reasonable.  In URLs people are encouraged to support both mixed-case and lower-case forms; if that's impractical, convert to lower case (to support case-insensitive matching).

The [SPDX license list](https://spdx.org/licenses/) has a much more complete list of SPDX license identifiers, and is occasionally updated with new ones. As of September 2005 it defines standard license identifiers for over 300 licenses. 

## SPDX license expressions

In some situations there isn't just one license, e.g., software might be offered with a choice between two or more licenses.
SPDX version 2.0 added support for SPDX *license expressions*, which let you clearly express this when you need to.

A SPDX license expression can be simple SPDX license identifier or a SPDX "user defined license reference" (aka LicenseRef).
A LicenseRef is used if a license isn't already defined by a standard SPDX identifier.
However, SPDX license expressions also support a "+" suffix after a license identifier; this means
"this license or any later version".  For example, you can express "GNU General Public License (GPL) version 2.0 or later"
using the SPDX license expression "GPL-2.0+".

You can also use three different operators to combine license expressions to create other license expressions:

* "OR": Recipients can choose between two or more licenses.
  For example, "(LGPL-2.1 OR MIT OR BSD-3-Clause)" means recipients can choose between any of those three licenses.
* "AND": Recipients are required to simultaneously comply with two or more licenses.
  For example, "(LGPL-2.1 AND MIT AND BSD-2-Clause)" means recipients must comply with all three licenses.
* "WITH": Add the collowing exception(s).  For example, "(GPL-2.0+ WITH Classpath-exception-2.0)"
  means recipients must comply with the GPL version 2.0 or later with the Classpath 2.0 license exception.
  See the [SPDX license exceptions list](https://spdx.org/licenses/exceptions-index.html) for the list of
  standard names for license exceptions.

There is an order of operations: "+", then "WITH", then "AND", then "OR".
This order can be overridden by parentheses, and if it's complex parentheses are good for clarity.
Any license expression that consists of more than one license identifier and/or LicenseRef should be 
encapsulated by parentheses.
SPDX expressions are case-insensitive, but by convention the operation names should be capitalized.

## SPDX Files

SPDX files are a way to capture and exchange license information.
The SPDX specification actually supports two formats: tag/value and RDF/XML.
They can do the same thing, but tag/value is easier to write, so that's what we'll use here.
A tag/value file is simply a sequence of lines, where each line is a tag name, a colon, a space, and its value.

There are many tags defined in the SPDX specification, but many of them may not apply to your specific situation.
Note that unlike license expressions, tag names are case-sensitive.
A few especially important tags are:

* SPDXVersion: The version of the spec used, normally "SPDX-2.0".
* DataLicense: The license for the license data itself (!); normally this is "CC0-1.0".
  Note that this is *not* the license for the software or data being packaged.
* Creator: Who or what created this SPDX file.  This is in one of 3 formats:
    - For a person: person name, optionally followed by email in parentheses.
    - For an organization: organization name, optionally followed by email in parentheses.
    - For a tool: toolidentifier-version
* PackageName: The full name of the package as given by Package Originator.
* PackageLicenseDeclared: The license identified in text in one or more files (for example a COPYING or LICENSE file)
  in the source code package.  This field is not intended to capture license information
  obtained from an external source, such as the package website.
* PackageHomePage: The package's home page URL.
   
For example, a SPDX file with these lines states that this uses the SPDX 2.0 format (the current one),
the license information can be shared with everyone, this SPDX file was created by David A. Wheeler,
it describes the Foo package, and
the package maintainers declare that all the software in this package is released using the MIT license:

    SPDXVersion: SPDX-2.0
    DataLicense: CC0-1.0
    Creator: David A. Wheeler
    PackageName: Foo
    PackageLicenseDeclared: MIT

There are a lot more details and options explained in the
[SPDX specification](https://spdx.org/SPDX-specifications/spdx-version-2.0).

Now, a personal note.
Many of the tags and features are useful only when exchanging SPDX files that are isolated from their packages
and describe large collections of packages.
In particular, the specification officially considers many tags mandatory, but I personally would interpret them only as
"mandatory" if you are trying to exchange SPDX files that are isolated from their packages.
If you are a package developer releasing software, I suggest starting with at least the four lines listed
in the example above.

Conventionally a SPDX filename has the extension ".spdx" (in either XML or tag-value format).
When combining SDPX files together they often use the project name, though I think
"LICENSE.spdx" at the top level would be perfectly reasonable when including a SPDX file in a project.
To my knowledge SPDX has not been registered as a MIME type, so currently it would probably
have a MIME type mapping of ".spdx" to MIME type "application/x-spdx".

## SPDX license expressions in asource code files

You can also refer to SPDX license expressions from source code, instead of trying to
embed large quantities of legalese in your source code headers.
The SPDX group have not endorsed a particular way to do this, however.
I recommend using a case-sensitive tag "SPDX-License-Expression" in a comment near the top of the file,
followed by the SPDX license expression.  For example, in a language that uses "#" as the
rest-of-line comment character, use this to express "this software is under GPL version 3.0 or later":

    # SPDX-License-Expression: GPL-3.0+

Note that this says license expression, not license identifier; this name makes it clear that you can
use the full power of SPDX license expressions if you need to.

## More information

For more information, visit the [SPDX website](http://spdx.org/).
Common destinations include the
[SPDX license list](https://spdx.org/licenses/) and the [SPDX specifications](http://spdx.org/spdx-specifications).

## About this tutorial

This tutorial was originally developed by [David A. Wheeler](http://www.dwheeler.com).
It is released under the Creative Commons Attribution License 3.0 Unported (CC-BY-3.0) license or later;
the 3.0 license is used for the SPDX specification.
In SPDX notation, this tutorial is released under "CC-BY-3.0+".
