#Setting up Puppet Master Slave.
##Setting up Puppet Server. 
Install ntp to synchronize the timing of the servers. 

1. yum install -y ntp
2. systemctl enable ntpd
3. systemctl start ntpd
2. ntpdate pool.ntp.org
3. Go to /etc/ntp.conf edit the file and replace centos.pool.ntp.org to us.pool.ntp.org
4. Reference to [**NTP**](https://www.certdepot.net/rhel7-set-ntp-service/) service on **Centos and RHEL**
 
