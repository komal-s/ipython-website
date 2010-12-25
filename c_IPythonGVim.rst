==========================
 Using IPython with GVim 
==========================

-------------------------
 Quick Start on Ubuntu 
-------------------------

First, copy ipy.vim plugin to vim plugin folder ::

    $ cp /usr/lib/python2.5/site-packages/ipython-0.9.1-py2.5.egg/share/doc/ipython/examples/core/ipy.vim ~/.vim/plugin/


In ~/.ipython/ipythonrc, write ::

editor gvim


Next ::

    $ ipython
    In [1]: import ipy_vimserver
    In [1]: ipy_vimserver.setup("session1")
    In [1]: %vim


In GVim ::

    <F5> => execute buffer code content in ipython session


