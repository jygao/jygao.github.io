---
layout: post
title: git alias 使用总结
date: 2019-05-26 21:05
author: jygao
comments: true
categories: [technology]
tags:
    - git
---
<!-- wp:paragraph {"fontSize":"medium"} -->
<p class="has-medium-font-size"><strong>git alias有什么作用？</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 可以允许你自己设置缩写命令，让你不必每次打很长的命令，类似于快键键。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"medium"} -->
<p class="has-medium-font-size"><strong>git alias配置</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>git alias 需要配置才能使用，配置包括全局和本地的。全局的配置（windows系统）一般在c:\username\.gitconfig下；本地配置.git文件夹config下。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"medium"} -->
<p class="has-medium-font-size"><strong>git alias如何设置</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>第一种方法是直接使用git config命令：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git config --global alias.co checkout
git config --local alias.co checkout</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>第二种方法是在对应的配置文件中加入：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>[alias]<br>&nbsp;&nbsp;&nbsp;co&nbsp;=&nbsp;checkout </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"medium"} -->
<p class="has-medium-font-size"><strong>我的alias配置</strong></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>[alias]
  #list alias
  la = "!git config -l | grep alias | cut -c 7-"

  #branch
  br = branch
  ## Deleting local branches that have been merged and deleted:
  delete-merged = "!git branch --merged | grep -v \"^\\s*master$\" | grep -v \"\\*\" | xargs -n 1 git branch -d"
  dm = !git delete-merged
  ## git track branch-name (to track with origin)
  track = "!f() { git branch --set-upstream-to=origin/$1 $1; }; f"
  ##git del branch-name (to delete locally)
  del = branch -D
  co = checkout
  ##To create a new branch at the current commit, or move an existing branch,
  mb = checkout -B
  ##To checkout a particular file from a different branch git cb other-branch /file/to/checkout
  cb = "!cb() { git checkout $1 -- $2; }; cb"
  recent = for-each-ref --count=10 --sort=-committerdate refs/heads/ --format=\"%(refname:short)\"

  #log
  l = log --pretty=format:"%C(yellow)%h\\ %ad%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=short
  ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat
  lll = log -u
  last = log -1 HEAD

  graph = log --graph --all --decorate --stat --date=iso
  local = log --oneline --no-merges origin/master..HEAD
  upstream = log --oneline --no-merges ..origin/master
  overview = log --all --oneline --no-merges
  today = log --since=00:00:00 --all --no-merges --oneline --author='your email'
  recap = log --all --no-merges --oneline --author='your email'
  # changelog = log --oneline --no-merges ${1-$(git describe --abbrev=0)}..HEAD
  who = shortlog -n -s --no-merges

  #diff
  diffc = diff --cached HEAD^
  wdiff = !git diff --word-diff

  #stage
  st = status
  s = "!stage() { git add `git st | sed -n $1p | awk -F' ' '{ print $2 }'`; git st; }; stage"
  u = "!unstage() { git reset HEAD `git st | sed -n $1p | awk -F' ' '{ print $2 }'`; git st; }; unstage"
  unstage = reset HEAD --
  aa = !git add -u &amp;&amp; git add . &amp;&amp; git st

  #commit
  ci = commit
  cm = "!cm() { git commit -m \"$1\"; }; cm"
  ca = commit --amend --no-edit
  rof = "!f() { git show HEAD -- $1 | git apply --reverse;}; f"
  ccfl = !git commit-changed-file-list
  commit-changed-file-list = "!f() { git show $1 --name-only; }; f"

  #reset
  ##git undo (to undo the last commit and leave the files there)
  undos = reset --soft HEAD~1
  undoh = reset --hard HEAD~1

  re1 = reset HEAD^
  reh = reset --hard
  rec = !git reh &amp;&amp; git clean -fd

  #rebase
  ##To rebase interactively from a number of commits back git ri 5:
  ri = "!ri() { git rebase -i HEAD~$1; }; ri"

  #push
  pp = "!f() { git pull -r &amp;&amp; git push; }; f"


  #stash
  # git stan 3 (to apply the 3rd item in the stash)
  stan = "!f() { git stash apply stash@{$1}; }; f"</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>参考链接：</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://github.com/csswizardry/csswizardry.github.com/issues/66">https://github.com/csswizardry/csswizardry.github.com/issues/66</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://github.com/jakubnabrdalik/gitkurwa">https://github.com/jakubnabrdalik/gitkurwa</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://dev.to/committedsw/useful-git-alias-47in">https://dev.to/committedsw/useful-git-alias-47in</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
