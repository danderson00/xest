# `@xest` Tutorial - Part 10 - Conclusion

We're at the end of our tutorial! This last part draws together some key points to consider and discusses the current
and future states of the `xest` platform.

## A Word On Complexity

A reduction in the complexity of building applications has always been a goal of the `xest` platform. Taking a step 
back and looking at our online shop as a whole, there are some significant benefits over more traditional development
approaches that become apparent.

### Simpler, More Focused Code

And less of it! Let's take our online shop as case in point:

- "Data access" is located in a single file (`vocabulary.js`) and consists of 34 lines of simple query logic.
- All of our UI components are simple function components, and refactoring is a breeze
- There are far fewer concepts to understand - consumers never need to see low level concepts like router, 
  subscriptions, HTTP, socket, etc.  

### Decoupling Of Responsibilities

Changing our fundamental data building blocks from tables or documents to simple messages allows us to clearly
separate the core responsibilities of an application:

- Models are simple derivations from messages, declared in a flexible, intuitive, centralised fashion.
- Process flows become explicit and visual, derived from messages and centrally declared rather than hidden and 
  scattered across a codebase.
- UI components are simple data components that display models or capture events - nothing else.

The creation of a new model in `xest` is as simple as declaring an expression, no heavy infrastructure like tables or
documents, removing the temptation to reuse the same model for functionally different purposes. This leads to lower
coupling between system components and higher cohesion within those modules.

### Everything Is Distributed in Real Time

This pretty much speaks for itself. Our models are distributed naturally as changes occur without a single line of 
code. When `xstate` is properly integrated, our process flows will be the same, even UI flows can be shared between
consumers.

### Everything is Declarative

This may seem like a minor detail, but the declarative nature of expressions and state charts means that a machine can 
extract useful information for purposes such as optimisation and scalability. 

### Complete History Of Everything

Capturing every event within a system provides a rich source of analytics. Every step a user takes through your system
is captured and available for analysis for usability studies, optimisations and auditing. Notably, this is a huge
benefit for debugging as the complete set of steps for reproducing failures is available. 

## Roadmap

We're incredibly excited about the future of `xest`. Among a whole stack of improvements, the immediate future consists 
of implementing:

- deeper integration with `xstate`
- message publishing constraints
- better indexing and queryability

Longer term, we're looking at using TypeScript to enable the creation of richer vocabularies, better compile time
validation and the ability to extract metadata from expressions to power visual tools. We're also looking at building
a set of visual design tools to further reduce the complexity of building applications.

We hope you enjoyed this tutorial and are looking forward to seeing what you build with `xest`!