## Erlang Macro Processor (v2) ##

Put on GitHub by Andreas Stenius.

This is the emp2 written by Philip Robinson.
See his blog for full details: http://chlorophil.blogspot.com/2007/04/erlang-macro-processor-v2-part-v.html

## License ##

This is the license as stated on Mr. Robinsons blog site:

    Unless otherwise noted, all code appearing on this blog is released into the public domain and provided "as-is", 
    without any warranty of any kind, express or implied, including but not limited to the warranties of merchantability, 
    fitness for a particular purpose and noninfringement. 
    
    In no event shall the author(s) be liable for any claim, damages, or other liability, 
    whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software 
    or the use or other dealings in the software.

### Example usage ###

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
	    <<_:Offset/binary,Value:8/integer,_/binary>> =
	        example_macro:lookup_binary(4),
	    Value.

Example output:

	1> [example:lookup(N) || N <- lists:seq(0, 3)].
	[0,2,4,6]
	2>


#### Erlang Macro Processor (v1) ####

Is also supported (see http://chlorophil.blogspot.com/2007/04/erlang-macro-processor-v1-part-i.html).
