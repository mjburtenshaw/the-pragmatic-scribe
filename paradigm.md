[🔝 TOP: The Pragmatic Scribe](README.md)

[🔙 BACK: The Pragmatic Scribe Paradigm](README.md#the-pragmatic-scribe-paradigm)

The Pragmatic Scribe Paradigm
====================================================

The purpose of documentation is to transfer knowledge from your mind to those of others across time and space.

Table of Contents
---------------------------

- [Discovery](#discovery)
  - [`TOP`](#top)
  - [`BACK`](#back)
  - [`NEXT`](#next)
- [Detail](#detail)
  - [Control Flow](#control-flow)
  - [Functions](#functions)
  - [Files and Modules](#files-and-modules)
  - [Directories and Packages](#directories-and-packages)
- [See Also](#see-also)

Discovery
--------------

> *"We shall not cease from exploration, and the end of all our exploring will be to arrive where we started and know the place for the first time."*
> 
> *- T.S. Eliot*

Good documentation is discoverable because it loses all utility and value if kept offsite.

The discoverability of documentation changes based on the lens from which you approach it. For example, when debugging a routine, the most discoverable documentation is closest to the code you're debugging. But if you want to contribute to an unfamiliar project, discoverability is better the higher-up the docs are.

However, scattering all docs across the project is impractical. But so is putting minute details in highly abstracted documents. You want ***accessibility:* the balance of discoverability and detail.**

An important aspect of accessible documentation is how easy it is to navigate. You should be able to get to any document from any other document in a project via a daisy chain of hyperlinks where platforms allow. In particular, every documentation file should have the following navigation elements:

### `TOP`

- Takes you to the parent document in the doc tree.
- This should be located on the first line of the file.
- The only file that is exempt is the root document.

### `BACK`

- Takes you to the one of the following:
	- The previous document in a doc series.
	- The anchor element of the parent document referencing int.
- This should be located below the `TOP` navigation element.
- The only file that is exempt is the root document.

### `NEXT`

- Takes you to one of the following:
	- The next document in a doc series.
	- The next anchor element of the parent document referencing it.
	- The parent document in the doc tree. Label this as `BACK TO TOP` in this case.
- This should be located on the last line of the file.
- No file is exempt. There should always be another document or back-to-top.

Let's talk about which details should go where.

Detail
----------

> *"But then it hit me. Code is not literature and we are not readers. Rather, interesting pieces of code are specimens and we are naturalists."[^1]*
> 
> *-Peter Seibel*

If we approach code as scientists, it starts to make more sense why documentation is important and why it's important to be nuanced in that pursuit. Just as great naturalists of the 19<sup>th</sup> century fundamentally changed our understanding of the world by documenting their findings, I think documenting code methodically will fundamentally change how you approach creating it.

Let's dive in.

### Control Flow

The best docs for control flow are semantic code, i.e., code-as-docs. Abstractions above should focus on procedural aberrations & deviations and are best formed as comments:

✅ Do this:
- The function, iterator, and mutable have meaningful names.
- The comment abstraction is just right; if someone changes the loop, it's easier to change the comment.

```javascript
function countNames(people) {
	let nameCount = 0;

	// Use a `for in` loop instead of `Array.prototype.length()` because `people` could be an Object.
	for (i in people) {
		const person = people[i];
		if (person?.name) {
			nameCount += 1;
		}
	}
	return nameCount;
}
```

❌ Avoid this:
- The comment abstraction is too high; The docs are more likely to become outdated if the code it explains changes.
- The function name doesn't capture the business case when it could.
- The parameter name could be more narrow because it has context.
- Abbreviations should be avoided when possible.

```javascript
/**
 * Counts how many given people have names.
 * 
 * Use a `for in` loop instead of `Array.prototype.length()` because `people` could be an Object.
 */
function count(iterable) {
	let c = 0;
	for (i in iterable) {
		const person = iterable[i];
		if (person?.name) {
			c += 1;
		}
	}
	return c;
}
```

### Functions

Documentation at the function level is best for explaining its metadata, such as intent, usage, contract, examples, steadiness, plans, etc. If there are curious implementation details, it is usually better to leave those as comments on [control flow](#control-flow).

Many programming languages support a mechanism for embedding documentation directly within the code, but the specifics and conventions differ from one language to another.

Python's [Docstrings](https://peps.python.org/pep-0257/) are a particular convention where a string placed immediately after the definition of a function, method, class, or module serves as its documentation. This string can then be accessed at runtime using the `help()` function or the `.__doc__` attribute:

```python
def add(a, b):
    """
    Add two numbers together and return the result.
    
    Parameters:
    - a (int or float): The first number.
    - b (int or float): The second number.
    
    Returns:
    int or float: The sum of a and b.
    
    Example:
    >>> add(1, 2)
    3
    >>> add(1.5, 2.5)
    4.0
    """
    return a + b

# You can view the docstring in the REPL using print:

print(add.__doc__)

# or help:

help(add)
```

Let's look at a few popular languages and their mechanisms for inline documentation:

- **Java**: Java has [JavaDoc](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javadoc.html) comments. These are multiline comments that start with `/**` and end with `*/`. Tools like `javadoc` can then be used to generate HTML-based documentation from these comments:

   ```java
   /**
    * This method does something.
    * @param param1 Description of param1.
    * @return Description of the return value.
    */
   public int someMethod(int param1) {
       // ...
   }
   ```

- **JavaScript**: Many JavaScript developers use tools like [JSDoc](https://jsdoc.app), which has a style similar to JavaDoc. However, these are convention-based and not built into the language:

   ```javascript
   /**
    * Calculates the sum of two numbers.
    * @param {number} a - The first number.
    * @param {number} b - The second number.
    * @returns {number} Sum of a and b.
    */
   function sum(a, b) {
       return a + b;
   }
   ```

- **C#**: C# uses [XML documentation comments](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/documentation-comments), which start with `///`. Visual Studio and other tools can use these comments to provide Intellisense tooltips, and tools like Sandcastle can generate HTML documentation:

   ```csharp
   /// <summary>
   /// Calculates the sum of two numbers.
   /// </summary>
   /// <param name="a">The first number.</param>
   /// <param name="b">The second number.</param>
   /// <returns>Sum of a and b.</returns>
   public int Sum(int a, int b) {
       return a + b;
   }
   ```

- **Ruby**: Ruby has [RDoc](https://docs.ruby-lang.org/en/2.1.0/RDoc/Markup.html), which is used to produce documentation for Ruby projects. Comments preceding a method or class definition are treated as documentation for that method or class:

   ```ruby
   # Computes the sum of two numbers.
   # @param a [Integer] The first number.
   # @param b [Integer] The second number.
   # @return [Integer] The sum of a and b.
   def sum(a, b)
     a + b
   end
   ```

- **Go**: Go uses [a convention](https://tip.golang.org/doc/comment) where comments directly preceding a declaration serve as documentation for that declaration. The `godoc` tool can be used to generate and view this documentation:

   ```go
   // Sum computes the sum of two integers.
   func Sum(a, b int) int {
       return a + b
   }
   ```

While the specific mechanisms and conventions vary, the general idea of embedding documentation within the code is a common practice across many programming languages.

### Files and Modules

File docs often include providing an overview of the file's purpose, listing authors or maintainers, noting dependencies, and so on.

Here's a breakdown of a few languages and tools made expressly for this purpose:

- **Python**:
   In Python, the module-level Docstring is typically placed at the top of the file, right after any import statements but before any globals or function/class definitions:

   ```python
   """
   This module provides utility functions for mathematical operations.

   Functions:
   - add: Returns the sum of two numbers.
   - subtract: Returns the difference of two numbers.

   Author: John Doe
   Date: January 2023
   """

   import math

   def add(a, b):
       """Return the sum of a and b."""
       return a + b

   # ... rest of the module ...
   ```

- **JavaScript**:
   JSDoc supports file-level or module-level documentation using the `@file` or `@fileoverview` tags:

   ```javascript
   /**
    * @fileoverview Utility functions for mathematical operations.
    * @author John Doe
    * @version 1.0.0
    */

   /**
    * Returns the sum of two numbers.
    * @param {number} a - The first number.
    * @param {number} b - The second number.
    * @returns {number} The sum of a and b.
    */
   function add(a, b) {
       return a + b;
   }

   // ... rest of the module ...
   ```

- **Go**:
   In Go, comments preceding package declarations are treated as package-level documentation:

   ```go
   // Package mathutils provides utility functions for mathematical operations.
   package mathutils

   // ... rest of the package ...
   ```

For many languages, it's also common (though not required) to include licensing information at the top of the file, especially for open-source projects.

No matter the specific conventions of a language or documentation tool, the key goal is to give the reader an overview of the file's or module's purpose and contents so they have a clear starting point when diving into the details.

### Directories and Packages

The main goal of documenting directories or packages is to provide clarity to new readers or contributors, giving them an easy entry point to understand its structure and organization.

The approach to documenting directories often varies by language and tooling ecosystem, but there are some general conventions and best practices.

- **README files**: 
   
   One of the most widely accepted practices across languages and platforms is the inclusion of a `README` file (often `README.md` if using Markdown format) in a directory. This file typically provides an overview of the directory's contents, its purpose, usage examples, and other pertinent details.

   For example, in a multi-module or multi-package project, each module or package might have its own `README` file explaining its specifics.

- **Python**:

   For packages (i.e., directories containing an `__init__.py` file), the module-level docstring in the `__init__.py` file often serves to describe the entire package. Tools like Sphinx can then pick up this docstring when generating documentation.

- **JavaScript/Node.js**:

   In the Node.js ecosystem, the `package.json` file (while primarily a manifest for package details and dependencies) often sits alongside a `README.md` file that documents the package or directory's purpose and usage.

- **Java**:

   In larger Java projects, especially those using Maven or Gradle, it's common to have a multi-module setup where each module (or directory) might have its own `README.md` or `README.adoc` (for AsciiDoc). Additionally, the module's `pom.xml` in Maven will contain metadata about the module, which can be used by documentation generators.

- **Go**:

   Go documentation tools typically use the package-level comments in the package's primary file (often named after the package) to generate documentation. However, adding a `README.md` in the directory can be useful for developers browsing the codebase directly.

- **Rust**:

   In the Rust ecosystem, the `Cargo.toml` file describes package metadata, but again, a `README.md` is a widely accepted convention for describing the package or directory when it's part of a larger workspace or when the package has multiple components.

- **Directory structure documentation**: 

   For very large projects, it can be helpful to have a dedicated documentation file (or section in a main `README`) that explains the directory structure. This might look like:

   ```
   ├── src/          # Source files
   ├── tests/        # Automated tests
   ├── docs/         # Documentation sources
   ├── assets/       # Graphics, icons, and other static assets
   └── scripts/      # Utility scripts for development
   ```

Beyond this, we get to project and initiative-level documentation, which will get its own section.

[⏭️ NEXT: First Class Documentation](README.md#first-class-documentation)

See Also
-------------

[^1]: [CODE IS NOT LITERATURE](https://gigamonkeys.com/code-reading/)
