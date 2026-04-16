A self made project to learn Git with boot.dev's guidence

Version Control:
    -Git is a tool for tracking changes(snapshots/photos) to files over time, allowing you to "travel back in time" to previous versions of your work.


Command Syntax:
    -Arguments in between '' are commands or pices of one in between normal text.
    -Arguments in angle brackets <> are mandatory and must be provided when running the command.
    -Arguments in square brackets [] are optional and can be included if needed.
        -For example, to create a new directory in your terminal, you would run:
            -'`mkdir `<directory-name>'
                -"mkdir" is the command
                -<directory-name> is a required argument
        Flags:
            - A flag is like a special instruction you give to a command, usually starting with a -. For example, '-m' tells Git "I want to attach a message to this record."


Configuring Git:
    -Before the work begin, you need to tell Git who you are! This information is attached to everything you do so people know who made the changes.(go to "The Inside Cover (--global)/The “Notebook” Commands" to see how to apply your Identity)
    ______________________________________________
    -The Rule of Overriding: If a setting exists in more than one "level", Git always listens to the most specific one. (Worktree (The Annex) overrides Local (Sticky Note), Local (Sticky Note) overrides Global (Inside Cover), Global (Inside Cover) overrides System (Kingdom's Law).)
    _____________________________________________________________________________________________
    The Kingdom’s Law (--system): Settings for everyone on this computer. (File: /etc/gitconfig).
        -These are the foundational rules set for every single user and project on this machine. You rarely need to edit this, as it is like changing the laws of the land itself.
            -Storage: /etc/gitconfig
            -Commands:
                -'`git config set --system `<key> <value>' (Requires "Administrative Magic" / '`sudo`' to change).
        -Think of this as the stone tablet in the town square. Everyone can read it, but only the High Wizards can pick up the chisel to change it.
    ________________________________________________________________________________________________
    The Inside Cover (--global): Your personal identity for all your projects. (File: ~/.gitconfig).
        Your default identity. These settings follow you across every project you ever start.
            -Identity: You set your user.name and user.email.
            -Default Branch: You can tell Git what to call your main workspace (usually main).
            -Storage: '~/.gitconfig' (The "inside cover" for all your projects).*(1)*
            -init.defaultBranch: The rule that decides the name of the very first bookmark for every new notebook you ever start.
        -Think of your Git config like a notebook. Your user.name and user.email are written on the inside cover (the global config).
            .........................................................................................
            -The “Notebook” Commands: (Config Porcelain)
                -These are the specialized tools for writing, reading, and erasing the rules in your Config Notebooks.
                    -'`git config set `[scope] <key> <value>' : Writing a new rule in a specific notebook.(e.g. '`git config set user.name "Your Name"`')*(2)* *(3)*
                    -'`git config get `[scope] <key>': Reading a specific rule.
                        Example: '`git config get init.defaultBranch`'
                    -'`git config unset [scope] `<key>': The "Eraser" for a single line.
                    -'`git config remove-section `[scope] <section>': The "Chapter Eraser" to rip out an entire page of rules.
                    -'`git config list`': Your "Peek" tool to see every rule Git is currently following.*(4)*
                if scope isn't specified git will use the default one ('--local')*(5)*.
            -Keys:
                -If the key is user.name, the value is "Your Name".
                -If the key is user.email, the value is "email@example.com".
                -If the key is init.defaultBranch, the value is "main"
    ___________________________________________________________________________________________________________
    *(1): [scope] tells Git which notebook to write in.*
        -**'--global' (The Inside Cover): For all projects in your kingdom.**
        -**'--local' (The Sticky Note): Just for this specific project.**
        -**The ~ (tilde) is a shortcut that means "my home folder," which is where Git looks for your default identity.**
    *(2):You can't just ask for <key>; you must ask for <section>.<key>**(1)**, git follows a strict format, It is like looking for a specific word in a dictionary. You don't just look for "Definition"; you look for "Bear.Definition" so Git knows exactly which section to check.**(2)***
        -**(1):a Section is like a Chapter Header in your notebook. If you haven't written any notes (keys) under that header yet, the header doesn't really "exist" in Git's eyes. As soon as you add your first "sticky note" (e.g., git config set webflyx.ceo "ThePrimeagen"), Git creates the "webflyx" chapter automatically to hold it. To find it use 'git config list'**
        -**(2):Existence Rule: A section only exists as long as it has at least one key. If you unset the last key, it will leave a empty [section] header behind in the .git/config file. (To remove it, go to "The Sticky Note ('--Local')/Commands/Remove/Chapter Eraser").**
    *(3):[scope] can be --global (the inside cover) or --local (the sticky note).*
        -**The Default Rule: If you don't pick one, Git usually defaults to --local.**
        -**The Project Requirement: Because the default is --local, Git must be able to find the hidden .git cave to write the note. If you aren't inside a project, it will throw a "fatal" error because it has no "Sticky Note" to write on!**
    *(4):'git config list' works by itself just fine it will give you a list of all your git config that has been set.***(1)** **(2)**
        -**(1)The Filter: If you only want to see one rule, use '`git config get `<key>'.if it exist.**
        -**(2)The Full Scroll: You can also use 'cat ~/.gitconfig' to see all.(global only, for local use 'cat .git/config')**
    *(5):The Safety Rule: You must be "inside" a Git project folder to use or see 'Local' settings. If you try to 'set' a local key while standing outside a project, Git will get confused and tell you: "fatal: not in a git directory".*
        -**(When Setting: If you don't specify a scope, Git tries to write to the "Sticky Note" (--local) in your current project. If you aren't inside a Git repository, the command will actually fail because there is no .git/config file to write to!)**
        -**(When Getting(The Search Rule): When reading a setting (get), Git is a detective. It searches from the most specific (Local) to the most general (System) until it finds an answer. The first answer it finds is the one it gives you)**
    ______________________________________________________________________________________________
    The Sticky Note (--local):Project-specific settings just for this folder. (File: .git/config).
        -Project-specific rules. These only exist inside the hidden .git cave of a specific repository.
        -For specific projects, you can add "sticky notes" that only apply to that folder.
            -Find: '`git config get `<section>.<key>' (Used to pluck one specific "sticky note" out of the pile).
            -Peek: '`git config list`' (Used to see every single setting Git is currently using).
            -Scope: Local (the default if you don't say otherwise).
            -Storage: These live in .git/config inside your project.
            -Priority: If you have a user.name set in both Global and Local, Git will always listen to the Local one first. The "sticky note" inside the chapter overrides the "inside cover" of the notebook.
        -The "Hoarder" Rule (Duplicates): while most notebooks only let you have one <section>.<key> <value> per page, Git lets you stick as many as you want if you use a special "plus-sign" instruction ('`git config set --append `<section>.<key> <value>').*(1)*
            -Commands:
                Set: '`git config set `<key> <value>'
                    -Append: '`git config set --append `<key> <value>' (The "Stapler": adds a duplicate instead of replacing the old one).
                List: '`git config list --local`'*(3)*
                Remove:
                    -one: '`git config unset `<key>'
                    -all: '`git config unset --all `<key>'*(4)*
                    -Chapter Eraser:
                        -the whole section: '`git config remove-section `<section>'
                        -If unset is an eraser for a single line (a key), remove-section is like ripping an entire page out of your notebook. Sometimes you create a "Chapter" (section) like [webflyx] that you realize you don't need anymore. Instead of erasing every single sticky note one by one, you can delete the whole header and everything inside it in one go.*(5)*
    ___________________________________________________________________________________________________________
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
    -Think of a Repository as a Wizard's Tower. The rooms and scrolls inside are your project files (the Working Directory). But hidden in the basement is a Secret Cave (.git) — a magical vault that remembers every version of every scroll that has ever existed in the tower, who changed them, and when.
    -essentially is just a directory that contains a project (other directories and files). The only difference is that it also contains a hidden ".git" directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project. The ".git" directory is the heart of your project, containing the entire history and configuration of your repository.
        -In order to make a ".git" repo you should:
            -make you project's directory
            -inside it execute the '`git init `[directory]' command
        -Once done you should now have a hidden ".git" directory in your project's directory. This means you've successfully created a new Git repository! List (ls -a) the contents of the directory to confirm.


The Three States:
    Git tracks your work through three different stages. A good analogy to think of it is:
    ____________________________________
    1-Working Directory (The Live Room):
        -You are in this step when you're changing/modifying a directory. think of it as a room, you can move furniture, paint walls, or add new items.
    ____________________________________________
    2-Staging Area/Index(The Camera Viewfinder):
        -after you made the changes in step 1, you're now going to "prepare to take a photo of the room" and mark the changes made.
        -Technical Detail: The "Viewfinder" is physically stored in a file called .git/index. It’s the "Camera Sensor" holding all the data perfectly still until you’re ready to snap the photo (commit).*(1)*
        Command: '`git add `<file-path>'.
    _________________________________
    3-Commit History(The Photo Album):
        -Where Git takes the photo and permanently stores snapshots (photos) of your project
        Command: '`git commit -m `<message>' (the <message> must be in "").
        -The “Family Tree” (Branch Visualization):
            -history isn't always a single straight line.
                -The Trunk (Main): The primary story of your quest.
                -The Side-Quests (Branches): When a wizard wants to try a new spell without ruining the main story, they create a "Side-Quest."
                -The Fork in the Road: Use the text diagrams from the lesson to show how primes_branch or lanes_branch split off from a specific "photo" (commit) in the album.
            -The Map of Diverging Paths (Branches)
                Sometimes a Wizard must work on two spells at once. We visualize this using a "Map":*(2)*
                       G - H    (The Sky-Castle Branch)
                      /
                 A - B - C - D   (The Main Road)
                  \
                   E - F        (The Deep-Sea Branch)
            -The Parallel Reality (Divergence):
                Sometimes, the Master Scroll (Main) moves forward while you are still away on a Side-Quest. This creates a "Fork" where neither branch is ahead of the other; they have simply lived different lives.
                 A - B - C - E  (Main: Added Contents.md)
                          \
                           D     (Side-Quest: Found the Classics)
                Commit E and Commit D are "cousins." They share a grandfather (C), but they don't know about each other's treasures(details) yet.
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
                -The Timestamp: The exact second the commit was made. *(3)*
                -The Message: Whatever you wrote after the -m flag.
            Blob Hash: Only depends on the content. (Same words = Same hash) *(4)*:
                The Size: It adds how many characters are in the file.
                The Content: It adds every single letter and space inside the file.
    ___________________________________________________________________________________________________________
    *(1)The Sensor's Memory: The Index doesn't store the "books" themselves (the Blobs do that), but it stores the exact list of which fingerprints (hashes) are currently on the "Preparation Table" waiting to be photographed for the next Commit.*
    *(2)The Rule of Memory: The Deep-Sea Branch consists of commits A, E, and F. It remembers where it came from (A), even if the Main Road travels further to D.(1)*
        **(1)checkpoint hash: git also stores the last commit of each branch in a file inside the secret folder '.git/refs/heads' (These files are the Physical Bookmarks. If you peek inside .git/refs/heads/main, you will find the 40-character Fingerprint of the very last photo taken on that road).**
    *(3):even if you make two identical commits with the same files and message, they will have different hashes because they happened at different times*
    *(4):Git does not include the filename in a blob's hash! That's why two files with different names but the same content will have the identical hash (Deduplication)*

    Deduplication:
        -Because Git uses hashes, it is efficient. If a file remains unchanged between commits, Git simply points to the existing hash rather than storing a duplicate copy.
        -types of deduplication:
            -Duplicate Content (Space):
                -if apple.txt and orange.txt both contain the word "Fruit", Git only saves one copy of "Fruit" and gives both names the same hash.
            -Unchanged Files (Time):
                -If you have a project with 100 files, but you only change one file and make a new commit, Git is smart! It doesn't save 100 new files. It only saves the one you changed and points the new commit to the 99 hashes it already has from the previous commit.


The Workflow Hierarchy: Porcelain and Plumbing (50/40/10 Rule)
    -In Git, commands are divided into high-level ("porcelain") commands and low-level ("plumbing") commands. The porcelain commands are the ones that you will use most often as a developer to interact with your code.
    __________________
    -50% Solo Mastery: Half of git 
        -These are your most common Porcelain commands. Use these to move through your day-to-day workflow
            -'`git status`': To see what's happening in your room right now.
            -'``git add ``<file-path>': To point the camera at what you want to save.*(1)*
            -'git commit -m <message>': To snap the photo and save it forever.
            -'`git log`': Flipping through the photo album to see your past work.
                -'`-10`': adding this flag make git show you only the n amount of past commits equivilent to the number input.(e.g. -10=10 last commits, -100=100 last commits.) 
                -'`--oneline`': Shrinks each page to a single line.
                -'`--decorate=full`': Reveals the "Ref’s" full path.*(2)*
                -'`--decorate=no`': The branch names are no longer shown at all.
                -'`--graph`': Adding this flag draws the physical vines and paths connecting the photos, showing exactly where the paths split at the Crossroads.
                -'`--all`': Adding this flag allows you to see every side-quest and parallel world at once, even those you aren't currently standing in.
                -'`--parents`': Adding this flag to git log reveals the two-parent nature of the Bridge Commit directly in the output, showing the raw hashes of both parents side by side directly in the log output.(You can verify the two-parent nature of any merge commit by running '`git log --parents`'. The Bridge Commit will be the only one in your history with two parent hashes listed beside it.), A normal commit shows one parent hash.
                -The Lens of All-Sight (`--graph --all --oneline --decorate=full`): use these flags to read commit history like a world map.
                    Normally, git log only shows the path you are currently standing on.
                -The Map Renderer (`--graph --parents`): use these flags to read commit history like a map.
            -'`git branch `[name]'(vine branch): To name a new "What If?" portal (bookmark) and place it exactly where you are standing.*(3)*
                -'`git branch -d `<name>': The Safe Eraser. Deletes the branch named in the command, but      refuses if its photos haven't been merged yet.
                -'`git branch -D `<name>': The Force Eraser. Deletes the branch regardless of merge status. Use with caution.
                -'`git branch -m `<old> <new>': This allows you to rename a bookmark(branch) without moving it to a different photo.
            -'git branch': Reveals all the bookmarks currently tucked into your album. The one with the star (or the different color) is the world you are currently standing in.*(4)*
            -'git switch <name>': Moves your Focus (HEAD) to a different branch (allows you to switch branches)
                -'`git switch -c `<name>': Creates a new branch at your current location and switches to it immediately.
                -'`git switch -c `<name> <COMMITHASH>': Creates a new branch starting specifically at that COMMITHASH (instead of where you are currently standing) and switches to it.
        It's most of what you need to work effectively as a solo developer.
    __________________________
    -40% Remote Collaboration: The "Post Office"
        -Another 40% of Git is about collaborating and storing your work on a remote server (sharing your photo album with others), the commands are:
            -'`git remote add `<name> <url>' (Adding a "Post Office" to send your photos to).
            -'`git push `[remote] [branch]' (Sending your photos to the server).
            -'`git pull `[remote] [branch]' (Getting photos from your friends' albums).
            -'`git clone `<repository-url>': Copying an entire library from another kingdom to your local desk.
    ______________________
    -10% Emergency Spells: Precision tools for fixing "cursed" repositories
        -The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics and "Emergency Spells" for when things go wrong:
            -Reverting: How to undo a photo if you don't like it.
            -Resetting: Moving your bookmark back in history, with optional control over whether the Camera Viewfinder and Live Room are also rewound.
                Think of git reset as a time-adjustment spell with different strengths.
                All reset modes move the branch bookmark.
                The difference is whether Git also resets the staged snapshot and the files in your room.
                -The Gentle Rewind ('`git reset --soft `<COMMITHASH>')
                    Moves your branch's Sticky Note back to an earlier photo, but leaves the Camera Viewfinder and the Live Room untouched.
                    -Commit History: moved back
                    -Staging Area: unchanged
                    -Working Directory: unchanged
                    -The Result: the undone commit's changes are still staged and ready to be committed again.
                    -Use Case: when you committed too early and want to re-shoot that work as part of a better commit.
                -The Full Rewind ('`git reset --hard`<COMMITHASH>')
                    Moves your branch's Sticky Note back to an earlier photo and forces both the Camera Viewfinder and the Live Room to match that older photo exactly.
                        -Commit History: moved back
                        -Staging Area: reset
                        -Working Directory: reset
                        -The Result: local staged and unstaged changes are discarded so everything matches the target commit.
                        -Use Case: when you want to completely abandon the current local changes and return to an earlier snapshot.
                        -Warning: this spell is destructive for uncommitted work.
                -The Reset Rule
                    The true power of git reset is not only where the Sticky Note moves, but also whether Git forces the Camera Viewfinder and the Live Room to rewind with it.
                        | Mode | Branch Pointer | Staging Area | Working Directory |
                        |---|---|---|---|
                        | `--soft` | moved | unchanged | unchanged |
                        | `--hard` | moved | reset | reset |
            -Rebasing: Moving the foundation of a branch.
                Instead of stitching two histories together with a bridge (Merge), Rebase picks up your entire branch and moves it so it starts from the very latest photo on the Main Road.
                -Command: '`git rebase main`' (run while standing on your feature branch).
                -The Result: A perfectly straight line of history. It’s like rewriting the past so your changes always happen on top of the newest work.
                -⚠️ Warning: Never rebase the Main Road or any branch other wizards are already walking on. It rewrites the past and causes time-travel curses (conflicts) for your allies!
            -The Handshake (rule of thumb): Rebase your own branch to polish your work, then ask the Manager to Merge it into the Main Road
            -Merging: Stitching two different realities back together into one.
                -command: '`git switch main`''`git merge` <name>'(<name>=name of the branch).
                When you cast a Merge Spell, Git follows three steps:
                    1. Find the Last Shared Moment (Merge Base): Git hunts backwards through both timelines to find the last commit they both walked through together. This is your "Crossroads" commit.
                    2. Replay the Changes: Git replays what each branch did since the Crossroads, then weaves those changes together.
                    3. Snap the Bridge Commit: The result is a special photo with two parents instead of one — one parent from each branch. This is the Bridge Commit (F below).
                 A - B - C - F    (The Main Road)
                    \     /
                     D - E        (Deap-Sea Branch)
                    -F remembers both C and E. It is the moment two parallel worlds became one again.
                    -however, If the Main Road has no new photos since the Crossroads, Git skips the Bridge Commit entirely. It simply slides the main sticky note forward to the Tip of the other branch. No new photo is taken. Main must be a direct ancestor of the branch being merged.
                           C - D    (The Sky-Castle Branch)                             
                          /                                 ------>                   (The Sky-Castle Branch)
                     A - B       (The Main Road)                      A - B - C - D   (The Main Road)
                (This is often where "Conflicts" or "Curses" happen — when both branches changed the same line of the same file since the Crossroads, Git cannot decide which version wins and asks you to resolve it by hand.)
            -'`git rev-parse `<name>': Finding the true 40-character "Fingerprint" (Hash) of a bookmark. *(5)*
            -'`git cat-file `<type> <hash>': Peeking inside a specific object in the library.*(6)*
            -'`git hash-object `<file-path>': Computes and returns the 40-character hash of any file's content without storing it. It is your "Fingerprint Calculator" — useful for checking what hash Git would assign to a file before committing it.
            -'`git ls-tree `<tree-ish>': Listing everything inside a snapshot to see their "Mode." *(7)*
            - Manually editing '.git/config' or '~/.gitconfig' with a text editor. (Changing the kingdom's rules by hand instead of using the git config tool.)
                - (This is the "Plumbing" way to change settings without using the 'git config' tool).
    ___________________________________________________________________________________________________________
    *(1):(git add ., where the . acts as the <file-path> for "everything in the current directory.")*
    *(2)The True Name Lens (--decorate=full): Reveals the "Ref’s" full path. This shows the exact "Shelf" in the Secret Library (refs/heads/) where the sticky note is kept.*
    *(3)Branching: In a normal book, you read from page 1 to page 100 in a straight line. But a Wizard's Notebook is magical. Think of it like puting an extra bookmark in the project(book) instead of a copy of the whole library; it is just a sticky note (a pointer) that says "I am currently looking at this specific photo in the album.". The Master Scroll (master/main): This is the "True History" of the kingdom. It is the story everyone agrees is real.***(1)** **(2)**
        -**(1)The Movement: When you snap a new photo (commit), you don't need a new bookmark; you simply peel the sticky note off the old photo and slap it onto the new one. It always stays at The Tip of the Wand.***(1)*
            -*(1)The Tip of the Wand: The most recent photo in a branch is called the Tip. As you add new photos, the bookmark automatically slides forward to stay at the very end of that specific story.*
        -**(2)The Starting Point: Every notebook starts with a first bookmark already placed. This is why, when you run git branch for the first time in your project directory, you aren't standing in a "nameless void." You are already standing on the main branch.**
    *(4)The Wizard's Focus (HEAD):*
        *While you can have many bookmarks (Branches) in your album, you only have one set of eyes.*
            *-HEAD is the Wizard's Focus. It usually points to a Sticky Note (Branch).*
            *-When you move your focus to a different branch, Git quickly rearranges the furniture in the Live Room (Working Directory) to match the photo that bookmark is pointing to.*
            *-Plumbing Fact: If you look inside the secret file .git/HEAD, you won't see a hash; you'll see something like ref: refs/heads/main. It’s literally a pointer to a pointer!*
            *-The Shifting Reality Rule:*
                *-When you move your Focus (HEAD) to a different bookmark(branch), Git physically replaces the items in your Live Room (Working Directory). If you created a magical item on a Side-Quest and then teleport back to the Main Road, the item will vanish from your hand. It isn't gone; it is simply waiting for you back in the other reality.*
    *(5):<name>:It tells you the full 40-character SHA-1 hash that the name points to. If you ask Git git rev-parse HEAD, it will tell you the exact hash of the commit you are currently standing on.*
    *(6):If a flag is used '<type>' isn't needed. Common flags: -p (print content), -t (show type).*
    *(7):<tree-ish>: This is a fancy Git term for "something that points to a tree." Usually, this is the hash of a tree object, or simply HEAD. It lists everything inside that snapshot and shows their "Mode"—a special code that tells Git if a file is a regular file, a folder, or a special 'executable' file (like a script that can run like a toy car on its own).*

            
Content Addressing(The Secret Library):
    -Git doesn't find your files by their names (like notes.txt). Instead, it gives every single thing a unique ID Number called a Hash.
    -It’s like a library where every book is filed by its exact fingerprint.
    _________________
    -The Eraser Rule: Git's eraser is precision-tipped! While you can erase a whole "Chapter" (Section) using the 'remove-section' tool, using 'unset' only erases a single line. If you unset every line in a chapter, the empty Header might still hang around until you use the "Chapter Eraser" to scrub it away.
    _________________
    Lineage and Inheritance (branches)
        -The Ancestor Rule: A branch isn't just the new photos; it includes every photo that led up to it. A is the grandparent, E is the parent, and F is the current moment. Even if main moves on to D, primes_branch still remembers its roots at A.
    _________________
    -If you change even a single letter in a book, its fingerprint changes, and Git gives this new version a new spot on the shelf. This way, nothing ever gets lost or mixed up!
    -Object Types (The Library Shelves):
        -Blob (File): Stores the content of a single file. (The Leaf).
        -Tree (Folder): Stores a list of Blobs and other Trees. (The Branch).
        -Commit (Snapshot): Points to a specific Tree to show how the whole project looked at one time.
        -Branch (The Sticky Note): A branch is not a folder or a copy! It is just a lightweight "Sticky Note" (Pointer) stuck to the side of a Commit.
    --The Lightweight Rule (The Ghostly Bookmarks):
        -Because a branch is just a tiny "Sticky Note" pointing to a Commit ID, it takes up almost zero space in your bag. 
        -You could have 1,000 branches (1,000 different *"What If?"(1)* realities) and your library wouldn't get any heavier! 
        -You aren't duplicating the books; you are just adding more bookmarks to the same shelves.
    ___________________________________________________________________________________________________________
    *(1)The "What If?" Portals: Creating a branch is like opening a portal to a parallel world. You can change the color of the castle to pink in the color-scheme portal without affecting the "True History" scroll. When you create 1000 branches, you aren't building 1000 new castles. You are just making 100 small bookmarks. They all point to the same stones and wood until you actually decide to change something. This is why branches are "cheap" and "lightweight."***(1)**
        -**(1)The weight of a Bookmark: A branch (sticky note) weighs exactly 41 bytes (40 characters for the Hash ID + 1 newline character). That is why the library doesn't get heavier!**


Inspection Tools:
    Sometimes you need to look "under the hood" to see what Git is thinking. These are your tools:
        -'git status': Your Map. It shows you where you are and what you've changed since the last photo.
        -'git log': Your Photo Album. It shows you a list of every photo you’ve ever taken, who took it, and when.
            -If you want to avoid the pager entirely and just dump the whole history into your terminal so you can scroll with your mouse like a normal web page, you can use this command: ('git --no-pager log')
            -Or,the "Thumbnail View" is often much easier to read: ('git log --oneline')
        -'`git cat-file -p `<hash>': Your X-Ray Machine. If you have a secret ID number (hash), this tool lets you look inside and see the actual words of the file or the details of the photo.
        -'`git ls-tree `<tree-ish>': Your Packing List. While cat-file shows you what is inside one item, ls-tree shows you a list of every file and folder inside a specific "Tree" (folder) snapshot.
        -'git config list --show-origin': It doesn't just show the settings; it tells you exactly which file (Kingdom, Inside Cover, or Sticky Note) each rule came from.



Context Summary: Git Apprentice Reference Guide
    -this summary is from a README.md in "~/workspace/bootdotdev/curriculum/webflyx" that has the intent of grow it's documentacion alongside the development of webflix (project self made to teach how to use git with the help of boot.dev's curriculum's guidence) and to not be posted on GitHub, cause I'm just making it to help myself learn, if I wasn't I'd be forgetting most of the thing I'm "learning".
    -Core Goal: A living README.md that explains Git concepts chronologically as they appear in the Boot.dev curriculum. It uses a "Wizard’s Notebook" theme to simplify complex version control mechanics, simplified in a way so that a 5-year-old could understand it while maintaining technical accuracy.

    Key Analogies & Frameworks:

        The Config Notebook (The Hierarchy of Power):
            -The Annex (--worktree): A shared scroll for wizards working in two places at once.
            -The Sticky Note (--local): Project-specific settings that override the more general books.
            -The Inside Cover (--global): Your personal identity that follows you through every quest.
            -The Kingdom's Law (--system): The stone tablet in the town square — rules for every wizard in the land.
            -The Detective (The Search Rule): Git always searches from the most specific (Annex/Local) to the most general (System) and stops as soon as it finds an answer.
            -The Chapter Eraser (remove-section): Ripping out a whole section of settings when they become "nonsensical."
            -The Eraser Rule: Distinguishes between erasing a single line (unset) and scrubbing a whole chapter (remove-section).
        The Three States (The Room Photo):
            -Working Directory: The "Live Room" where you move furniture (modify files).
            -Staging Area/Index: The "Camera Viewfinder" (preparing the shot via git add).
            -Commit History: The "Photo Album" (the permanent record via git commit).
        The Map of Diverging Paths (Branching):
            -The Family Tree: History is not a straight line; it is a series of "Side-Quests" (Branches) that split from the "Trunk" (Main).
            -The Ancestor Rule (Lineage): A branch isn't just a single photo; it is the entire collection of photos leading back to the beginning. (e.g., A-E-F for primes_branch).
            -The Tip of the Wand: The most recent photo in a branch. As the Wizard works, the "Sticky Note" automatically slides forward to stay at the Tip.
            -The Wizard's Focus (HEAD): A glowing highlight that shows which "Sticky Note" the wizard is currently looking through. It is a "Pointer to a Pointer."
            -The Crossroads: The specific commit where a side-quest originally split from the main road. In our map:
                       G - H    (The Sky-Castle Branch)
                      /
                 A - B - C - D   (The Main Road)
                  \
                   E - F        (The Deep-Sea Branch)
                -B is the Crossroads for the Sky-Castle branch. It is the last moment both paths were the same. Understanding the Crossroads is the secret to eventually performing "Merge Spells" to bring those two worlds back together!
                -The Parallel Reality (Divergence): When Main moves forward while a Side-Quest is still in progress, neither branch is ahead of the other — they have simply lived different lives.
                -The Safe Eraser Rule: -d refuses to delete an unmerged branch. -D forces the deletion regardless.
        The Workflow Hierarchy (50/40/10 Rule):
            -50% Solo Mastery: The daily loop of status, add, and commit.
            -40% Remote Collaboration: The "Post Office" (sharing albums via push/pull).
            -10% Emergency Spells: Precision tools for fixing "cursed" repositories (reset/revert/merge).
        The Merge Spells:
            -The Bridge Commit: A special photo with two parents — one from each branch — created when Git weaves two timelines together. Git finds the Crossroads, replays the changes from both sides, then snaps the Bridge Commit.
            -The Fast-Forward Merge (The Sliding Sticky Note): If Main has no new photos since the Crossroads, Git skips the Bridge Commit entirely and simply slides the main sticky note forward to the Tip of the other branch. No new photo is taken. Main must be a direct ancestor of the branch being merged.
            -Conflicts (The Curse): When both branches changed the same line of the same file since the Crossroads, Git cannot decide which version wins and asks you to resolve it by hand.
        Porcelain vs. Plumbing:
            -Porcelain: User-friendly tools for daily work (e.g., git config set).
            -Plumbing: Under-the-hood X-ray tools (e.g., cat-file, rev-parse) and manual file editing.
        The Secret Library (Content Addressing):
            -Blobs (The Leaves): Fingerprinted content only (Deduplication via "Space").
            -Trees (The Branches): Packing lists showing the "Mode" (The Toy Car analogy).
            -Commits (The Snapshots): The dated, signed entry in the library log (Deduplication via "Time").
            -The Lightweight Rule (Ghostly Bookmarks): Branches are "cheap" because they aren't copies of the library. They are 41-byte files — tiny slips of paper that point to a Fingerprint (Hash). You can have 1,000 parallel worlds without the library getting any heavier.
        Inspection Tools:
          A dedicated section covering the "under the hood" tools:
            -'git status': Your Map.
            -'git log' (with flags: --oneline, --graph, --all, --decorate=full, --parents, -n): Your Photo Album.
            -'git --no-pager log': Dumps the full history without the pager.
            -'git cat-file -p <hash>': Your X-Ray Machine.
            -'git ls-tree <tree-ish>': Your Packing List.
            -'git config list --show-origin': Shows every rule and which file it came from.
    Documentation Standards:
        -Command Syntax: Mandatory <>, optional [].
        -Scope Rule: Defined at first mention; defaults to --local for writing but searches all levels for reading.
        -The Naming Convention: Named concepts always follow the pattern The [Name] ([Technical Term]), e.g. "The Inside Cover (--global)." This makes scanning easier.
        -The Verb Rule: Command descriptions start with an action verb in present tense ("Reveals," "Stores," "Deletes") for consistency.
        -The Footnote Rule: Supplementary detail, exceptions, and plumbing facts go in numbered footnotes *(n)* rather than inline, to keep the main entry readable.
        -The Analogy-First Rule: Every technical concept is introduced through its Wizard's Notebook analogy before the technical explanation is given.
        -The Nesting Rule: Flags and sub-behaviors are indented as children of their parent command, not listed as separate top-level entries.
        -The Bold Distinction: Sub-rules and named concepts within a footnote use **(bold)** to separate them from the footnote's plain text.
    Current Status:
        Foundations are fully complete, covering configuration scopes, repository initialization, object hashing/deduplication, branch visualization/lineage, and merge mechanics (Bridge Commit, fast-forward merge, and conflict awareness). Within the 10% Emergency Spells section, merging is fully documented, but Resetting is half flesh out and Reverting remain high-level placeholder with no command or detail yet. The "Remote Collaboration" chapter (push/pull/clone) remains a high-level placeholder for future curriculum milestones.

     so, based on this entry of mine and this lesson what about this lesson I could add in my README.md entry to make it more complete? I could give the full README.md file if you wish
