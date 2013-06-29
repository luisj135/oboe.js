

Applicaiton and Reflection
==========================

**40 to 60 pages**


Focus on replacing ajax, rather than streaming. In older browsers, getting the whole message
at once is no worse than it is now.

![
   Over several hops of aggregation, the benefits of finding the interesting parts early 
](images/timeline)


choice of technologies
----------------------
can justify why js as:

Most widely deployable.

Node: asynchronous model built into language already, no 'concurrent' library needed. Closures convenient for picking
up again where left off.

Node programs often so asynchronous and callback based they become unclear in structure.
Promises approach to avoid pyramid-shaped code and callback spaghetti. 

```javascript
// example of pyramid code
```  

In comparison to typical Tomcat-style threading model. Threaded model is powerful for genuine parallel computation but
Wasteful of resources where the tasks are more io-bound than cpu-bound. Resources consumed by threads while doing nothing
but waiting. 
  
Compare to Erlang. Waiter model. Node resturaunt much more efficient use of expensive resources.


funcitonal, pure functional possible [FPR] but not as nicely as in a pure functional language, ie function caches
although can be implemented, not universal on all functions.  

easy to distribute softare (npm etc)

summary of json
---------------

Why json?

A bridge between languages that isn't too different from the common types in the languages themselves
a common bridge between languages

Also very simple. Easy to parse.

identifying interesting objects in the stream
---------------------------------------------

The failure of sax: requires programmer to do a lot of work to identify interesting things. Eg, to find tag address
inside tag person with a given name, have to recognise three things while reieving a callback for every single element
and attribute in the document. As a principle, the programmer should only have to handle the cases which are interesting
to them, not wade manually through a haystack in search of a needle, which means the library should provide an expressive
way of associating the nodes of interest with their targetted callbacks.  

First way to identify an interesting thing is by its location in the document. In the absense of node typing beyond
the categorisation as objects, arrays and various primative types, the key immediately mapping to the object is often
taken as a lose concept of the type of the object. Quite fortunately, rather than because of a well considered object 
design, this tends to play well with automatically marshaling of domain objects expressed in a Java-style OO language
because there is a stong tendency for field names -- and by extension, 'get' methods -- to be named after the *type*
of the field, the name of the type also serving as a rough summary of the relationship between two objects. See figure
\ref{marshallTypeFig} below.

![
   UML class diagram showing a person class in relationship with an address class. In implementation as
   Java the 'hasAddress' relationship would typically be reified as a getAddress method. This co-incidence of 
   object type and the name of the field referring to the type lends itself well to the tendency for the immediate
   key before an object to be taken as the type when Java models are marshaled into json \label{marshallTypeFig}
](images/marshall)      

By sensible convention, even in a serialisation format with only a loose definition of lists, lists contain only items 
of the same type. This gives way to a sister convention, that for lists of items, the key immediately linking to the
 

Essentially two ways to identify an interesting node - by location (covered by existing jsonpath) 

Why duck typing is desirable in absense of genuine types in the json standard (ala tag names in XML).  or by a loose concept of type
which is not well supported by existing jsonpath spec. Object syntax for listing fields.

```javascript

Examples of jsonpath syntax

```



 


breaking out of big/small tradeoff
----------------------------------

Best of both modes

Granularity: only need read as far as necessary. Services could be designed to write the big picture first.
Alternatively, where resources link to one another, can stop reading at the link. Eg, looking for a person's
publications, start with an index of people but no need to read whole list of people.

Aborting http request may not stop processing on the server. Why this is perhaps desirable - transactions, leaving
resources in a half-complete state.



styles of programming
---------------------

some of it is pure functional (jsonPath, controller)
ie, only semantically different from a Haskell programme
others, syntactically functional but stateful
to fit in with expected APIs etc

JsonPath implementation allows the compilation of complex expressions into an executable form, but each part implementing
the executable form is locally simple. By using recursion, assembling the simple functions into a more function expressing 
a more complex
rule also follows as being locally simple but gaining a usefully sophisticated behaviour through composition of simple 
parts. Each recursive call of the parser identifies one token for non-empty input and then recursively digests the rest.

The style of implementation of the generator of functions corresponding to json path expressions is reminiscent of
a traditional parser generator, although rather than generating source, functions are dynamically composed. Reflecting
on this, parser gens only went to source to break out of the ability to compose the expressive power of the language
itself from inside the language itself. With a functional approach, assembly from very small pieces gives a similar
level of expressivity as writing the logic out as source code.   



composition of several source files into a distributable binary-like text file
-------------------------------------------------

Why distributed javascript is more like a binary than a source file. Licencing implications?

Inherent hiding by wrapping in a scope.

Names of functions and variable names which are provably not possible to reference are lost for the sake of reduction
of size of the source. 

Packaging for node or browser. No need to minify for node but concatenation still done for ease of inclusion in projects


```javascript
typical pattern for packaging to work in either a node.js server or a web browser
```

automated testing
-----------------

How automated testing improves what can be written, not just making what is written more reliable.

![ 
   The testing pyramid is a common concept, relying on the assumption that verification of small parts provides
   a solid base from which to compose system-level behaviours. A Lot of testing is done on the low-level components
   of the system, whereas for the high-level tests only smoke tests are provided. \label{testingPyramidFig}
](images/pyramid)

Jstd can serve example files but need to write out slowly which it has no concept of. Customistation is via configuration
rather than by plug-in, but even if it were, the threading model is not suitable to create this kind of timed output.

Why jstd's built in proxy isn't sufficient. An example of a typical Java webserver, features thread-based mutlithreading
in which threads wait for a while response to be received.

Testing via node - slowserver. Proxy.

The test pyramid concept \ref{testingPyramidFig} fits in well with the hiding that is provided. Under the testing pyramid only very high level
behaviours are tested as ??? tests. While this is a lucky co-incidence, it is also an unavoidable restriction.
Once compiled into a single source file, the
individual components are hidden, callable only from withing their closure. Hence, it would not be possible to test
the composed parts individually post-concatenation into a single javascript file, not even via a workarround for data
hiding such as found in Java's reflection. Whereas in Java the protection is a means of protecting otherwise addressable
resources, once a function is trapped inside a javascript closure without external exposure it is not just protected 
but, appearing in no namespaces, inherently unreferenceable.

TDD fits well into an object pattern because the software is well composed into separate parts. The objects are almost
tangible in their distinction as separate encapsulated entities. However, the multi-paradigm style of my implementation 
draws much fainter borders over the implementation's landscape.



One dilemma in implementing the testing is how far to test the more generic sections of the codebase as generic components.
A purist approach to TDD would say    


Could implement a resume function for if transmission stops halfway 

```javascript      
   .onError( error ) {
      this.resume();
   }
```      

stability over upgrades
-----------------------

why jsonpath-like syntax allows upgrading message semantics without causing problems [SOA]
how to guarantee non-breakages?
could publish 'supported queries' that are guaranteed to work



support for older browsers
--------------------------

Still works as well as non-progressive json
Could be used for content that is inherently streaming (wouldn't make sense without streaming)


suitability for databases
-------------------------

Databases offer data one row at a time, not as a big lump.


weaknesses
----------
implementation keeps 'unreachable' listeners
difficult decidability/proof type problem to get completely right
but could cover most of the easy cases

Parse time for large files spread out over a long time. Reaction to parsed content spread out over a long time, for
example de-marshalling to domain objects. For UX may be preferable to have many small delays rather than one large one. 

Doesn't support all of jsonpath. Not a strict subset of the language.

Rest client as a library is passing mutable objects to the caller.
too inefficient to re-create a new map/array every time an item is 
not as efficient in immutability as list head-tail type storage

An imutability wrapper might be possible with defineProperty. Can't casually overwrite via
assignment but still possible to do defineProperty again.

Would benefit from a stateless language where everything is stateless at all times to 
avoid having to program defensively. 
