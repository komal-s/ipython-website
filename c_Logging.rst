====================
 Logging to a file 
====================
Here is an alternative logging solution that lets you record your sessions in a daily time-stamped log-files.

Add the following lines or make it a-like to your ipy_user_conf.py::

    from time import strftime
    
    def main():
    try:
        ldir = '/home/$YOUR_USERNAME_HERE/.ipython/'
        filename = os.path.join(ldir, strftime('%Y-%m-%d')+".py")
        notnew = os.path.exists(filename)
        ip.IP.logger.logstart(logfname=filename, logmode='append')
        log_write = ip.IP.logger.log_write
        if notnew:
            log_write("# =================================")
        else:
           log_write("#!/usr/bin/env python \n# %s.py \n"
                     "# IPython automatic logging file"
                     % strftime('%Y-%m-%d'))
        log_write("# %s \n# =================================" %
                  strftime('%H:%M'))
        print " Logging to "+filename
      
    except RuntimeError:
        print " Already logging to "+ip.IP.logger.logfname  
