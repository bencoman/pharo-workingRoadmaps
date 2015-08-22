Sometimes people outside the Pharo community publish wish lists for their ideal programming language. 
It would be interesting to review these lists against Pharo to understand how we can satisfy people's needs, and occasionally identify items we deem worthy to adopt.

#A Wish List - David R. MacIver
http://www.drmaciver.com/2015/07/a-wish-list/

Here’s a laundry list of stuff that would feature in my dream language. Advance warning that I am a grumpy old man and this is a super boring list that contains almost no cool features. Also it’s not in any particular order of priority – it’s mostly the order I thought of things in – and it’s definitely not complete – it’s just the stuff I thought of before I got bored of writing this post.

Also, most of these are things that you can’t without get large amounts of time, effort and money. They’re boring, not easy, and if anything them being boring makes them harder because you can’t really get people excited about working on them.

### Community

Community is so important. Here is what I want out of a programming language community:

1. Large. Small communities are nice but I want a community who I can share the work load with, and a small community isn’t it.

2. Friendly. Elitism is toxic, and a community that isn’t helpful to beginners or goes on and on about how they’re super smart for using this language that other people don’t get is the worst.

3. Diverse, and committed to it. Codes of Conduct for everyone all round.

4. Committed to quality. Documentation matters. Testing matters. We *like* having high quality libraries and we’re prepared to put the work (and, where possible, money) in to get them.

### Packaging Infrastructure

Good packaging infrastructure is vital. And so hard to do. Basically nobody does it well. Packages should be:

1. Easy to create new packages. If a problem could be solved by creating a new package it should be easier to solve by creating a new package than by not. You should never find yourself going “oh god but I have to write all that XML”.

2. Versioned, with version constraints between dependencies automatically resolved

3. Local to a project
  * without a lengthy compile each time you install into a new project
  * no pollution in the global install namespace

4. Easy to mirror

5. But with a good standard central repository

6. Clearly marked for stability

7. Try hard at maintaining compatibility between package versions

8. Easy to write in a way that is portable to other versions of the language (e.g. don’t be Scala where a package compiled for one version of the language doesn’t work with any others, even between point releases)

9. Easy to write in a way that is compatible with multiple operating systems.

Most languages eventually get something which is approximately this. Cabal with sandboxes, pip with virtualenv, ruby with bundler, all manage most of this (mirroring is typically not handled well. I think maybe it is in Cabal but it’s not in python or ruby).

### Testing tools

1. There should be a standard test runner that works sufficiently well that nobody bothers writing their own unless they’re someone with an rspec fetish, and the community should politely suggest that maybe this isn’t adding very much to the testing workflow.

2. There should be good code coverage tools. They should work reliably with minimal overhead (if it takes twice as long to run under coverage then this is very sad and people will use it less). It should be able to do branch coverage. It would be great if it could do predicate coverage. More features – e.g. stats on paths and traces – would be amazing.

3. It would be fantastic to steal the CPAN feature that tests run on install and report back pass/fail information to somewhere sensible, which means you want testing integrated with the packaging system. Given the aforementioned versioning constraints and per project installs you probably only want to do this one per distinct set of versions of dependent libraries.

4. *Obviously* all languages should have a Quickcheck like testing tool (if I didn’t think this I probably wouldn’t have sunk more than six months of free full time labour into making Hypothesis).

### Good tools for working with source code

1. I’m mostly very indifferent to Go, but there’s one feature that I think it gets so very right and wish everyone else would steal right now. Your standard library should include a precise parser which for any valid (or, ideally, nearly valid) source code you can parse to an AST then print the AST as a bytewise identical file to the original.

2. It should also include a pretty printer that outputs code in a “standard” and correct format.

3. It should also be easy to make tools that use the AST representation to make changes to your source code.

4. Basically: I want good refactoring and reformatting tools, and in order to get that I want standard ways of building them.

5. I also want good static analysis tools. A well designed language obviates the need for a lot of these, but there’s always room for improvement.

### Foreign Function Interface

1. There should be a standard, good, foreign function interface which makes it easy to bind to C libraries which does not expose internals. 

2. In reality almost every language has too many ways to do it, none of them good.

3. Relatedly, *please* run valgrind clean by default. I know it’s a pain, and I know it hurts some microbenchmarks, but it makes debugging code integrated with C so much easier.

### Text handling

Text is:

1. Efficiently represented.

2. Always immutable (Note: Having a separate editable representation for text is perfectly reasonable, but it’s not your default).

3. Always unicode.

4. Always understood to be a variable length encoding that you cannot index into by an offset.

5. Easy to read and write to a variety of encodings.

6. Anything else is wrong and you are a sinner for contemplating it.

### Equality works correctly

1. There is a single operator you use for equality. You do not use different ones for different types.

2. Differently typed values are never equal. Yes I know this violates the Liskov Substitution Principle. I don’t even slightly care. Ideally comparing different types for equality should be an error.

3. Equality is reflexive. That is, x == x, always. I don’t know what to do about NaN here. So far my most practical solution involves a time machine. My second most practical answer is “Ignore IEEE and deal with the resulting confusion”.

4. Equality is symmetric.

5. Equality is transitive.

### Good numeric hierarchy

Your language should be able to represent:

1. Signed and unsigned fixed size integers of various machine sizes

2. Arbitrary precision integers

3. Double and single precision floats

4. Arbitrary precision rational numbers

5. Fixed width decimal arithmetic

These should all be easy to convert to eachother (but not compare equal if they are of different types!), and they should certainly all consistently use standard operators.

Most of this should be implementable as libraries rather than needing to be baked in to the language.

### Packed data

1. At some point you are going to need to deal with arrays of “primitive” types – bytes, doubles, machine words, etc. If you cannot represent this in an efficient way when you come to do this, you will be sad. Ideally you want to do this in a way that makes it easy to interact with the aforementioned foreign function interface.

2. Ideally this would also support arrays of structs of some sort. I don’t really care about representing structs as individual values efficiently, but for large arrays of data it matters.

### Namespacing and scoping

1. Everything should have a clear, lexical, scope. It should be obvious where a variable is introduced. The answer should never be “into the global namespace”. It should be hard to make typos and not notice.

  As far as I can tell, basically the only languages which get this right are statically typed or a lisp. (ETA: Apparently perl with use strict also gets this right).

### Higher order functions

1. Languages should have first class functions, and higher order functions like map or filter.

  This one… is doing pretty well actually. This debate is over and we won. The last time I checked, Java was the only mainstream language that didn’t do this. Since Java 8 last year there are no mainstream languages that don’t do this.

### A REPL

1. Not much to say here except that a REPL is so invaluable to how I work that it’s really painful using languages without one. I can do it of course, but it tends to involve writing lots of tiny little throwaway programs that act as a poorer version of a REPL.

### Take typing seriously

1. I’m fine with dynamically typed languages. I’m also fine with languages with fairly serious static type systems (Haskell, OCaml, F#. Even C++ and C# are pretty OK). But if you’re going to have a type system don’t half-arse it. Good type systems are good, but bad type systems are worse than no type systems.

Note: Type system wars in the comments will not make it through moderation.

### Solid, high performance, implementation

Why are we all using slow and unreliable implementations? It makes me really sad.

I mean, I do know the answer, it’s because writing a concurrent garbage collector and a high performance compiler is hard and reusing language-specific VMs mostly works but has its own set of problems.

Basically I want garbage collections and threading to just work, and I want to be able to write code that looks as if it should be reasonably low level and have it not produce something that’s hundreds of times slower than the equivalent C. If you can compile high level abstractions down to low level code, that’s great too.

Yes I know that low level concurrency is passé and we’re all doing message passing now. A good message passing API on top of the concurrency primitives would be great, but I want the primitives too.

### Rich standard library

It has major problems, but the size and (mostly) quality of the Java standard library is one of the few things I miss about it.

The standard library should have all of the normal really boring things we need to get things done.

1. File system access

2. Sockets – client and server

A solid HTTP client (I’m ambivalent as to whether there should also be a server. Experience of how little ones from the standard libraries of existing languages are used suggests no)

3. Parsing for standard formats – XML, JSON, etc.

4. Good concurrency primitives

5. Pseudo random number generators

6. Invoking and running external programs

7. Probably many others I’m forgetting

There are plenty of things that *shouldn’t* be in the standard library because you want a faster release cycle or because there are multiple good ways to do them, but in general there are things that we’ve basically got figured out and are commonly needed and those should be standardized.

### Collections Library

I *really* want a good collections library. With standard interfaces for things. We seem to have settled on “Eh, you’ve got hash tables and dynamically sized arrays, what more do you want?”. I’ll tell you what more I want:

1. *Uniform interfaces*. There are many things I dislike about Python but high up on the list is that if I write add when I meant append or append when I meant add one more time I’m going to scream

2. Immutable collections (not just frozenset. I want efficiently updateable immutable collections)

3. Sorted collections

4. Heaps

5. Priority queues

Java collections library I miss you. Please come back?

### Database access

There should be a standard API for talking to a relational database. It doesn’t need to (and shouldn’t) bundle everything into it, but it would be nice if the API were standard and the standard library came with e.g. a sqlite3 adapter.

### Summary

I think this can mostly be summarized by saying that I want is completeness and quality. The domain of programming is large, messy, and broken, and it would be nice if the language that I use to interact with it were a bastion of things mostly working and being easy rather than fighting against me every step of the way. There’s enough stuff that is common and known how to do well that it would be great to just do it well and then stop having to worry about it.

### Interesting Comments

##### Franklin Chen July 20, 2015 at 11:18 pm*
I used OCaml as my main language in 1997-1999, but a lot of the pain points that existed then still exist now, despite OPAM. For example, people are still coming up with their own project directory structures, Makefiles, and the like. There is still no single standard command to create an empty “hello world” project that is immediately buildable, testable, and publishable. This is unbelievable to me.

##### Levi July 15, 2015 at 9:00 am
Something I’d really like to see that doesn’t really seem to be addressed often is an extension to your wish for ‘packed’ data. That’d be great, if not a huge improvement over what’s sometimes available now, but I’d really like the means to map a standardized algebraic data type interface to a completely user-defined representation, including reliable control over data alignment/padding, bit field ordering, byte ordering, and user-defined discrimination functions for variants/unions that can be constrained enough to produce helpful compile-time warnings when ambiguity or un-covered cases arise. 

As a frequent programmer of device drivers and network stack components for embedded systems that need to be portable across various architectures, I find C’s support for this level of programming very tedious; there are a number of options to end up with the code you need, but none of them are *great* options and it seems to me that we could do a lot better. Erlang’s binary pattern matching is a nice answer to one part of this, and there are pieces of it in various research languages like Habit from PSU/Galois’ HASP project and old systems languages like DEC BLISS, but they seem to focus *either* on interpreting binary stream *or* on memory-mapped register programming, not on a solution that encompasses both. Alas, the closest thing I can find right now to my ideal in this area is probably Ada, and that hasn’t exactly got a thriving community.

##### Mark Pawelek July 15, 2015 at 11:05 am
A central package repository would be nice. The repo doesn’t necessary need to store the package, just point to where it is. A central repo would allow one to search for a namespace and have the package manager automatically download the best package for your language version. It should be possible to do this from the IDE. In fact, as soon as the syntax checker knows that namespace is not being referenced the IDE should give you the option to install the missing package after the cursor has moved off the line which declares a namespace to import with missing reference.

