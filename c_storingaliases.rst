
======================
 Persistent aliases 
======================



Like macros, aliases can be %store'd. Writing a simple alias can be a good alternative to writing a trivial script; here I introduce an alias called 'publish' to copy the specified folder to a "publishing area" where others can acces it, and make the alias available in subsequent sessions::

    > alias publish cp -r %s n:/Public/Files/pub
    > %store publish


Now, after %Exit and restarting the ipython session, "publish" remains available. Also note the use of '%s' as an alias argument placeholder.

This can also be a handy way to avoid adding places to PATH::

    [\]|79> alias vlc q:/opt/VLC/vlc.exe
    [\]|80> %store vlc
    Alias stored: vlc (0, 'q:/opt/VLC/vlc.exe')


Or to override stupid binaries like window's "FIND.exe" with cygwin's find::

    [~]|47> alias find q:/cygwin/bin/find.exe
    [~]|48> store find

To see what aliases you have stored, check the 'stored_aliases' variable in the persistent "pickleshare" database::

    [ipython]|44> _ip.db.keys
    ------------> _ip.db.keys()
             <44>
    ['autorestore/radio_chillout',
     'autorestore/radio_harddance',
     'autorestore/radio_trance',
     'bookmarks',
     'stored_aliases',
     'syscmdlist']
    [ipython]|45> _ip.db['stored_aliases']
             <45>
    {'d': (0, 'ls.exe -F --color=auto'),
     'find': (0, 'q:\\cygwin\\bin\\find.exe'),
     'hg': (0, 'q:\\opt\\Mercurial\\hg.exe'),
     'irssi': (0, 'Q:\\opt\\irssi\\irssi.bat'),
     'kdiff': (0, 'q:/opt/KDiff3/kdiff3.exe'),
     'np': (0, 'q:/opt/np/notepad++.exe'),
     'rm': (0, 'q:\\cygwin\\bin\\rm.exe'),
     'telkku': (0, 'start http://telkku.com'),
     'vi': (0, 'vim'),
     'vlc': (0, 'q:/opt/VLC/vlc.exe')}
    [ipython]|46>
    
Or more conveniently, execute %alias without arguments and see the last few aliases - they are probably either %store'd, er defined manually in some config file::

    [ipython]|43> alias
    Total number of aliases: 710
             <43>
    [('ACE', 'ACE'),
     ('ACE2', 'ACE2'),
     ('ACE32', 'ACE32'),
    
    ...
    
     ('xxd', 'xxd'),
     ('yes', 'yes'),
     ('zcat', 'zcat'),
     ('d', 'ls.exe -F --color=auto'),
     ('ddir', 'dir /ad /on'),
     ('find', 'q:\\cygwin\\bin\\find.exe'),
     ('hg', 'q:\\opt\\Mercurial\\hg.exe'),
     ('irssi', 'Q:\\opt\\irssi\\irssi.bat'),
     ('kdiff', 'q:/opt/KDiff3/kdiff3.exe'),
     ('ldir', 'dir /ad /on'),
     ('np', 'q:/opt/np/notepad++.exe'),
     ('rm', 'q:\\cygwin\\bin\\rm.exe'),
     ('telkku', 'start http://telkku.com'),
     ('vi', 'vim'),
     ('vlc', 'q:/opt/VLC/vlc.exe')]
    [ipython]|44>

