---
author: Zihui
pubDatetime: 2023-07-31T0:00:00Z
title: How to use lldb
postSlug: how-to-use-lldb
featured: true
draft: false
tags:
  - debug
  - C
  - C Plus Plus
ogImage: ""
description: LLDB is an alternative for GDB on M1 chips.
---

### Start and set breakpoints

1. run lldb: `lldb <filename>` (the executable one, not xx.c)
2. run the executable file: `run` or `r`
3. set breakpoint:
   1. on a line:
      `break set -f <c or c++ filename> -l <row number>` or `b <c or c++ filename> : <linenumber> `
   2. on symbols:
      1. on a function: `b square` (square is a function that takes in an int)
      2. on a class method: `b Demo::demo()`
      3. inside a namespace: `b LLDBDemo::add(int,int)`
4. list all the breakpoints: `br list`
5. delete all the breakpoitns: `br del`

### Navigate through the code

1. next line:`n`
2. step into(e.g. step into a function when it is called): `s`
3. print the value of a variable: `p <variable_name>`

### Inspecting Variables

1. print variable content: `p <varName>`
2. frame variables: `frame variable` or `fr v`
3. current line inside the code:`frame select`

#### Backtrace and frames

1. backtrace: `bt` all frames currently called(like a stack view)
2. switching frames: `frame select <frame number>` `fr <frame number>`

### Using Watchpoints

**Program must be running in order to set the watch point**

1. global variable:
   1. `watchpoint set variable <variable name>` or `w s v <variable name>`
   2. `watchpoint set variable -w read | write |read_write <variable name>`
2. member variable
   1. `b main`
   2. `w s v <d.memberVar>`

### Others

1. quit: `quit`
2. kill process: `kill`
