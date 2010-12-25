===================
 Shadow History and %rep 
===================

This is a 0.8.2 feature.

Both readline history and standard IPython history are fragile beasts - readline history disappears when you have multiple IPython sessions open, and standard IPython history (shown by %hist) disappears when you quit IPython.

In shadow history, however, all the commands (well, most - the ones starting at the beginning of input line) are stored forever. You can search through shadow history by running the command '''%hist -g''' (g is for grep)::

    [d:ipython/dist]|38> hist -g change
    0720: svn revert doc/ChangeLog
    0721: svn ci -m "whitespace cleanup changelog"
    0766: hist -g change
    ===
    ^shadow history ends, fetch by %rep <number> (must start with 0)
    === start of normal history ===
    38: _ip.magic("hist -g change")
    [d:ipython/dist]|39>


Note how the command greps through the shadow and normal history. Shadow history entries have the 0 as prefix (e.g. 0721) to tell them apart from normal history entries.

To fetch the command for editing at command line, invoke '''%rep''' with history line number::

    [d:ipython/dist]|39> %rep 0721
    [d:ipython/dist]|40> svn ci -m "whitespace cleanup changelog"

The command 0721 is not executed directly; rather, the cursor is left blinking at the end of line 40 for editing.

-------------------
 Other uses of %rep 
-------------------

The most typical meaning of %rep is "fetch this history item for command line editing". More creative uses are:

***%rep line1-line2 line3-line4...'**::

    [d:ipython/dist]|42> print "hello"
    hello
    [d:ipython/dist]|43> print "world"
    world
    [d:ipython/dist]|44> rep 42-43
    lines [u'print "hello"\nprint "world"\n']
    hello
    world


And just **calling %rep without arguments**, which fetches the contents of _ (last computation result) for command line editing, allowing you to create elaborate command lines without manual copy-pasting with mouse::

    [~/_ipython]|60> a = !ls *.pyo
     ==
    ['ipy_profile_sh.pyo', 'ipy_user_conf.pyo']
    [~/_ipython]|61> a
              <61> SList (.p, .n, .l, .s available). Value:
    0: ipy_profile_sh.pyo
    1: ipy_user_conf.pyo
    [~/_ipython]|62> 'rm ' + a.s
              <62> 'rm ipy_profile_sh.pyo ipy_user_conf.pyo'
    [~/_ipython]|63> rep
    [~/_ipython]|64> rm ipy_profile_sh.pyo ipy_user_conf.pyo

And the cursor is left blinking at the line 64.

-----------------------
 Advanced topics 
-----------------------
 * Every command is only once in the shadow history. The history line number indicates when the command was last run.
 * Shadow history can be cleared by '%clear shadow_nuke'
 * If your IPython prompt becomes slow because there is too much stuff in shadow history (every command is stored immediately after pressing enter), enter '%clear shadow_compress'. This is yet to prove a problem, though.
 * If you want to know how much space shadow history is taking, it's stored at ~/_ipython/db/shadowhist (on pickleshare hashed storage). %clear shadow_nuke is essentially the same is deleting that directory.

