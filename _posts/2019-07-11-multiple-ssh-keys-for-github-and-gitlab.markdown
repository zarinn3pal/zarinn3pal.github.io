---
layout: post
title:  "Multiple SSH keys for Github and Gitlab"
date:   2019-07-11 13:00:00 +0545
categories: [bash]
---

Git is one of the most used version control management software used by software engineers all over the world. Github has been well known as a leading remote repository hosting service for more than a decade.However collaboration limitations on private repositiries have attracted teams to use Gitlab.So why not use both and harness the best features provided by each repository hosting service.

Generate `ssh keys` with different names.

{% highlight ruby %}
$ ssh-keygen -t rsa -C "emailfor@github.com"
{% endhighlight %}

When you see the following message, enter a unique name. I will be using `id_rsa_home` for github and `id_rsa_work` for gitlab.

{% highlight ruby %}
Generating public/private rsa key pair. 
Enter file in which to save the key (/home/username/.ssh/id_rsa): /home/username/.ssh.id_rsa_home
{% endhighlight %}

Next, you'll be asked to enter a passphrase. Enter a secure one.So, you'll have created SSH key for your home account(i.e Github). 

Generate SSH keys for gitlab.
{% highlight ruby %}
$ ssh-keygen -t rsa -C "emailfor@gitlab.com"
{% endhighlight %}

Enter name for file.

{% highlight ruby %}
Generating public/private rsa key pair. 
Enter file in which to save the key (/home/username/.ssh/id_rsa): /home/username/.ssh.id_rsa_work
{% endhighlight %}

Enter the paraphrase(a secure one again) and the key generation part is done.Now check all the keys created. You should see similar results.

{% highlight ruby %}
$ ls ~/.ssh
    id_rsa_home  id_rsa_home.pub  id_rsa_work  id_rsa_work.pub  known_hosts
{% endhighlight %}
Now you need a config file for organise these keys.
{% highlight ruby %}
$ cd ~/.ssh
$ touch config
$ vim config
{% endhighlight %}

Add into the config file.
{% highlight ruby %}
# GITLAB
Host gitlab.com 
   HostName gitlab.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa_work
#  You can also use gitlab.company_url.com as a Host and HostName instead
# GITHUB
Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa_home
{% endhighlight %}
Next you'll delete cached keys

{%highlight ruby %}
$ ssh-add -D
#If the following message appears:
Could not open a connection to your authentication agent. Enter command below:
$ eval `ssh-agent -s` && ssh-add -D
{% endhighlight %}

Now add your keys
{%highlight ruby %}
$ ssh-add ~/.ssh/id_rsa_work
$ ssh-add ~/.ssh/id_rsa_home
{% endhighlight %}

Now you can check connection
{%highlight ruby %}
$ ssh -T git@github.com
#enter paraphrase for github
Hi github_user! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T git@gitlab.com
#enter paraphrase for gitlab
Welcome to GitLab, gitlab_user!
{% endhighlight %}

You may need to set git config user details for any project.
It's required to distinguish your accounts.

{% highlight ruby %}
$ cd github_project
$ git config user.name "home_user"
$ git config user.email "emailfor@github.com" 
{% endhighlight %}

