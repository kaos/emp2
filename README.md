== Erlang Macro Processor (v2) ==

Put on GitHub by Andreas Stenius.

This is the emp2 written by Philip Robinson.
See his blog for full details: http://chlorophil.blogspot.com/2007/04/erlang-macro-processor-v2-part-v.html

=== Example usage ===

The macro definition (in it's own module):

	-module(example_macro).
	-export([lookup_binary/1]).
	
	lookup_binary(Size) ->
	    [[$,,FirstVal]|NumberString] = lists:map(
	        fun(Offset) -> io_lib:format(",~B", [Offset * 2]) end,
	        lists:seq(0, Size - 1)),
	    "<<" ++ [FirstVal] ++ NumberString ++ ">>".
	

Then you may use it from other modules:

	-module(example).
	-export([lookup/1]).
	
	-compile({parse_transform, emp2}).
	-macro_modules([example_macro]).
	
	lookup(Offset) ->
	    <<_:offset/binary,value:8/integer,_/binary>> =
	        example_macro:lookup_binary(4),
	    Value.
	
==== Erlang Macro Processor (v1) ====

Is also supported (see http://chlorophil.blogspot.com/2007/04/erlang-macro-processor-v1-part-i.html).
