---
layout: post
title: Vagrant issues on upgrading to Fedora 24
date: 2016-07-17
categories:
- Vagrant
- Fedora
tags:
- vagrant
- issue
status: publish
type: post
published: true
author: BRG
thumbnail_path: blog/fedora_24.jpg
---

Recently, I have upgraded my system from Fedora 23 to Fedora 24 which was released on 21st June, 2016.
Even I got the perfect time to upgrade it - [**Bangalore Fedora Release Party**](https://fedoraproject.org/wiki/Release_Party_F24_Bangalore_India).

I was happy after updrade :smile:.

But the very next day, I got upset :disappointed: as Fedora 24 upgrade had broken my development environment in
[Vagrant](https://www.vagrantup.com/).

The issue was related to [libvirt](http://libvirt.org/) virtualization provider.

{% highlight text %}
$ vagrant plugin install vagrant-libvirt

Bundler, the underlying system used to manage Vagrant plugins,
is reporting that a plugin or its dependency can't be found.
This is usually caused by manual tampering with the 'plugins.json'
file in the Vagrant home directory. To fix this error, please
remove that file and reinstall all your plugins using `vagrant
plugin install`.
Ignoring json-2.0.1 because its extensions are not built.  Try: gem pristine json --version 2.0.1
Ignoring nokogiri-1.6.8 because its extensions are not built.  Try: gem pristine nokogiri --version 1.6.8
/opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/resolver.rb:356:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'fog-libvirt (= 0.0.3)' in any of the gem sources listed in your Gemfile or available on this machine. (Bundler::GemNotFound)
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/resolver.rb:331:in `each'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/resolver.rb:331:in `verify_gemfile_dependencies_are_found!'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/resolver.rb:200:in `start'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/resolver.rb:184:in `resolve'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/definition.rb:200:in `resolve'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/definition.rb:140:in `specs'
  from /opt/vagrant/embedded/gems/gems/bundler-1.12.5/lib/bundler/definition.rb:185:in `specs_for'
  from /opt/vagrant/embedded/gems/gems/vagrant-1.8.4/lib/vagrant.rb:76:in `<top (required)>'
  from /opt/vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:128:in `require'
  from /opt/vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:128:in `rescue in require'
  from /opt/vagrant/embedded/lib/ruby/2.2.0/rubygems/core_ext/kernel_require.rb:39:in `require'
  from /opt/vagrant/embedded/gems/gems/vagrant-1.8.4/bin/vagrant:105:in `<main>'
{% endhighlight %}

After googling, I found that similar issue had already been filed in [vagrant-libvirt repo](https://github.com/vagrant-libvirt/vagrant-libvirt/issues/618).

Most of the responses in the above mentioned issue will work but I finally managed to restore my dev environment by
going through following series of steps:

{% highlight text %}
$ sudo dnf remove vagrant

$ wget https://releases.hashicorp.com/vagrant/1.8.4/vagrant_1.8.4_x86_64.rpm

$ sudo dnf install vagrant_1.8.4_x86_64.rpm

$ vagrant -v
Vagrant 1.8.4
{% endhighlight %}

Bang! I got my `vagrant` back now :relieved:.

But still I was getting above error on performing `$ vagrant plugin list` command.

It was resolved by following steps:

{% highlight text %}
$ vim ~/.vagrant.d/plugins.json
# Try removing each individual plugin or Hash entry
# "fog-libvirt" plugin was cluprit in my case

$ vagrant plugin install vagrant-libvirt
Installing the 'vagrant-libvirt' plugin. This can take a few minutes...
Installed the plugin 'vagrant-libvirt (0.0.33)'!

$ vagrant plugin list
vagrant-libvirt (0.0.33)
{% endhighlight %}

Finally, all issues blocking my development got resolved :smile:.
