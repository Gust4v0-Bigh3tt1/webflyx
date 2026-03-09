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
    Before the work begin, you need to tell Git who you are! This information is attached to everything you do so people know who made the changes.
        -Identity: You set your user.name and user.email.
        -Default Branch: You can tell Git what to call your main workspace (usually main).
        -Commands:
            Set: 'git config set [scope] <key> <value>'
            Get: 'git config get [scope] <key>'
            Example: 'git config get init.defaultBranch'
        -Keys:
            -If the key is user.name, the value is "Your Name".
            -If the key is user.email, the value is "email@example.com".
            -If the key is init.defaultBranch, the value is "main"


The Repository:
    A repo is essentially just a directory that contains a project (other directories and files). The only difference is that it also contains a hidden '.git' directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project. The '.git' directory is the heart of your project, containing the entire history and configuration of your repository.
        -In order to make a '.git' repo you should:
            -make you project's directory
            -inside it execute the 'git init [directory]' command
        Once done you should now have a hidden '.git' directory in your project's directory. This means you've successfully created a new Git repository! List (ls -a) the contents of the directory to confirm.

Porcelain and Plumbing(90/10 % rule):
    In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. The porcelain commands are the ones that you will use most often as a developer to interact with your code.
        -Some porcelain commands are:
            -git config set [scope] <key> <value> (*1)
                -Example: git config set user.name "Your Name"
            -git status
            -git add <file-path> (*2)
            -git commit -m <message>
            -git log
            -git push
            -git pull
            -git clone <repository-url>
            -git config set [scope] <key> <value>
            -git config get [scope] <key>
        -Some examples of plumbing commands are:
            -git cat-file <type> <hash>
                -If a flag is used '<type>' isn't needed. Common flags: -p (print content), -t (show type).
            -git hash-object <file-path>
            -git ls-tree <tree-ish> (*3)
            -git rev-parse <name> (*4)
    (*1):[scope] is global
    (*2):(git add ., where the . acts as the <file-path> for "everything in the current directory.")
    (*3):<tree-ish>: This is a fancy Git term for "something that points to a tree." Usually, this is the hash of a tree object, or simply HEAD
    (*4):<name>:It tells you the full 40-character SHA-1 hash that the name points to. If you ask Git git rev-parse HEAD, it will tell you the exact hash of the commit you are currently standing on.

The Three States:
    Git tracks your work through three different stages. A good analogy to think of it is:

    1-Working Directory (The Live Room):
    You are in this step when you're changing/modifying a directory. think of it as a room, you can move furniture, paint walls, or add new items.

    2-Staging Area/Index(The Camera Viewfinder):
    after you made the changes in step 1, you're now going to "prepare to take a photo of the room" and mark the changes made.
        Command: 'git add <file-path>'.

    3-Commit History(The Photo Album):
    Where Git takes the photo and permanently stores snapshots (photos) of your project
        Command: 'git commit -m <message>' (the <message> must be in "").

    The Snapshot Model:
    Unlike some systems that store only "changes" (deltas), Git stores an entire snapshot (photo) of your files for every commit.
    When Git hashes a file(blob), it only cares about two things: The Size and the Content.
    Each "photo" is saved with a hash, the "photos" "ID".
        Commit Hash(ID): Depends on the content + context (Who, when, and what message):
            which is made by taking:
            -The Tree Hash: reference to the "snapshot" of all files/folders at that moment (a photo of the roots in which the file belongs).
            -The Parent Hash: The ID of the commit that came before it (this creates the "chain" of history).
            -The Author & Committer: Your user.name and user.email.
            -The Timestamp: The exact second the commit was made. (*1)
            -The Message: Whatever you wrote after the -m flag.
        Blob Hash: Only depends on the content. (Same words = Same hash) (*2):
            The Size: It adds how many characters are in the file.
            The Content: It adds every single letter and space inside the file.

    (*1):even if you make two identical commits with the same files and message, they will have different hashes because they happened at different times
    (*2):Git does not include the filename in a blob's hash! That's why two files with different names but the same content will have the identical hash (Deduplication)

    Deduplication:
         Because Git uses hashes, it is efficient. If a file remains unchanged between commits, Git simply points to the existing hash rather than storing a duplicate copy.
         types of deduplication:
            Duplicate Content (Space):
                -if apple.txt and orange.txt both contain the word "Fruit", Git only saves one copy of "Fruit" and gives both names the same hash.
            Unchanged Files (Time):
                -If you have a project with 100 files, but you only change one file and make a new commit, Git is smart! It doesn't save 100 new files. It only saves the one you changed and points the new commit to the 99 hashes it already has from the previous commit.


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