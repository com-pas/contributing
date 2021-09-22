[Styleguides](#styleguides)
* [Copyright Guidelines](#copyright-guidelines)
* [English language convention](#cnglish-language-convention)

# Styleguides

## Copyright Guidelines

Contributing to the CoMPAS project also requires to use correct copyright headers in source files.

For each CoMPAS repository, we created / will create a Github Action featuring [REUSE](https://reuse.software/). REUSE is a piece of software which checks for correct copyright information in files defined in a [specification](https://reuse.software/spec/). The specification is based on best practices and the use of [SPDX](https://spdx.dev/) identifiers.

Example Alliander copyright header for Java files:
```Java
// SPDX-FileCopyrightText: 2020 Alliander N.V.
//
// SPDX-License-Identifier: Apache-2.0
```

Every commit on a Pull Request is being scanned by REUSE. If it fails, the pull requests cannot be merged.

For more tips on using REUSE (for example with a small command line tool), check the [Tips: Copyright & Licensing](https://wiki.lfenergy.org/pages/viewpage.action?pageId=10996220) wiki page.

## English language convention

The convention for all the project's documents, including code documentation, website, is to write American English.
A list of spelling differences between British and American English is available
[here](https://www.britishcouncilfoundation.id/en/english/articles/british-and-american-english) for example.
