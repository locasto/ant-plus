# ant-plus

An example implementation of a safer subset of the ANT+ protocol using
the Hammer parser combinator library.  This implementation is possibly
more secure than the existing C client because:

 - it chooses to not implement a number of inherently unsafe ANT+
   protocol messages (in other words, the ANT+ protocol is
   fundamentally broken, and this current implementation removes that
   broken-by-design set of messages)

   See message-types.md for an enumeration of safe and unsafe message
   types and related discussion.

 - this implementation uses parser combinators for the C language to
   precisely define the input-handling routines and thus minimize or
   eliminate bugs in input parsing.

ANT+ has a "source visible" (one hesitates to say open source) C
implementation; the implementation offered in this repository is one
that demonstrates the construction of a parser using a parser
combinator rather than hand-crafted C.