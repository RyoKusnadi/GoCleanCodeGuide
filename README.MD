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