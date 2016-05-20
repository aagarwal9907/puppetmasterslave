#Setting up Puppet Master Slave.
##Setting up Puppet Server. 
Install ntp to synchronize the timing of the servers. 

1. `yum install -y ntp`
2. `systemctl enable ntpd`
3. `systemctl start ntpd`
2. `ntpdate pool.ntp.org`
3. Go to /etc/ntp.conf edit the file and replace centos.pool.ntp.org to us.pool.ntp.org
4. Reference to [**NTP**](https://www.certdepot.net/rhel7-set-ntp-service/) service on **Centos and RHEL**
 
## Installing puppet master
 `rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm`
Install puppetpackage server
`yum -y install puppetserver`
## Configurinng puppet server configuration of memory
By Default the puppet server is configured to take 2 gb memory. we can edit the server configuration to enable it to run with either more or less memory depends upon amount of fixed memory or no of agents that this server has to manage. 

`/etc/sysconfig/puppetserver` this connfiguration file 
