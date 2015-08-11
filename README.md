# ionic-android-devstax

Ionic/Android Development Stack built with Vagrant & Chef

Because Vagrant will build an Ubuntu VM with Chef, only Android development is supported. For iOS, it would have to be on OSX which
would have the tooling necessary for Cordova to build iOS applicaitons. Building an OSX VM with Chef is slightly more complicated.

## Install
- [ChefDK](https://downloads.chef.io/chef-dk/)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- A Git client because Chef may be cloning remote repositories

Verify the ChefDK installation with
```shell
$ chef verify
```
### Vagrant Plugins
The following plugins are required
```shell
$ vagrant plugin install vagrant-berkshelf
$ vagrant plugin install vagrant-omnibus
```
The following plugins are recommended but not required
```shell
$ vagrant plugin install vagrant-hostmanager
$ vagrant plugin install vagrant-vbguest
$ vagrant plugin install vagrant-cachier
```

### Startup the Vagrant Box
Assuming you checked out this repository to /ionic-android-devstax
```shell
$ cd /ionic-android-devstax
$ vagrant up
```

### Connect USB device to VM
In the Vagrant file you'll note
```ruby
    config.vm.provider :virtualbox do |vb|
        vb.customize ['modifyvm', :id, '--usb', 'on']
        vb.customize ["usbfilter", "add", "0",
            "--target", :id,
            "--name", "Samsung Devices",
            "--vendorid", "04E8"]
    end
```
This will expose the USB device to the VM based on the vendor id, which happens to be for Samsung devices.
You may need to change this filter based on your USB device. Use the `VBoxManage list usbhost` command to get furter details on the USB devices attached.
Refer to the [VBoxManage](https://www.virtualbox.org/manual/ch08.html) documentation for the options available for the `usbfilter` sub command.

Once your machine is up and running, use `adb devices` command. Your USB device should be listed. If you see something
like `????????????	no permissions` use the following commands
```
    sudo adb kill-server
    sudo adb start-server
    adb devices
```

### [Getting Started](http://ionicframework.com/getting-started/) with Ionic
```
$ cd /vagrant
$ ionic start myApp tabs
$ cd myApp
$ ionic platform add android
$ ionic build android
$ ionic run android
```
If your Android was connected via USB, and adb saw it, then the last command should of pushed the Ionic app to your phone.
