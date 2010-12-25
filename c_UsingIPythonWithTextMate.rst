================================
Using IPython with TextMate
================================

`TextMate <http://macromates.com/>`_ is a text editor for OS X. It's (to some) EMACS done the Mac way. Here are some commands that I've found handy for integrating TextMate a bit with an IPython session running in OS X's Terminal.app. There's not much error checking in any of these, so YMMV. All of these scripts check if the current tab in Terminal.app is running a Python process. If it is, the script assumes it's an IPython instance and runs the appropriate command in that session. If the current tab is not running a Python process, these commands will open a new terminal window and start ipython (make sure it's on your PATH) and then run the appropriate command.


All of these code snippets should be entered into the "Command(s)" field of a new command. Set the scope to "source.python".  You probably also want to set the Save: popup to "Current file", the Input: popup to "None" and the Output popup to "Discard". Of course you can set the key equivalent to be anything you want.


I've added the following commands to the Python bundle in my copy of TextMate and found them to be useful...

--------------------------------------------
 Run the current file using IPython's %run 
--------------------------------------------

::

    #!/bin/bash
        
    osascript <<- APPLESCRIPT
	    tell application "Terminal"
	    	set currentTab to (selected tab of (get first window))
	    	set tabProcs to processes of currentTab
	    	set theProc to (end of tabProcs)
	    	if theProc is not "Python" then
	    		set currentTab to (do script "ipython")
	    	end if
		
	    	do script "%run -e -i -n '$TM_FILEPATH'" in currentTab
	    end tell
    APPLESCRIPT

------------------------------
 Toggle IPython's %pdb state 
------------------------------

::

    #!/bin/bash
    
    osascript <<- APPLESCRIPT
    	tell application "Terminal"
    		set currentTab to (selected tab of (get first window))
    		set tabProcs to processes of currentTab
    		set theProc to (end of tabProcs)
    		if theProc is not "Python" then
    			set currentTab to (do script "ipython")
    		end if
    	
    		do script "%pdb" in currentTab
    	end tell
    APPLESCRIPT

--------------------------------------------
 Add a pdb breakpoint at the current line 
--------------------------------------------
You must be in pdb/ipdb::

    #!/bin/bash
    
    osascript  <<- APPLESCRIPT
    	tell application "Terminal"
    		set currentTab to (selected tab of (get first window))
    		set tabProcs to processes of currentTab
    		set theProc to (end of tabProcs)
    		if theProc is not "Python" then
    			set currentTab to (do script "ipython")
    		end if
    		
    		do script "break $TM_FILEPATH:$TM_LINE_NUMBER" in currentTab
    	end tell
    APPLESCRIPT

----------------------------------------
 Clear breakpoint for the current line 
----------------------------------------

::

    #!/bin/bash
    
    osascript  <<- APPLESCRIPT
	    tell application "Terminal"
	    	set currentTab to (selected tab of (get first window))
	    	set tabProcs to processes of currentTab
	    	set theProc to (end of tabProcs)
	    	if theProc is not "Python" then
	    		set currentTab to (do script "ipython")
	    	end if
	    	
	    	do script "clear $TM_FILEPATH:$TM_LINE_NUMBER" in currentTab
	    end tell
    APPLESCRIPT


