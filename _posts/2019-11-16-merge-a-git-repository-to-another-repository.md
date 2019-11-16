---
layout: post
title:  "Merge a git repository to another repository"
date:   2019-11-16 10:00:00 +0545
categories: [bash, git]
---

Software development can be a tedious project. Sometimes you can have multiple forks of the same project with different teams working on individual forks. But the need to incoporate good functionalities of one fork to another can lead to merging one git repository to another which can be a headache for the project lead.

The following steps can be followed in order to merge one git repository inside another.Let us consider `fork_first` and `fork_second` be the forks of the same project exixting in two git repositories.

First you have to be in a directory of the project where you want to merge another repository to. In our case , let us merge master branch of `fork_second` inside master branch of `fork_first`

{% highlight ruby %}
$ cd /path/to/fork_first
$ git checkout master #switch to master branch(or any other branch to merge to)
{% endhighlight %}

Now the next step would be to add the remote url of `fork_second`
{% highlight ruby %}
$ git remote add -f fork_second git@github.com:gituser/fork_second.git #git remote url
{% endhighlight %}

Now you can easily merge `master` of `fork_second` into `fork_first`

{% highlight ruby %}
$ git merge fork_second/master
{% endhighlight %}



**References:**
* [Digitalocean Tutorial](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)
* [RFC 4251](https://www.ssh.com/a/rfc4251.txt)
* [Image Source](https://www.ssh.com/s/ssh-tunneling-3138x956-4zmvrU8b.png)

