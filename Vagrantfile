# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    # cache stuff
    if Vagrant.has_plugin?("vagrant-cachier")
        config.cache.scope = :box
    end

    config.vm.box = "chef/ubuntu-14.04"
    config.vm.network :forwarded_port, guest: 8080, host: 8080, auto_correct: true
    config.vm.network :forwarded_port, guest: 27017, host: 27017, auto_correct: true

    # allow the VM to see host's USB devices
    config.vm.provider :virtualbox do |vb|
        vb.customize ['modifyvm', :id, '--usb', 'on']
        vb.customize ["usbfilter", "add", "0",
            "--target", :id,
            "--name", "Samsung Devices",
            "--vendorid", "04E8"]
    end

    config.vm.provision :chef_solo do |chef|

        chef.log_level = :info
        chef.json = {
            :java => {
                :install_flavor => "oracle",
                :jdk_version => "7",
                :oracle => {
                    :accept_oracle_download_terms => true
                    }
                },
            "android-sdk" => {
                :version => "24.3.3",
                :checksum => "a05023aaf149d40a0a848ad49d5c6b43ec730443efb89d1dfa584a132a642bdf",
                :components => ["platform-tools", "build-tools-22.0.0", "android-22"]
                },
            :nodejs => {
                :install_method => "source",
                :version => "0.12.7",
                :source => {
                    :checksum => "b23d64df051c9c969b0c583f802d5d71de342e53067127a5061415be7e12f39d"
                    },
                :npm_packages => [
                    { :name => "phonegap" },
                    { :name => "cordova" },
                    { :name => "ionic" }
                    ]
                }
            }

        chef.add_recipe "nodejs"
        chef.add_recipe "mongodb"
        chef.add_recipe "android-sdk"

    end
end
