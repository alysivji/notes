# Head First Design Patterns

By Eric Freeman and Elisabeth Robson

#### Table of Contents

<!-- TOC -->

- [Book](#book)
  - [Chapter 1: Introduction to Design Patterns](#chapter-1-introduction-to-design-patterns)
- [Design Principles](#design-principles)
- [Patterns](#patterns)
  - [Strategy Pattern](#strategy-pattern)

<!-- /TOC -->

## Book

### Chapter 1: Introduction to Design Patterns

- all patterns provide a way to let some part of your system vary independently of all other parts
- design patterns give you a shared vocabulary with other developers. Once you've got the vocabulary, you can more easily communicate with other develoeprs and inspire those who don't know patterns to start learning them. It also elevates your thinking about architectures by letting you think at the pattern level, not the nitty-gritty object level
- patterns allow you to say more with less. When you use a pattern in a description, other developers quickly know precisely the design you have in mind
- talkign at the pattern level allows you to stay in the design longer; high-level discussions make design meetings easier to understand
- shared vocabularies can turbo-charge your development team
- design patterns don't go directly int your code, they first go into your brain. Once you've got a working knowledge of patterns, you can apply them to new designs and rework old code

## Design Principles

#### Identify the aspect of your application that vary and separate them from what stays the same

- take parts that vary and encapsulate them so that later you can alter or extend the parts that vary without affecting those that don't

#### Program to an interface, not an implementation

- program to a supertype

#### Favor composition over inheritance

- `HAS-A` is better than `IS-A`

## Patterns

### Strategy Pattern

- Defines a familiy of algorithms, encapsulates each one, and makes them interchangeable.
- Lets the algorithm vary independently from clients that use it.
