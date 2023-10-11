# git command

1. Clone as specific branch

- > git clone -b branchname remote-repo-url

2. Remove origin

- >  git remote rm origin

3. Change origin

- > git remote set-url origin git://new.location

3. ignore a commited file

- > git rm --cached /path/to/file

4. merge

- > git merge branch

5. remove local branch

- > git branch -d local-branch

- > git branch | grep " < pattern > " | xargs git branch -D

6. change branch

- > git checkout branch

7. Automate password

- > git config --global credential.helper store

8. Find deleted branch

 - > git reflog show remotes/origin/dev
   
9. Git lfs

- > curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.python.sh | bash

10. display branch by default
```sh

parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="\u \[\e[32m\]\w \[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "


```
