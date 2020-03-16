ranger wiki

ranger is a text-based file manager written in Python. Directories are displayed in one pane with three columns. Moving between them is accomplished with keystrokes, bookmarks, the mouse or the command history. File previews and directory contents show automatically for the current selection.
Features include: vi-style key bindings, bookmarks, selections, tagging, tabs, command history, the ability to make symbolic links, several console modes, and a task view. ranger has customizable commands and key bindings, including bindings to external scripts. The closest competitor is Vifm, which has two panes and vi-style key bindings, but fewer features overall.
Contents
 [hide] 
·	1 Installation
·	2 Usage
·	3 Configuration
o	3.1 Defining commands
o	3.2 Colorscheme
o	3.3 File association
·	4 Tips and tricks
o	4.1 Archives
o	4.1.1 Archive extraction
o	4.1.2 Compression
o	4.2 External drives
o	4.3 Image mounting
o	4.4 New tab in current folder
o	4.5 Shell tips
o	4.5.1 Synchronize path
o	4.5.2 Start a shell from ranger
o	4.5.3 Start new ranger instance only if it's not running in current shell
·	5 Troubleshooting
o	5.1 Artifacts in image preview
·	6 See also
Installation
ranger can be installed from the official repositories. Use ranger-git in the AUR for the development version. Optional dependencies, e.g for use with file previews in scope.sh, are described in the ranger-git PKGBUILD.
Usage
To start ranger, launch a terminal and run ranger.
Key
Command
?
Open the manual
1?
List keybindings
2?
List commands
3?
List settings
l, Enter
Launch files
Configuration
After startup, ranger creates a directory ~/.config/ranger. To copy the default configuration to this directory issue the following command:
ranger --copy-config=all

·	rc.conf - startup commands and key bindings
·	commands.py - commands which are launched with :
·	rifle.conf - applications used when a given type of file is launched.
rc.conf only needs to include changes from the default file as both are loaded. For commands.py, if you do not include the whole file, put this line at the top:
from ranger.api.commands import *

To add a keybind that moves files to a created directory ~/.Trash/ with DD, add to ~/.config/ranger/rc.conf:
map DD shell mv -t /home/user/.Trash %s

See man ranger for general configuration.
Defining commands
Continuing the above example, add the following entry to ~/.config/ranger/commands.py to empty the trash directory ~/.Trash.
class empty(Command): 
    """:empty

    Empties the trash directory ~/.Trash 
    """

    def execute(self): 
        self.fm.run("rm -rf /home/myname/.Trash/{*,.[^.]*}")

To use it, type :empty and Enter with tab completion as desired.
Warning: [^.] is an essential part of the above command. Without it, all files and directories of the form ..* will be deleted, wiping out everything in your home directory.
Colorscheme
Create the colorschemes subfolder in .config/ranger/colorschemes:
mkdir .config/ranger/colorschemes

then copy your new newscheme.py into that folder. Alter the default color scheme in the .config/ranger/rc.conf file:
set colorscheme newscheme

File association
Modify ~/.config/ranger/rifle.conf. As the beginning lines are executed first, you should put modifications at the top of the file. For example, the following entry will open a tex file with kile:
ext tex = kile "$@"

To open all files with xdg-utils:
has xdg-open, flag f = xdg-open "$1"

Tips and tricks


This article or section needs language, wiki syntax or style improvements.
Reason: (Discuss in Talk:Ranger#)
Archives
These commands use atool to perform archive operations.
Archive extraction
The following command implements archive extraction by copying (yy) one or more archive files and then executing :extracthere on the desired directory.
import osf
from ranger.core.loader import CommandLoader
c
class extracthere(Command): 
    def execute(self): 
        """ Extract copied files to current directory """ 
        copied_files = tuple(self.fm.copy_buffer)

        if not copied_files: 
            return

        def refresh(_): 
            cwd = self.fm.get_directory(original_path) 
            cwd.load_content()

        one_file = copied_files[0] 
        cwd = self.fm.thisdir 
        original_path = cwd.path 
        au_flags = ['-X', cwd.path] 
        au_flags += self.line.split()[1:] 
        au_flags += ['-e']

        self.fm.copy_buffer.clear() 
        self.fm.cut_buffer = False 
        if len(copied_files) == 1: 
            descr = "extracting: " + os.path.basename(one_file.path) 
        else: 
            descr = "extracting files from: " + os.path.basename(one_file.dirname) 
        obj = CommandLoader(args=['aunpack'] + au_flags \ 
                + [f.path for f in copied_files], descr=descr)

        obj.signal_bind('after', refresh) 
        self.fm.loader.add(obj)

Compression
The following command allows the user to compress several files on the current directory by marking them and then calling :compress package name. It supports name suggestions by getting the basename of the current directory and appending several possibilities for the extension. You need to have atool installed. Otherwise you will see an error message when you create the archive.
import osf
from ranger.core.loader import CommandLoader
c
class compress(Command): 
    def execute(self): 
        """ Compress marked files to current directory """ 
        cwd = self.fm.thisdir 
        marked_files = cwd.get_selection()

        if not marked_files: 
            return

        def refresh(_): 
            cwd = self.fm.get_directory(original_path) 
            cwd.load_content()

        original_path = cwd.path 
        parts = self.line.split() 
        au_flags = parts[1:]

        descr = "compressing files in: " + os.path.basename(parts[1]) 
        obj = CommandLoader(args=['apack'] + au_flags + \ 
                [os.path.relpath(f.path, cwd.path) for f in marked_files], descr=descr)

        obj.signal_bind('after', refresh) 
        self.fm.loader.add(obj)

    def tab(self): 
        """ Complete with current folder name """

        extension = ['.zip', '.tar.gz', '.rar', '.7z'] 
        return ['compress ' + os.path.basename(self.fm.thisdir.path) + ext for ext in extension]

External drives
External drives can be automatically mounted with udev or udisks. Drives mounted under /media can be easily accessed by pressing gm (go, media).
Image mounting
The following command assumes you are using cdemu as your image mounter and some kind of system like autofs which mounts the virtual drive to a specified location ('/media/virtualrom' in this case). Do not forget to change mountpath to reflect your system settings.
To mount an image (or images) to a cdemud virtual drive from ranger you select the image files and then type ':mount' on the console. The mounting may actually take some time depending on your setup (in mine it may take as long as one minute) so the command uses a custom loader that waits until the mount directory is mounted and then opens it on the background in tab 9.
import os, timef
from ranger.core.loader import Loadablef
from ranger.ext.signals import SignalDispatcherf
from ranger.ext.shell_escape import *
c
class MountLoader(Loadable, SignalDispatcher): 
    """ 
    Wait until a directory is mounted 
    """ 
    def __init__(self, path): 
        SignalDispatcher.__init__(self) 
        descr = "Waiting for dir '" + path + "' to be mounted" 
        Loadable.__init__(self, self.generate(), descr) 
        self.path = path

    def generate(self): 
        available = False 
        while not available: 
            try: 
                if os.path.ismount(self.path): 
                    available = True 
            except: 
                pass 
            yield 
            time.sleep(0.03) 
        self.signal_emit('after')
c
class mount(Command): 
    def execute(self): 
        selected_files = self.fm.thisdir.get_selection()

        if not selected_files: 
            return

        space = ' ' 
        self.fm.execute_command("cdemu -b system unload 0") 
        self.fm.execute_command("cdemu -b system load 0 " + \ 
                space.join([shell_escape(f.path) for f in selected_files])) 

        mountpath = "/media/virtualrom/"

        def mount_finished(path): 
            currenttab = self.fm.current_tab 
            self.fm.tab_open(9, mountpath) 
            self.fm.tab_open(currenttab)

        obj = MountLoader(mountpath) 
        obj.signal_bind('after', mount_finished) 
        self.fm.loader.add(obj)

New tab in current folder
You may have noticed there are two shortcuts for opening a new tab in home (gn and Ctrl+n). Let us rebind Ctrl+n:
rc.confmap <c-n>  eval fm.tab_new('%d')

Shell tips
Synchronize path
ranger provides a shell function /usr/share/doc/ranger/examples/bash_automatic_cd.sh. Running ranger-cd instead of ranger will automatically cd to the last browsed folder.
If you launch ranger from a graphical launcher (such as $TERMCMD -e ranger, where TERMCMD is an X terminal), you cannot use ranger-cd. Create an executable script:
ranger-launcher.sh#!/bin/she
export RANGERCD=true$
$TERMCMD

and add at the very end of your shell configuration:
.shellrc$RANGERCD && unset RANGERCD && ranger-cd

This will launch ranger-cd only if the RANGERCD variable is set. It is important to unset this variable again, otherwise launching a subshell from this terminal will automatically relaunch ranger.
Start a shell from ranger
With the previous method you can switch to a shell in last browsed path simply by leaving ranger. However you may not want to quit ranger for several reasons (numerous opened tabs, copy in progress, etc.). You can start a shell from ranger (S by default) without losing your ranger session. Unfortunately, the shell will not switch to the current folder automatically. Again, this can be solved with an executable script:
shellcd#!/bin/she
export AUTOCD="$(realpath "$1")"
$
$SHELL

and - as before - add this to at the very end of your shell configuration:
shellrccd "$AUTOCD"

Now you can change your shell binding for ranger:
rc.confmap S shell shellcd %d

Alternatively, you can make use of your shell history file if it has any. For instance, you could do this for zsh:
shellcd## Prepend argument to zsh dirstack.B
BUF="$(realpath "$1")$
$(grep -v "$(realpath "$1")" "$ZDIRS")"e
echo "$BUF" > "$ZDIRS"
z
zsh

Change ZDIRS for your dirstack.
Start new ranger instance only if it's not running in current shell
Put this in your shell's startup file:
rg() { 
    if [ -z "$RANGER_LEVEL" ] 
    then 
        ranger 
    else 
        exit 
    fi}
}

Execute rg to start or restore ranger.
Troubleshooting
Artifacts in image preview
Borderless columns may cause stripes in image previews. [1] In ~/.config/ranger/rc.conf set:
set draw_borders true
