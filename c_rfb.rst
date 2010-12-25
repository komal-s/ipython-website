==============================
Running File Background
==============================
Using "%run -i foo.py" allows you to run the python file "foo.py" as a script. The "-i" allows you to inspect the variables created by the script once it is finished, as they are loaded in ipython's namespace.

But what if the script is a long running calculation, and you would like to keep working with your session of ipython ? Well you can run actions in the background with "%bg python_statement". The problem is that "%run -i" is not a python statement, but an ipython magic command. To be able to run a script in the background, you have to use "%bg _ip.magic('run -i foo.py') !

