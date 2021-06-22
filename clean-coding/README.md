# Clean Code

- [Clean Code](#clean-code)
- [1. Naming](#1-naming)
  - [1.1. Use Intention-Revealing Names](#11-use-intention-revealing-names)
  - [1.2. Avoid Disinformation](#12-avoid-disinformation)
  - [1.3. Make Meaningful Distinctions](#13-make-meaningful-distinctions)
  - [1.4. Use Pronounceable Names](#14-use-pronounceable-names)
  - [1.5. Use Searchable Names](#15-use-searchable-names)
  - [1.6. Class Names](#16-class-names)
  - [1.7. Method Names](#17-method-names)
  - [1.8. Pick One Word per Concept](#18-pick-one-word-per-concept)
  - [1.9. Pick One Word per Purpose](#19-pick-one-word-per-purpose)
  - [1.10. Use Solution Domain Names](#110-use-solution-domain-names)
  - [1.10. Use Problem Domain Names](#110-use-problem-domain-names)
  - [1.11. Add Meaningful Context](#111-add-meaningful-context)
- [2. Functions](#2-functions)
  - [2.1. Functions Should Be Small](#21-functions-should-be-small)
  - [2.2. Functions Should Do One Thing](#22-functions-should-do-one-thing)
  - [2.3. One Level of Abstraction per Function](#23-one-level-of-abstraction-per-function)
  - [2.4. Functions Must be Readable Top-to-Bottom](#24-functions-must-be-readable-top-to-bottom)
  - [2.5. Switch Statements](#25-switch-statements)
  - [2.6. Use Descriptive Names](#26-use-descriptive-names)
  - [2.7. Few Arguments](#27-few-arguments)
  - [2.8. Functions Should Have No Side Effects](#28-functions-should-have-no-side-effects)
  - [2.9. Command Query Separation](#29-command-query-separation)
  - [2.10. Error Handling](#210-error-handling)
- [3. Comments](#3-comments)
  - [3.1. Comments Do Not Make Up for Bad Code](#31-comments-do-not-make-up-for-bad-code)
  - [3.2. Good Comments](#32-good-comments)
    - [3.2.1. Legal Comments](#321-legal-comments)
    - [3.2.2. Explanation of Intent](#322-explanation-of-intent)
    - [3.2.3. Warning of Consequences](#323-warning-of-consequences)
    - [3.2.4. TODO Comments](#324-todo-comments)
  - [3.3. Bad Comments](#33-bad-comments)
    - [3.3.1. Redundant Comments](#331-redundant-comments)
    - [3.3.2. Mandated Comments](#332-mandated-comments)
    - [3.3.3. Commented-Out Code](#333-commented-out-code)
- [4. Formatting](#4-formatting)
  - [4.1. Vertical Formatting](#41-vertical-formatting)
    - [4.1.1. The Newspaper Metaphor](#411-the-newspaper-metaphor)
    - [4.1.2. Vertical Openness Between Concepts](#412-vertical-openness-between-concepts)
    - [4.1.3. Vertical Density](#413-vertical-density)
    - [4.1.4. Vertical Distance](#414-vertical-distance)
    - [4.1.5. Variable Declarations](#415-variable-declarations)
    - [4.1.6. Dependent Functions](#416-dependent-functions)
    - [4.1.7. Conceptual Affinity](#417-conceptual-affinity)
  - [4.2. Horizontal Formatting](#42-horizontal-formatting)
    - [4.2.1. Horizontal Openness and Density](#421-horizontal-openness-and-density)
  - [4.3. Conventions](#43-conventions)

# 1. Naming

## 1.1. Use Intention-Revealing Names

The name of a variable, function, or class, should tell you **why it exists**, **what it does**, and **how it is used**. If a name requires a comment, then the name does not reveal its intent.

```python
days_since_creation: int
get_flagged_cells() -> List[Cell]
```

## 1.2. Avoid Disinformation

Avoid words whose meanings vary from our intended meaning.

- Do not refer to a grouping of accounts as `account_list` unless it's actually a `List`.
- Avoid using names which vary in small ways. Ex: `ABCThisIsMySuperClass` and `ABCThisIsMySubClass`

## 1.3. Make Meaningful Distinctions

Noise words are redundant, and can lead to confusion and misuse.

- `variable` should never appear in a variable name.
- `table` should never appear in a table name.
- `moneyAmount` is indistinguishable from `money`, or `customerInfo` from `customer`

## 1.4. Use Pronounceable Names

- Avoid naming conventions such as `DtaRcrd102` or `pszqint`

## 1.5. Use Searchable Names

Single-letter names and numeric constants are difficult to locate across a body of text.

Single-letter names should ONLY be used as local variables inside short methods.

## 1.6. Class Names

Classes and objects should have noun on noun phrase names like `Customer`, `WikiPage`, `Account` and `AddressParser`. A class name should not be a verb.

## 1.7. Method Names

- Methods should have verb or verb phrase names like `post_payment`, `delete_page` or `save`.
- Accessors, Mutators and Predicates should be named for their value and prefixed with `get`, `set`, and `is`, respectively.
- When constructors are overloaded, use static factory methods with names that describe the arguments. Ex: `DataFrame.from_records()`

## 1.8. Pick One Word per Concept

For each abstract concept, pick a word and stick with it.

- `fetch`, `retrieve` and `get` all mean the same abstract concept. Pick only one. Same thing with `controller`, `manager` and `driver`.

## 1.9. Pick One Word per Purpose

Related to the last point, avoid using the same word for different purposes.

- If `add` deals with adding or concatenating numbers, avoid using `add` to append items to a list. Use `append` or `insert` instead.

## 1.10. Use Solution Domain Names

When applicable, use computer science terms, algorithm names, pattern names, math terms and so forth.

- `AccountVisitor` for a `Visitor` pattern implementation
- `JobQueue` for a `queue` that stores jobs.

## 1.10. Use Problem Domain Names

When applicable, use problem domain names. The code that has more to do with problem domain concepts should have names drawn from the problem domain.

## 1.11. Add Meaningful Context

For a variable like `state`, context is often necessary to answer the question: "the state of *what*?".

Context can be added by prefixing, like `user_state`, or even better by creating a `User` class with a `state` attribute.

# 2. Functions

Remember that the reason we write functions is to decompose a larger concept into a set of steps at the next level of abstraction.

## 2.1. Functions Should Be Small

- Functions should rarely be over 20 lines long.
- Functions should be transparently obvious.
- Functions should tell a story.
- Each function should lead you to the next in a compelling order.
- Blocks within `if`, `else` and `while` statements should be one line long. Probably that line should be a function call.
- Functions should not be large enough to hold nested structures. Therefore, the indent level of a function should not be greater than one or two.

```python
def render_page_with_setups_and_teardowns(page_data, is_suite):
    if is_test_page(page_data):
        include_setup_and_teardown_pages(page_data, is_suite)
    return page_data.get_html()
```

## 2.2. Functions Should Do One Thing

If the function does only those steps that are one level of abstraction below the stated name of the function, then the function is doing one thing.

We can see this by expressing the purpose of the above function:

> To *RenderPageWithSetupsAndTeardowns*, we check to see whether the page *is a test page* and if so, we *include the setups and teardowns*. In either case we *render the page in HTML*.

Another way to know that a function is doing more than one thing is if you can extract another function from it with a name that is not merely a restatement of its implementation. For example, if we redefine the function above as:

```python
def render_page_with_setups_and_teardowns(page_data, is_suite):
    include_setup_and_teardown_pages_if_test_page(page_data, is_suite)
    return page_data.get_html()
```

we're merely restating the `if` block.

## 2.3. One Level of Abstraction per Function

We need to make sure that the statements within our function are all at the same level of abstraction. Avoid mixing concepts of different levels of abstraction.

- High level: `get_html()`
- Intermediate level: `page_path_name = PathParser.render(page_path)`
- Low level: `.append("\n")`

Mixing levels of abstraction within functions is always confusing. Readers may not be able to tell whether a particular expression is an essential concept or a detail.

## 2.4. Functions Must be Readable Top-to-Bottom

We want the code to read like a top-down narrative. We want every function to be followed by those at the next level of abstract so that we can read the program, descending one level of abstraction at a time as we read down the list of functions.

```
To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.

    To include the setups, we include the suite setup if this is a suite, then we include the regular setup. 

    To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page and add an include statement with the path of that page.
    
    To search the parent...
```

## 2.5. Switch Statements

Switch statements are tricky. By their very nature they do *N* things.

The general solution when using `switch` statements is to bury them in the basement of an `Abstract Factory`, and never let anyone see it. The factory will use the `switch` statement to create appropriate instances of the classes and functions it is meant to create.

## 2.6. Use Descriptive Names

- A long descriptive name is better than a short enigmatic name with a long descriptive documentation.
- Be consistent in your names. Use the same phrases, nouns, and verbs in the function names you choose for your modules. Ex: `includeSetupPages` and `includeSuiteSetupPage`

## 2.7. Few Arguments

Functions should have as few arguments as possible. Triadic (three-argument) functions should be avoided where possible.

- Monadic forms are valid in functions like `file_exists` or `open_file`
- Flag arguments are bad practices. It complicates the signature of the method and proclaims that the function does more than one thing. It does one thing if the flag is true, and another if the flag is false.
- Dyadic functions are harder to understand than monadic functions. But they can often be converted into monadic functions by making them methods of a `class` that takes the other argument in its constructor.
- Triadic functions are significantly harder to understand than dyads. Usually, this means that some of those arguments should be wrapped into a class of their own and the object should be passed instead.

```python
# dyadic function
writeField(output_stream, name)

# convert to monadic function
output_stream = OutputStream(stream)
output_stream.writeField(name)
```

```python
# triadic function
make_circle(x, y, radius)

# convert to diadic function
center = Point(x, y)
make_circle(center, radius)
```

## 2.8. Functions Should Have No Side Effects

Side effects are lies. Your function promises to do one thing, but it also does other *hidden* things. This can cause strange temporal couplings and order dependencies.

Watch out for unexpected changes to the variables of its own class, passed parameters, or system globals. If your function must change the state of something, have it change the state of its owning object. In other words, make it a method of the class.

## 2.9. Command Query Separation

Functions should either do something or answer something, but not both.

Either your function should change the state of the object, or it should return some information about the object.

## 2.10. Error Handling

Functions should do one thing. Error handling is one thing. Thus, a function that handles errors should do nothing else.

This implies that if the keyword `try` exists in a function, it should be the very first word in the function and that there should be nothing after the `catch/finally` blocks. Extract everything else into happy-path functions.

# 3. Comments

Comments are a necessary evil. Well-placed comments are helpful, dogmatic comments clutter up modules and inaccurate comments are extremely damaging. Comments are used to compensate for our failure to express ourselves in codes.

## 3.1. Comments Do Not Make Up for Bad Code

When you find yourself in a position where you need to write a comment, think it through and see if there is a way to express yourself in code.

```python
# check to see if the employee is eligible for full benefits
if employee.flags and HOURLY_FLAG and employee.age > 65:
    ...

# rewritten to make the code expressive
if employee.is_eligible_for_full_benefits():
    ...
```

## 3.2. Good Comments

Some comments are necessary or beneficial.

### 3.2.1. Legal Comments

Copyright and authorship statements.

```python
# LEGAL: Copyright (C) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.
# Released under the terms of the GNU General Public License version 2 or later.
```

### 3.2.2. Explanation of Intent

Comments that provide the intent behind a decision.

```python
# INTENT: Stricter offers are ranked higher because there is less competition for them.
```

### 3.2.3. Warning of Consequences

Used to amplify the importance of something that may otherwise seem inconsequential.

```python
# WARNING: Test case disabled because it takes a long time
# WARNING: Casting to float8 is required to avoid excessive memory consumption.
```

### 3.2.4. TODO Comments

To-Do notes are used to explain why the function has a degenerate implementation, or when a better solution can be implemented in the future.

```python
# TODO: Wrap the logic with a decorator
```

## 3.3. Bad Comments

Usually they are excuses for poor code or justifications for insufficient decisions, amounting to little more than the programmer talking to himself.

### 3.3.1. Redundant Comments

Comments that say exactly the same as the code.

```python
# the processor delay for this component
background_processor_delay = -1
```

### 3.3.2. Mandated Comments

Not every function must have a docstring, and not every variable must have a comment. Comments are technical debt, they need to be maintained and can create the potential for lies and misdirection.

```python
def add_CD(title: str, author: str):
    """
    Adds a new CD

    Args:
        title [str]: the title of the CD
        author [str]: the author of the CD
    """
    ...
```

### 3.3.3. Commented-Out Code

Simply raises the question: "why are those lines of code commented out?" - Are they important? Were they left as reminders for some imminent change? Or are they just garbage that no one ever cleaned up?

Even if you need them, source control will take care of that. Don't leave commented-out code.

# 4. Formatting

Formatting is important, it's about communication, which is the professional developer's first order of business.

## 4.1. Vertical Formatting

Small files are easier to understand than large files. Ideally, files should be between 200 and 500 lines long.

### 4.1.1. The Newspaper Metaphor

Source code files should be like a newspaper article.

- The name should be simple but explanatory, and by itself should be sufficient to tell us whether we are in the right module or not.
- The topmost parts of the source file should provide the high-level concepts and algorithms. Detail should increase as we move downward, until at the end we find the lowest level functions and details.

### 4.1.2. Vertical Openness Between Concepts

Vertical openness separates concepts. Blank lines should separate the package declaration, the imports, and each of the functions.

```python
import pandas as pd

class StringFormatter:

    def __init__():
        ...

    def format():
        ...
```

```python
import pandas as pd

class StringFormatter:
    def __init__():
        ...
    def format():
        ...
```

### 4.1.3. Vertical Density

Vertical density implies close association. So lines of code that are tightly related should appear vertically dense. Avoid breaking this density with useless comments.

```python
class ReporterConfig:

    # the class name of the reporter listener
    reporter_listener_class_name = 'EventListener'

    # the properties of the reporter listener
    listener_properties = []

    def add_property(self, property: Property):
        self.listener_properties.append(property)
```

```python
class ReporterConfig:

    reporter_listener_class_name = 'EventListener'
    listener_properties = []

    def add_property(self, property: Property):
        self.listener_properties.append(property)
```

### 4.1.4. Vertical Distance

Concepts that are closely related should be kept vertically close to each other, avoiding jumping up and down through the source file just to understand what the system does.

### 4.1.5. Variable Declarations

Variables should be declared as close to their usage as possible.

- Control variables for loops should usually be declared within the loop statement.
- Instance variables should be declared at the top of the class, or in the constructor. In well-designed classes they are used by many, if not all, of the methods of the class.

### 4.1.6. Dependent Functions

If one function calls another, they should be vertically close, and the caller should be above the callee. This gives the program a natural flow. Readers should be able to trust that function definitions will follow shortly after their use.

This is closely related to the idea of functions only dealing with one level of abstraction and having lower-level functions at the bottom. With well-designed functions, this flow should be natural.

### 4.1.7. Conceptual Affinity

Strong conceptual affinity should have small vertical distance. For example, `validation` functions, or `assertion` functions should be packed together in the same vertical space.

## 4.2. Horizontal Formatting

The Hollerith limit says the maximum line length should be 80 characters, but you could stretch it out to up to 120 characters.

### 4.2.1. Horizontal Openness and Density

We use horizontal whitespace to associate things that are strongly related and disassociate things that are more weakly related.

- Assignment operators should have whitespace around them.
- No whitespace between the function name and the opening parenthesis.
- Function arguments separated with a whitespace after the comma.

```python
a = b + c
return determinant(a, b, c)
```

## 4.3. Conventions

When working in a team, set up a collection of conventions to follow. The objective is to make the software have a consistent style. We don't want it to appear to have been written by a bunch of disagreeing individuals. These conventions include:

- Indent size
- Class naming
- Variable naming
- Method naming

