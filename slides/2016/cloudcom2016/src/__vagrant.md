
### What is [Vagrant](http://vagrantup.com/) ?

\wbegin{}

\centering Create and configure _lightweight_, _reproducible_, and _portable_ development environments

\wend

* _Command line_ tool \hfill{}`vagrant [...]`
* Easy and Automatic per-project VM management
    - Supports many hypervisors: [VirtualBox](https://www.virtualbox.org/), [VMWare](http://www.vmware.com/)...
    - Easy text-based configuration (Ruby syntax) \hfill{}`Vagrantfile`
* Supports _provisioning_ through configuration management tools
    - Shell
	- [Puppet](https://puppet.com/)    \hfill\myurl{https://puppet.com/}
    - [Salt](https://saltstack.com/)...\hfill\myurl{https://saltstack.com/}

\exbegin{}\centering

_Cross-platform_: runs on Linux, Windows, MacOS

\exend


### Installation Notes

\begin{textblock}{0.3}(0.67,0.14)
  \imgw{0.5}{screenshot_vagrant_manager.png}
  \imgw{0.5}{screenshot_vagrant_manager2.png}
\end{textblock}

\myurl{http://rr-tutorials.readthedocs.io/en/latest/setup/}

* _Mac OS X_:
    - best done using [Homebrew](http://brew.sh/) and [Cask](http://sourabhbajaj.com/mac-setup/Homebrew/Cask.html)

~~~bash
$> brew install caskroom/cask/brew-cask
$> brew cask install virtualbox    # install virtualbox
$> brew cask install vagrant
$> brew cask install vagrant-manager # cf http://vagrantmanager.com/
~~~


* _Windows / Linux_:
    - install [Oracle Virtualbox](https://www.virtualbox.org/) and the Extension Pack
    - install [Vagrant](https://www.vagrantup.com/downloads.html)


### Why use Vagrant?

* Create new VMs quickly and easily: only one command!
     - `vagrant up`
* Keep the number of VMs under control
     - All configuration in `VagrantFile`
* _Reproducability_
     - Identical environment in development and production
* _Portability_
     - _avoid_ sharing 4 GB VM disks images
	 - [Vagrant Cloud](https://vagrantcloud.com/) to share your images
* _Collaboration made easy_:
\begin{cmdline}
\cmdlineentry{git clone ...}\\
\cmdlineentry{vagrant up}\\
\end{cmdline}

### Minimal default setup

\command{vagrant init [-m] <user>/<name>\hfill\textit{\# setup vagrant cloud image}}

* A `Vagrantfile` is configured for box `<user>/<name>`
     - Find existing box: [Vagrant Cloud](https://vagrantcloud.com/) \hfill\myurl{https://vagrantcloud.com/}
     - You can have multiple (named) box within the **same** `Vagrantfile`
           * See [`ULHPC/puppet-sysadmins/Vagrantfile`](https://github.com/ULHPC/puppet-sysadmins/blob/devel/Vagrantfile)

\cbegin{0.57\textwidth}

~~~ruby
Vagrant.configure(2) do |config|
   config.vm.box = 'ubuntu/trusty64'
   config.ssh.insert_key = false
end
~~~

\column{0.4\textwidth}\tiny

| Box name                  | Description               |
|---------------------------|---------------------------|
| `ubuntu/trusty64`         | Ubuntu Server 14.04 LTS   |
| `centos/7`                | CentOS Linux 7 x86_64     |
| `debian/contrib-jessie64` | Vanilla Debian 8 "Jessie" |

\cend





### Pulling and Running a Vagrant Box

\command{vagrant up \hfill\textit{\# boot the box(es) set in the Vagrantfile}}

* Base box is downloaded and stored locally \hfill{}`~/.vagrant.d/boxes/`
* A new VM is created and configured with the base box as template
     - The VM is booted and (eventually) provisioned
     -  Once within the box: `/vagrant` = directory hosting `Vagrantfile`

. . .

\command{vagrant status \hfill\textit{\# State of the vagrant box(es)}}

. . .

\command{vagrant ssh \hfill\textit{\# connect inside it, CTRL-D to exit}}



### Stopping Vagrant Box

\command{vagrant \{ destroy | halt \} \hfill\textit{\# destroy / halt}}

* Once you have finished your work within a \textit{running} box
     - save the state for later with `vagrant halt`
	 - reset changes / tests / errors with `vagrant destroy`
	 - commit changes by generating a new version of the box


### Back to Hands-on 1

\wbegin{Your Turn! \hfill\myurl{http://rr-tutorials.readthedocs.io/en/latest/hands-on-01/}}

* **\alert{Steps [1-4]}** to cover the following elements:
    - _Basic Usage of Vagrant_
    - _Build these Slides_
           * find the prerequisite software environment \hfill{}`apt-get`
           * [un]common mix here: `make`, `latex-beamer`, `biber`, `pandoc`...

\wend

<!-- - design a provisioning script -->
<!-- - commit / package the updated box (for incremental share) -->

### Vagrant Box \just{2-}{Inline }Provisioning

* Now you have \textit{hopefully} a working _documented procedure_
     - it's time to _bundle it_ for provisioning the box upon boot
     - key for sustainable reproducible environment

* Simple case: **\alert{inline}** provisioning _i.e._ list commands to run

. . .

~~~ruby
config.vm.provision "shell", inline: <<-SHELL
  sudo apt-get update --fix-missing
  sudo apt-get upgrade
  sudo apt-get --yes --quiet install make latex-beamer biber \
       texlive-latex-extra texlive-fonts-recommended texlive-science \
       latex-make pandoc
SHELL
~~~

. . .

\command{vagrant provision \hfill\textit{\# test your provisioning config}}


### Back to Hands-on 1

\wbegin{Your Turn! \hfill\myurl{http://rr-tutorials.readthedocs.io/en/latest/hands-on-01/}}

* **\alert{Steps 5}**:

    - Relative paths, such as above, are expanded relative to the location of the root Vagrantfile for your project.

to cover the following elements:
    - _Basic Usage of Vagrant_
    - _Build these Slides_
           * find the prerequisite software environment \hfill{}`apt-get`
           * [un]common mix here: `make`, `latex-beamer`, `biber`, `pandoc`...

\wend



### Vagrant Box Customization

     - Shell/Python/Ruby script that list the bootstrapping commands
* _Obj_: customize / specialize the configuration of a _running_ box
* This can be done in two ways:
     1. use **provisionning** within the `Vagrantfile` (using puppet etc.)
	 2. re-package the box via `vagrant package`

~~~ruby
# (1) Vagrantfile with Puppet provisioning
Vagrant.configure(2) do |config|
  config.vm.box = 'svarrette/centos-7-puppet'
  config.vm.provision :puppet  do |puppet|
    puppet.hiera_config_path = 'hieradata/hiera.yaml'
    puppet.working_directory = '/vagrant'
    puppet.manifests_path    = "manifests"
    puppet.module_path       = "modules"
    puppet.manifest_file     = "init.pp"
    puppet.options = [ '-v','--report','--show_diff','--pluginsync' ]
  end
end
~~~

### Box Re-packaging (1/2)

* \alert{WARNING}: ensure you **DO NOT** reset the [(insecure) SSH key](https://github.com/mitchellh/vagrant/tree/master/keys)
     - **before** `vagrant up`, use the following `Vagrantfile` configuration:

            config.ssh.insert_key = false

* Zero out the free space to save space -- run the following script:

~~~bash
$> dd if=/dev/zero of=/EMPTY bs=1M
$> rm -f /EMPTY
~~~

* Ensure Virtualbox Guest additions match using the [vbguest](https://github.com/dotless-de/vagrant-vbguest) plugin

~~~bash
$> vagrant plugin install vagrant-vbguest
$> vagrant vbguest --status
GuestAdditions versions on your host (5.0.4) & guest (4.3.26) mismatch
# Upgrade the GuestAdditions
$> vagrant vbguest --do install --auto-reboot
~~~

### Box Re-packaging (2/2)

~~~bash
# Locate the internal name of the running VM and repackage it
$> VBoxManage list runningvms
"vagrant-vms_default_1431034026308_70455" {...}
$> vagrant package \
    --base vagrant-vms_default_1431034026308_70455 \
	--output <os>-<version>-<arch>.box
~~~

. . .

* Now you can upload the generated box on [Vagrant Cloud](http://vagrantcloud.com).
    - select '`New version`', enter the new version number
	- add a new box provider (`Virtualbox`)
    - upload the generated box

. . .

* Upon successful upload:  **release** the uploaded box
    - by default it is unreleased
    - Now people using the `<user>/<name>` box will be notified of a pending update


### Vagrant Box Generation

* You might rely on [Falkor/vagrant-vms](https://github.com/Falkor/vagrant-vms)
     - use it at your own risks
	 - based on [packer](http://www.packer.io/) and [veewee](https://github.com/jedi4ever/veewee)

~~~bash
$> git clone https://github.com/Falkor/vagrant-vms.git
$> cd vagrant-vms
$> gem install bundler && bundle install
$> rake setup
~~~

\pause

~~~bash
# initiate a template for a given Operating System:
$> rake packer:{Debian,CentOS,openSUSE,scientificlinux,ubuntu}:init
# Build a Vagrant box
$> rake packer:{Debian,CentOS,openSUSE,scientificlinux,ubuntu}:build
# If things goes fine:
$> vagrant box add packer/<os>-<version>-<arch>/<os>-<version>-<arch>.box
~~~
