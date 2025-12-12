```
ec2 - ubuntu server 24.04 | t3.medium | puppet.pem 

SG - 80, 22, 8140 

sudo apt update -y
sudo apt install -y wget
wget https://apt.puppet.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt update
sudo apt install -y puppet-agent

echo 'export PATH=/opt/puppetlabs/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

puppet --version
  
sudo systemctl start puppet 
sudo systemctl enable puppet 
sudo systemctl status puppet

mkdir -p puppet-apache/{manifests,files}
cd puppet-apache
cd manifests
sudo nano site.pp 

```
```
package { 'apache2':
  ensure => installed,
}

service { 'apache2':
  ensure => running,
  enable => true,
  require => Package['apache2'],
}

file { '/var/www/html/index.html':
  ensure  => file,
  content => "Hello from Puppet on Ubuntu EC2!\n",
  owner   => 'www-data',
  group   => 'www-data',
  mode    => '0644',
  require => Package['apache2'],
}
```
```
cd .. 

cd /home/ubuntu/puppet-apache
sudo /opt/puppetlabs/bin/puppet apply manifests/site.pp
```
