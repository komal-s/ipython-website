=================================================
 Saving the session on exit like the R shell 
=================================================

Even if we can start the logger in the current session by calling the magic function 
%logstart, and control it with %logoff, %logon and %logstate, I like the way R ask to save 
the session before exit.
Here is a simple code to transform the Ipython shell logger like the R one. Just copy
the code below in ~/.ipython/logger.py (or whatever you want) and append 
ip.ex("import logger")
to ~/.ipython/ipy_user_conf.py

::

    import IPython.ipapi
    import sys, os
    
    ip = IPython.ipapi.get()
    LOGNAME = ".ipython_history.log"

    
    def startup_logger(self):
        """ Executed at the ipython startup for loading last saved session
        
        """
        if os.path.exists(LOGNAME):
            p = raw_input("Load last saved session in the current directory ([y]/n)? ")
            if (p.lower() == 'y') or (p == ''):
                ip.magic('run -i -e %s' %LOGNAME)
        return
    
    def shutdown_logger(self):
        """ Prompts for saving the current session during shutdown
    
        """
        
        p = raw_input("Save current session (y/[n])? ")
        if p.lower() == 'y':
            #ip.magic('logstart %s over' %LOGNAME)
            shell = ip.IP.shell
            logger = shell.logger
            head = shell.loghead_tpl % (LOGNAME, '[]')
            #head = "#saved session in the current directory"
            try:
                started = logger.logstart(logfname=LOGNAME, loghead=head, logmode='append')
            except:
                print("Couldn't save session: %s" % sys.exc_info()[1])
            input_hist = shell.input_hist
            logger.log_write(input_hist[1:])
        return
    
    ip.set_hook('late_startup_hook', startup_logger)    
    ip.set_hook('shutdown_hook', shutdown_logger)
        
