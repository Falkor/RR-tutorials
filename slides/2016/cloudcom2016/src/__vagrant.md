
###  What is [Vagrant](http://vagrantup.com/) ?

\hfill\myurl{http://vagrantup.com/}

\includegraphics[width=\textwidth]{screenshot_vagrant}

### What is [Vagrant](http://vagrantup.com/) ?

\wbegin{}

\centering Create and configure _lightweight_, _reproducible_, and _portable_ development environments

\wend

* Command line tool
* Automates VM creation with
    - VirtualBox
    - VMWare etc.
* Integrates well with configuration management tools
    - Shell
	- Puppet etc.

####

\centering
Runs on Linux, Windows, MacOS


### Why use Vagrant?

* Create new VMs quickly and easily: only one command!

\command{vagrant up}

* Keep the number of VMs under control
     - All configuration in `VagrantFile`
* Reproducability
     - Identical environment in development and production
* Portability
     - _avoid_ sharing 4 GB VM disks images
	 - [Vagrant Cloud](https://vagrantcloud.com/) to share your images
* Collaboration made easy:
\begin{cmdline}
\cmdlineentry{git clone ...}\\
\cmdlineentry{vagrant up}\\
\end{cmdline}

### Installation Notes: Mac OS

* Best done using [Homebrew](http://brew.sh/) and [Cask](http://sourabhbajaj.com/mac-setup/Homebrew/Cask.html)

~~~bash
$> brew install caskroom/cask/brew-cask
$> brew cask install virtualbox    # install virtualbox
$> brew cask install vagrant
$> brew cask install vagrant-manager # see http://vagrantmanager.com/
~~~

\hfill{}\includegraphics[height=0.2\textheight]{screenshot_vagrant_manager.png}\ \includegraphics[height=0.2\textheight]{screenshot_vagrant_manager2.png}


### Installation Notes: Windows / Linux

\bbegin{}

* Install [Oracle Virtualbox](https://www.virtualbox.org/)
* Go on the [Download Page](http://www.vagrantup.com/downloads)
     - select the appropriate OS, in 64 bits versions

\bend

<!--
> _Tip [Windows]:_ quickly open a command prompt to your project:
>    `shift` + right-clicking the project folder, then "`open command window here`".
-->

* _Notes for windows users_:
    - you will also need both [PuTTY and PuTTYGen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
    - Vagrant boxes are located in `%userprofile%/.vagrant.d/boxes`
    - To configure the appropriate Putty profile:
	     * run _`vagrant ssh-config`_ to collect IP and port (after `vagrant up`)
		 * load `%userprofile%/.vagrant.d/insecure_public_key`
		 * Use `Save Public Key`to convert the OpenSSH key to PPK format
		 * Create the PuttY profile accordingly (username: `vagrant`)



<!--

### Definition

* _Host_: Host operating system (Windows, Mac, RHEL),
* _Guest_: Guest operating system aka the VM
      - Ubuntu, Debian, CentOS...
* _Provider_: Provider of virtual machines or containers used
      - [VirtualBox](https://www.virtualbox.org/) is upstream default
* _Provisioner_: Configuration management system to set up (provision) your machine
	- Puppet etc.
* _Vagrant box_: Package format for the images
* _`Vagrantfile`_:  project configuration and provisioning

-->

### Minimal default setup

\command{vagrant init [-m] <user>/<name>\hfill\textit{\# setup vagrant cloud image}}

* A `Vagrantfile` is configured

. . .

\command{vagrant up \hfill\textit{\# boot the box(es) set in the Vagrantfile}}

* The base box is downloaded and stored locally
    - in `~/.vagrant.d/boxes/`
* A new VM is created and configured with the base box as template
* The VM is booted and (eventually) provisioned

. . .

\command{vagrant ssh \hfill\textit{\# connect inside it}}

### Find a vagrant box

- [Vagrant Cloud](https://vagrantcloud.com/) \hfill\myurl{https://vagrantcloud.com/}
- [VagrantBox.es](https://vagrantcloud.com/) \hfill\myurl{http://www.vagrantbox.es/}

. . .

\toyou

~~~bash
$> vagrant init ubuntu/trusty64  # Ubuntu Server 14.04 LTS
$> vagrant up
$> vagrant ssh
~~~

\vspace*{-2em}
\cbegin{0.5\textwidth}

\tiny

| Box name                     | Description               |
|------------------------------|---------------------------|
| `ubuntu/trusty64`            | Ubuntu Server 14.04 LTS   |
| `centos/7`                   | CentOS Linux 7 x86_64     |
| `debian/jessie64`            | Vanilla Debian 8 "Jessie" |
| `jhcook/osx-elcapitan-10.11` | OS X 10.11 El Capitan     |
|                              |                           |

\column{0.5\textwidth}

* Once within the box:
     - `/vagrant`: root directory hosting `Vagrantfile`




\cend




### Configuring Vagrant

* Minimal `Vagrantfile` (Ruby syntax)

~~~ruby
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
   config.vm.box = 'svarrette/centos-7-puppet'
   config.ssh.insert_key = false
end
~~~

* Configure Multiple box within the **same** `Vagrantfile`
     - See [`ULHPC/puppet-sysadmins/Vagrantfile`](https://github.com/ULHPC/puppet-sysadmins/blob/devel/Vagrantfile)



### Vagrant Box Status / Stop

\command{vagrant status \hfill\textit{\# State of the vagrant box(es)}}

. . .

\command{vagrant \{ destroy | halt \} \hfill\textit{\# destroy / halt}}

* Once you have finished your work within a \textit{running} box
     - save the state for later with `vagrant halt`
	 - reset changes / tests / errors with `vagrant destroy`
	 - commit changes by generating a new version of the box

<!--

~~~bash
$> vagrant destroy
$> vagrant up
$> vagrant ssh
~~~

-->

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

### Vagrant Box Customization

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
