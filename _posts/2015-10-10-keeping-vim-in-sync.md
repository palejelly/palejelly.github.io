---
layout: post
title: Syncing Vim Across Different Environments
comments: true
image: https://jonathanmann.github.io/public/img/vimrc.png
excerpt: One of the great things about Vim is that it requires very little customization. For the things that you do want to customize, however, it's really good when you can keep everything in sync across your various environments.
---

One of the great things about Vim is that it requires very little customization. For the things that you do want to customize, however, it's really good when you can keep everything in sync across your various environments. 

![vimrc](https://jonathanmann.github.io/public/img/vimrc.png)

### Putting Your Vim Configuration in Version Control

My first suggestion is to store your .vimrc configuration file in your ~/.vim folder so that you can easily keep everything in version control. By default, Vim looks for .vimrc in your home directory, so, in order for Vim to work properly, you will need to create a symbolic link called .vimrc in your home directory. This can be accomplished with the command

{% highlight sh %}
$ ln -s ~/.vim/.vimrc ~/.vimrc
{% endhighlight %}

### Managing Plugins with Vundle

Now that your Vim configuration is in version control, managing your plugins can be made easy with [Vundle](https://github.com/VundleVim/Vundle.vim). I use Vundle instead of Pathogen because running commands like "execute pathogen#infect()" might make the security team where I work nervous, and I want to use the same configuration at work as I do at home, but I'm sure Pathogen could work just as well. 

Here is a snippet from [my vim configuration](https://github.com/jonathanmann/vim_config):

{% highlight vim %}
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
Plugin 'gmarik/Vundle.vim'
Plugin 'tpope/vim-fugitive'
" All of your Plugins must be added before the following line
call vundle#end()            " required
{% endhighlight %}

With Vundle, you can easily manage all of your plugins in one convenient location, and, if you add a new plugin in one environment, when you pull your .vimrc into your other environments and run  

{% highlight vim %}
:PluginInstall
{% endhighlight %}

in the ex line, the plugins you added in your other environments will be automatically installed on your current environment.
