# Clean Code Guide In Go

## Preface
There is always an options in your life. So does your code, maintaining a good code or having a bad code that hard to debug in production environment. It's all depends of your option how you made it. 

How to make sure you are in the right path? it's all about the <b>Craftmanship</b>, there are two part of it: 
- Knowledge: You must gain the knowledge of principles, patterns, practices, and heuristics that as a programmers should know
- Work: After you know it, you should learn constantly to implement it in your daily development cycle.

There is no quick way to do this since this is hard and i believe this is the cycle that everyone as professional developer must go through.

NB: Many resources i used in this guide is not originally from me. This guide is basically my note to do the daily development, and feel free to contribute by creating PR if intrested. Hit me up if you have any question / discussion on the linkedin.


## Table of Contents
- [Introduction: What Is My Definition of Clean Code?](#introduction-what-is-my-definition-of-clean-code)



## Introduction: What Is My Definition of Clean Code?
As programmer, i always believe there is no definition of stop learning. But continuous improvement of your skill is the <b>MUST</b> thing you need to do. My definition of "Code" is the language in which we ultimmately express the requirements. Clean code refers to writing code that is easy to understand, maintain, and modify. It's about crafting code that is not only functional but also readable and elegant. Here's the characteristics of it
- <b>Readable</b> - Clean code is easy to read and understand. It follows consistent naming conventions for variables, functions, and classes. Meaningful names should be chosen that accurately describe the purpose of each component.
- <b>Simplicity</b> - Clean code is simple and straightforward. It avoids unnecessary complexity and keeps the logic as simple as possible while still achieving the desired functionality.
- <b>Modular</b> - Clean code is modular, meaning it is organized into small, cohesive units such as functions or classes. Each module should have a single responsibility and should be easily understandable on its own.
- <b>Well-Structured</b>: Clean code has a clear and logical structure. It follows standard coding practices and design patterns, making it easier to navigate and maintain.
- <b>Efficient</b> - Clean code is efficient in terms of both execution and development time. It avoids redundant or inefficient operations and follows best practices for performance optimization.
- <b>Documented</b> - Clean code is well-documented, with comments and documentation that explain the purpose of each component and how it fits into the overall system. However, it also aims to be self-explanatory, with code that is clear enough to understand without excessive comments.
- <b>Testable</b> - Clean code is designed with testing in mind. It is structured in a way that makes it easy to write unit tests and verify the correctness of its behavior.
- <b>Maintainable</b> - Clean code is easy to maintain and extend. It is designed with future changes in mind, making it easier for other developers to understand and modify as requirements evolve.

In Summary, clean code is about more than just functionality—it's about creating code that is a pleasure to work with, both for the original developer and for anyone who comes across it in the future.

Also during your daily development, if you found out odd or messy code,  instead of simply working around it or ignoring it, you should take the time to refactor and clean it up. You can refer to <b>Boy Scout Rule</b> - <i>leaving the code better than you found it</i>


## Clean Code In GO
### Naming
Meaningful names describe the purpose and functionality of variables, functions, types, and packages. They help improve code readability, understandability, and maintainability by providing context and clarity to developers. easier for yourself and others to understand and work with the codebase In summary a meaningful will help us more easily understand what does the logic does or store, so does the bad name. 

Here's the characteristics of the good names:
- Not feel repetitive when they are used
- Take the context into consideration
- Not repeat concepts that are already clear

In Go, we should know below convention regarding naming:

<b>Underscores</b>  

Names in Go should in general not contain underscores. There are three exceptions to this principle:
- Package names that are only imported by generated code may contain underscores.
- Test, Benchmark and Example function names within *_test.go files may include underscores.
- Low-level libraries that interoperate with the operating system or cgo may reuse identifiers, as is done in syscall. This is expected to be very rare in most codebases.

<b>Package Name</b>

Package names should be short and contain only lowercase letters. A package name composed of multiple words should be left unbroken in all lowercase. For example, the package 'tabwriter' is not named tabWriter, TabWriter, or tab_writer.

Avoid selecting package names that are likely to be shadowed by commonly used local variable names. For example, <b>usercount</b> is a better package name than <b>count</b>, since count is a commonly used variable name.

Go package names should not have underscores. If you need to import a package that does have one in its name (usually from generated or third party code), it must be renamed at import time to a name that is suitable for use in Go code.

An exception to this is that package names that are only imported by generated code may contain underscores. Specific examples include:
- Using the _test suffix for an external test package, for example an integration test
- Using the _test suffix for package-level documentation examples

Avoid uninformative package names like util, utility, common, helper, and so on. Go package names should be related to what the package provides. Naming a package just util, helper, common or similar is usually a poor choice (it can be used as part of the name though). Uninformative names make the code harder to read, and if used too broadly they are liable to cause needless import conflicts.

Instead, consider what the callsite will look like.
```go
// Good:
db := spannertest.NewDatabaseFromFile(...)

_, err := f.Seek(0, io.SeekStart)

b := elliptic.Marshal(curve, x, y)
```

```go
// Bad:
db := test.NewDatabaseFromFile(...)

_, err := f.Seek(0, common.SeekStart)

b := helper.Marshal(curve, x, y)
```



When an imported package is renamed (ex: import foopb "path/to/foo_go_proto").the local name for the package must comply with the rules above, as the local name dictates how the symbols in the package are referenced in the file. If a given import is renamed in multiple files, particularly in the same or nearby packages, the same local name should be used wherever possible for consistency.




 