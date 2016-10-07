Git Commands
===================

Git Basic Commands
------------------
1. `git init`: initializing a Repository in an existing directory
2. `git clone`: cloning an existing respository
3. `git status`: check the status of files
4. `git add`: tracking or staging files
5. `git diff`/`git diff --cached`: viewing changes
6. `git commit`: commit changes
7. `git commit -a`: skipping the staging area
8. `git rm`: removing files
9. `git rm --cached`: keep the file in working tree but remove it from staging area
10. `git mv`: moving files, rename files
11. `git log`: view the commit history, `-p` shows the diff introduced in each commit, `-2` limits the output to only the last two entries.
12. `git log --stat`: show abbreviated stats for each commit. 
13. `git log --pretty=oneline, full, short, format`: specify output format.
14. `git remote -v`: showing remotes
15. `git remote add [shortname] [url]`: adding remote repositories
16. `git fetch [remote-name]`: pulls down all the data from that remote project that you don't have yet. If you cloned a repository, the command automatically adds that remote repository under the name origin.(it doesn't automatically merge it with any of your work or modify what you're currently working on.)
17. `git pull`: if you have a branch set up to track a remote branch, use this command to automatically fetch and then merge a remote branch into your current branch. `git clone` command automatically sets up your local master branch to track the remote master branch on the server you cloned from.
18. `git push [remote-name] [branch-name]`: pushing to remotes.
19. `git remote show [remote-name]`: inspecting a remote.
20. `git remote rename`/`git remote rm`: removing and renaming remotes.
21. `git tag`: listing tags.
22. `git tag -a v1.0 -m 'my version 1.0'`: creating an annotated tag.
23. `git show v1.0': see the tag data along with the commit that was tagged.
24. `git tag v1.0`: creating an lightweight tag.

Git Branch Command
-------------------
1. `git branch [branch-name]`: creating a new branch.
2. `git checkout [branch-name]`: switching to an existing branch.
3. `git branch -v`: see the last commit on each branch
4. `git branch --merged`: to see which branches are already merged into branch you're on.
5. `git branch --no-merged`: to see all the branches that contain work you haven't yet merged in.
6. `git checkout -b [branch] [remotename]/[branch]` or `git checkout --track [remotename]/[branch]`：set up other tracking branches, not **origin/master**
7. `git branch --list`: list git branches. `-r` with remote information, `-a` with all remote and local information.
8. 

