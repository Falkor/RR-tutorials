VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box      = 'ubuntu/trusty64'
  config.vm.hostname = 'vagrant.RR-tutorial'
  config.ssh.insert_key = false
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update --fix-missing
    sudo apt-get upgrade
    sudo apt-get -yq install git make latex-beamer pandoc biber texlive-fonts-recommended texlive-latex-extra 	texlive-science 	latex-make
  SHELL
end
