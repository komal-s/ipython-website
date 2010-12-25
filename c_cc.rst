==========================
Custom Completers
==========================


You can write your own completer e.g. to make using your favourite command line programs quicker.

You can look at `ipy_stock_completers.py <http://projects.scipy.org/ipython/ipython/browser/ipython/trunk/IPython/Extensions/ipy_stock_completers.py>`_] for examples on how to build your own completer. The key is writing a function like::

    def apt_completers(self, event):
        """ This should return a list of strings with possible completions.
        
        Note that all the included strings that don't start with event.symbol
        are removed, in order to not confuse readline.
        
        """
    
        return ['update', 'upgrade', 'install', 'remove']


Where you can examine event.line (whole line), event.symbol (the symbol under completion right now) and event.command (the first word in line, for convenience).

After that, you need to hook the completer with IPython by doing::

    import IPython.ipapi
    ip = IPython.ipapi.get()
    ip.set_hook('complete_command', apt_completers, re_key = '.*apt-get')
    
    # or instead of re_key, do str_key = 'apt-get' which is quicker but doesn't handle 'sudo apt-get'.
    
 
There are some completers already in place.

For example the 'cd' completer that handles directory history (-NUM) and bookmarks (-b)::

    [dl]|6> cd /cygwin/
    [cygwin]|7> cd <tab>
    bin/ etc/ lib/ tmp/ usr/ var/
    [cygwin]|7> cd -<tab>
    -0 [Q:\ipython] -1 [q:\home]    -2 [q:\dl]      -3 [q:\cygwin]
    [cygwin]|7> %bookmark cyg
    [cygwin]|8> cd -b <tab>
    lscr cyg
    [cygwin]|8> cd -b cyg
    
Or the 'import' completer::

    [cygwin]|8> import IPyt<tab>
    IPython. ipython
    [cygwin]|8> import IPython.<tab>
    IPython.                   IPython.Logger             IPython.excolors           IPython.platutils_posix
    IPython.ColorANSI          IPython.Magic              IPython.genutils           IPython.platutils_win32
    IPython.ConfigLoader       IPython.OInspect           IPython.hooks              IPython.rlineimpl
    IPython.CrashHandler       IPython.OutputTrap         IPython.ipapi              IPython.strdispatch
    IPython.DPyGetOpt          IPython.Prompts            IPython.iplib              IPython.ultraTB
    IPython.Debugger           IPython.PyColorize         IPython.ipmaker            IPython.upgrade_dir
    IPython.Extensions.        IPython.Release            IPython.ipstruct           IPython.usage
    IPython.FakeModule         IPython.Shell              IPython.irunner            IPython.wildcard
    IPython.Gnuplot2           IPython.background_jobs    IPython.macro              IPython.winconsole
    IPython.GnuplotInteractive IPython.completer          IPython.numutils
    IPython.GnuplotRuntime     IPython.deep_reload        IPython.platutils
    IPython.Itpl               IPython.demo               IPython.platutils_dummy
    [cygwin]|8> import IPython.Extensions.<tab>
    IPython.Extensions.                           IPython.Extensions.ipy_linux_package_managers
    IPython.Extensions.InterpreterExec            IPython.Extensions.ipy_pydb
    IPython.Extensions.InterpreterPasteInput      IPython.Extensions.ipy_stock_completers
    IPython.Extensions.PhysicalQInput             IPython.Extensions.ipy_system_conf
    IPython.Extensions.PhysicalQInteractive       IPython.Extensions.jobctrl
    IPython.Extensions.astyle                     IPython.Extensions.ledit
    IPython.Extensions.clearcmd                   IPython.Extensions.numeric_formats
    IPython.Extensions.ext_rehashdir              IPython.Extensions.path
    IPython.Extensions.ext_rescapture             IPython.Extensions.pickleshare
    IPython.Extensions.ibrowse                    IPython.Extensions.pspersistence
    IPython.Extensions.ipipe                      IPython.Extensions.pydb_ipy
    IPython.Extensions.ipy_defaults               IPython.Extensions.win32clip

