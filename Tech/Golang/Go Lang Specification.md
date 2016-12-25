# Go Lang Specification

## Introduction

- Go is *strongly typed* and *garbage-collected* and has explicit support for *concurrent programming*.
- Go programs are constructed from *packages*, whose properties allow efficient management of dependencies.
- The existing implementations use traditional *compile/link* model to generate the executable binaries.

## Notation

- The syntax is specified using **Extended Backus-Naur Form (EBNF，扩展巴科斯范式)**. 

## Source Code Representation

Source code is Unicode text encoded in **UTF-8**.

## Lexical Element

### Comments

1. **Line comments** : `// comments`
2. **General comment**: `/* comments */`

> Comments do not nest.

### Tokens

Tokens form the vocabulary of the Go language. There are four classes:
- identifies
- keywords
- operators and delimiters
- literals

### Semicolons

### Identifiers

identifiers name program entities such as **variables** and **types**. A identifier is a sequences of one or more letters and digits. 

> The first character in an identifier must be a letter.


## Types

A type determines the set of **values** and **operations** specific to values of that type.

