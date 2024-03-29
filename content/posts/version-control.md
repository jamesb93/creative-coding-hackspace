# Creative Version Control

### [James Bradbury](https://www.jamesbradbury.net) and [Jacob Hart](https://jacob-hart.com)
---
# Introduction
All creative coders can benefit from some form of version control for their creative work. Not only does version control provide a mechanism for backing up work it can aid collaboration, assist in tracking strands of ideas and maintain a chronological history of steps that culminate in the final output.

To really understand version control (and to fix some gnarly issues) it is useful to know a bit about the command-line and its relationship to the operating system of your machine. Knowing a bit about the command-line can also be useful to you when trying to integrate code from other people's work into your practice. I can definitely recall times when I would come across code, programs or bits of software left to rot that needed to be compiled and I had no idea where to start.

In this week's hackspace **Francesco** and **James** gave a short tutorial on the basics of command-line for both MacOS and Linux. They covered fundamentals such as moving around the filesystem, making directories and other fundamental principles. They also demonstrated how to create a git repository, commit changes and have another coder collaborate on that work with you. The workshop was aimed at UNIX systems, so a lot or all of these commands won't work on windows. However, if you are a windows user you might like to take a look at the Windows Subsystem for Linux (WSL) and [MSYS](http://www.mingw.org/wiki/msys) which provides a UNIX like interface on windows.

# What is the command line?
The command-line is an interface to your computer that emulates the text-based interface of an arcane terminal. All you are presented with is text and all programs that operate on the command-line accept and produce text. They hark back to an era where network connections had minimal bandwidth and CPU cycles and RAM were precious resources. Even today though, the command-line can be more powerful and flexible than a GUI application and once you become familiar with the commands it can replace most of the day-to-day operations on your machine.

On MacOS you can open up the default terminal by opening Terminal.app. Many people change their default terminal to [iTerm2](https://iterm2.com) as it has additional features. If you are just beginning though I would recommend sticking to the default app and once you find that limiting you can investigate other terminal emulators. On Linux, popular terminal emulators are GNOME terminal, Guake, Yakuake and xterm but entirely depends on your distribution.

## The shell
When you open the terminal you are presented with almost nothing, just a small block of text on the left of the screen with the name of your computer. At this point you are ready to execute some commands.

{{< image "/img/cvc/blank-terminal.png" "A blank ZSH terminal" >}}

The first thing to know is that the application which interprets the commands you send to the terminal is a program or _shell_. There are many flavours of _shells_ that you can use and by default MacOS will use **bash** or **zsh** depending on your OS version. There are small differences between these two shells and for the purpose of what this article covers it is not necessary to understand too deeply how they vary. 

## ls
The second concept to understand is that the shell is always _somewhere_ in your filesystem. When you boot up a terminal, it starts at your home folder. You can see at the top of the preceding image that my terminal tells me where it currently is (/Users/james) and that it is running **zsh** as the shell. The home folder can also be abbreviated to **~/** which you can see on the left side of the image. Whenever you see `~/` you can translate this to `/Users/yourmachinename` for MacOS or `/home/yourmachinename` for Linux.

Try typing the command **ls** now and see what happens.

{{< image "/img/cvc/ls.png" "Executin the ls command" >}}

Congratulations, you just executed your first program from the command-line! **ls** is a program that lists information about the files and folders of the current directory that you are in. As you can see below, what is displayed by executing **ls** has parity the files and folders listed in finder at the same location.

{{< image "/img/cvc/finder.png" "Parity with the finder" >}}

## cd
Now you might be wondering how you can navigate to different folders and files. The **cd** (change directory) allows you to move around as you would in Finder or a GUI application. As you can see below, I execute the command `cd dev` which then transports my **current working directory** to the dev folder. The **prompt** of my command-line changes to reflect this movement throughout the filesystem and I can then execute `ls` again to show files inside **~/dev**. Whenever we pass additional bits of text to a program these are called _arguments_. In this case `dev` is an argument to `cd` and cd interprets what you mean by the simple virtue that it immediately comes after `cd`. Other programs can take arguments with a flag in both a long and short format like `-l` or `-limit` for example.

{{< image "/img/cvc/cd.png" "Changing directories" >}}

## mkdir

Say now you want a new folder in this directory and to create a new file inside of that folder.

Let's execute the command `mkdir` (make directory) with an argument for the name of the folder that we would like to create. We can verify that the folder was created by looking at Finder and by changing directory into this new folder.

{{< image "/img/cvc/mkdir.png" "The mkdir command" >}}

## touch

Now to create a file, we can execute the program `touch` with the new file name as the first argument. Again we can verify that this new file exists by looking in the finder as well as calling `ls` again to print the files inside of a directory.

{{< image "/img/cvc/touch.png" "The touch command" >}}

## piping

One final concept to understand is that the inputs and outputs of the shell can be 'piped' between each other as they emit and receive information that fits a specification. For example, I can pipe `ls` to another program called `grep` which you might like to think of as a filter of that text like find/replace in a text editor. In the below example, I pipe the output of `ls` to `grep` so that I can find all the files that contain the phrase _gesture_maker_. You might also be interested in the `>` redirection pipe which allows you to pipe the output of a command line program to a file. For example, try `ls > ls_output.txt` which will create a new file contaning the output of ls.

{{< image "/img/cvc/piping.png" "Piping commands" >}}

This just about covers enough material to get you started in the terminal. Of course, the best way to learn is to try using the terminal and to get stuck. Once you become more fluent in the commands it can be a great way to interact with your operating system with just your keyboard and with a minimal interface. It is also helpful for later when you want to create and manage git respositories more fluently and without a GUI application.

---

# git

Okay, so now lets start learning some **git**. The first thing to know is **git** is __NOT__ github. git can exist without the internet ever being connected to your machine and is just a program which tracks changes inside a directory. All that this is is a normal directory on your computer with a small hidden folder named **.git**. This hidden folder contains information about changes, branches, remotes and other git related concepts which help you to version control the files that exist in the same directory.

Conveniently, most UNIX based operating systems are shipped with git installed and you can use it without every having to install it yourself. What we're going to do is learn first how to _init_ a git repository, _commit_ changes to a file inside the repository and then _push_ those changes to a _remote_. After that, it will be demonstrated how someone else can _fork_ your repository, _commit_ their own changes and subsequently submit a _pull request_ which you integrate back into the original project.

To make things easier, it can be useful to download a _git client_ on your machine. Although most of the following things can be done via the command line, one of these programs will make your life a whole lot easier. We can recommend [Fork](https://git-fork.com), [GitKraken](https://www.gitkraken.com) or [GitHub Desktop](https://desktop.github.com).

## initialising (init) a repository

So, you have an idea for a great project where version control would be useful. Maybe this project exists already, maybe you haven't even started it yet. You'll want to make sure that all the files concerning your project are grouped into one folder. Next, we need to create the hidden **.git** folder inside this project folder which will handle all the git functionalities. We can do this from the command line - below we've gone to the desktop, created our project folder, then used the command `git init` and we're done!

{{< image "/img/cvc/01_innit_cmd_line.png" "git init command" >}}

If you go to the project folder in your finder, you'll find that it appears to be empty. This is because the **.git** folder is hidden - to show it, hit `Cmd+Shift+.` and you should see this...

{{< image "/img/cvc/02_hidden_git_folder.png" "see hidden folders" >}}

Alternatively you could use the `ls -a` in the command line to see all the hidden files and folders. You won't ever need to do anything with this folder, so you can go ahead and hide it again. But know that this is where all the magic of git happens! When you create a repository with one of the programs like _Fork_ or _GitKraken_, this folder gets created automatically. Now we can start adding files to our project.

## commiting (commit) to a repository

Let's create some simple, empty files using the `touch` command. These files are now in the folder, but we haven't told git to add them yet. To create a new _commit_ (a state of your project that you can go back to), there are two parts: _adding_ files and modifications, then _commiting_ these added files and modifications. Here we add files using the  `git add` command, then commit using the `git commit` command. When you commit, you'll be asked to describe the modifications made - we just wrote _initial commit_, to indicate that this was the first commit to git (we then hit ESC and type :wq to exit the text editor).

{{< image "/img/cvc/03_commit.png" "command line commit" >}}

If we open up our repository in one of the programs talked about above, we can visualise these changes we made. Here, in [Fork](https://git-fork.com), we see our initial commit, with the two files we added. A quick note, we could have just used the command `git commit -a` to add and commit everything, and `git commit -m "initial commit"` to bypass the text editor part.

{{< image "/img/cvc/04_initial_commit_fork.png" "initial commit in fork" >}}

How can we add the other file to our git repository? We could do it in the same way, using the command line. However, it may be easier to use the GUI in _Fork_. If we look at the top left, we see that there is one change that can be commited. Let's click on this to see the changes. We see that the file we didn't initially commit is there. To make a new commit, we first need to _stage_ the changes we want to make, add a _comment_ for out commit in the bottom right (this was the text editor part from before), then click _commit_! Now if we go back to the _master branch_, we see that there is a new commit.

{{< image "/img/cvc/05_second_commit.png" "second commit" >}}

We now see how we can keep a log of how a project progresses, and use this to go back to a project's previous state. But what if we want to have this log online, and let other people work on it also? This is where something like [github](https://github.com) will come in.

## pushing (push) changes to a repository

First of all, we're going to have to get our repository linked up to _github_. To do this, go to _github_ and select _new repository_. You'll be presented with a screen like the one below. You can give it a name and a description, and set a few other parameters. The _.gitignore_ is for giving git files you don't want to be tracked (for example, _.wav_ audio files are heavy and not necesarily something you would want to upload to github). We can also choose to make a _README.md_ file - this is a file in the root directory of you project folder which will appear on the github page of the repository, usually explaining what the project is about. It's written in markdown format, and fairly simple to format. 

{{< image "/img/cvc/06_github_repo_create.png" "creating a repo on github" >}}

Next you'll be directed to the page below. Github makes our life easier by telling us exactly what to do! We need to link up the folder on our local machine to the repository made on github. To do this, in the command line we copy the the text under the **...or push an existing repository from the command line** section, the `git remote add origin <ssh or https>` command. This will link up your repositories.

{{< image "/img/cvc/07_github_ssh.png" "created repo github" >}}

Note that all of this can be done in the programs talked about earlier with an easy to use GUI, but it's useful to know how it's done in the command line. The last step is to _push_ content on your local machine onto the github repo. You can do this using the `git push` command, or by finding the _push_ button within you program. Note that when pushing, you can choose which _branch_ to push to, we'll go over branches in the next, final section.

## forking (fork) a repository

Say we would like to work off of someone else's repository. We will need to _fork_ the repository in question. This is as easy as going to the repository on github, and pressing _fork_ in the top right of the screen. We will then need to _clone_ this repository onto our machine. We can do this from the command line with the `git clone <repository url>` or through one of our programs. Below we see that we have _forked_ the _FrameLib_ repository and cloned it onto our machine. This is our own version of _FrameLib_ at the time it was forked.

{{< image "/img/cvc/08_forked_repo.png" "forked framelib repository" >}}

Note that there are several _branches_ which can run in parallel indicated by the coloured lines along the side. This is so that you can work with serveral versions of the code, versions which could potentially break or conflict with each other. When working with branches, we recommend you use a GUI as things can get quite complicated otherwise. Creating a new branch is as simple as selecting _create new branch_ in _Fork_. You can then merge branches back into the main branch, or let them die! The best part about using **git** branches and commits is that you can now revert between states of a project as well as states that belong to a specific branch. Perhaps you intend to make implement a new synth in your Supercollider instrument but you would like to keep a stable branch of the code. In this situation you could create a new branch to do this work on. At any stage you can swap between this new experimental branch and your master branch to alternate between two different versions of the codebase. In a GUI application like _Fork_ its as simple as double clicking the different branches on the left-most side of the window. The same goes for commits allowing you to explore different states of your project.

Finally, once you've made changes to a forked repository, you can ask the original creator if they would like to incorporate the changes you've made into their repository. This is called a _pull request_. This is as easy as pushing any changes you've made to your github fork of the repository, and then selecting _pull requests_.