# ~/.bash_profile
```
[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion

GIT_PS1_SHOWDIRTYSTATE=true

source ~/.bashrc
source ~/.git-prompt.sh

export PS1='[\u@mbp \w$(__git_ps1)]\$ '

export PATH=~/bin:/usr/local/bin:/bin:$PATH

export EDITOR='subl -w'

export PROMPT_COMMAND='echo -ne "\033]2;${USER}@${HOSTNAME%%.*}: ${PWD/#$HOME/~}\007"'

get_git_branch() {
  echo `git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`
}
get_master_ancestor() {
  echo `git merge-base HEAD master 2> /dev/null`
}
get_integration_ancestor() {
  echo `git merge-base HEAD integration 2> /dev/null`
}

## Dev Specific
alias fs='foreman start -f Procfile.dev'
alias demo:reset='foreman run rake db:reset demo:seed db:test:prepare'

## Git
alias pull='git pull origin `get_git_branch`'
alias pullr='git pull --rebase origin `get_git_branch`'
alias push='git push origin `get_git_branch`'
alias pushf='if [ `get_git_branch` = "master" ]; then echo error: force pushing to master cannot be done automatically; else git push origin `get_git_branch` -f; fi'
alias gs='git status'
alias gc='git add . && git add -u && git commit -m'
alias wip='git add . && git add -u && git commit -m "wip"'
alias gch='git checkout'
alias gl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"
alias gcam="git commit -a --amend"
alias squash="git fetch origin master:master && git rebase \`get_master_ancestor\` -i"
alias rebase="git fetch origin master:master && git rebase master -i"
alias isquash="git fetch origin integration:integration && git rebase \`get_integration_ancestor\` -i"
alias irebase="git fetch origin integration:integration && git rebase integration -i"
alias spp='git stash && git pull --rebase origin `get_git_branch` && git stash pop'
alias bduc='_BRANCH=`get_git_branch` && git stash && git checkout master && git branch -D $_BRANCH && git remote update origin && git checkout $_BRANCH'

## Github
# gh    - opens github.com to current repo/branch page
alias gh="open \`git remote -v | grep git@github.com | grep fetch | head -1 | cut -f2 | cut -d' ' -f1 | sed -e's/:/\//' -e 's/git@/http:\/\//'\`/tree/\`get_git_branch\`"
export PATH="/usr/local/opt/qt@5.5/bin:$PATH"
export PATH="/usr/local/opt/qt/bin:$PATH"

alias gcr="git checkout development && git pull origin development && git commit --allow-empty -m 'Release' && git push origin development && git checkout master && git merge development --ff-only && git push origin master"

. /usr/local/opt/asdf/asdf.sh

. /usr/local/opt/asdf/etc/bash_completion.d/asdf.bash
```
