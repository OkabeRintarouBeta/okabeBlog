---
author: zihui
pubDatetime: 2022-09-23T15:22:00Z
title: Effective C++ notes
postSlug: effective-c++1
featured: true
draft: false
tags:
  - docs
ogImage: ""
description: Notes for the first chapter of book effective C++
---

Here are some rules/recommendations, tips & ticks for creating new posts in AstroPaper blog theme.

## Table of contents

## Introduction

### Declaration and Definition

#### Declaration

> A declaration tells compilers about the name and type of something, but it omits certain details

```C++
    extern int x;
    std::size_t numDigits(int number);
    class Widget;
    template<typename T>;
    class GraphNode;
```

> Declaration reveals its **signature**,i.e., its parameter and return types.

#### Definition

> A definition provides compilers with the details a declaration omits.
> **Initialization** is the process of giving an object its first value. For ob- jects generated from structs and classes, initialization is performed by constructors.

## Chapter 1: Accustom yourself to C++

### C++: a federation of related languages

- C
- Object-Oriented C++: classes (including constructors and destructors), encapsulation, inheritance, polymorphism, virtual functions (dynamic binding), etc.
- Template C++
- the STL

### Prefer consts, enums and inlines to #define

- Risk of `#define`:
  - e.g. `#define APSECT_RATIO 1.653`
    > the symbolic name ASPECT_RATIO may never be seen by compilers; it may be removed by the preprocessor before the source code ever gets to a compiler.
    > As a result, the name ASPECT_RATIO may not get entered into the symbol table. This can be confusing if you get an error during compilation involving the use of the constant, because the error message may refer to 1.653, not ASPECT_RATIO. If ASPECT_RATIO were defined in a header file you didn’t write, you’d have no idea where that 1.653 came from, and you’d waste time tracking it down.
    > This problem can also crop up in a symbolic debugger, because, again, the name you’re programming with may not be in the symbol table.
- How to change?
  1. use const
  ```C++
  const double AspectRatio=1.653;
  // uppercase names are usually for macros, hence the name change
  ```
  > May yeild smaller code than `#define`, just one copy
  - Things to notice when using const:
    - defining const pointers
      - it’s important that the pointer be declared const, usually in addition to what the pointer points to when using `char*`
        - `const char * const authorName = "Scott Meyers";`
      - or, better, `const std::string authorName("Scott Meyers");`
    - class-specific constants
      - ensure there is at most one copy of the constant
      ```C++
          class GamePlayer { private:
          static const int NumTurns = 5;
          int scores[NumTurns];
          ... };
      ```
      _This is a delaration, not a definition._
      > Usually, C++ requires that you provide a definition for anything you use, but class-specific constants that are static and of integral type (e.g., integers, chars, bools) are an exception.
      - If you need to take the address of a constant, then
        ```C++
            const int GamePlayer::NumTurns;
        ```
  2. Use `enum`
  3.
