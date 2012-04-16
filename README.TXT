del - A tiny, safer /bin/rm replacement

WHAT IS THIS?
    del is a simple 'rm' replacement with 'Trash'.  Instead of unlink(2)ing
    a file, just move file(s) into 'Trash' directory (default ~/.TRASH), so
    that you can recover files when you deleted them by mistake.


USAGE
    After having del script in your PATH (see next section), just use
    it like rm.
        $ del FILE1 [FILE2...]
    Unfortunately, not all options of rm(1) are available; most notably,
    -f,--foce and -i,--interactive are not implemented.
    -r,-R,--recursive are ignored for compatibility, as del's default
    behaviour is operate on directories recursively.

    See --help output for full list of options
        $ del --help


INSTALL
    Just copy 'del' into /usr/local/bin or wherever in the $PATH.  If your
    /bin/sh does not is not ksh-compatible (such as Bash or Zsh), then modify
    the first line of the script to something like "#!/usr/local/bin/bash"
    '#!/bin/ksh', etc.  Don't forget to give 'x' permission.

    You can have alias like "alias rm=del" in your shell's rc file, but I
    strongly recommend NOT to do it.  Instead of aliasing del to rm, just
    disable rm command like this.  Why?  Read the next section.

        ----------- .bash_profile ------------
        # disable rm command
        alias rm='echo Use "del" instead!!;: '
        --------------------------------------

    The files are moved to $TRASHDIR, which is set to $HOME/.TRASH by default.
    To cleanup the trash directory periodically, use cron to run the script
    with --clean option daily

        -------- User's crontab --------
        59 23 * * * /Your/Path/To/del --clean
        # or if you want to keep old file for more or less other than
        # default 14 days...
        59 23 * * * /Your/Path/To/del --clean=30


ALIASING rm to 'rm -i' CONSIDERED HARMFUL
    People often put alias rm='rm -i' in their .profile or .bash_profile.
    But this isn't safe.
    You will be careless when using rm when you know it's aliased to
    have -i option.  This is OK as long as you only play with your
    own box.  But when you use a system without such alias and yet you're
    not aware that, what happens?  You would carelessly type "rm *" and
    lose all the files.

    c.f. http://superuser.com/questions/384769


BUGS
    The script doesn't have locking mechanism.  If the script is invoked by
    same user at the same time and for files with an identical name, either
    would overwrite the other in $TRASHDIR.  So, just use this script
    interactively and don't call from other (non-interactive) script.

    If your $TRASHDIR sits in different filesystem to files you want to
    delete, it takes much longer time as the content of the files sould
    be copied from the filesystem of the files to $TRASHDIR.


TODO
    * Fix BUGS.
    * Write and execute test code. (Especially for bash/ksh compatibility)


HISTORY
    ver 0.5.0 2012-04-16
         * trash-clean script is now integrated to 'del' script itself,
           as --clean option
         * --verbose and --dry-run option are implemented
    ver 0.4.2 2010-01-24
         * Start using git.
         * Modify trash-clean to use ${TRASHDIR}
    ver 0.4.1 2008-03-22
         * Change grep to case-statement in hope of speed up.
    ver 0.4.0 2003-08-30
         * Use built-in command as much as possible to speed up.
         * Now empty files will be unlinked immediately in addition to symlinks.
         * And other minor bug fix.
    ver 0.3.2 2002-04-21
         * Minor bug fix
    ver 0.3.1 2002-04-21
         * Added comments for "IBM Linux Program Development" course.
    ver 0.3   2001-12-17
         * Renamed env variable: $TRASH -> $TRASHDIR
    ver 0.2   2001-10-02
         * Introduced logging feature.
    ver 0.1   2001-04-03
         * Bug fix.
    ver 0.0   2000-03-25
         * Initial release.


COPYING
    This is free software; you can redistribute it and/or modify it under
    the terms of the GNU General Public License as published by the Free
    Software Foundation; either version 2, or (at your option) any later
    version.

    This is distributed in the hope that it will be useful, but WITHOUT
    ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
    FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
    for more details.


AUTHOR
    Ryoichi KATO <ryo1kato(a)gmail.com>
