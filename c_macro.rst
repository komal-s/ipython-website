=========================
 Passing arguments to macros 
=========================

It's now (svn rev 2337) possible to call macros with arguments on the command line. This is handy if you have constructed an elaborate macro and want to %edit it to be more generic - specify different target directories to copy files etc.

The arguments to macros are always contained in IPython user namespace, in '''_margv''' list variable, created using the normal IPython autocall handling.

Here we create a macro that takes an argument by "simulating" the argument passing on the first run (line #1)::

    [C:\ipython]|1> _margv = (34,)
    [C:\ipython]|2> print "hello",_margv[0]
    hello 34
    [C:\ipython]|3> macro morjensta 2   
    Macro `morjensta` created. To execute, type its name (without quotes).
    Macro contents:
    print "hello",_margv[0]

    [C:\ipython]|4> morjensta "jukka"
    hello jukka
    [C:\ipython]|6> hist
    1: _margv = (34,)
    2: print "hello",_margv[0]
    3: _ip.magic("macro morjensta 2")
    4: morjensta("jukka")
    5: print "hello",_margv[0]
    6: _ip.magic("hist ")
    [C:\ipython]|7>


Of course you can't now call the macro without arguments anymore::

    [C:\ipython]|9> morjensta
    C:\ipython\<ipython console> in <module>()
    <type 'exceptions.IndexError'>: tuple index out of range

