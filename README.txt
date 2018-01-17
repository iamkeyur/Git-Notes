# Git-Notes

Notes:
  Git doesn't track empty files.
  To keep a directory, create inside it a .gitkeep file with
    touch /emptyFolder/.gitkeep

  Referencing to commits.
    using SHA, or HEAD, HEAD^ (One previous), HEAD^^ (2 back), master^
    
  HEAD points to the very last commit
    
    
Branching Overview
  - try new ideas
  - isolate features or sections of work
    
  one working directory
  fast context switching
  
========================================
==== Basics | Working on local repo ====
========================================

Def: POINTER --> HEAD, HEAD^, HEAD^^, COMMITSHA, Branch Name

git config
      --global
      --system
      --local
      
git add [<file>|.]													<---- Add a file to the stage index.

git reset HEAD <file>												<---- Unstage | it's suggested in "git status"

git status

git log
      --oneline
      --oneline -2													<---- only two lines
      --format=oneline
      --format=oneline -2										<---- return last two lines
              =short
              =full
              =fuller
              =email												<---- format useful for email
              =raw
      --graph
      --graph --oneline --all --decorate		<---- Cooler info
      --since="2016-06-20" (or --after)			<---- Ranges
      --until="2 weeks ago" (or --before)
      --since=2.days --until=1.day
      --since=1.day --until=1.hour
      --author="eddy"
      --grep="temp"
      [POINTER]..[POINTER] --oneline		    <---- range low to high, sha commits, branch names
      -p [POINTER].. [file] 								<---- shows changes applied to this X file since that commit
          --stat --summary									<---- stats of what was changed
          --stat --summary [POINTER]..			<---- show stats starting from this commit.

git show [POINTER]
      --oneline
      --format=...
      
git commit -m "message"
      --amend -m "new message"    					<---- fixes current commit or very last commit
      
git diff
      --staged															<---- If staged, show the difference local with stage
      -w																		<---- ignore all whitespaces chages
      [POINTER]..[POINTER]									<---- shows a difference between these two commits
      --stat --summary [POINTER]..[POINTER|HEAD]
      
git checkout 
      -- [file]  														<---- Revert last changes to the very last commit.
      [POINTER] -- [file]  									<---- Revert changes on file to the specified commit|pointer
      
git revert [POINTER]  											<---- Revert everything to the specific commit. After that, it commits them.

git reset																		<---- "Use it to Reset to Undo commits; use it only when everything went wrong!"
      --soft
      --mixed (default)
      --hard

git clean -n  															<---- show untracked file that would be removed

git rm [file]																<---- Remove file
git rm --cached [file]  										<---- Remove from staging index / not from local, nor from repository

git mv [file]																<---- Move a file; same as bash mv command.

git ls-tree [POINTER]												<---- Similar to ls command, but shows in specific pointer, branch name, commit.
git ls-tree [POINTER] [Folder]

===================
==== Branching ====
===================

git log ([branch]) --graph --oneline --decorate	<----- Shows all information about all branches

git branch											<----- Shows the list of branches and the current branch
git branch [newBranch] ([commitSHA|HEAD|branch|<alias>/<branch>])				<----- Creates a new branch
      --merged									<----- Show if the current branch has all commits of other branches
      --move or -m							<----- Rename a branch
      --delete or -d						<----- Delete a branch

git checkout [branch]						<----- Switches to the specific branch
    checkout -b [branch]				<----- Create and switch to this branch
    
git diff [branch1]..[branch2]		<----- comparing branches
      [branch1^]..[branch2^^]		<----- Previous commits in those branches
      --color-words							<----- colors word chenges instead of lines
      
=================
==== Merging ====
=================

git merge [branch]							<----- Merge the current branch with the specific branch
                                       (If there were no changes in master or current branch)
                                       or fast forward merge.
      --ff-only	[branch]				<----- Merge only if I can do a fast forward merge
      --no-ff [branch]					<----- Force commits, mainly for documentation

==========================
==== Stashing Changes ====
==========================

git stash 
      save "[MESSAGE]"					<----- Save in stash, when file(s) are not ready to be commited
      list											<----- List all stash messages | It can be executed in any branch
      show [-p] [stash@]				<----- Show what was changed | -p show changes as patch
      pop [stash@]							<----- Bring and apply the patch | it removes from stashed list
      apply [stash@]						<----- Bring and apply the patch | it doesn't remove from list
                                       (It doesn't move to the branch; needs to git checkout branch)
      drop [stash@]							<----- Drop from stash
      clear											<----- Get rid of everything

=================
==== Remotes ====
=================

git remote [-v]									<----- List all remote repositories
      add <alias> <url>					<----- Add remote repository
      rm <alias>								<----- Remove remote repository
      
git push (-u|--set-upstream)		<----- Push our changes to a remote repository 
      <alias> <branch>								 Upstream allows tracking in the remote repo.
                                       If cloning repo, it also allows tracking.
      <alias> branch:<branch>		<----- Push ... . If branch has different name than the one in server.
      <alias> :<branch>					<----- Removes the remote branch | Push empty to remote branch
      <alias> --delete <branch> <----- ...
                             
git push												<----- No other arguments | If tracking upstream pushes changes to
                                       remote repository
git push <alias> :<branch>			<----- Deletes the branch from origin, remote.

git branch {--remote|-r}				<----- Shows remote branches
    branch {--all|-a}						<----- Shows local and remote branches
    branch {--set-upstream,-u} [branch] <alias>/<branch>		<----- If upstream was not set before.
    
git clone <url>	[folderName] [-b branch] 										<----- Clones a repository and creates a folder

git fetch <alias>								<----- Fetch lastest changes in the repo. to <alias>/<branch>
                                       Fetch doesn't change the local working repository
                                NOTES: * Fetch before you work.
                                       * Fetch before you push.
                                       * Fetch often.
                                       
git diff <alias>/<branch>				<----- Show the difference of local repo with remote repo.

git merge <alias>/<branch>			<----- Merge local branch with the one from <alias>/<branch>
                                       Specially after fetching.
                                       
git pull												<----- Shortcut like = git fetch + git merge

git branch <branch> <alias>/<branch>  		<----- Make a copy from remote branch to local repo.

git checkout -b <branch> <alias>/<branch> <----- Same as above; but also move to that branch.

=====================================
== Notes about working with GitHub ==
=====================================
I can add collaborators to give them write access to the repository.
If not, they/I can fork the repo, work on it, and then send a pull request for them to merge the
changes if accepted.

=======================================
== A Collaboration Workflow example: ==
=======================================
Me:
  git checkout master
  git fetch
  git merge origin/master
  git checkout -b new_feature_branch
  git add file.html
  git commit -m "Add a message."
  git fetch
  git push -u origin new_feature_branch
  
Collaborator:
  git checkout master
  git fetch
  git merge origin/master
  git checkout -b new_feature_branch origin/new_feature_branch
  git log
  git show COMMITSHA1|new_feature_branch
  git commit -am "Add another feature"
  git fetch
  git push
Me:
  git fetch
  git log -p new_feature_branch..origin/new_feature_branch   <---- Patch
  git merge origin/new_feature_branch
  git checkout master
  git fetch
  git merge origin/master
  git merge new_feature_branch					<---- Merge master with new_feature_branch
  git merge origin/new_feature_branch		<---- Same as above
  git push
  
=================================
== Aliases for common commands ==
=================================
git config -e 													<---- Edit config file
git config -l|--list										<---- List config
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.df diff
git config --global alias.logg "log --graph --decorate --oneline --abbrev-commit -all"
git config --global alias.dfs "diff --staged"
USE: git br -r

==============================
==== Tools and Next Steps ====
==============================
:/