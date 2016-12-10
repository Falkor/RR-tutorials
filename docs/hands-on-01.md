
# Reproduce Slides Building in Vagrant

The objective of this first exercise is to familiarize you with [Vagrant](https://www.vagrantup.com) and see how it can be used to bundle an environment out of a recipe.

In this context, we will:

1. grab the sources of the slides
2. try to _reproduce_ an environment that permits to **build the slides** used for this tutorial **within a Vagrant Box**, knowing that this relies on a _complex set of software_: [LaTeX](https://www.latex-project.org/), [Beamer](http://www.ctan.org/pkg/beamer), [Biber](http://biblatex-biber.sourceforge.net/), [Pandoc](http://pandoc.org/) etc.)
2. create a _recipe_ that would be used to _provision_ an empty box automatically
3. commit and repackage the modified box for further sharing on [Vagrant Cloud](https://vagrantcloud.com/)

## Step 0: Pre-requisites

You **should** have finalized the setup steps before going further -- see [setup instructions](/setup/).

## Step 1: Grab the sources

Clone the [tutorial repository](https://github.com/Falkor/RR-tutorials) from a Terminal (Powershell as `administrator` under windows  to ensure the proper symbolic links managements within Vagrant upon deployment):

~~~bash
$> git clone https://github.com/Falkor/RR-tutorials.git
$> cd RR-tutorials
$> make setup    # Under Mac OS / Linux
~~~

The `make setup` step permits to grab the [Git submodules of this repository](https://git-scm.com/book/en/v2/Git-Tools-Submodules). If for some reason, you cannot run this step on your system, initiate the git submodules as follow:

~~~bash
$> git submodule init
$> git submodule update
~~~

## Step 2: Pulling and running an existing Vagrant box

In this case, you should have a `Vagrantfile` at the root of the cloned repository.
Take a look at it

~~~bash
$> cat Vagrantfile
~~~

To run (and pull eventually the latest version of) the configured Vagrant box from [Vagrant Cloud](http://vagrantcloud.com)

~~~bash
$> vagrant up
~~~

When you run this command:

* The base box(es) (configured in the `Vagrantfile`) is downloaded and stored locally in `~/.vagrant.d/boxes/`
* A new Virtual Machine VM is created and configured with the base box as template
* The VM is booted and (eventually) provisioned to install every dependency that your project needs, and to set up any networking or synced folders, so you can continue working from the comfort of your own machine.

After running the above command, you will have a fully running VM in VirtualBox running Ubuntu  14.04 LTS (Trusty Tahr) 64-bit.
You can now SSH into this machine with:

~~~bash
$> vagrant ssh
~~~

This command will drop you into a full-fledged SSH session. Go ahead and interact with the machine as follows:

~~~bash
(vagrant)$> whoami                       # current user (should be 'vagrant')
(vagrant)$> lsb_release -a               # current OS details
(vagrant)$> ls -l /vagrant/              # what do you notice?
(vagrant)$> figlet                       # should fail
The program 'figlet' can be found in the following packages:
(vagrant)$> sudo apt-get install figlet  # install a missing package package
(vagrant)$> figlet 'I like this Tutorial'
~~~

`/!\ IMPORTANT:` Although it may be tempting, be careful about `rm -rf /`, since Vagrant shares a directory `/vagrant/` with the directory on the host containing your `Vagrantfile`, and this can delete all those files.

The SSH session can be terminated (as usual) with `CTRL+D` or the `logout` command.

~~~bash
(vagrant)$> logout
Connection to 127.0.0.1 closed.
~~~

Now that you're back on your machine, check the status of the box:

~~~bash
$> vagrant status
Current machine states:

default                   running (virtualbox)
~~~

No perform a __graceful__ shutdown of the running Vagrant bax  and check back the status

~~~bash
$> vagrant halt
$> vagrant status
~~~

At this level, we have made some changes to the initial box (_i.e._ installing the `figlet` package).
We can check that they are still effective and recall the last run command (with `up arrow`):

~~~bash
$> vagrant up
$> vagrant ssh
# Now press the 'up' arrow to get the last run command, or 'CTRL-R' to search for a pattern ('fig' for instance)
(vagrant)$> figlet I like this Tutorial
(vagrant)$> logout
~~~

Now imagine that in your (reproducible) attempts, you messed up the VM, you can terminate it with

~~~bash
$> vagrant destroy
~~~

This command stops the running machine Vagrant is managing and destroys all resources that were created during the machine creation process. After running this command, your computer should be left at a clean state, as if you never created the guest machine in the first place.

Check that the last changes we performed (_i.e._ installing the `figlet` package) do not exist anymore:

~~~bash
$> vagrant up
$> vagrant ssh
(vagrant)$> figlet
The program 'figlet' can be found in the following packages:
...
~~~

So you can always rollback to a stable configuration by destroying (with `vagrant destroy`) a messed up VM.


## Step 3: A first attempt to reproduce the slides

So let's configure the Vagrant box to make it able to compile the slides used in this tutorial!

Make a first try -- it __should fail__:

~~~bash
$> vagrant up
$> vagrant ssh
$> cd /vagrant/slides/2016/cloudcom2016/src
$> make
~~~

So you will need to install the prerequisite software environment necessary to build these slides

## Step 3: Install the necessary software environment

**Hint** (from the author _i.e._ me):

> I use LaTeX Beamer with Pandoc




I use an [un]common mix here: `make`, `latex-beamer`, `biber`, `pandoc`.
