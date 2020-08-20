---
layout: post
title: 熟练掌握Rebase
date: 2019-06-22 21:44
author: jygao
comments: true
categories: [technology]
tags:
    - git
---
<!-- wp:paragraph -->
<p>Git 'rebase' 可能是你听过可以替代‘merge’的诸多命令的一种。但事实是，‘rebase’是一个完全不同的一套东西，只是它的子集能实现同‘merge’一样的目标。对此感到疑惑？不要担心，这篇博客讲的是‘rebase’的与之相区别的用法。首先说明的是，它既是在两个分支见合并和交换你的工作的内容（类似于‘merge’做的）的一种方式，也是一个重写历史记录的工具。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>从 merge 到 rebase </h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>合并分支是Git中合并两分支修改的最普遍的方式。一个Git工作流提供最普遍的服务（例如GitHub或者Gitlab）见如下:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>基于主分支，例如‘master’或者‘develop’，创建一个名为‘my-new-feature’的特性分支</li><li>在此特性分支上做你的工作并提交（commit）你的更改</li><li>推送该特性分支到远程共享服务器</li><li>为‘my-new-feature’分支新开一个Pull Request，请求合并</li><li>收集来自你队友的反馈，等待测试和通过</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>来到这里，一切都很美好。你最终拿到了一条漂亮的、干净的分支，假设如下：</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/06/02/5cf34894c1dd175203.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>然而，这个世界并不完美，接下来可能出现如下情形：</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>代码走查的同事在你的第一个提交中发现了一些小问题和拼写错误。你的第一个提交没通过</li><li>你本地做了些修改并提交去修正这些错误</li><li>你推送你的更新在远程共享仓库中的特性分支（见下图的C6和C7）</li><li>在此期间，其他人的提交（C8和C9）被合并到主分支</li><li>你的pull request 最终被接受，被合并进主分支（C10）</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>然后从这里开始，你的提交历史变得有一点点复杂了：</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/06/02/5cf3a89d3207197737.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>这样的工作流没有什么问题。特别地，你不必为你的同事做了什么而操心，而只需关注你自己的工作。这里有一个关键点，就是你的更改与Git的 主分支内容的结合（或者说合并）只会反生一次。也就是说，你将只需处理一次最终的产生的冲突——在做合并的步骤时。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>然而，这里也有一点东西过了头。首先，如果你在你的分支工作很长一段时间，你可能数天或者数周没有跟主分支同步。这可能不是什么问题，但是有时你可能意识到也包括了你的团队合并进来其他的修正，或者摆脱那繁杂的依赖让你每次编译时效率低下。其次，历史记录是如此的复杂，以致于你不能一下子了解你的同事把他们自己的分支合并到主分支的所有更改。最后，这可能有点主观，你可能使你分支上的提交有点逻辑紊乱了。一个包含了你对你所有文件的所有更改的节点可能不是你最终期待暴露的。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>让我们看看rebasing是如何可以帮你解决所有的这些问题。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>在 主分支上Rebasing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>在2016年9月，GitHub引入了一种新的合并请求的方式： 那个"Rebase and merge"按钮。其他仓库托管平台同样也有，如GitLab，它是Rebase的前门。它允许你对合并请求做一次单独的rebase操作然后在执行一次合并（merge）。这两个操作是按顺序执行的，rebase并不是merge的替代品，认识到这一点很重要。因此rebase不是过去所说的替代了merge，而是完成了它。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>让我们回到上面的例子。在最终的合并之前，我们处在这样的状况：</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/06/02/5cf3be501a3f953077.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>当点击“Rebase and merge”按钮（当没有冲突时），想知道发生了什么？你可以模拟执行如下命令：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git checkout my-new-feat
git rebase master
git checkout master
git merge my-new-feat --ff</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>通过如此做法，你最终得到一条“线性的历史”：</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/06/02/5cf3bfb2363d258987.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>如你所见，rebasing不是替代了merge那步。解释先前的，这两操作不是执行在同一分支的：‘rebase’是作用于特性分支的，而‘merge’是作用于主分支的。现在，这个操作防止了产生一个包含了所有更改的单独的提交节点。它仍然像你最后一次贡献那样是一个单独的操作（例如，当你想分享你的工作时）。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 目前为止，我们只拿‘master’分支作为我们的主分支。与主分支保持同步，它仅是用最新的主分支去执行rebase操作的问题。你等待地越久去做这个，你将会有越多内容没有同步。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>你的主分支的最新版通常被隐藏了。它是主分支的一个只读版本，以你连接的远程仓库的名字为前缀，或者更简单说：它是远程仓库（例如GitHub或者GitLab）分支的一份拷贝。当你第一次拷贝远程仓库到本地时，默认的前缀是‘origin’。更加准确的说，你的‘master’分支是主分支的本地版本，它被拷贝到你的电脑上，当你上次执行‘git fetch’操作时，‘origin/master’是主分支的远程版本。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>我们跳过了一系列的理论知识，但是最终的结果是比较简单的。这里是如何同步远程的最新更改方法：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git fetch
git checkout my-new-feat
git rebase origin/master</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p> 第一步是从远程“master"分支上获取最新的更改到本地的“origin/master”分支。第二步切换到你的特性分支。最后一步执行“rebase”把你的提交加到最新的更改之上，这些更改原本跟你的工作是并行的。通过在我们最简单的第一个例子中执行以上命令，我们会得到如下的结果：</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://i.loli.net/2019/06/02/5cf3cc52d7ab389902.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> 正如你所见，你的特性分支上已经包含了最新的更改，所以你可以在同步更新你的同事的更改的基础上工作。 通过以上工作流，你可以尽早地逐渐地处理可能产生的潜在的冲突而不是在最后一刻再处理（当你想合并你的更改进主分支的时候）。 人们通常会忽略“Rebase and merge”按钮，因为他们认为在过程的最后一步有太多的冲突（因此他们更喜欢使用常规的合并（merge）提交） 。最终，我们稍稍努力一下，几乎不怎么费劲地就可以和最新的更改保持同步了。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Rebasing 你自己的工作</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>直到现在，我们只是用“rebase”从一条分支到另一条分支去生成提交。这是rebase的广泛的基础用法：仅是默认的选项，行为和结果。 此外，我们只是用“rebase”去合并不同分支的变化到我们自己的分支。但是，你也可以在你自己的分支上直接用add/change/remove 你的提交！你的rebase几乎可以基于任何的提交——甚至是它的直接的祖先。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 事实上，如果你想看到在我们做的rebase过程中发生了什么，你可以加上"-i"或者“-interactive”参数 ，进入rebase的交互模式。这样做的话，git将会打开你的选择的编辑器（在你的"EDITOR"环境变量里定义的）并且列出所有将被这次rebase操作所影响到的提交，和每一个单独的提交应该被怎么样处理。这是rebase的真正地力量所在。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>从你的编辑器那里，Git可以让你重新排序、重新命名或者移除提交，但是你也可以把一个提交分拆为多个提交，合并两个或者更多的提交，或者同时修改它们的提交信息！使用“rebase”你可以做几乎所有和你的历史相关的东西。并且令人惊叹的是，你可以很容易地告诉Git怎么做。每个提交都有一行记录，线性排列，以它将生效的命令为前缀。重新排序提交变成了重新排序行记录即可， 位于最低的行是最新的一次提交。移除提交变成了移除相应的行，或者在行记录前面指定“d”或者“drop”的命令标记。有一个提交信息有拼写错误？只需使用“r”或者“reword”命令标记去保留此次提交，然后改变相关的提交信息。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 总结一下，“rebase”是Git一条可以让你如此的命令：</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>选择一个或者多个线性提交</li><li>以你仓库任何一个提交作为为它们的基础</li><li>更改会应用到这条提交线，生成新的提交，这些提交会被添加到新的基础上面</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p> 为了更好地解释这个，考虑如下一系列提交：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ git --no-pager log --oneline
57f15b4 (HEAD -> master) add D and E files
61681da add B file
7d4a28d add C file
f92bb1d add A
78b3f67 root commit</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p> 如你所见，这里我们最初有一个“root commit”——这个提交会作为我们的基础提交——跟着它后面的是把5个文件加入到仓库的4次提交。为了练习，让我们认为这些提交是你的合并请求，然后你对它并不满意因为如下的原因：</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>第一个提交信息错了，它应该是“ add A file ”，而不是“ add A ”</li><li>文件B和文件C被添加的顺序错了</li><li>文件D应该跟着文件C一起提交的，而不是跟着文件E</li><li>最后，文件E应该单独提交</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>以上所有这些改变可以在一个rebase操作中解决。最后的提交记录看起来是这样子的：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ git --no-pager log --oneline
2d6361f (HEAD -> master) add E file
1e33d62 add C and D files
180b4bd add B file
eb6beee add A file
78b3f67 root commit</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>请注意，除了我们基础提交，所有的提交hash id 都变了。这是因为Git不单单根据它们的更改，还包括它的父提交节点和其他元数据生成这些提交hash值。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>不管怎样，让我们rebase吧！ </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>让我们从“git rebase -i HEAD~4“开始。这条命令告诉Git以交互的方式把从包含HEAD的最后4个提交进行rebase。"HEAD~4"指向的是“root commit，它是我们这次rebase的基石。在按了回车键后，你选择的编辑器会打开（或者默认在Unix-style中是“vi”）。在此，Git简单的问你想怎么处理这些你指定的提交。</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>pick f92bb1d add A
pick 7d4a28d add C file
pick 61681da add B file
pick 57f15b4 add D and E files

# Rebase 78b3f67..57f15b4 onto 78b3f67 (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>如先前解释过的，每一行代表一个单独的提交，以将要生效的对应的rebase命令为前缀。在rebase的过程中，所有被注释的行是可以忽略的，这里只是提示你怎么个用法。在我们例子里，我们将要像如下那样修改命令：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>reword f92bb1d add A
pick 61681da add B file
pick 7d4a28d add C file
edit 57f15b4 add D and E files</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>这里，在rebase过程中，我们告诉Git去执行3个任务：</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>停在第一次提交，让我们去改变提交的信息</li><li>第二个和第三个提交重新排序，让它们按照正确的顺序</li><li>停在最后一个提交那里，让我们做些手动修改</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><br>当你保存文件并且推出编辑器，你将会再次看到你的编辑器，第一次提交的信息会展示在你眼前。Rebase操作正在进行，你会看到提示修改提交信息的提示。让我们把“Add A”改为“Add A file”,保存并退出。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 第二个和第三个的提交的重排序Gi可以让你很易见地换行就完成了。剩下留给我们的是是我们需要执行的对最后一个提交的修改。在这里，我们停在了"add D and E files"这个提交之后。由于我们想要用C和D文件去生成一个单独的提交，还要单独为E生成一个新的提交，我们需要像在我们的分支的顶部正在修改额外的提交一样执行如下的步骤：<br></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git reset HEAD~1
git add D
git commit --amend -m ‘add C and D files’
git add E
git commit -m ‘add E file’
git rebase --continue</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>这些命令（除了最后一个）使得“ add C file ” 和“add D and E files”这两个提交变成了我们想要的“ add C and D files ”和“add E file”这两个提交。最后一条命令，是用来告知Git我们要对“edit”这个步骤所做的。在那之后，Git会很高兴地告诉你rebase操作成功完成了。太好了！<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>我们提及了几乎所有你可能要对你的提交历史做的任何操作。还有不少的一些命令可以帮你做得更好，这依赖你使用的场景。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>处理冲突</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>当使用"rebase"时，人们常常会对在rebasing 一条分支到另一条分支的顶部时可能产生冲突的解决方式感到迷惑。实际上我们解决Git产生的冲突会很方便，有如下理由：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>首先，当冲突产生时，Git不会尝试变得比你还要聪明——它会停在当前的“rebase”然后询问你去解决这个冲突。那些冲突的文件会被标记为“已经修改的文件”，然后冲突的部分会有一些标记去帮助你找到它们之间的差异。当你处理完这些修改时，你执行“git add” 那些修改的文件之后，在运行“git rebase --continue”去让rebase继续之心下去。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>第二，当你对正在运行rebase或者rebase引发错误结果不太自信时，有两个工具非常强大可以供你使用。考虑使用“ git rebase –abort”，它会让你回到当前rebase执行之前的提交状态。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>有了以上技巧，“rebase”产生的变化可以被撤销，因此冒险犯错的影响可以降至最小。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>最后，你可能发现自己要解决一段很长而且很枯燥的冲突，甚至某个时候，同一个冲突在不同的时间又重新出现了。例如，当你工作在自己地分支时，你的基础的分支改变了，这个不幸地很常见。另一个场景是，当你取消了rebase然后现在准备重新做“rebase”。为了避免重复解决同一个冲突，Git提供了一个默认没有开启的解决方案。这个特性被叫做“reuse recorded resolution” 或者“rerere”，它可以用如下命令开启：“git config –global rerere.enabled true”。 这样子的话，Git会记住你执行过的所有解决冲突的操作。当一样的冲突再次产生时，你可以看到Git会输出一个被记录的解决方案已经生效了。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2><a href="https://blog.algolia.com/master-git-rebase/#going-further"></a>更多</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>我希望这篇文章可以帮你看到"rebase"的可能性。当然，最好的方式学习Git并且使用它。第二好的方式是阅读它。如果你想读更多内容，我强烈推荐  <a rel="noreferrer noopener" href="https://git-scm.com/book/" target="_blank">Pro Git</a> 这本书 – 特别是关于rebase那个部分的。然后，因为我们可能某时终究会碰到坏结果，我们可能要看一下数据恢复相关的内容  <a rel="noreferrer noopener" href="https://git-scm.com/book/en/v2/Git-Internals-Maintenance-and-Data-Recovery" target="_blank">Maintenance and Data Recovery section</a> 。如果不打算读完所有的文档，也许你最好读下 <a rel="noreferrer noopener" href="https://github.com/k88hudson/git-flight-rules" target="_blank">Git Flight Rules</a>.。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>有额外rebase的使用技巧或者对本文有啥意见？我们很乐意去听到： <br><a rel="noreferrer noopener" href="https://twitter.com/aseure" target="_blank">@aseure</a> or <a rel="noreferrer noopener" href="http://twitter.com/algolia" target="_blank">@Algolia。</a>感谢大家的阅读，使用Git愉快！</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>本文是中文翻译版，原文链接： <br><a href="https://blog.algolia.com/master-git-rebase/">https://blog.algolia.com/master-git-rebase/</a> </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
