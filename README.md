# Vagrant Gopher

A Vagrantfile for running Go VMs (Linux, BSD and Solaris). While cross-compilation works great in Go, having actual VMs can be useful to run tests or debug programs under other operating systems.

### Requirements

[Vagrant][], [VirtualBox][] and a [Go workspace][workspace].

On a Mac, you can use [Homebrew Cask](https://caskroom.github.io/) to install:

```
brew cask install virtualbox vagrant
```

### Usage

Copy this [Vagrantfile][] to $GOPATH/src/Vagrantfile:

```bash
src:~$ curl -O https://raw.githubusercontent.com/nathany/vagrant-gopher/master/Vagrantfile
```

Then run `vagrant up` from any subfolder. 

This will mount your `src` folder as a shared folder inside the VMs, while creating new bin/pkg folders to avoid collisions (particularly bin).

Use `vagrant ssh linux`, `vagrant ssh bsd`, or `vagrant ssh solaris` to login and look around, or run a single command like `vagrant ssh linux -c 'go version'`. 

You will likely need to change directories. [CDPATH][] is configured for GitHub to save a few keystrokes, e.g. 

```bash
vagrant ssh linux -c 'cd fsnotify/fsnotify; go test ./... --race'
```

Use `vagrant halt` to shutdown or `vagrant destroy` to free up disk space.

See the [Vagrant Command Line documentation][cli] for details.

### Notes

* Currently this shares the src/ folder into `$HOME/src` on the virtual machine (`$HOME/go/src` [did not work](https://github.com/mitchellh/vagrant/issues/2257)).
* It would be nice to have a wrapper/plugin around [the ssh command](https://github.com/mitchellh/vagrant/tree/master/plugins/commands/ssh) that runs commands based on the current folder.
* Shared folders [aren't compatible](https://twitter.com/mitchellh/status/376408213203062784) with tools that watch files inside the VM using fsnotify.
* 64-bit boxes are used to support the Go race detector.
* The BSD box does not support Windows hosts.

[Vagrant]: https://www.vagrantup.com/
[VirtualBox]: https://www.virtualbox.org/
[cli]: https://docs.vagrantup.com/v2/cli/index.html
[workspace]: https://golang.org/doc/code.html
[Vagrantfile]: https://raw.github.com/nathany/vagrant-gopher/master/Vagrantfile
[CDPATH]: http://theunixtoolbox.com/cdpath/
