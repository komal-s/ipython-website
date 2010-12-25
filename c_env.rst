=====================================
%env that remembers modifications 
=====================================


envpersist.py replaces the minimal %env with a turbocharged version that allows you to set/edit environment variables easily. All the modifications are remembered across IPython invocations, so there is no need to edit startup files, registry etc.

It is enabled by default in 'sh' profile (pysh).

It is even possible to append and prepend stuff to PATH, without overwriting it altogether.

Here is the doc::

    [d:/ipython]|23> %env?
    ...
        Store environment variables persistently

    IPython remembers the values across sessions, which is handy to avoid
    editing startup files.

    %env - Show all environment variables
    %env VISUAL=jed  - set VISUAL to jed
    %env PATH+=;/foo - append ;foo to PATH
    %env PATH+=;/bar - also append ;bar to PATH
    %env PATH-=/wbin; - prepend /wbin; to PATH
    %env -d VISUAL   - forget VISUAL persistent val
    %env -p          - print all persistent env modifications


The usage should be obvious from the docstring.

