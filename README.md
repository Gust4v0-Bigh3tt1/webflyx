A self made project to learn Git with boot.dev's guidence

Version Control:
    -Git is a tool for tracking changes(snapshots/photos) to files over time, allowing you to "travel back in time" to previous versions of your work.


Command Syntax:
    -Arguments in angle brackets <> are mandatory and must be provided when running the command.
    -Arguments in square brackets [] are optional and can be included if needed.
        -For example, to create a new directory in your terminal, you would run:
            -'mkdir' <directory-name>
                -'mkdir' is the command
                -<directory-name> is a required argument
        Flags:
            - A flag is like a special instruction you give to a command, usually starting with a -. For example, '-m' tells Git "I want to attach a message to this record."


Configuring Git:
    -Before the work begin, you need to tell Git who you are! This information is attached to everything you do so people know who made the changes.(go to 'The Inside Cover (--global)' to see how to apply your Identity)
    ______________________________________________
    -The Rule of Overriding: If a setting exists in more than one "level", Git always listens to the most specific one. (Worktree (The Annex) overrides Local (Sticky Note), Local (Sticky Note) overrides Global (Inside Cover), Global (Inside Cover) overrides System (Kingdom's Law).)
    _____________________________________________________________________________________________
    The Kingdom’s Law (--system): Settings for everyone on this computer. (File: /etc/gitconfig).
        -These are the foundational rules set for every single user and project on this machine. You rarely need to edit this, as it is like changing the laws of the land itself.
            -Storage: /etc/gitconfig
            -Commands:
                -git config set --system <key> <value> (Requires "Administrative Magic" / 'sudo' to change).
        -Think of this as the stone tablet in the town square. Everyone can read it, but only the High Wizards can pick up the chisel to change it.
    ________________________________________________________________________________________________
    The Inside Cover (--global): Your personal identity for all your projects. (File: ~/.gitconfig).
        Your default identity. These settings follow you across every project you ever start.
            -Identity: You set your user.name and user.email.
            -Default Branch: You can tell Git what to call your main workspace (usually main).
            -Storage: '~/.gitconfig' (The "inside cover" for all your projects).*(1)*
            -Commands:
                Set: 'git config set [scope] <key> <value>'*(2)*
                Get: 'git config get [scope] <key>'
                    Example: 'git config get init.defaultBranch'
                Unset: 'git config unset [scope] <key>' (The "Eraser").
                if scope isn't specified git will use the default one ('--local')*(3)*.
            -Keys:
                -If the key is user.name, the value is "Your Name".
                -If the key is user.email, the value is "email@example.com".
                -If the key is init.defaultBranch, the value is "main"
        -Think of your Git config like a notebook. Your user.name and user.email are already written on the inside cover (the global config).
        *(1): [scope] tells Git which notebook to write in. Use --global for the "Inside Cover" (all projects) or --local for a "Sticky Note" (just this project).*
            -**The ~ (tilde) is a shortcut that means "my home folder," which is where Git looks for your default identity.**
        *(2):You can't just ask for <key>; you must ask for <section>.<key>**(1)**, git follows a strict format, It is like looking for a specific word in a dictionary. You don't just look for "Definition"; you look for "Bear.Definition" so Git knows exactly which section to check.**(2)***
            -**(1):a Section is like a Chapter Header in your notebook. If you haven't written any notes (keys) under that header yet, the header doesn't really "exist" in Git's eyes. As soon as you add your first "sticky note" (e.g., git config set webflyx.ceo "ThePrimeagen"), Git creates the "webflyx" chapter automatically to hold it. To find it use 'git config list'**
            -**(2):Existence Rule: A section only exists as long as it has at least one key. If you unset the last key, it will leave a empty [section] header behind in the .git/config file. (To remove it, go to 'The Sticky Note ('--Local')/Commands/Remove/Chapter Eraser').**
        *(3):The Safety Rule: You must be "inside" a Git project folder to use or see 'Local' settings. If you try to 'set' a local key while standing outside a project, Git will get confused and tell you: "fatal: not in a git directory".*
            -**(When Setting: If you don't specify a scope, Git tries to write to the "Sticky Note" (--local) in your current project. If you aren't inside a Git repository, the command will actually fail because there is no .git/config file to write to!)**
            -**(When Getting(The Search Rule): When reading a setting (get), Git is a detective. It searches from the most specific (Local) to the most general (System) until it finds an answer. The first answer it finds is the one it gives you)**
    ______________________________________________________________________________________________
    The Sticky Note (--local):Project-specific settings just for this folder. (File: .git/config).
        -Project-specific rules. These only exist inside the hidden .git cave of a specific repository.
        -For specific projects, you can add "sticky notes" that only apply to that folder.
            -Find: 'git config get <section>.<key>' (Used to pluck one specific "sticky note" out of the pile).
            -Peek: 'git config list' (Used to see every single setting Git is currently using).
            -Scope: Local (the default if you don't say otherwise).
            -Storage: These live in .git/config inside your project.
            -Priority: If you have a user.name set in both Global and Local, Git will always listen to the Local one first. The "sticky note" inside the chapter overrides the "inside cover" of the notebook.
        -The "Hoarder" Rule (Duplicates): while most notebooks only let you have one <section>.<key> <value> per page, Git lets you stick as many as you want if you use a special "plus-sign" instruction ('git config set --append <section>.<key> <value>').*(1)*
            -Commands:
                Set: 'git config set <key> <value>'
                    -Append: 'git config set --append <key> <value>' (The "Stapler": adds a duplicate instead of replacing the old one).
                List: 'git config list --local'*(3)*
                Remove:
                    -one: 'git config unset <key>'
                    -all: 'git config unset --all <key>'*(4)*
                    -Chapter Eraser:
                        -the whole section: 'git config remove-section <section>'
                        -If unset is an eraser for a single line (a key), remove-section is like ripping an entire page out of your notebook. Sometimes you create a "Chapter" (section) like [webflyx] that you realize you don't need anymore. Instead of erasing every single sticky note one by one, you can delete the whole header and everything inside it in one go.*(5)*
        *(1): The --append flag is like using a stapler. Instead of replacing the old sticky note, you are stapling a new one right on top of it. Now you have a pile of notes for the same key!*
        *(2):When you have duplicates (like multiple <value>'s to a <section>.<key>), git config list will show all of them in a row. It’s the best way to see if your config has become "cursed" with too many entries!*
        *(3): It is worth noting that git config list (without flags) is the "Plumbing" way to see everything Git currently knows about your setup from all levels (system, global, and local).*
        *(4): unset: Removes just one instance of the key. unset --all: The "Deep Clean." It purges every single duplicate of that key from the config at once.**(1)***
            -**(1):If you have duplicates, a regular unset will fail because Git is too scared to pick just one. You must use --all to clear the pile, or specify exactly which one to remove.**
        *(5):Rule of Thumb: Use this when a section is "nonsensical"—meaning Git doesn't use it for its own magic, and you don't want it cluttering your workspace.*
    _____________________________________________________________________________________________________________
    The Annex (--worktree): A separate scroll for shared drafts of the same project. (File: .git/config.worktree)
        -A Worktree allows you to have multiple branches of the same project checked out in different folders at the same time. This scroll holds settings that apply only to that specific branch's workspace. (Extremely Rare).
            -Storage: .git/config.worktree
            -Usage: Only exists if you have enabled the 'extensions.worktreeConfig' spell.
        -Think of this as a shared annex to your "Secret Cave" (Local). It’s for when a wizard needs to be in two places at once, working on two different versions of the same spell.

The Repository:
    -A repo is essentially just a directory that contains a project (other directories and files). The only difference is that it also contains a hidden '.git' directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project. The '.git' directory is the heart of your project, containing the entire history and configuration of your repository.
        -In order to make a '.git' repo you should:
            -make you project's directory
            -inside it execute the 'git init [directory]' command
        -Once done you should now have a hidden '.git' directory in your project's directory. This means you've successfully created a new Git repository! List (ls -a) the contents of the directory to confirm.

Porcelain and Plumbing(90/10 % rule):
    -In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. The porcelain commands are the ones that you will use most often as a developer to interact with your code.
        -Some porcelain commands are:
            -git config set [scope] <key> <value> *(1)*(e.g. 'git config set user.name "Your Name"')
            -git config get [scope] <key>
            -git config unset [scope] <key>
            -git config remove-section [scope] <section> (The "Chapter Eraser")
            -git config list (Your "Peek" tool)
            -git status
            -git add <file-path> *(2)*
            -git commit -m <message>
            -git log
            -git push
            -git pull
            -git clone <repository-url>
        -Some examples of plumbing commands are:
            -git cat-file <type> <hash>
                -If a flag is used '<type>' isn't needed. Common flags: -p (print content), -t (show type).
            -git hash-object <file-path>
            -git ls-tree <tree-ish> *(3)*
            -git rev-parse <name> *(4)*
            - Manually editing '.git/config' or '~/.gitconfig' with a text editor.
                - (This is the "Plumbing" way to change settings without using the 'git config' tool).
    *(1): [scope] can be --global (the inside cover) or --local (the sticky note). If you don't pick one, Git usually defaults to --local for writing.*
    *(2):(git add ., where the . acts as the <file-path> for "everything in the current directory.")*
    *(3):<tree-ish>: This is a fancy Git term for "something that points to a tree." Usually, this is the hash of a tree object, or simply HEAD. It lists everything inside that snapshot and shows their "Mode"—a special code that tells Git if a file is a regular file, a folder, or a special 'executable' file (like a script that can run like a toy car on its own).*
    *(4):<name>:It tells you the full 40-character SHA-1 hash that the name points to. If you ask Git git rev-parse HEAD, it will tell you the exact hash of the commit you are currently standing on.*

The Three States:
    Git tracks your work through three different stages. A good analogy to think of it is:
    ____________________________________
    1-Working Directory (The Live Room):
        -You are in this step when you're changing/modifying a directory. think of it as a room, you can move furniture, paint walls, or add new items.
    ____________________________________________
    2-Staging Area/Index(The Camera Viewfinder):
        -after you made the changes in step 1, you're now going to "prepare to take a photo of the room" and mark the changes made.
        Command: 'git add <file-path>'.
    _________________________________
    3-Commit History(The Photo Album):
        -Where Git takes the photo and permanently stores snapshots (photos) of your project
        Command: 'git commit -m <message>' (the <message> must be in "").
    ___________________
    The Snapshot Model:
        -Unlike some systems that store only "changes" (deltas), Git stores an entire snapshot (photo) of your files for every commit.
        -When Git hashes a file(blob), it only cares about two things: The Size and the Content.
        -Each "photo" is saved with a hash, the "photos" "ID".
            Commit Hash(ID): Depends on the content + context (Who, when, and what message):
                which is made by taking:
                -The Tree Hash: reference to the "snapshot" of all files/folders at that moment (a photo of the roots in which the file belongs).
                -The Parent Hash: The ID of the commit that came before it (this creates the "chain" of history).
                -The Author & Committer: Your user.name and user.email.
                -The Timestamp: The exact second the commit was made. *(1)*
                -The Message: Whatever you wrote after the -m flag.
            Blob Hash: Only depends on the content. (Same words = Same hash) *(2)*:
                The Size: It adds how many characters are in the file.
                The Content: It adds every single letter and space inside the file.
    *(1):even if you make two identical commits with the same files and message, they will have different hashes because they happened at different times*
    *(2):Git does not include the filename in a blob's hash! That's why two files with different names but the same content will have the identical hash (Deduplication)*

    Deduplication:
        -Because Git uses hashes, it is efficient. If a file remains unchanged between commits, Git simply points to the existing hash rather than storing a duplicate copy.
        -types of deduplication:
            -Duplicate Content (Space):
                -if apple.txt and orange.txt both contain the word "Fruit", Git only saves one copy of "Fruit" and gives both names the same hash.
            -Unchanged Files (Time):
                -If you have a project with 100 files, but you only change one file and make a new commit, Git is smart! It doesn't save 100 new files. It only saves the one you changed and points the new commit to the 99 hashes it already has from the previous commit.


Half of Git(50/100%):
    -Half of your workflow as a developer will just be 3 simple commands:
        -git status: To see what's happening in your room right now.
        -git add: To point the camera at what you want to save.
        -git commit: To snap the photo and save it forever.
    -It's most of what you need to work effectively as a solo developer. Another 40% of Git is about collaborating and storing your work on a remote server (sharing your photo album with others), the commands are:
        -'git remote add <name> <url>' (Adding a "Post Office" to send your photos to).
        -'git push [remote] [branch]' (Sending your photos to the server).
        -'git pull [remote] [branch]' (Getting photos from your friends' albums).
    -The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics and "Emergency Spells" for when things go wrong:
        -Reverting: How to undo a photo if you don't like it.
        -Resetting: Moving your camera back to a previous spot in the room.
        -Branching & Merging: How to have two different versions of the room at the same time and then bring them together.

Content Addressing(The Secret Library):
    -Git doesn't find your files by their names (like notes.txt). Instead, it gives every single thing a unique ID Number called a Hash.
    -It’s like a library where every book is filed by its exact fingerprint.
    _________________
    -The Eraser Rule: Git's eraser is precision-tipped! While you can erase a whole 'Chapter' (Section) using the 'remove-section' tool, using 'unset' only erases a single line. If you unset every line in a chapter, the empty Header might still hang around until you use the 'Chapter Eraser' to scrub it away.
    _________________
    -If you change even a single letter in a book, its fingerprint changes, and Git gives this new version a new spot on the shelf. This way, nothing ever gets lost or mixed up!
    -Object Types (The Library Shelves):
        -Blob (File): Stores the content of a single file. (The Leaf).
        -Tree (Folder): Stores a list of Blobs and other Trees. (The Branch).
        -Commit (Snapshot): Points to a specific Tree to show how the whole project looked at one time.


Inspection Tools:
    Sometimes you need to look "under the hood" to see what Git is thinking. These are your tools:
        -'git status': Your Map. It shows you where you are and what you've changed since the last photo.
        -'git log': Your Photo Album. It shows you a list of every photo you’ve ever taken, who took it, and when.
            -If you want to avoid the pager entirely and just dump the whole history into your terminal so you can scroll with your mouse like a normal web page, you can use this command: ('git --no-pager log')
            -Or,the "Thumbnail View" is often much easier to read: ('git log --oneline')
        -'git cat-file -p <hash>': Your X-Ray Machine. If you have a secret ID number (hash), this tool lets you look inside and see the actual words of the file or the details of the photo.
        -'git ls-tree <tree-ish>': Your Packing List. While cat-file shows you what is inside one item, ls-tree shows you a list of every file and folder inside a specific "Tree" (folder) snapshot.
        -'git config list --show-origin': It doesn't just show the settings; it tells you exactly which file (Kingdom, Inside Cover, or Sticky Note) each rule came from.

*Context Summary: Git Apprentice Reference Guide
    -this summary is from a README.md in "~/workspace/bootdotdev/curriculum/webflyx" that has the intent of grow it's documentacion alongside the development of webflix (project self made to teach how to use git with the help of boot.dev's curriculum's guidence)
    -Core Goal: A living README.md that explains Git concepts chronologically as they appear in the Boot.dev curriculum. It uses a "Wizard’s Notebook" theme to simplify complex version control mechanics, simplified in a way so that a 5-year-old could understand it while maintaining technical accuracy.

    Key Analogies & Frameworks:

        The Config Notebook (The Hierarchy of Power):
            The Annex (--worktree): A shared scroll for wizards working in two places at once.
            -The Sticky Note (--local): Project-specific settings that override the more general books.
            -The Inside Cover (--global): Your personal identity that follows you through every quest.
            -The Kingdom’s Law (--system): The stone tablet in the town square—rules for every wizard in the land.
            -The Detective (The Search Rule): Git always searches from the most specific (Annex/Local) to the most general (System) and stops as soon as it finds an answer.
            -The Chapter Eraser (remove-section): Ripping out a whole section of settings when they become "nonsensical."
        The Three States (The Room Photo):
            -Working Directory: The "Live Room" where you move furniture (modify files).
            -Staging Area/Index: The "Camera Viewfinder" (preparing the shot via git add).
            -Commit History: The "Photo Album" (the permanent record via git commit).
        The 90/10 Rule (Workflow Hierarchy):
            -50% Solo Mastery: The daily loop of status, add, and commit.
            -40% Remote Collaboration: The "Post Office" (sharing albums via push/pull).
            -10% Emergency Spells: Precision tools for fixing "cursed" repositories (reset/revert).
        The Secret Library (Content Addressing):
            -Blobs (The Leaves): Fingerprinted content only (Deduplication via "Space").
            -Trees (The Branches): Packing lists showing the "Mode" (The Toy Car analogy).
            -Commits (The Snapshots): The dated, signed entry in the library log (Deduplication via "Time").
        Porcelain vs. Plumbing:
            -Porcelain: User-friendly tools for daily work (e.g., git config set).
            -Plumbing: Under-the-hood X-ray tools (e.g., cat-file, rev-parse) and manual file editing.
    Documentation Standards:
        -Command Syntax: Mandatory <>, optional [].
        -Scope Rule: Defined at first mention; defaults to --local for writing but searches all levels for reading.
        -The Eraser Rule: Distinguishes between erasing a single line (unset) and scrubbing a whole chapter (remove-section).
    Current Status: Foundations are fully complete, covering configuration scopes, repository initialization, object hashing/deduplication, and the inspection toolkit. The "Collaboration" and "Emergency Spell" chapters remain high-level placeholders for future curriculum milestones.

    so, based on my entry and this lesson what about this lesson I could add in my README.md entry to make it more complete? I could give the full entry if you wish.*
