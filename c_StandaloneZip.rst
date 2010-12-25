==============================================
 IPython in a standalone, executable zip file 
==============================================

[Contributed by Lisandro Dalcin of MPI4Py and Cython fame, thanks!]. 

With the code below, you can make a standalone executable zip file that contains all of IPython.  The code as written is made for 0.10, but it's easy enough to adapt (we'll likely include it in IPython proper as a utility).  If you update the library layout for the post-0.10 refactoring, please post an updated version below.::

    #!python
    ## Author:  Lisandro Dalcin
    ## Contact: dalcinl@gmail.com
    
    import sys, os
    import glob
    from zipfile import ZipFile
    from zipfile import ZIP_STORED
    from zipfile import ZIP_DEFLATED
    try:
        import zlib
    except ImportError:
        zlib = None
    
    origin = 'IPython'
    
    includes = [
    'IPython/*.py',
    'IPython/config/*.py',
    'IPython/Extensions/*.py',
    'IPython/external/*.py',
    'IPython/frontend/*.py',
    'IPython/frontend/*/*.py',
    'IPython/gui/*.py',
    'IPython/gui/*/*.py',
    'IPython/kernel/*.py',
    'IPython/kernel/*/*.py',
    'IPython/testing/*.py',
    'IPython/testing/*/*.py',
    'IPython/tools/*.py',
    'IPython/UserConfig/*.py',
    ]
    
    target = 'ipython.zip'
    
    header = """\
    #!/bin/sh
    exec python -c "
    import sys, os
    sys.path.insert(0, os.path.abspath('$0'))
    import IPython.Shell
    IPython.Shell.start().mainloop()
    " $@
    """
    
    header_win = '\r\n'.join("""\
    @echo off
    setlocal
    set _cmd=%_cmd%import sys, os;
    set _cmd=%_cmd%sys.path.insert(0,os.path.abspath('%0'));
    set _cmd=%_cmd%import IPython.Shell;
    set _cmd=%_cmd%IPython.Shell.start().mainloop();
    python.exe -c "%_cmd%" %*
    goto :eof
    """.split('\n'))
    
    if 'win' in sys.platform:
        target = os.path.splitext(target)[0]+'.bat'
        header = header_win
    
    script = ("ipython.py", """\
    #!/usr/bin/env python
    import IPython.Shell
    IPython.Shell.start().mainloop()
    """)

    try:
        prefix = sys.argv[1]
        prefix = os.path.expanduser(prefix)
        prefix = os.path.normpath(prefix)
    except IndexError:
        package = __import__(origin)
        pkgroot = os.path.dirname(package.__file__)
        prefix = os.path.dirname(pkgroot)
    assert os.path.isdir(prefix)
    
    filelist = []
    for pattern in includes:
        files = glob.glob(os.path.join(prefix, pattern))
        filelist += sorted(files, key=str.lower)
    assert filelist

    # white shell-script header
    fobj = open(target, 'wb')
    fobj.write(header.encode('ascii'))
    fobj.close()

    # add files to the ZIP archive
    if zlib is not None:
        compression = ZIP_DEFLATED
    else:
        compression = ZIP_STORED
    fobj = ZipFile(target, 'a', compression)
    fobj.writestr(script[0], script[1])
    for filename in filelist:
        arcname = filename[len(prefix)+1:]
        fobj.write(filename, arcname)
    fobj.close()

    # make the ZIP file executable
    try:
        try:
            mode = eval('0o755')
        except SyntaxError:
            mode = eval('0755')
        os.chmod(target, mode)
    except:
        pass
