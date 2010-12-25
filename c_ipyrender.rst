========================
 Rendering templates to clipboard 
========================

This is a 0.8.2 feature, `but you can download it separately <http://ipython.scipy.org/svn/ipython/ipython/trunk/IPython/Extensions/>`_ for older IPython versions.

Suppose that you need to send a "thank you" letter every once in a while, and it's always almost the same. You decide to create a template that you will reuse every time. (ok, a defect submission template is a more realistic example, but let's keep it simple)

The process is this:

Create the template and store it into variable 'letter'::

    [d:/ipython]|9> jed letter.txt
    [d:/ipython]|10> letter = !cat letter.txt
     ==
    ['Thank you $recipient.',
     'We really enjoyed your letter.',
     'Best regards, $myname']
    [d:/ipython]|11> letter = letter.n
    [d:/ipython]|12> letter
              <12> 'Thank you $recipient.\nWe really enjoyed your letter.\nBest re
    gards, $myname'
    [d:/ipython]|13> %store letter
    Stored 'letter' (str)


Now let's try to render it::

    [d:/ipython]|14> import ipy_render
    [d:/ipython]|15> ipy
    ipy_exportdb             ipython.kpf              ipykit
    ipy_render               ipython.pyc              ipykit.zip
    IPython                  IPython_crash_report.txt
    ipython.py               ipykit.py
    [d:/ipython]|15> render letter

    ...

    NameError: name 'recipient' is not defined


Now let's define recipient and myname, and store myname so we don't have to provide it every time::

    [d:/ipython]|16> recipient = 'Yucca'
    [d:/ipython]|17> myname = 'Olli'
    [d:/ipython]|18> store myname
    Stored 'myname' (str)
    

Now we just render the final result and go::

    [d:/ipython]|19> render lett
    letter     letter.txt
    [d:/ipython]|19> render letter
    ---------------> render(letter)
                <19> 'Thank you Yucca.\nWe really enjoyed your letter.\nBest regards
    , Olli'
    [d:/ipython]|20>


The rendered result is also copied to your clipboard.

The template syntax used is `Ka-Ping Yee's 'Itpl' module <http://lfw.org/python/Itpl.py>`_, the same syntax that is used elsewhere in ipython.

