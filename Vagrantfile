# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure('2') do |config|
  config.vm.box      = 'ubuntu/trusty64'
  config.vm.hostname = 'rails-dev-box'

  # Ruby
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  # Postgres
  config.vm.network :forwarded_port, guest: 5432, host: 5432
  # MySql
  config.vm.network :forwarded_port, guest: 3306, host: 3306
  # Redis
  config.vm.network :forwarded_port, guest: 6379, host: 6379

  # rsync
  config.vm.synced_folder '.', '/vagrant', type: 'rsync'

  # bootstrap
  config.vm.provision :shell, path: 'bootstrap.sh', keep_color: true
end
