=====
**FAQ**
=====

This is the IPython Frequently Asked Questions page. Feel free to contribute new information to it!


------------------------------------------------------------
 Q. Running IPython against multiple versions of Python 
------------------------------------------------------------
    I would like to know if there is an easy way to install parallel versions of ipython ? Actually, I would like to be able to use ipython with python 2.4 which is the default on my machine, and with python2.5 (compiled by hand); is changing the python interpreter in the ipython script the only required change ? (I was thinking about having ipython with the default interpreter and ipython2.5 using the python2.5 interpreter) ?
**Answer:** You can install IPython against various interpreters by using the following call::

    $ python2.4 setup.py install --prefix=$HOME/usr
    $ python2.5 setup.py install --prefix=$HOME/usr


In this case, the second call will overwrite the startup script of the first in `$HOME/usr/bin`, but those are the same. The important point is that each Python will find the IPython imports under `$HOME/usr/lib/python2.X/site-packages/IPython`, and assuming you have set up your `$PYTHONPATH` variable accordingly, this will work. The final step is to have aliases to call each of the IPython versions quickly, those can be defined as (using tcsh syntax)::

    alias ip4 "python2.4 $HOME/usr/bin/ipython"
    alias ip5 "python2.5 $HOME/usr/bin/ipython"


or you can just have two copies of the ipython startup script named differently and with their first line changed to point to the actual interpreter.

--------------------------------
 Q. IPython crashes under OS X when using the arrow keys
--------------------------------
Under some circumstances, using the arrow keys to navigate your input history can cause a complete crash of the Python interpreter.

**Answer:** This is due to a bug in the readline library from the official builds. There are a few solutions you can take:

 1. Use a different Python version from Apple's default (MacPython or Fink have been reported to work)

 1. You can disable in your ipythonrc file the following lines by commenting them out::

    readline_parse_and_bind "\e[A": history-search-backward
    readline_parse_and_bind "\e[B": history-search-forward

You will lose searching in your history with the arrow keys, but at least Python won't crash.

------------------------------
 Q. Does IPython play well with Windows? 
------------------------------
Yes, it most definitely does! See IpythonOnWindows for some things that should be noted, though.

---------------------------
 Q. What is the best way to install IPython? 
---------------------------
 * The classic "python setup.py install" still works, of course, and is recommended for Linux boxes where you have root privileges.
 * On windows you'll probably want to run the .exe installer to get the shortcuts and ipython.py in \python25\scripts.
 * "easy_install ipython==dev" is the easiest way to install the SVN trunk version.
 * "easy_install ipython" is a quick way to get ipython without downloading or unzipping anything manually, but you'll miss windows shortcuts, and documentation (man pages, pdf...) will not be where it normally is.
 * If you want to run the latest ipython instead of an older version provided with your linux distro, and don't want to remove/mess with the already existing version, don't have root privileges to install ipython, or just want to run multiple ipython versions for some reason, just untar the source distribution somewhere and launch ipython.py.

--------------------------
 Q. Why do I get garbage characters when long information is displayed by the pager? 
--------------------------

**Answer:** The color escapes used by IPython are not correctly interpreted by default by many common pagers ('less' included). The manual describes `here <http://ipython.scipy.org/doc/stable/html/config/initial_config.html#object-details-types-docstrings-source-code-etc>`_ the problem and its solution in detail, but the short version is that your bashrc file should contain::

    export PAGER=less
    export LESS=-r



---------------------------
 Q. Am I doing something foolish, or is this a limitation of using ipython with doctest? 
---------------------------
**Answer:** The latter, I'm afraid, but there's a workaround.  The reason, deep
down, is a clash between ipython's modification of sys.displayhook so
you get nice output prompts, and the fact that the exec builtin is
internally hardcoded to run sys.displayhook.  So ipython collides with
doctest (which uses 'exec') and all hell breaks loose. 

Fernando Perez fperez.net at gmail.com 
Thu Jan 18 01:01:39 CST 2007 

--------------------------------------------------------------------------------

Here's the workaround.  First, your error (I called your file 'dtest')::

    In [5]: doctest.testmod(dtest)
    Out[5]: 1
    **********************************************************************  
    File "dtest.py", line 8, in dtest
    Failed example:
        x()
    Expected:
        1
    Got nothing
    **********************************************************************
    1 items had failures:
    1 of   3 in dtest   
    ***Test Failed*** 1 failures.
    Out[5]: (1, 3)


Now the workaround::

    In [6]: iphook = sys.displayhook
    In [7]: sys.displayhook = sys.__displayhook__
    In [8]: doctest.testmod(dtest)

*** DocTestRunner.merge: 'dtest' in both testers; summing outcomes.
(0, 3)


Now you can reactivate ipython's displayhook if you want::

    In [9]: sys.displayhook = iphook


You could wrap this little sys.displayhook dance in a utility function
to ease things up.

Cheers, f

ps - brownie points for you if you contribute the above to the ipython wiki FAQ

-----------
 Q. Can IPython run under IronPython? It would be great to have access to all of the .Net libraries via IPython. If not are there any plans to make this possible? =
-----------

I (FPerez) don't know specifically, because I don't have a windows machine to test on.  The most likely problems would come from Readline and from having sys._getframe(). On Win32 we ship our own pyreadline, and that might be a valid solution under IronPython, the _getframe() issue would need to be answered by an IronPython expert.  

Updates to this answer by anyone more knowledgeable are welcome!

