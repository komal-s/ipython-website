======================
 Interrupting Threads 
======================

ipython supports controlling several GUI toolkits interactively by running the GUI mainloop in a thread.  Currently, GTK, WX, and Qt are the threaded GUI toolkits that ipython supports.  Tkinter is also supported, but because of its privileged relationship with the python shell, it does not need to be run in a thread.


To launch a GUI toolkit in ipython in a thread, use a special flag at launch time

  * ipython -gthread   # GTK

  * ipython -wthread   # WX

  * ipython -qthread   # Qt

One of the most common use cases, indeed the one that gave rise to ipython's support of GUI threading, is running matplotlib in a thread.  matplotlib supports several GUI toolkits, and if you want to interact with your plot from the ipython shell, you need to start the GUI mainloop in a separate thread.  ipython takes care of these complexities for you if you launch ipython with::

    ipython -pylab


ipython will detect your default GUI toolkit from your matplotlib backend (see http://matplotlib.sf.net/matplotlibrc), start the GUI mainloop in a thread, and import the matplotlib.pylab namespace.

Unfortunately, one of the consequences of running ipython in a threaded mode is that scripts that are "run" with ipython's "run" command are no longer interruptable with CTRL-C.  The details of why this are so are complex and go to the heart of POSIX and python internals, and are beyond the scope of your humble author's limited mental capacity, but there is an idiom to work around this problem.  

Here is a sample script which is used to illustrate the problem::

    from pylab import figure, show
    #lots-o-imports imports and data loading here
    storevalues = SomeStorageClass()
    for somedata in lots_of_data:
        storevalues.add( some_really_expensive_routine(somedata))
    
    fig = figure()
    storevalues.plot(fig)
    show()
    


Typically we want to run this script in ipython with:: 

    > ipython -pylab
    IPython 0.7.2.svn -- An enhanced Interactive Python.
    In [1]: run myscript.py


The problem is, if in the middle of running this script, we discover a problem that does not raise an exception, we want to interrupt the script with CTRL-C, but because the script is running in a separate thread, and python cannot handle cross thread signal handling, the script cannot get the interrupt signal.  Killing the ipython process externally with "kill" will work, but then we may lose a lot of data we've loaded and computed and cached that we may want to reuse in the next "run" with "run -i". ipython, however, sets a state flag KBINT  when it sees a CTRL-C which we can check in the inner loop.::

    def ipbreak():
        import IPython.Shell
        if IPython.Shell.KBINT:
            IPython.Shell.KBINT = False
            raise SystemExit


    for somedata in lots_of_data:
        storevalues.add( some_really_expensive_routine(somedata))
        ipbreak()


If you are disciplined enough to intersperse "ipbreak" calls into the parts of your scripts that are expensive, then when you hit CTRL-C ipython will set the state flag and your script, run in a separate thread, will detect it and raise an exception.  I use SystemExit here which will break out of your script as desired but will not terminate the ipython shell.

Rest assured that though this approach seems inelegant, Fernando is staying awake nights reading papers on cross thread signal handling in POSIX and cannot find anything better at the moment without fundamental and structural changes in python and/or POSIX.

