Vagrant.configure(2) do |config|
  config.vm.box = "genebean/centos6-puppet-64bit"
  config.vm.network "forwarded_port", guest: 80,  host: 8080
  config.vm.network "forwarded_port", guest: 443, host: 8443
  config.vm.provision "shell", inline: <<-SHELL1
    rpm -Uvh http://yum.spacewalkproject.org/2.4/RHEL/6/x86_64/spacewalk-repo-2.4-3.el6.noarch.rpm
    rpm --import http://yum.spacewalkproject.org/RPM-GPG-KEY-spacewalk-2015
    cat << EOL >/etc/yum.repos.d/jpackage-generic.repo
[jpackage-generic]
name=JPackage generic
mirrorlist=http://www.jpackage.org/mirrorlist.php?dist=generic&type=free&release=5.0
enabled=1
gpgcheck=1
gpgkey=http://www.jpackage.org/jpackage.asc
EOL
  
  rpm --import http://www.jpackage.org/jpackage.asc
  yum install -y spacewalk-setup-postgresql spacewalk-postgresql

  cat << EOL >/tmp/answers
admin-email = root@localhost
ssl-set-org = Spacewalk Org
ssl-set-org-unit = spacewalk
ssl-set-city = My City
ssl-set-state = My State
ssl-set-country = US
ssl-password = spacewalk
ssl-set-email = root@localhost
ssl-config-sslvhost = Y
db-backend=postgresql
db-name=spaceschema
db-user=spaceuser
db-password=spacepw
db-host=localhost
db-port=5432
enable-tftp=Y
EOL

  spacewalk-setup --disconnected --answer-file=/tmp/answers
  yum -y install spacewalk-utils
  SHELL1

  config.vm.provision "shell", inline: <<-SHELL2
    echo "Go to https://localhost:8443/rhn/ and create an account using 'admin' & 'password' for its credentials"
    echo "After doing first login run the following command to add a large list of repos and channels:"
    echo "/usr/bin/spacewalk-common-channels -u admin -p password -k unlimited '*' -v"
  SHELL2
end

