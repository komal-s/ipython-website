==============================
Exporting %store'd stuff: ipy_exportdb.py extension 
==============================


Sometimes you have created a nifty macro you don't want to lose, or want to distribute to your friends. You can use ipy_exportdb.py extension for this.

Let's create a macro and an alias::

    [d:/ipython]|13> a = 12
    [d:/ipython]|14> print "a is ",a
    a is  12
    [d:/ipython]|15> macro foo 13-14
    Macro `foo` created. To execute, type its name (without quotes).
    Macro contents:
    a = 12
    print "a is ",a
    
    [d:/ipython]|16> store foo
    Stored 'foo' (Macro)
    [d:/ipython]|17> alias bar hubba
    [d:/ipython]|18> store bar
    Alias stored: bar (0, 'hubba')



Now, we have it in our IPython database, but it's not really easy to distribute them to others. You can do this with ipy_exportdb::


    [d:/ipython]|19> import ipy_exportdb
    [d:/ipython]|20> ipy_exportdb.export


This prints out the "export file" (which is legal python), the relevant portions of which are::

    import IPython.ipapi
    ip = IPython.ipapi.get()

    # === Macros ===
    ...

    ip.defmacro('foo',
     u'a = 12\n'
     u'print "a is ",a\n'
    )
    ...
    
    # === Alias definitions === 
    ...
    ip.defalias('bar', 'hubba')
    ...


You can also provide the output file name to exportdb().

The resulting file can be imported normally, e.g. in ipy_user_conf.py.

Following things are exported: macros, variables (basic data types only, i.e. no pickling), bookmarks, aliases, stored %env modifications.

