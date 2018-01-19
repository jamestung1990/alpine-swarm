# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 sts=2 :

# if edge is needed:
#cat /etc/apk/repositories | sed -e 's/\\/v.*\\//\\/edge\\//g' > /etc/apk/repositories-new
#mv /etc/apk/repositories-new /etc/apk/repositories

$alpinestorage = <<SCRIPT
echo I am provisioning...
apk --update --no-cache upgrade
apk add --update --no-cache nfs-utils
sed -e 's/^\\(ttyS0.*\\)/#\\1/' -i /etc/inittab
rc-update add nfs default
mkdir -p /srv/storage
echo '/srv/storage 192.168.250.0/24(rw,async,no_subtree_check)' >> /etc/exports
SCRIPT

$alpinescript = <<SCRIPT
echo I am provisioning...
tail -n1 /etc/apk/repositories | sed -e 's/main/community/' >> /etc/apk/repositories
apk --update --no-cache upgrade
apk add --update --no-cache docker nfs-utils
sed -e 's/^\\(ttyS0.*\\)/#\\1/' -i /etc/inittab
sed 's/^\\(docker.*\\)/\\1vagrant/' -i /etc/group
rc-update add docker default
rc-update add nfsclient default
rc-update add nfsmount default
mkdir -p /srv/storage
echo '192.168.250.21:/srv/storage /srv/storage nfs defaults,rw 0 0' >> /etc/fstab
SCRIPT

Vagrant.configure("2") do |config|

  (1..1).each do |a|
    config.vm.define "storage-#{a}" do |storage|
      storage.vm.box = "maier/alpine-3.7-x86_64"
      storage.vm.synced_folder '.', '/vagrant', disabled: true
      storage.vm.hostname = "storage-#{a}"
      storage.vm.network :private_network, ip: "192.168.250.2#{a}"
      storage.vm.provision "shell", inline: $alpinestorage
    end
  end

  (1..2).each do |b|
    config.vm.define "manager-#{b}" do |manager|
      manager.vm.box = "maier/alpine-3.7-x86_64"
      manager.vm.synced_folder '.', '/vagrant', disabled: true
      manager.vm.hostname = "manager-#{b}"
      manager.vm.network :private_network, ip: "192.168.250.3#{b}"
      manager.vm.provision "shell", inline: $alpinescript
    end
  end

  (1..2).each do |c|
    config.vm.define "node-#{c}" do |node|
      node.vm.box = "maier/alpine-3.7-x86_64"
      node.vm.synced_folder '.', '/vagrant', disabled: true
      node.vm.hostname = "node-#{c}"
      node.vm.network :private_network, ip: "192.168.250.4#{c}"
      node.vm.provision "shell", inline: $alpinescript
    end
  end

end
