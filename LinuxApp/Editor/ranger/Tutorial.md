Ranger tutorial

In my last post, I briefly mentioned autojump’s awesome j command to quickly move between directories. 
But once in a while you need that little extra bang when working with files and directories. 
Well, there are file managers like Midnight Commander, for those stuck in an era when people shared Monkey Island on floppy disks but it’s certainly not suited for vi afficionados. 
For those, there is ranger – “a file manager withVI key bindings. 
It provides a minimalistic and nice curses interface with a view on the directory hierarchy. 
The secondary task of ranger is to psychically guess which program you want to use for opening particular files.”


ranger previewing a Ruby source file

Ranger is packaged for most distribution but you can also install a Git checkout withsudo make install or just run it directly from the source tree. Just like Vim itself, ranger can be intimidating, even more so when considering that you are dealing with files and not text buffers on which changes can be reverted with u. However, whenever in doubt just type ? to read ranger’s manpage.
After starting it with ranger, the first thing you will notice is a window layout similar to MacOS’ finder: The left column shows the parent directory, the middle column the content and selection of the current directory and the right column child directories and files or a preview of the currently selected file.
General usage
You can navigate through the file system with either h, j, k and l or the arrow keys. To quickly browse large lists you can also use the well-known gg, G, Ctrl+d and Ctrl+u key bindings. Once you are on a file and go left, you open it with whatever ranger thinks is appropriate. One important key map to remember is Ctrl+h, that shows and hides hidden dot files. You will thank me for this.
By default, ranger maps several locations to the g key binding: gr brings you to/, ge to /etc, gh to $HOME etc. You can define more of these by bookmarking the current directory with m and change to it with the backtick key. For example, I like to bookmark ~/Downloads with md to the d register. Another quick way to descend into the hierarchy is the :find tool that you can activate with f. Type some letters and it will change to the first directory or open the first file that matches the prefix exactly.
One of the greatest features of ranger, is its preview capability. However, it relies on external programs and out of the box, ranger displays only text files as … plain text. Therefore, you should immediately do a
sudo apt-get install highlight atool caca-utils w3m poppler-utils

on Debian systems to install programs for previewing source code, HTML files, images, archives and PDF files.
Selecting and filtering files
To modify files (from now on the term file also includes directories), you need to select them with Space. If nothing is actively selected, the file under the current cursor position is selected. A special selection key is v, that selects all files that are currently not selected (including the one under the cursor) and un-selects those that are. To quickly un-select all selected files, use V.
To get a better view of a potentially large list of unrelated files, you can filter the view with the :filter command (accessible through zf) that executes a simple strstr()search. To view all files again, run :filter without any arguments.
Another way to restrict the files without hiding them is to search for them with / and navigate through the result with n and N.
Modifying files
With files selected, you can use ranger’s command line accessible with : to :delete,:rename and :chmod the permissions of the selected files. Most shell commands are also available as ranger commands, so you don’t need to leave ranger to :touch a file or:mkdir a new directory. However, if you want to quickly access a shell you can type S.
Moving files works similar to yanking and pasting in vi, but not exactly the same. You usually don’t yank into registers but really mark files to be moved. Yanked files For example, yy yanks the current file whereas ya adds the current file to the list of already yanked files. You can remove files from the yank list with
yr
. Pasting a yanked file somewhere else with pp copies it there, pasting a deleted file using d moves it.
Customizing ranger
To make any customizations to ranger, you should first copy the configuration files to~/.config/ranger with
ranger --copy-config=all

and edit these files locally. In the directory, you will find five configuration and customization files: 1) apps.py defines applications to launch for certain file types, 2)commands.py defines commands to be executed in rangers command line mode, 3)options.py is ranger’s actual configuration, 4) rc.conf contains simple configuration such as key maps and 5) scope.sh defines applications to preview certain file types.
In principle, everything is extremely well documented and you should be able to figure out how to add new key maps or commands. But for the lazy ones, I will give a quick tour.
Binding keys
All default key maps are defined in ~/.config/ranger/rc.conf. Usually, I don’t customize key maps too heavily but in this case I like to have F unset the currently set filter:
map F filter

If you want to wait for user input, you would map to the console
map F console filter pdf

and see :filter pdf upon pressing F.
New commands
Commands are subclasses of the ranger.api.commands.Command class and in most cases you will just implement the execute() method. In this first example, I adapted the :renamecommand to rename underscored names to dashed names and vice versa.
class toggled(Command):
    """
    :toggled

    Changes the name of currently highlighted files from foo_bar_baz.txt to
    foo-bar-baz.txt and vice versa.
    """

    def execute(self):
        from ranger.fsobject import File
        from os import access

        # yes, this is pathetic
        s = str(self.fm.env.cf)
        s1 = s.replace('-', '_')
        s2 = s.replace('_', '-')
        new_name = ''.join([b if a != b else c for (a,b,c) in zip(s,s1,s2)])

        if access(new_name, os.F_OK):
            return

        self.fm.rename(self.fm.env.cf, new_name)
        f = File(new_name)
        self.fm.env.cwd.pointed_obj = f
        self.fm.env.cf = f

The next example shows that you don’t have to be bothered with files at all. If passed a command name it will print the corresponding docstring. Not too useful actually but it demonstrates the ease of customization.
class cmdhelp(Command):
    """
    :cmdhelp <command>

    Show docstring of <command>
    """

    def execute(self):
        from sys import modules
        from inspect import getmembers, isclass

        clsname = self.rest(1)
        clsmembers = dict(getmembers(modules[__name__], isclass))
        if clsname in clsmembers:
            return self.fm.notify("%s" % clsmembers[clsname].__doc__, bad=False)
        else:
            return self.fm.notify("`%s' is not a command!" % clsname, bad=True)

New previewers
Again, adding a new previewer is pretty simple. Basically, you decide how to check what kind of type a file is (e.g. by extension or mime type), use a tool on that file that outputs some useful information as (colored) text and put the call in scope.sh. The following example shows information about Debian packages. However, to make it work, you need to remove the deb handling from the archive section above.
case "$extension" in 
    # ...
    deb)
        dpkg --info "$path" | head -n $maxln
        success && exit 5 || exit 1;;
esac

As you can see, I simply call dpkg and trim the output. Depending on the return value, ranger will show the result in a certain way or discard it.

ranger previewing a deb package
Apart from the added deb previewer, there is another nice detail that one can easily overlook: The path to this directory, which is actually /var/cache/apt/archives, is trimmed quite intelligently!


In this post, I barely scratched the surface of ranger and there is still a binders full of possibilities. So, check it out, use it and give those guys a beer when you see them. Thumbs up for this nice tool!
