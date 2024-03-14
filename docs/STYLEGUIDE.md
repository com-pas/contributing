# Styleguides

#### Table Of Contents

* [English language convention](#english-language-convention)
* [Copyright Guidelines](#copyright-guidelines)
* [Java StyleGuide](#java-styleGuide)
* [Git Commit Messages](#git-commit-messages)

## English language convention

The convention for all the project's documents, including code documentation, website, is to write American English.
A list of spelling differences between British and American English is available
[here](https://www.britishcouncilfoundation.id/en/english/articles/british-and-american-english) for example.

## Copyright Guidelines

Contributing to the CoMPAS project also requires using correct copyright headers in source files.

For each CoMPAS repository, we created / will create a Github Action featuring [REUSE](https://reuse.software/). 
REUSE is a piece of software which checks for correct copyright information in files defined in a [specification](https://reuse.software/spec/). 
The specification is based on best practices and the use of [SPDX](https://spdx.dev/) identifiers.

Example Alliander copyright header for Java files:
```Java
// SPDX-FileCopyrightText: 2020 Alliander N.V.
//
// SPDX-License-Identifier: Apache-2.0
```

Every commit on a Pull Request is being scanned by REUSE. If it fails, the pull requests cannot be merged.

For more tips on using REUSE (for example with a small command line tool), check the 
[Tips: Copyright & Licensing](https://wiki.lfenergy.org/pages/viewpage.action?pageId=10996220) wiki page.

## Java StyleGuide

- The project uses modern java, feel free to use any new APIs provided by the current java version (currently java 17).
- New API classes and methods should be documented with Javadoc. Write higher level documentation for classes and lower level documentation for methods. For example, ...
- User-facing configuration options and general design decisions should be documented (where?)
- We use standard configurations of well known tools like checkstyle and SonarQube to enforce a coherent coding style, please consult those tools for justifications on these rules. 
    - ESlint is already in place for the front-end.
    - Sonarcloud already checks the code style with best practices
    - IntelliJ Default checkstyle is used for java (an example could be found in contribution project named .editorconfig)

As a simple yet instructive example, consider ...
```java
/**
 * As checkstyle file content
 * <?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE module PUBLIC "-//Puppy Crawl//DTD Check Configuration 1.3//EN" "http://www.puppycrawl.com/dtds/configuration_1_3.dtd">

 <module name="Checker">
 <module name="TreeWalker">
 <module name="ConstantName"/>
 </module>
 </module>

As in java code
 
 private static final String voltageLevel //Not compliant
 private static final String VOLTAGE_LEVEL //Compliant
 
 */
```

## Git Commit Messages

As usual, please start the commit message with a short line describing the commit, then leave a blank line, then give more context and explanations.
You can use GitHub's integrations, for example to link to existing issues. In general, pull requests with more than one commits will be squashed when merged in master.
