# Scope/Closure

Notes from "You Don't Know JS"



## Compiler Theory

- Javascript <u>is</u> a compiled language.
- This typically happens immediately before it is invoked or during.
- Other languages, like C, do this before running. It is called **compilation** and typicaly consists of three steps:
  - **Tokenizing/Lexing**
    - Breaking up characters into chunks called tokens
      - Ex: var a=2; would equal the following tokens: var, a, =, 2, ;
  - **Parsing**
    - Taking an array of tokens and turning into a tree of nested elements called the abstract syntax tree AST.
  - **Code Generation**
    - Taking the AST and turning into executable code
    - basically this is where the variable is actually created but in machine language
- Again, JS doesn't compile this ahead of time, but immediately before execution

