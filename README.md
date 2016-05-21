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
## Configurinng puppet server memory
By Default the puppet server is configured to take 2 gb memory. we can edit the server configuration to enable it to run with either more or less memory depends upon amount of fixed memory or no of agents that this server has to manage. 

`/etc/sysconfig/puppetserver` this connfiguration file 

## Starting Puppet Server

`systemctl start puppetserver`
enable the puppet server so that it starts when the master server reboots
`systemctl enable puppetserver`

#Setting up the Agents
The Puppet agent software must be installed on any server that the Puppet master will manage.
`rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm`

installing the puppet-agent
`yum -y install puppet-agent`
Starting Puppet agent
`sudo /opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true`
>At this point the agent will start and it will generate an ssl certificate and send signal to puppet master. The master will be able to sign the agent ssl certificate and then it will be able to communicate with the agent in a secured way. This certificate signing has to be done for each agent we want to associate with this master. 
The puppet-agent certificate request should be generated automatically but it can be simulated using the below command

`puppet agent --server 103bunty --waitforcert 60 --test`

The most important step now is to sign certificate at master server

`/opt/puppetlabs/bin/puppet cert list`

`[root@103bunty ~]# /opt/puppetlabs/bin/puppet cert list
  "101bunty" (SHA256) CB:2D:82:C0:A9:84:52:96:25:33:F8:B4:73:B9:DB:4A:18:C1:17:4C:DF:14:4F:E9:C1:4A:87:AB:C6:44:CD:61`
 
 >The following ouput indicates that an unsigned certificate was received from the client, now the server has to sign this certificate inorder to apply the catalog
 
` # /opt/puppetlabs/bin/puppet cert sign 101bunty
Notice: Signed certificate request for 101bunty
Notice: Removing file Puppet::SSL::CertificateRequest 101bunty at '/etc/puppetlabs/puppet/ssl/ca/requests/101bunty.pem'`

The above step will sign the certificate from the client and now the following output will be received on the client

`Info: Caching certificate for 101bunty
Info: Caching certificate_revocation_list for ca
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Caching catalog for 101bunty
Info: Applying configuration version '1463804736'
Notice: Applied catalog in 0.01 seconds`

## More configuration on the agent node
The Agent node need to be configured to fetch the catalog from the puppet master. For this the following file need to be edited

`/etc/puppetlabs/puppet/puppet.conf`

Add following Lines in it
`[main]
server=103bunty #the puppet server master
report=true
pluginsync=true
`




