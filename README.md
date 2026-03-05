A self made project to learn Git with boot.dev's guidence

Version Control:
    Git is a tool for tracking changes(snapshots/photos) to files over time, allowing you to "travel back in time" to previous versions of your work.


Command Syntax:
    Arguments in angle brackets <> are mandatory and must be provided when running the command.
    Arguments in square brackets [] are optional and can be included if needed.
        -For example, to create a new directory in your terminal, you would run:
            -'mkdir' <directory-name>
                -'mkdir' is the command
                -<directory-name> is a required argument''
        Flags:
            - A flag is like a special instruction you give to a command, usually starting with a -. For example, '-m' tells Git "I want to attach a message to this record."


Configuring Git:
    You've learned to set your identity (name and email) and default branch names using git config set.
    -git config set [scope] <key> <value> *1
        -Example: git config set user.name "Your Name"


The Repository:
    A repo is essentially just a directory that contains a project (other directories and files). The only difference is that it also contains a hidden '.git' directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project. The '.git' directory is the heart of your project, containing the entire history and configuration of your repository.
        -In order to make a '.git' repo you should:
            -make you project's directory
            -inside it execute the 'git init [directory]' command
        Once done you should now have a hidden '.git' directory in your project's directory. This means you've successfully created a new Git repository! List (ls -a) the contents of the directory to confirm.

Porcelain and Plumbing(90/10 % rule):
    In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. The porcelain commands are the ones that you will use most often as a developer to interact with your code.
        -Some porcelain commands are:
            -git config set [scope] <key> <value> *1
                -Example: git config set user.name "Your Name"
            -git status
            -git add <file-path> *2
            -git commit -m <message>
            -git log
            -git push
            -git pull
            -git clone <repository-url>
        -Some examples of plumbing commands are:
            -git cat-file <type> <hash>
                -If a flag is used '<type>' isn't needed. Common flags: -p (print content), -t (show type).
            -git hash-object <file-path>
            -git ls-tree <tree-ish> *3
            -git rev-parse <name> *4
    *1:[scope] is global
    *2:(git add ., where the . acts as the <file-path> for "everything in the current directory.")
    *3:<tree-ish>: This is a fancy Git term for "something that points to a tree." Usually, this is the hash of a tree object, or simply HEAD
    *4:<name>:It tells you the full 40-character SHA-1 hash that the name points to. If you ask Git git rev-parse HEAD, it will tell you the exact hash of the commit you are currently standing on.

The Three States:
    Working Directory: Where you modify your files.

    Staging Area (Index): Where you "prepare" changes for the next commit using git add.

    Commit History: Where Git permanently stores snapshots of your project using git commit.
    
    The Snapshot Model:
    Unlike some systems that store only "changes" (deltas), Git stores an entire snapshot of your files for every commit.

    Deduplication:
         Because Git uses hashes, it is efficient. If a file remains unchanged between commits, Git simply points to the existing hash rather than storing a duplicate copy.


Half of Git:
    Half of your workflow as a developer will just be 3 simple commands:
        -git status
        -git add
        -git commit
    It's most of what you need to work effectively as a solo developer. Another 40% of Git is about collaborating and storing your work on a remote server.
    The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics.

Content Addressing:
    Git identifies files (blobs) and commits by a unique SHA-1 hash based on their content. If the content doesn't change, the hash doesn't change.


Inspection Tools:
    git status: Shows the state of the working directory and staging area.

    git log: Displays the history of commits.

    git cat-file -p: Allows you to peer into Git's internal objects to see the actual content of blobs, trees, and commits.


()