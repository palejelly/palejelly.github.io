---
layout: post
title: Configuring Vim for Use in the Terminal
comments: true
image: https://jonathanmann.github.io/public/img/vim_for_terminal.png
excerpt: GVim is great for a local machine, but sometimes it isn't possible to use when working on a remote server. Never fear, with the right configuration, Vim can look just as great even when it is running in terminal mode. 
---

GVim is great for a local machine, but sometimes it isn't possible to use when working on a remote server. Never fear, with the right configuration, Vim can look just as great even when it is running in terminal mode.

![candy_man](https://jonathanmann.github.io/public/img/vim_for_terminal.png)

### Configuration

When I'm using GVim, I prefer to use the solarized theme, but that colorscheme really does not work well in terminal mode. In order to get the right colorscheme, we need to set the configuration so that a different theme appears when Vim is not running in GUI mode.

Here is the snipit from my [.vimrc](https://github.com/jonathanmann/vim_config/blob/master/.vimrc) that does what we want:

{% highlight vim %}
if has('gui_running')
    " GUI colors
    set background=dark 
    colorscheme solarized
else
    " Non-GUI (terminal) colors
    set t_Co=256
    colorscheme spacegray 
endif
{% endhighlight %}

The colorscheme that I really wanted to use in terminal mode was candyman, but I found its terminal colorscheme lacking. I ended up creating a [custom theme](https://github.com/jonathanmann/vim_config/blob/master/colors/spacegray.vim) to resemble candyman based on the spacegray terminal theme. 


