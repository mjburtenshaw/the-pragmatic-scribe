[🔝 TOP: README](README.md)

[🔙 BACK: Formats](README.md#formats)

Formats
====================================

> *"Plain text not only suffices, it is superior."[^1]*
> 
> *- Google*

Table of Contents
---------------------------

- [Semantic Code](#semantic-code)
- [Comments](#comments)
- [Markdown](#markdown)
	- [Sample](#sample)
- [DBML](#dbml)
	- [Sample](#sample-2)
- [See Also](#see-also)

Semantic Code
--------------------

> *"There are only two hard things in computer science: cache invalidation and naming things."*
> 
> *- Phil Karlton*

Giving symbols meaningful names can help reduce the amount of documentation that might otherwise be necessary to describe a program.

Semantic code is all about *readability*. There's a fine line between symbol names that are too short to provide semantic value and too long to make sense among all the other code.

In general, you should name things what they are, which is more difficult than it sounds because accurate naming depends on context, which can change. Relying on context to describe symbols can help reduce variable names' length. I find it helps to ask a few questions:
- What context will the reader have just by meeting the requirements to see this code?
- Can this code reasonably be used in more contexts later?
- What context will be missing in an error trace?

For example, `data` is generally considered a bad name for a variable:

```ruby
data = []
data.push('some data')
```

But within the proper context, it might be the only thing that makes sense. For example, if it captures the result of an external call:

```ruby
# google_service.rb

def get_address_options(address_query)
  data = google_maps_client.get_matching_addresses(address_query)

  # ...
end
```

Naming booleans requires extra consideration. There are a few guidelines I use:
- **What question does this boolean answer?**
- **Is that question void of negators?** If not, flip the evaluation logic.
- **Is that question void of conjunctions?** If not, refactor the evaluation if possible.

✅ Do this:

```java
public class Widget {
    public static void main(String[] args) {
        boolean isFeatureFlagOn = true;
        boolean isDataCompatible = true;
        boolean userHasPermission = false;

        boolean canUseWidget = isFeatureFlagOn && isDataCompatible;
        boolean shouldUseWidget = canUseWidget && userHasPermission;
    }
}
```

❌ Avoid this:

```java
public class Widget {
    public static void main(String[] args) {
        boolean isFeatureFlagOffAndDataNotCompatibleOrUserIsSuperadmin = true;
    }
}
```

Comments
-------------

> *"Programs must be written for people to read and only incidentally for machines to execute."*
>
> *- Hal Abelson*

Comments are human-readable annotations in source code that are generally ignored by compilers and interpreters. There are a few use cases for comments, including, but not limited to, the following:

- **Planning and reviewing:** Things like pseudocode to explain intent and control flow at a human-readable abstraction.
- **Describing code:** Generally, comments describing what code does are considered superfluous. Typically, these comments are best left to explain exceptions to standard conventions or best practices.
- **Algorithmic description:** Sometimes source code contains a novel or noteworthy solution to a specific problem.
- **Resource inclusion:** Sometimes, providing rich resources like logos, diagrams, flowcharts, ASCII art, and copyright notices can embellish a file.
- Metadata.
- Debugging.
- **Automatic documentation generation:** comments that a code editor can parse to provide rich details about code.
- Directives for interpreters.
- **Stress relief:** Sometimes developers need to let off steam, and the editor is the only one listening.

Markdown
-------------

Markdown[^2] is a lightweight markup language that you can use to add formatting elements to plaintext text documents. Created by John Gruber in 2004, Markdown is now one of the world’s most popular markup languages.

> *"The overriding design goal for Markdown’s formatting syntax is to make it as readable as possible. The idea is that a Markdown-formatted document should be publishable as-is, as plain text, without looking like it’s been marked up with tags or formatting instructions. While Markdown’s syntax has been influenced by several existing text-to-HTML filters, the single biggest source of inspiration for Markdown’s syntax is the format of plain text email."*
> 
> *- John Gruber, Markdown Introduction[^3]*

Because Markdown is an abstraction of XHTML[^3], any Markdown viewers worth their salt should support inline XHTML. This should avoided when possible, but it's handy if you want to add color or underlines to your document.

### Sample

``````markdown
Sample Markdown Document
=========================

> "A quotation at the right moment is like bread to the famished." — The Talmud

Paragraphs and Text Styling
---------------------------

This is an example of a paragraph. **This text is bold.** 

Here is another paragraph. *This text is italicized.* 

And, one more paragraph for good measure. __Bold text here__ and _italic text here_ are being used within the same paragraph.

Here's an unordered list:

- [San Francisco](https://en.wikipedia.org/wiki/San_Francisco)
- New York
  - Manhattan
  - Queens
  
And now an ordered one:

1. First Item
2. Second Item
   1. Sub-item 1
   2. Sub-item 2

Below is an example of how to display an image:

![Markdown Logo](https://markdown-here.com/img/icon256.png)

Coding Examples
----------------

Use the `print()` function to print text in Python.

```python
def say_hello():
    print("Hello, Markdown World!")
    
say_hello()
```
``````

DBML
-------

Database schema documentation is a critical part of many projects. DBML[^4] can document your database schema effectively with the help of tools like [dbdiagram.io](https://dbdiagram.io/home).

DBML is born to solve the frustrations of developers working on large, complex software projects:

- Difficulty building up a mental "big picture" of an entire project's database structure
- Trouble understanding tables and what their fields mean, and which feature are they related to;
- The existing ER diagram and/or SQL DDL code is poorly written and hard to read (and usually outdated).

They recommend keeping a `database.dbml` file in your projects' root directories.

### Sample

```dbml
Table users {
  id integer
  username varchar
  role varchar
  created_at timestamp
}

Table posts {
  id integer [primary key]
  title varchar
  body text [note: 'Content of the post']
  user_id integer
  status post_status
  created_at timestamp
}

Enum post_status {
  draft
  published
  private [note: 'visible via URL only']
}

Ref: posts.user_id > users.id // many-to-one
```

[🔙 Next: The Pragmatic Scribe Paradigm](README.md#the-pragmatic-scribe-paradigm)

See Also
-------------

- [Wikipedia - Comment (computer programming)](https://en.wikipedia.org/wiki/Comment_(computer_programming))
- [Best practices for writing code comments](https://stackoverflow.blog/2021/12/23/best-practices-for-writing-code-comments/)

[^1]: [Google styleguide](https://github.com/google/styleguide/blob/gh-pages/docguide/philosophy.md#readable-source-text)
[^2]: [Markdown Guide](https://www.markdownguide.org)
[^3]: [Markdown by John Gruber](https://daringfireball.net/projects/markdown/)
[^4]: [DBML - Database Markup Language](https://dbml.dbdiagram.io/home/#dbml-database-markup-language)
