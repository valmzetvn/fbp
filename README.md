FBP flow definition language parser
===================================

The *fbp* library provides a parser for the [FBP domain-specific language](https://github.com/bergie/noflo#language-for-flow-based-programming) used for defining graphs for flowbased programming environments like [NoFlo](http://noflojs.org).

## Language for Flow-Based Programming

FBP is a Domain-Specific Language (DSL) for easy graph definition. The syntax is the following:

* `'somedata' -> PORT Process(Component)` sends initial data _somedata_ to port _PORT_ of process _Process_ that runs component _Component_
* `A(Component1) X -> Y B(Component2)` sets up a connection between port _X_ of process _A_ that runs component _Component1_ and port _Y_ of process _B_ that runs component _Component2_

You can connect multiple components and ports together on one line, and separate connection definitions with a newline or a comma (`,`). 

Components only have to be specified the first time you mention a new process. Afterwards, simply append empty parentheses (`()`) after the process name.

Example:

```fbp
'somefile.txt' -> SOURCE Read(ReadFile) OUT -> IN Split(SplitStr)
Split() OUT -> IN Count(Counter) COUNT -> IN Display(Output)
Read() ERROR -> IN Display()
```

### Exporting ports

When FBP-defined graphs are used as subgraphs in other flows, it is often desirable to give more user-friendly names to their available ports. In the FBP language this is done by `EXPORT` statements.

Example:

```fbp
EXPORT=READ.IN:FILENAME
Read(ReadFile) OUT -> IN Display(Output)
```

This line would export the *IN* port of the *Read* node as *FILENAME*.

### Node metadata

It is possible to append metadata to Nodes when declaring them by adding the metadata string to the Component part after a colon (`:`).

Example:

```fbp
'somefile.txt' -> SOURCE Read(ReadFile:main)
Read() OUT -> IN Split(SplitStr:main)
Split() OUT -> IN Count(Counter:main)
Count() COUNT -> IN Display(Output:main)
Read() ERROR -> IN Display()
```

In this case the route leading from *Read* to *Display* through *Split* and *Count* would be identified with the string *main*.
