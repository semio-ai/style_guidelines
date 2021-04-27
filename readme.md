# Semio Coding Style Guide

# Definitions and Conventions

This document follows [RFC2119](https://tools.ietf.org/html/rfc2119) for requirement specification, including definitions for MUST (NOT), SHOULD (NOT), and MAY.

# API-level Guidelines

## *State is the root of all evil*

Program complexity is directly related to the amount of mutable state.

- Functions and methods **SHOULD** generally be written in a functional style. `f(a, b)` should always produce `c` for the same `a`, `b`.

- Classes **SHOULD** be immutable. Setter methods that mutate object state after construction, particularly in public interfaces, are highly discouraged. To perform a mutation, return a new object with the mutation applied.

- Const-correctness matters. Methods that do not mutate their object **MUST** be marked const. Variables that do not mutate **MUST** be marked const. Function/method paramenters **SHOULD** be marked const, even if not a reference/pointer. Variable declarations in functions/methods **SHOULD** be marked `const`.

- Classes **SHOULD** contain no more than a few instance variables. Refactor large classes into many classes that generalize behavior into reuseable components.

## *Objects should do one thing, and do it well*

- Classes **SHOULD NOT** combine multiple unrelated behaviors.

- Data structures and algorithms **SHOULD** be generic, time permitting.

## *Composition over inheritance*

- Abstract classes (e.g., classes with both regular and pure virtual methods) **MUST NOT** be used, unless required for type erasure. Pure virtual base classes (e.g., classes with only pure virtual methods and no instance variables) (i.e., *interfaces*) are fine.

# Syntax-level Guidelines

## Type Names

Classes, structs, and typedefs **MUST** be `UpperCamelCase`. Initialisms and acronyms do not override this rule (e.g. `Pdf`, not `PDF`). The keywords `public`, `protected`, and `private` **SHOULD NOT** be indented. As a result of the composition over inheritance rules above, `protected` **SHOULD NOT** be used in almost all circumstances.

```.cpp
class MyGreatClass
{
public:
  struct Data
  {

  };

  typedef int A;
};

```

## Variable Names

Variables **MUST** be `lower_snake_case`. Private instance variables **MUST** be suffixed by `_`.

### Example
```.cpp
class A
{
public:
  int variable;

private:
  bool my_weird_flag_;
};
```

## Functions and Method Names

Functions and methods **MUST** be `lowerCamelCase`. Initialisms and acronyms do not override this rule (e.g. `printPdf`, not `printPDF`).

Private methods **MUST** be suffixed by `_`.

### Example

```.cpp
class A
{
public:
  std::size_t publicFunction();

private:
  void privateFunction_();
}
```

## Indentation and Spacing

- Code **MUST** use a two space tab stop. Brackets should be placed on newlines, unless the bracket inside the arguments of a function/method call.

- One space **MUST** be placed after all keywords. 

- Pointers and references are right associative. A space **MUST** be placed between the type and the reference or pointer character (e.g., `int *a`, not `int* a`). Some more complex examples of this rule:
  - `int *const a`
  - `const int *const b`
  - `std::vector<double> &&values`
  - `Args &&...args`
  - `const Image &image`

- One space **MUST** be placed on either side of an infix operator (e.g., `a + b`).

- Spaces **MUST NOT** be placed between prefix/suffix operators and the operand (e.g., `!a`, `f()`).

- Spaces **SHOULD NOT** be placed between parentheses and the expression (e.g., `(a + b)`, not `( a + b )`).

- Spaces **SHOULD** be placed between curly brackets (e.g., initializer lists) and the expression (e.g., `{ 1, 2, 3 }`).

### Example

```.cpp
void callFunction(const std::function<void ()> &f)
{
  f();
}

int main(int argc, char *argv[])
{
  // Note the space after if
  if (argc != 2) return 1;

  callFunction([]() {
    std::cout << "Hello, World!" << std::endl;
  });

  return 0;
}
```

## Files

- C++ headers **MUST** use the extension `.hpp`.

- C++ sources **MUST** use the extension `.cpp`.

- One class **SHOULD** be declared per header/source file. These files **MUST** be the name of the class in `UpperCamelCase`.

- C++ headers/sources with functions, variables, and typedefs (e.g., math helpers), **MUST** be a descriptive name in `lower_snake_case`. 

- Files in a `namespace` **MUST** be in a folder of the same name (e.g., `namespace semio` members should be in a `semio` folder).

- Public headers **MUST** be placed in a top-level `include` directory. Private headers are sources **MUST** be placed in a top-level `src` directory.

## Namespaces

Namespaces **MUST** be `lower_snake_case`.

`using namespace` **SHOULD NOT** be used in the global scope, unless it is the namespace of the header being implemented. The exception applies recursively to all subnamespaces.

### Example
`include/semio/Class.hpp`:
```.cpp
#ifndef _SEMIO_CLASS_HPP_
#define _SEMIO_CLASS_HPP_

namespace semio
{
  class Class
  {
  public:
    Class();
  };
}

#endif
```

`src/semio/Class.cpp`:
```.cpp
#include "semio/Class.hpp"

using namespace semio;

Class::Class()
{
}
```

## Primitives

STL primitives typedefs **SHOULD** be used over their keyword counterparts.

### Examples

```.cpp
// unsigned long
std::uint64_t value;

// signed int
std::int32_t integer;

std::size_t length;
```

# Compilation Guidelines

- `-Werror` (i.e., warnings are errors) **SHOULD** be used, particularly in production software.

- Release builds **SHOULD** be compiled with `-Ofast` (i.e., `-O3` with fast math).
