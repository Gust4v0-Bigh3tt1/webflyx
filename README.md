A self made project to learn Git with boot.dev's guidence

Version Control:
    Git is a tool for tracking changes(snapshots/photos) to files over time, allowing you to "travel back in time" to previous versions of your work.


Command Syntax:
    Arguments in angle brackets <> are mandatory and must be provided when running the command.
    Arguments in square brackets [] are optional and can be included if needed.
        -For example, to create a new directory in your terminal, you would run:
            -'mkdir' <directory-name>
                -'mkdir' is the command
                -<directory-name> is a required argument
        Flags:
            - A flag is like a special instruction you give to a command, usually starting with a -. For example, '-m' tells Git "I want to attach a message to this record."


Configuring Git('--global'):
    Before the work begin, you need to tell Git who you are! This information is attached to everything you do so people know who made the changes.
        -Identity: You set your user.name and user.email.
        -Default Branch: You can tell Git what to call your main workspace (usually main).
        -Storage: '~/.gitconfig' (The "inside cover" for all your projects).*1
        -Commands:
            Set: 'git config set [scope] <key> <value>'
            Get: 'git config get [scope] <key>'
            Example: 'git config get init.defaultBranch'
            if scope isn't specified git will use the default one ('--local').
        -Keys:
            -If the key is user.name, the value is "Your Name".
            -If the key is user.email, the value is "email@example.com".
            -If the key is init.defaultBranch, the value is "main"
    Think of your Git config like a notebook. Your user.name and user.email are already written on the inside cover (the global config).
    (*1):The ~ (tilde) is a shortcut that means "my home folder," which is where Git looks for your default identity.
______________________________
Configuring Git ('--Local'):
    For specific projects, you can add "sticky notes" that only apply to that folder.
        -Scope: Local (the default if you don't say otherwise).
        -Storage: These live in .git/config inside your project.
        -Priority: If you have a user.name set in both Global and Local, Git will always listen to the Local one first. The "sticky note" inside the chapter overrides the "inside cover" of the notebook.
        -Commands:
            Set: 'git config set <key> <value>'
            List: 'git config list --local'


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
    (*3):<tree-ish>: This is a fancy Git term for "something that points to a tree." Usually, this is the hash of a tree object, or simply HEAD, it lists all the files and sub-folders inside that tree. It shows you their permissions, whether they are a blob (a file) or another tree (a folder), and their unique hashes.
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


Half of Git(50/100%):
    Half of your workflow as a developer will just be 3 simple commands:
        -git status: To see what's happening in your room right now.
        -git add: To point the camera at what you want to save.
        -git commit: To snap the photo and save it forever.
    It's most of what you need to work effectively as a solo developer. Another 40% of Git is about collaborating and storing your work on a remote server (sharing your photo album with others), the commands are:
        -'git remote add <name> <url>' (Adding a "Post Office" to send your photos to).
        -'git push [remote] [branch]' (Sending your photos to the server).
        -'git pull [remote] [branch]' (Getting photos from your friends' albums).
    The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics and "Emergency Spells" for when things go wrong:
        -Reverting: How to undo a photo if you don't like it.
        -Resetting: Moving your camera back to a previous spot in the room.
        -Branching & Merging: How to have two different versions of the room at the same time and then bring them together.

Content Addressing(The Secret Library):
    Git doesn't find your files by their names (like notes.txt). Instead, it gives every single thing a unique ID Number called a Hash.
    It’s like a library where every book is filed by its exact fingerprint.
    If you change even a single letter in a book, its fingerprint changes, and Git gives it a new spot on the shelf. This way, nothing ever gets lost or mixed up!
    Object Types (The Library Shelves):
        -Blob (File): Stores the content of a single file. (The Leaf).
        -Tree (Folder): Stores a list of Blobs and other Trees. (The Branch).
        -Commit (Snapshot): Points to a specific Tree to show how the whole project looked at one time.


Inspection Tools:
    Sometimes you need to look "under the hood" to see what Git is thinking. These are your tools:
        -git status: Your Map. It shows you where you are and what you've changed since the last photo.
        -git log: Your Photo Album. It shows you a list of every photo you’ve ever taken, who took it, and when.
        -git cat-file -p <hash>: Your X-Ray Machine. If you have a secret ID number (hash), this tool lets you look inside and see the actual words of the file or the details of the photo.
        -git ls-tree <tree-ish>: Your Packing List. While cat-file shows you what is inside one item, ls-tree shows you a list of every file and folder inside a specific "Tree" (folder) snapshot.

*Context Summary: Git Apprentice Reference Guide
    this summary is from a README.md in "~/workspace/bootdotdev/curriculum/webflyx" that has the intent of grow it's documentacion alongside the development of webflix (project self made to teach how to use git with the help of boot.dev's curriculum's guidence)
    Core Goal: A living README.md that explains Git concepts chronologically as they appear in the Boot.dev curriculum, simplified in a way so that a 5-year-old could understand it while maintaining technical accuracy.

    Key Analogies & Frameworks:

    The Three States (The Room Photo):
        Working Directory: The "Live Room" (modifying files).
        Staging Area: The "Camera Viewfinder" (preparing the shot via git add).
        Commit History: The "Photo Album" (the permanent record via git commit).
    The 90/10 Rule:
        50%: Daily solo workflow (status, add, commit).
        40%: Collaboration/Remotes (The "Post Office" analogy for push/pull).
        10%: Emergency Spells (Fixing mistakes like reset/revert).
    The Secret Library (Content Addressing):
        Blobs: The "Leaves" (file content only).
        Trees: The "Branches" (directory structure/packing lists).
        Commits: The "Snapshots" (The dated entry in the library log).
        Hashes: Unique "Fingerprints" based on content (Deduplication via "Space" and "Time").
    Documentation Standards:
        Command Syntax: Mandatory arguments in <>, optional in [].
        Formatting: Commands outside of lists are wrapped in ''.
        Flags: Defined as "special instructions" (e.g., -m for messages).
        Plumbing vs. Porcelain: The distinction between user-friendly tools (Porcelain) and "under-the-hood" X-ray tools (Plumbing).
        
    Current Status: The foundations (Config, Repo, Syntax, Porcelain/Plumbing, Three States, Deduplication, and Inspection Tools) are drafted. The "40%" and "10%" sections are currently high-level placeholders to be expanded as the student progresses through the course.*