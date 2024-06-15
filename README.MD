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
### 1. Naming
Meaningful names describe the purpose and functionality of variables, functions, types, and packages. They help improve code readability, understandability, and maintainability by providing context and clarity to developers. easier for yourself and others to understand and work with the codebase In summary a meaningful will help us more easily understand what does the logic does or store, so does the bad name. 

Here's the characteristics of the good names:
- Not feel repetitive when they are used
- Take the context into consideration
- Not repeat concepts that are already clear

In Go, we should know below convention regarding naming:

#### <b>Underscores</b>  

Names in Go should in general not contain underscores. There are three exceptions to this principle:
- Package names that are only imported by generated code may contain underscores.
- Low-level libraries that interoperate with the operating system or cgo may reuse identifiers, as is done in syscall. This is expected to be very rare in most codebases.
- If you need to import a package that does have one in its name (usually from generated or third party code), it must be renamed at import time to a name that is suitable for use in Go code.


An exception to this is that package names that are only imported by generated code such as Test, Benchmark and Example function names for  may contain underscores. Specific examples include:
- Using the _test suffix for an external test package, for example an integration test
- Using the _test suffix for package-level documentation examples

#### <b>Package Name</b>

Package names should be short and contain only lowercase letters. A package name composed of multiple words should be left unbroken in all lowercase. For example, the package 'tabwriter' is not named tabWriter, TabWriter, or tab_writer.

Avoid selecting package names that are likely to be shadowed by commonly used local variable names. For example, <b>usercount</b> is a better package name than <b>count</b>, since count is a commonly used variable name.


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

When an imported package is renamed

Example: import foopb "path/to/foo_go_proto"

The local name for the package must comply with the rules above, as the local name dictates how the symbols in the package are referenced in the file. If a given import is renamed in multiple files, particularly in the same or nearby packages, the same local name should be used wherever possible for consistency.

#### <b>Receiver names</b>

Receiver variable names must be:
- Short (usually one or two letters in length)
- Abbreviations for the type itself
- Applied consistently to every receiver for that type
Example
<table>
<tr>
    <th>Long Name</th>
    <th>Better Name</th>
</tr>
<tr>
    <td>func (tray Tray)</td>
    <td>func (t Tray)</td>
</tr>
<tr>
    <td>func (info *ResearchInfo)</td>
    <td>func (ri *ResearchInfo)</td>
</tr>
<tr>
    <td>func (this *ReportWriter)</td>
    <td>func (w *ReportWriter)</td>
</tr>
<tr>
    <td>func (self *Scanner)</td>
    <td>func (s *Scanner)</td>
</tr>
</table>

#### <b>Constant names</b>
Constant names must use MixedCaps like all other names in Go. (Exported constants start with uppercase, while unexported constants start with lowercase.) This applies even when it breaks conventions in other languages. Constant names should not be a derivative of their values and should instead explain what the value denotes.

``` Go
// Good:
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)
```

Do not use non-MixedCaps constant names or constants with a K prefix.
``` Go
// Bad:
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```

Name constants based on their role, not their values. If a constant does not have a role apart from its value, then it is unnecessary to define it as a constant.

```
// Bad:
const Twelve = 12

const (
    UserNameColumn = "username"
    GroupColumn    = "group"
)
```

#### <b>Initialisms</b>
Words in names that are initialisms or acronyms (e.g., URL and NATO) should have the same case. URL should appear as URL or url (as in urlPony, or URLPony), never as Url. This also applies to ID when it is short for “identifier”; write appID instead of appId.

In names with multiple initialisms (e.g. XMLAPI because it contains XML and API), each letter within a given initialism should have the same case, but each initialism in the name does not need to have the same case.
In names with an initialism containing a lowercase letter (e.g. DDoS, iOS, gRPC), the initialism should appear as it would in standard prose, unless you need to change the first letter for the sake of exportedness. In these cases, the entire initialism should be the same case (e.g. ddos, IOS, GRPC).

<table>
<tr>
    <th>Initialism(s)</th>
    <th>Scope</th>
    <th>Correct</th>
    <th>Incorrect</th>
</tr>
<tr>
    <td>XML API</td>
    <td>Exported</td>
    <td>XMLAPI</td>
    <td>XmlApi, XMLApi, XmlAPI, XMLapi</td>
</tr>
<tr>
    <td>XML API</td>
    <td>Unexported</td>
    <td>xmlAPI</td>
    <td>xmlapi, xmlApi</td>
</tr>
<tr>
    <td>iOS</td>
    <td>Exported</td>
    <td>IOS</td>
    <td>Ios, IoS</td>
</tr>
<tr>
    <td>iOS</td>
    <td>Unexported</td>
    <td>iOS</td>
    <td>iOS	ios</td>
</tr>
<tr>
    <td>gRPC</td>
    <td>Exported</td>
    <td>GRPC</td>
    <td>Grpc</td>
</tr>
<tr>
    <td>gRPC</td>
    <td>Unexported</td>
    <td>gRPC</td>
    <td>grpc</td>
</tr>
<tr>
    <td>DDoS</td>
    <td>Exported</td>
    <td>DDoS</td>
    <td>DDOS, Ddos</td>
</tr>
<tr>
    <td>DDoS</td>
    <td>Unexported</td>
    <td>ddos</td>
    <td>dDoS, dDOS</td>
</tr>
</table>

#### <b>Getters</b>
Function and method names should not use a `Get` or `get` prefix, unless the underlying concept uses the word “get” (e.g. an HTTP GET). Prefer starting the name with the noun directly, for example use `Counts` over `GetCounts`.

If the function involves performing a complex computation or executing a remote call, a different word like `Compute` or `Fetch` can be used in place of `Get`, to make it clear to a reader that the function call may take time and could block or fail.

#### <b>Variable names</b>
The general rule of thumb is that the length of a name should be proportional to the size of its scope and inversely proportional to the number of times that it is used within that scope. A variable created at file scope may require multiple words, whereas a variable scoped to a single inner block may be a single word or even just a character or two, to keep the code clear and avoid extraneous information.

Here is a rough baseline. These numeric guidelines are not strict rules. Apply judgement based on context, clarity, and concision.
- A small scope is one in which one or two small operations are performed, say 1-7 lines.
- A medium scope is a few small or one large operation, say 8-15 lines.
- A large scope is one or a few large operations, say 15-25 lines.
- A very large scope is anything that spans more than a page (say, more than 25 lines).

A name that might be perfectly clear (e.g., c for a counter) within a small scope could be insufficient in a larger scope and would require clarification to remind the reader of its purpose further along in the code. A scope in which there are many variables, or variables that represent similar values or concepts, may necessitate longer variable names than the scope suggests.

The specificity of the concept can also help to keep a variable’s name concise. For example, assuming there is only a single database in use, a short variable name like db that might normally be reserved for very small scopes may remain perfectly clear even if the scope is very large. In this case, a single word database is likely acceptable based on the size of the scope, but is not required as db is a very common shortening for the word with few alternate interpretations.

The name of a local variable should reflect what it contains and how it is being used in the current context, rather than where the value originated. For example, it is often the case that the best local variable name is not the same as the struct or protocol buffer field name.

In general:
- Single-word names like `count` or `options` are a good starting point.
- Additional words can be added to disambiguate similar names, for example `userCount` and `projectCount`.
- Do not simply drop letters to save typing. For example `Sandbox` is preferred over `Sbx`, particularly for exported names.
- Omit `types and type-like` words from most variable names.
    - For a number, `userCount` is a better name than `numUsers` or `usersInt`.
    - For a slice, `users` is a better name than `userSlice`.
    - It is acceptable to include a type-like qualifier if there are two versions of a value in scope, for example you might have an input stored in `ageString` and use `age` for the parsed value.
- Omit words that are clear from the surrounding context. For example, in the implementation of a UserCount method, a local variable called userCount is probably redundant; count, users, or even c are just as readable.

#### <b>Single-letter variable names</b>

Single-letter variable names can be a useful tool to minimize repetition, but can also make code needlessly opaque. Limit their use to instances where the full word is obvious and where it would be repetitive for it to appear in place of the single-letter variable.

In general:
- For a method receiver variable, a one-letter or two-letter name is preferred.
- Using familiar variable names for common types is often helpful:
  - r for an io.Reader or *http.Request
  - w for an io.Writer or http.ResponseWriter
- Single-letter identifiers are acceptable as integer loop variables, particularly for indices (e.g., i) and coordinates (e.g., x and y).
- Abbreviations can be acceptable loop identifiers when the scope is short, for example for _, n := range nodes { ... }.


#### <b>Repetition</b>
A piece of Go source code should avoid unnecessary repetition. One common source of this is repetitive names, which often include unnecessary words or repeat their context or type. Code itself can also be unnecessarily repetitive if the same or a similar code segment appears multiple times in close proximity.

Repetitive naming can come in many forms, including:

<b>Package vs. exported symbol name</b>

When naming exported symbols, the name of the package is always visible outside your package, so redundant information between the two should be reduced or eliminated. If a package exports only one type and it is named after the package itself, the canonical name for the constructor is New if one is required.

Examples: Repetitive Name -> Better Name

- widget.NewWidget -> widget.New
- widget.NewWidgetWithName -> widget.NewWithName
- db.LoadFromDatabase -> db.Load
- goatteleportutil.CountGoatsTeleported -> gtutil.CountGoatsTeleported or goatteleport.Count
- myteampb.MyTeamMethodRequest -> mtpb.MyTeamMethodRequest or myteampb.MethodRequest

<b>External context vs. local names</b>
Names that include information from their surrounding context often create extra noise without benefit. The package name, method name, type name, function name, import path, and even filename can all provide context that automatically qualifies all names within.

```
// Bad:
// In package "ads/targeting/revenue/reporting"
type AdsTargetingRevenueReport struct{}

func (p *Project) ProjectName() string
```

```
// Good:
// In package "ads/targeting/revenue/reporting"
type Report struct{}

func (p *Project) Name() string
```

```
// Bad:
// In package "sqldb"
type DBConnection struct{}
```

```
// Good:
// In package "sqldb"
type Connection struct{}
```

```
// Bad:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    adsTargetingID := in.GetAdsTargetingID()
}
```

```
// Good:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    id := in.GetAdsTargetingID()
}
```

```
Repetition should generally be evaluated in the context of the user of the symbol, rather than in isolation. For example, the following code has lots of names that may be fine in some circumstances, but redundant in context:
```

### X. Others
<b>Pointer Receiver</b>

Example: func(ri *ResearchInfo)

Methods defined with a pointer receiver can modify the receiver (ri) directly. When you call a method on a pointer to a ResearchInfo instance, Go automatically dereferences the pointer for you, allowing the method to modify the original ResearchInfo instance.

<b>Mutability</b>: Methods with pointer receivers are typically used when you need to modify the state of the receiver struct (ResearchInfo) within the method. This is because passing a pointer allows the method to operate on the actual instance of ResearchInfo, rather than a copy of it.

```Go
type ResearchInfo struct {
    Title string
    Author string
}

// Method with pointer receiver
func (ri *ResearchInfo) UpdateTitle(newTitle string) {
    ri.Title = newTitle
}
```
When you call ri.UpdateTitle("New Title") on a ResearchInfo instance (ri), changes made inside the method (UpdateTitle) will modify the original ri.

<b>Value Receiver</b>

Example: func (t Tray)

Methods defined with a value receiver receive a copy of the Tray instance. Any modifications made to the receiver inside the method are local to the method and do not affect the original instance passed to it.

<b>Immutability</b>: Methods with value receivers are useful when you want to ensure that the method cannot modify the original state of the Tray instance. The method operates on a copy of the struct, leaving the original unchanged.

```
type Tray struct {
    width string
    height string
}

// Method with value receiver
func (t Tray) PrintInfo() {
    fmt.Printf("Width: %s, Height: %s\n", t.width, t.height)
}
```

When you call t.PrintInfo(), it prints the Width and Height of the Tray instance t, but modifications to t inside PrintInfo do not affect the original t.

<b>Variable name vs. type</b>
 
 The compiler always knows the type of a variable, and in most cases it is also clear to the reader what type a variable is by how it is used. It is only necessary to clarify the type of a variable if its value appears twice in the same scope.

<table>
<tr>
    <th>Repetitive Name</th>
    <th>Better Name</th>
</tr>
<tr>
    <td>var numUsers int</td>
    <td>var users int</td>
</tr>
<tr>
    <td>var nameString string</td>
    <td>var name string</td>
</tr>
<tr>
    <td>var primaryProject *Project</td>
    <td>var primary *Project</td>
</tr>
</table>

If the value appears in multiple forms, this can be clarified either with an extra word like raw and parsed or with the underlying representation:

```
// Good:
limitStr := r.FormValue("limit")
limit, err := strconv.Atoi(limitStr)
```


```
// Good:
limitRaw := r.FormValue("limit")
limit, err := strconv.Atoi(limitRaw)
```