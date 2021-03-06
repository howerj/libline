# linenoise.md
## Synopsis 

A line editor library that provides support for vi and emacs like key bindings
as well as a basic command line history.

## Files

* libline.c
* libline.h

## Rationale

Any program where you are going to have some kind of REPL benefits greatly from
some basic line editing functionality, as well as a command line history
accessible with the arrow keys. This library provides this functionality.

## Description

This is a modified version of "linenoise" library originally obtained from
<https://github.com/antirez/linenoise>, it has been modified to bring it
closer to how the rest of the code in the lisp repository is structured
but the changes are mostly superficial, but I have added in vi key binding
code and removed multi-line support. In the reference section bellow is the
original *readme.md* for the project, most of the information it contains does
not apply anymore but it is kept here for completeness. 

## Specification
## References

### Linenoise

A minimal, zero-config, BSD licensed, readline replacement used in Redis,
MongoDB, and Android.

* Single line editing mode with the usual key bindings implemented.
* History handling.
* Completion.
* About 1,100 lines of BSD license source code.

#### Can a line editing library be 20k lines of code?

Line editing with some support for history is a really important feature
for command line utilities. Instead of retyping almost the same stuff
again and again it's just much better to hit the up arrow and edit on
syntax errors, or in order to try a slightly different command. But
apparently code dealing with terminals is some sort of Black Magic:
readline is 30k lines of code, libedit 20k. Is it reasonable to link
small utilities to huge libraries just to get a minimal support for
line editing?

So what usually happens is either:

 * Large programs with configure scripts disabling line editing if
 readline is not present in the system, or not supporting it at all since
 readline is GPL licensed and libedit (the BSD clone) is not as known
 and available as readline is (Real world example of this problem: Tclsh).
 * Smaller programs not using a configure script not supporting line
 editing at all (A problem we had with Redis-cli for instance).
 
The result is a pollution of binaries without line editing support.

So I spent more or less two hours doing a reality check resulting in
this little library: is it *really* needed for a line editing library
to be 20k lines of code? Apparently not, it is possible to get a very
small, zero configuration, trivial to embed library, that solves the
problem. Smaller programs will just include this, supporting line editing
out of the box. Larger programs may use this little library or just
checking with configure if readline/libedit is available and resorting
to linenoise if not.

#### Terminals, in 2010.

Apparently almost every terminal you happen to use today has some kind of
support for VT100 like escape sequences. So I tried to write a library
using just the very basic VT100 features. The resulting library appears
to work everywhere I tried to use it.

Since it's so young I guess there are a few bugs, or the lib may not
compile or work with some operating system, but it's a matter of a few
weeks and eventually we'll get it right, and there will be no excuses
for not shipping command line tools without built-in line editing support.

The library is currently about 1100 lines of code. In order to use it in
your project just look at the *example.c* file in the source distribution,
it is trivial. Linenoise is BSD code, so you can use both in free software
and commercial software.

#### Tested with...

 * Linux text only console ($TERM = linux)
 * Linux xterm ($TERM = xterm)

Please test it everywhere you can and report back!

#### Let's push this forward!

Patches should be provided in the respect of linenoise sensibility for small
easy to understand code.

(Note; originally it said "Send feedback to antirez at gmail" however this
 library has been modified from the original, so make sure any bugs are
 present also in the original before sending feedback).

