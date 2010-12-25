~~~~~~~~~~~~~~~
The Sh profile
~~~~~~~~~~~~~~~


The 'sh' profile optimizes IPython for system shell usage. Apart from certain job control functionality that is present in unix (ctrl+z does "suspend"), the sh profile should provide you with most of the functionality you use daily in system shell, and more. Invoke IPython in 'sh' profile by doing 'ipython -p sh', or (in win32) by launching the "pysh" shortcut in start menu.

If you want to use the features of sh profile as your defaults (which might be a good idea if you use other profiles a lot of the time but still want the convenience of sh profile), add "import ipy_profile_sh" to your ~/.ipython/ipy_user_conf.py.

The 'sh' profile is different from the default profile in that:

 * Prompt shows the current directory
 * Spacing between prompts and input is more compact (no padding with empty lines). The startup banner is more compact as well.
 * System commands are directly available (in alias table) without requesting %rehashx - however, if you install new programs along your PATH, you might want to run %rehashx to update the persistent alias table
 * Macros are stored in raw format by default. That is, instead of '_ip.system("cat foo"), the macro will contain text 'cat foo')
 * Autocall is in full mode
 * Calling "up" does "cd .."

The 'sh' profile is different from the now-obsolete (and unavailable) 'pysh' profile in that:

 * '$$var = command' and '$var = command' syntax is not supported anymore. Use 'var = !command' instead (incidentally, this is available in all IPython profiles). Note that !!command *will* work.

Some extensions are enabled as default in this profile:

----------
envpersist
----------

%env can be used to "remember" environment variable manipulations. Examples::

	%env - Show all environment variables
	%env VISUAL=jed  - set VISUAL to jed
	%env PATH+=;/foo - append ;foo to PATH
	%env PATH+=;/bar - also append ;bar to PATH
	%env PATH-=/wbin; - prepend /wbin; to PATH
	%env -d VISUAL   - forget VISUAL persistent val
	%env -p          - print all persistent env modifications

---------
ipy_which
---------

%which magic command. Like 'which' in unix, but knows about ipython aliases.

Example::

 [C:/ipython]|14> %which st
 st -> start .
 [C:/ipython]|15> %which d
 d -> dir /w /og /on
 [C:/ipython]|16> %which cp
 cp -> cp
   == c:\bin\cp.exe
 c:\bin\cp.exe

------------------
ipy_app_completers
------------------

Custom tab completers for some apps like svn, hg, bzr, apt-get. Try 'apt-get install <TAB>' in debian/ubuntu.

-------------
ipy_rehashdir
-------------

Allows you to add system command aliases for commands that are not along your path. Let's say that you just installed Putty and want to be able to invoke it without adding it to path, you can create the alias for it with rehashdir::

 [~]|22> cd c:/opt/PuTTY/
 [c:opt/PuTTY]|23> rehashdir .
              <23> ['pageant', 'plink', 'pscp', 'psftp', 'putty', 'puttygen', 'unins000']

Now, you can execute any of those commams directly::

 [c:opt/PuTTY]|24> cd
 [~]|25> putty

(the putty window opens).

If you want to store the alias so that it will always be available, do '%store putty'. If you want to %store all these aliases persistently, just do it in a for loop::

 [~]|27> for a in _23:
    |..>     %store $a
    |..>
    |..>
 Alias stored: pageant (0, 'c:\\opt\\PuTTY\\pageant.exe')
 Alias stored: plink (0, 'c:\\opt\\PuTTY\\plink.exe')
 Alias stored: pscp (0, 'c:\\opt\\PuTTY\\pscp.exe')
 Alias stored: psftp (0, 'c:\\opt\\PuTTY\\psftp.exe')
 ...

-----
mglob
-----

Provide the magic function %mglob, which makes it easier (than the 'find' command) to collect (possibly recursive) file lists. Examples::

 [c:/ipython]|9> mglob *.py
 [c:/ipython]|10> mglob *.py rec:*.txt
 [c:/ipython]|19> workfiles = %mglob !.svn/ !.hg/ !*_Data/ !*.bak rec:.

Note that the first 2 calls will put the file list in result history (_, _9, _10), and the last one will assign it to 'workfiles'.


