=============================
IPythonEmacs23
=============================

http://www.emacswiki.org/emacs/PythonMode#toc10

rileyrgdev AT gmail DOT net

I found this which got me up and running with iPython integrated nicely into emacs 23.

http://lists.ipython.scipy.org/pipermail/ipython-user/2008-September/005791.html

my python integration is now like this::

    (setq load-path
          (append (list nil
                        "~/.emacs.d/python-mode-1.0/"
    		    "~/.emacs.d/pymacs/"
    		    "~/.emacs.d/ropemacs-0.6"
                        )
                  load-path))
    
    (require 'ipython)
    (setq py-python-command-args '( "-colors" "Linux"))
    
    (defadvice py-execute-buffer (around python-keep-focus activate)
      "return focus to python code buffer"
      (save-excursion ad-do-it))
    
    (setenv "PYMACS_PYTHON" "python2.5") 
    (require 'pymacs)
    
    (pymacs-load "ropemacs" "rope-")
    
    (provide 'python-programming)


Note the defadvice to return focus to the python buffer you py-execute-buffer from : it does not work. Can anyone “advise” why not?

More iPython info here: http://ipython.scipy.org/moin/FrontPage

