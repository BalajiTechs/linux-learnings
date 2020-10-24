

=========================================================>
Setting up network in multiuser installation of centos 7
1. Type “nmcli d” command in your terminal for quick list ethernet card installed on your machine:
2. Type “nmtui” command in your terminal to open Network manager. After opening Network manager chose “Edit connection” and press Enter (Use TAB button for choosing options).


================Virtual Box===================================>


Virtual Machine troubleshooting

1. Not able to create session ?? ---> try to start VirtualBox as administrator.
2. while installing virtual-machine guest OS in centos, if you get "buidling main madule [failed]" then first update kernel module and reboot.
to do so execute below command.
yum install kernel*
reboot
remove the inserted virtualOSGuest cd and then insert again


User below instructions to defrag vmdk storage of virtual box
http://www.netreliant.com/news/8/17/Compacting-VirtualBox-Disk-Images-Linux-Guests.html

before installing virtualbox guest addition on centos 7
yum -y install epel-release
yum -y install dkms
yum -y groupinstall "Development Tools"
yum -y install kernel-devel
yum -y install gcc make 


Torubleshooting

conflicting virtualboxguest edition cause problem.
conflicting Extension cause problem

==============389 DS ==============================>
running dirsrv and ds-admin  at startup
for this we need to enable dirsrv service.
execute below command as root

systemctl enable dirsrv-admin.service
systemctl enable dirsrv.target

systemctl start dirsrv@localhost.service
systemctl restart dirsrv-admin

LDAP commands

add user 
ldapadd -d 7 -x -D "cn=Directory Manager" -W  -h localhost -f newUser.ldif

Note : content of newUser.ldif is 

dn: uid=sitapaliwal@example.in,ou=site,ou=example,ou=People,dc=site,dc=example,dc=in
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetorgperson
givenName: sita
cn: sitaPaliwal
sn: paliwal
title: strongest mom of the world
mail: sitapaliwal@example.in
uid: sitapaliwal@example.in

Add userpassword as below
ldapmodify -d 7 -D "cn=Directory Manager" -W -h localhost -f mod.ldif

Content of mod.ldif file
dn: uid=sitapaliwal@example.in,ou=site,ou=example,ou=People,dc=site,dc=example,dc=in
changetype: modify
add: userPassword
userPassword: sitapaliwal
	Note : To change the password use replace: userPassword

 ldapsearch -x -h localhost -b "dc=cloud,dc=domain,dc=in"
 
-->> Remove 389-ds competely
remove-ds-admin.pl -y -f
yum remove 389-ds-base-libs 389-adminutil idm-console-framework -y   
rm -rf /etc/dirsrv /usr/lib*/dirsrv /var/*/dirsrv /etc/sysconfig/dirsrv*
-----------------
if SELinux creating problem for a port execute below command to see if the port is present in allowed list
semanage port -l | grep 9830

To install 389-DS 
yum install epel-release
yum install 389-ds 

you need an ip to setup ldap admin server so follow below steps
add entry in /etc/hosts providing your IP and FQDN. 
	If FQDN not available then you can put hostname.localdomain  against IP

While connecting to Windows 389-ds client when only IP is known then add the FQDN and IP set in system32/driver/etc/hosts
Doing above will let you connect to the 389-ds server where domain name server is not present
=========================================================>

==================PIDGIN ==============================>

================Linux Commands================================>

Searcing for a word in all file( use -ril option to list file name only)
grep -ri  <word> *

find -iname  "message*"

To see netowrk routing for ping and ssh 
mtr - network diagnostic tool

To start centos in server mode (command line only) execute below command as root user
systemctl set-default multi-user.target

FirewallD commands
Add IP ranges in trusted zone
firewall-cmd --add-source=192.168.9.1/24 --zone=trusted;firewall-cmd --reload

Change the zone of IPs if already present in a zone
firewall-cmd --change-source=192.168.9.0/24 --zone=public;firewall-cmd --reload
firewall-cmd --add-port=8080/tcp --permanent;firewall-cmd --reload


Command for minimal setup of centos 7

ip n 
ip addr
ip r ( to see route details)
yum install net-tools( to install netstat)

http://www.tecmint.com/things-to-do-after-minimal-rhel-centos-7-installation/5/


ADD Route to 10.208.22.14
ip route add 10.208.22.1 dev eno16777728


find out if the process running( all users)
ps aux | grep -i resourcemanager

#Find all directory in current folder where the commnad is fired
find -type d 

move all files with mactching pattern to current directory

Find files older than x days(only look in specified folder as depth is 1)
	find /tmp/ -maxdepth 1 -type f -mtime +x 
Find folder older that x days
	find /tmp/ -maxdepth 1 -type d -mtime +x 

Delete all files older than x days
	find /tmp/ -maxdepth 1 -type f -mtime +x -delete
	
find .  -iname "*.java" -type f  -newer start_date -exec cp {} /tmp/cid \;

Creating SUDO user on centos/redhat/fedora
adduser exampleuser -g wheel
=================Eclipse =============================>

Ecllipse plugins for smart developement

http://www.modelgoon.org/update ( used for model view and flow detection)

==========================================================>
mail server setup outlook

add account/manual configuration
username -> any
account type : IMAP
incoming: imap.example.in
outgoing : smtp.example.in

username => you ihrms user name
password=<ihrms password>

more setting/outgoing server/  check checkbox "my outgoing server require authentication

---> next --> ok

DONE
===========================================================>
hosts file in windows location C:\Windows\System32\Drivers\etc\hosts
===========================================================>

List entries in keystore file
keytool -list -v -keystore cfskeystore.jks
===========================================================>
SVN Commands
svn status -u   ---> check status of local files from repo

Adding new file/folder to svn
mkdir <folder>
cp <file> <folder>
svn add <folder>
svn add <folder>/<file>
svn commit -m <message>


===========================================================================>
			VIM editor tips

To specify vim property make a file in home folder with name .vimrc and add all reqired properties in it

for eg. to set default color scheme to desert copy below line in ~/.vimrc file(create if not exist)

:color desert

Spell check in VIM
setlocal spell spelllang=en_us


vimdiff commands

]c :        - next difference
[c :        - previous difference
do          - diff obtain
dp          - diff put
zo          - open folded text
zc          - close folded text
:diffupdate - re-scan the files for differences

Delete all lines not containing 16082017
:v/\v16082017/d  

ctrl + r to redo, u to undo
============================================================================================
To add startup bat script in windows follow below steps
win+R, and type shell:startup

copy the link of script in startup folder

============================================================================================
Mysql Tricks

To access remote mysql db on Cloud Machine. 
grant all privileges on *.* to root@'192.168.8.0/255.255.255.0' identified by '<groupPass>';

Grant MySQL user permission access from outside IP
$ mysql -u root -p
Enter password: #put your password here

mysql> grant all on *.* to 'root'@'%' IDENTIFIED BY 'securepassword';
 
 
Note: If above do not work then use this 
"GRANT ALL PRIVILEGES ON logicaldoc.* TO 'logicaldoc'@'10.208.21.0/255.255.255.0' IDENTIFIED BY 'securepassword' with grant option;"
Also use hostname as login


And then Open port 3306
firewall-cmd --add-port=3306/tcp --permanent;firewall-cmd --reload
Access the DB using heidiSQL with username = root password =securepassword
	
Clearing Binary files
The binary log contains all statements that update data or potentially could have updated it. To clear these binary log files execute below command
mysql -u root -p 'MyPassword' -e "PURGE BINARY LOGS BEFORE '2008-12-15 10:06:06';"

Also check http://www.cyberciti.biz/faq/what-is-mysql-binary-log/ site for more information

JOIN of table oid and commonmetadata to get all OID sorted by date(recievedat)
select b.objectid from index.commonmetadata a, exampleindex.oid b  where a.recordid=b.recordid order by a.recievedat
============================================================================================
Download java using wget 

wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.rpm

============================================================================================

Moving home partition

https://help.ubuntu.com/community/Partitioning/Home/Moving


Find the uuid of the Partition
	first mount empty partition(on which home will be mounted)
	sudo blkid
Setup Fstab
	sudo cp /etc/fstab /etc/fstab.$(date +%Y-%m-%d)
	cmp /etc/fstab /etc/fstab.$(date +%Y-%m-%d)
	sudo gedit /etc/fstab 
and add these lines into it 
	# (identifier)  (location, eg sda5)   (format, eg ext3 or ext4)      (some settings) 
	UUID=<uid obtained from blkid>  /media/home    ext3          defaults       0       2 

	sudo mkdir /media/home
	sudo mount -a
Copy /home to the New Partition
	sudo rsync -aXS --exclude='/*/.gvfs' /home/. /media/home/.
Check Copying Worked
	sudo diff -r /home /media/home -x ".gvfs/*"

Preparing fstab for the switch
	sudo gedit /etc/fstab
and now edit the lines you added earlier, changing the "/media/home" part to simply say "/home" so that it looks like this:
	# (identifier)  (location, eg sda5)   (format, eg ext3 or ext4)      (some settings) 
	UUID=????????   /home    ext3          defaults       0       2
Moving /home into /old_home
	cd / && sudo mv /home /old_home && sudo mkdir /home
Reboot or Remount all
	sudo mount -a
	
Troubleshooting
If you receive an error like 'The volume may already be mounted', use the following command to unmount the drive first before re-doing the last step again. (note the "n" should be missing from the command, making it "umount")
	sudo umount /media/home/
Then try mounting again
	sudo mount -a
Deleting the old Home
	cd /
	sudo rm -rI /old_home

===================================================================================================
Extending root partition by reducing home partition size
backup the contents of /home

> tar -czvf /root/home.tgz -C /home .

• test the backup

> tar -tvf /root/home.tgz

• unmount home

> umount /dev/mapper/centos-home

• remove the home logical volume

> lvremove /dev/mapper/centos-home

• recreate a new 400GB logical volume for /home, format and mount it

> lvcreate -L 400GB -n home centos
> mkfs.xfs /dev/centos/home
> mount /dev/mapper/centos-home

• extend your /root volume with ALL of the remaining space and resize (-r) the file system while doing so

> lvextend -r -l +100%FREE /dev/mapper/centos-root

• restore your backup

> tar -xzvf /root/home.tgz -C /home

• check /etc/fstab for any mapping of /home volume. IF it is using UUID you should update the UUID portion. (Since we created a new volume, UUID has changed)

============================================== Clean root space ===================================
Remove tmp files in /tmp folder
Remove old kernel rpms
	rpm -q kernel
	rpm -ev <old kernel>
		OR
	package-cleanup --oldkernels --count=1
	
Move /home directory to other storage

Clean /var/log/messages
clean core files

yum clean all

 du -sk * | sort -nr | head -10
 du -sh /*
 
 https://rbgeek.wordpress.com/2013/01/27/how-to-extend-the-root-partition-in-lvm/
 
  xfs_growfs  /dev/centos/root
  
  
Clean mysql binlogs
	If below folder consuming much space then you need to clean some mysql space
	du -sk /var/lib/mysql | sort -nr | head -10

Check  if binlog files are consuming space in /var/lib/mysql folder. If Yes then execute below command to purge logs older than 3 days
Attention Only execute below command if you are not using replication (see  http://www.cyberciti.biz/faq/what-is-mysql-binary-log/ site for more information)
mysql -u root -p 'MyPassword' -e "PURGE BINARY LOGS BEFORE '2008-12-15 10:06:06';"


========================================Install VNC server ===================================

Below are the steps to enable vnc-server. These steps need to be perfomed in GUI mode

yum install tigervnc-server
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service

edit vncserver@:1.service file
vi /etc/systemd/system/vncserver@:1.service

Replace the string <USER> with appropriate vncuser’s username. 

firewall-cmd --permanent --zone=public --add-service vnc-server; firewall-cmd --reload

su - <USER>
vncserver


Now make the service enabled on after every reboot with root credentials:

su -
systemctl daemon-reload

systemctl enable vncserver@:1.service

reboot
systemctl start vncserver@:1.service

Source : https://www.howtoforge.com/vnc-server-installation-on-centos-7
==============================================================================================

Start java in debug mode 
java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005 -jar <jar name>
==============================================================================================

Excel Tricks


CTRL+L ==> create table from sheet data
Convert all text to Lower
=LOWER(C8&".cloud.domain.in")
==============================================================================================
To get RabbitVCS under Thunar working in CentOS 7 x86_64

--------------------
Open terminal as root user and run following commands:

1. yum install thunar gtk-doc Thunar-devel
2. yum install meld pysvn python-dulwich python-simplejson subversion
3. rpm -ivh rabbitvcs-core-0.14.2.1-5.el7.centos.noarch.rpm rabbitvcs-cli-0.14.2.1-5.el7.centos.noarch.rpm thunarx-python-0.2.3-5.el7.centos.x86_64.rpm rabbitvcs-thunar-0.14.2.1-5.el7.centos.x86_64.rpm
--------------------
Updated steps for support of nautilus
1. rpm -e rabbitvcs-core rabbitvcs-cli thunarx-python rabbitvcs-thunar
2. cd Updated-for-nautilus
3. yum install *.rpm
4. Restart the system and check context menu for support

==============================================================================================
To indent xmlfile on linux
xmllint --format --recover foo.xml
==============================================================================================
Hadoop installation site reference

http://tecadmin.net/setup-hadoop-2-4-single-node-cluster-on-linux/#

Hadoop Namenode HA Steps
https://www.edureka.co/blog/how-to-set-up-hadoop-cluster-with-hdfs-high-availability/
==============================================================================================

Perl References
 
Installing perlcritic
yum install perl-Perl-Critic

==============================================================================================
Search for all MIME files accuring filedescriptor

ls -lart /proc/51985/fd/ |grep MIME| awk '{print $9}' > 1
==============================================================================================
Changing BASH shell tag
export PS1="[\u@localhost \W]\$ "

==============================================================================================
Eclipse problem
	1. Problem. Cannot install debug point as No line info found: 
Solution Please check if the deployed war is debug enabled or not. 
==============================================================================================
Time Synchronization in Linux

ntpdate comes preinstalled as ntp client. add your ntp server name in /etc/ntp/

using NTP
	command to see if ntp is synchronized
		ntpstat
	Command to see which server are configure as ntp server 
		ntpq -p
	See if NTPD is running
		systemctl status ntpd.service
		
Using chronyd
	Checking chrony Tracking
		chronyc tracking
	Checking chrony Sources
		chronyc sources
		note: ^ means ntp server and ? mean not connecting (^* mean connected and synched)
	 
		chronyc sourcestats
		
Sources: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-Checking_the_Status_of_NTP.html
		https://docs.fedoraproject.org/en-US/Fedora/18/html/System_Administrators_Guide/sect-Checking_if_chrony_is_synchronized.html
==============================================================================================
Screen Command
Help 
	ctrl+a ?

Detach a screen and go back to real console.
	“Ctrl-A” and “d“

Re-attach the screen: execute below command on shell 
	screen -r
If mulpiple deatched screens are runiing then check ID and execute below command
	screen -r <id>
	Create a screen with given name and enable window logging
screen -s -L remove_orphan_files
	
Using Multiple Screen
“Ctrl-A” and “c“    -----> create new screen
“Ctrl-A” and “n“	-----> Next Screen
“Ctrl-A” and “p“	-----> Previous Screen
“Ctrl-A” and  " 	-----> All screen window

Lock Screen
ctrl+a and x


“Ctrl-A” and “K” to kill the screen.

==============================================================================================
-----------Reinstalling broken printer driver--------------------

remove the printer from device and printers(only printers not working). 
restart print spoolers  services(open services.msc-> printer spooler)
delete entries of printer from "print management->all_printers" and "print management->all_drivers"
Install new printer drivers
==============================================================================================
==============================================================================================
Restrict CPU usage of a process in linux

https://www.ostechnix.com/how-to-limit-cpu-usage-of-a-process-in-linux/
==============================================================================================
See open ports on linux without netstat
lsof -i | grep 9830
cat /etc/services | grep 834
nmap -sT -O localhost

Get list of all tcp connection
lsof -i TCP

Get list of all tcp connection to remote 8080 port
lsof -i TCP:8080

==============================================================================================
To create a new partition on Linux
    Start fdisk 
    /sbin/fdisk /dev/sdb

    where /dev/sdb stands for the drive that you want to partition.
    In fdisk, to create a new partition, type the following command:

    n
        When prompted to specify the Partition type, type p to create a primary partition or e to create an extended one. There may be up to four primary partitions. If you want to create more than four partitions, make the last partition extended, and it will be a container for other logical partitions.
        When prompted for the Number, in most cases, type 3 because a typical Linux virtual machine has two partitions by default.
        When prompted for the Start cylinder, type a starting cylinder number or press Return to use the first cylinder available.
        When prompted for the Last cylinder, press Return to allocate all the available space or specify the size of a new partition in cylinders if you do not want to use all the available space.

    By default, fdisk creates a partition with a System ID of 83. If you're unsure of the partition's System ID, use the
    l

    command to check it.
    Use the
    w
    command to write the changes to the partition table.
    
    When restarted, create a file system on the new partition. We recommend that you use the same file system as on the other partitions. In most cases it will be either the Ext3 or ReiserFS file system. For example, to create the Ext3 file system, enter the following command:

    /sbin/mkfs -t ext3 /dev/hda3
    Create a directory that will be a mount point for the new partition. For example, to name it data, enter:

    mkdir /data
    Mount the new partition to the directory you have just created by using the following command:

    mount /dev/hda3 /data
    Make changes in your static file system information by editing the /etc/fstab file in any of the available text editors. For example, add the following string to this file:

    /dev/hda3 /data ext3 defaults 0 0

    In this string /dev/hda3 is the partition you have just created, /data is a mount point for the new partition, Ext3 is the file type of the new partition. For the exact meaning of other items in this string, consult the Linux documentation for the mount and fstab commands.
    Save the /etc/fstab file.
==============================================================================================
top -b -H -u root -n 1 | wc -l   //Check no of threads active
yum install gparted-0.19.1-6.el7.x86_64.rpm --downloadonly --downloaddir=./  // Download package and its dependecies 
==============================================================================================
Configuring Key Based Login
# su hadoop 
$ ssh-keygen -t rsa 
$ ssh-copy-id -i ~/.ssh/id_rsa.pub tutorialspoint@hadoop-master 
$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop_tp1@hadoop-slave-1 
$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop_tp2@hadoop-slave-2 
$ chmod 0600 ~/.ssh/authorized_keys 
$ exit

==============================================================================================
Find all java process on all servers(consucutive IPs)
for i in {1..3}; do ssh 192.168.8.15$i "hostname;jps;echo -e '\n'";done

#Execute a command on multiple hosts sequentially
for i in {node1,node2,node3}; do ssh $i "hostname ";done
for i in {1..3}; do ssh hadoop-node$i "echo "Hostname : `hostname`";zkServer.sh start;echo -e '\n'";done
==============================================================================================
HA using Pacemaker and Corosync
Node 1
	yum install pcs
	systemctl enable pcsd.service corosync pacemaker
	systemctl start pcsd.service
	
	Installation creates user hacluster so need to reset password 
	passwd hacluster
	
Node 2
	yum install pcs 
	systemctl enable pcsd.service corosync pacemaker
	systemctl start pcsd.service
	
Installation creates user hacluster so need to reset password
	passwd hacluster
	
Add Constraint to start both resorces on same machine
	pcs constraint colocation add VirtualIP LDS_INSTANCE


After installation pacemaker any one node
	pcs cluster auth node1 node2

Note: If auth fails then check if password for the hacluster is same on all nodes. Also check if nodes are connected. Check firewall not blocking 2224 port. Check pcsd is running on all nodes. Try using FQDN instead of just hostname.

Execute below command on any node to setup cluster
	pcs cluster setup --name mycluster node1 node2 

	pcs cluster enable --all	


See Full configration of pcs
	pcs cluster cib


Add a Resource IP
pcs resource create ClusterIP ocf:heartbeat:IPaddr2 ip=<any Free IP on network> cidr_netmask=32 op monitor interval=30s

Reference
http://clusterlabs.org/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/ch03.html

===========================================================================================
Remove password from certificate key
openssl rsa -in /root/all_certs/private/cakey.pem -out /root/all_certs/private/cakey_no_pass.pem
===========================================================================================
			Logical Volume Manager(LVM)
			
Steps to create lv, pv, vg

1. create a partition(use fdisk /dev/<disk> -----> n ---> p ---> enter --->enter --> w  ---- Done). This will create a disk partition(fdisk -l to check partition created) for e.g. /dev/sda1 .
2. Create Physical Volume(PV) --> pvcreate /dev/<disk Partition>
3. Create Volume Group(VG) ---> vgcreate <volume group name> /dev/<disk partition1> /dev/<disk partition2> ....
4. Create Logical Volume in above VG ---> lvcreate -n <name of logical volume>  -L <size> <volume group name> (for e.g. lvcreate -n hadoop_lv -l 100%FREE example_vg)
5. Format the new volume --> mkfs.ext4 /dev/<vg name>/<lv name>
6. Mount the LV -> mount /dev/<vg name>/<lv name> /path/to/mount/


Note: While extending the logical volume do 'resize2fs /dev/<vg>/<lv>' to gain the extended space.


Extending the partition
 569  fdisk /dev/sda
  570  reboot
  571  ls
  572  fdisk -l
  573  fdisk /dev/sda
  574  df -h
  575  fdisk -l
  576  df -h
  577  pvs
  578  pvcreate /dev/sda2
  579  vgextend /dev/example_vg /dev/sda2
  580  lvextend /dev/example_vg/hadoop_lv /dev/sda2
  581  df -h
  583  resize2fs /dev/example_vg/hadoop_lv
  584  df -h
  585  vgs
  586  lvextend /dev/example_vg/hadoop_lv /dev/sda1
  587  resize2fs /dev/example_vg/hadoop_lv
 
 
 
 Extending root(centos-root) volume
  692  fdisk /dev/sdb
  695  pvcreate /dev/sdb1
  698  vgextend /dev/centos /dev/sdb1
  699  lvextend /dev/centos/root /dev/sdb1
  703  xfs_growfs /dev/centos/root
  704  df -h
 
 
 
 Add SWAP from lvm
  To add a swap volume group (assuming /dev/VolGroup00/LogVol02 is the swap volume you want to add):

    Create the LVM2 logical volume of size 256 MB:

    lvm lvcreate VolGroup00 -n LogVol02 -L 256M

  Format the new swap space:

    mkswap /dev/VolGroup00/LogVol02

  Add the following entry to the /etc/fstab file:

    /dev/VolGroup00/LogVol02   swap     swap    defaults     0 0

  Enable the extended logical volume:

    swapon -va

  Test that the logical volume has been extended properly:

    cat /proc/swaps
    free
 
 ===========================================================================================
 
 Too many open files problem
 check limit using below command
	uname -a

for Dynamically changing the limit 
	ulimit -n 8192
	The updated value can then be printed
		ulimit -n

	check with uname -a
for permanent setting
	To set this, as root edit /etc/security/limits.conf

		and add a line such as

		*               soft    nofile            8192

		
=============================================================================================
Adding disk without reboot linux(SCSI only)

  171  df -ih
  172  ls
  173  jps
  174  ls /sys/class/scsi_device/
  175  echo 1 > /sys/class/scsi_device/0\:0\:0\:0/device/rescan
  176  fdisk -l
  177  ls /sys/class/scsi_device/
  178  echo 1 > /sys/class/scsi_device/2\:0\:0\:0/device/rescan
  179  fdisk -l
  180  fdisk /dev/sda
  181  fdisk
  182  fdisk -l
  183  partprobe
  184  pvcreate /dev/sda2
  185  vgs
  186  vgextend /dev/sda2
  187  vgextend hadoop_vg /dev/sda2
  188  lvextend hadoop_lv /dev/sda2
  189  lvextend /dev/hadoop_vg/hadoop_lv /dev/sda2
  190  lvs
  191  vgs
  192  pvs
  193  resize2fs /dev/hadoop_vg/hadoop_lv

  
  
  https://access.redhat.com/documentation/en-US/Red_Hat_Directory_Server/8.2/html/Administration_Guide/Managing_SSL.html
  
  =================================
Copy all images to external hard-drive

# ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory

Search all jpg images in the system and archive it.

# find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz



find . -name *.java | grep -v test | xargs grep "dfs.datanode.handler.count"


=================================================================================================
Clean old kernels

package-cleanup --oldkernels --count=1

=================================================================================================
add user to a group in linux
usermod -u example -g wheel
=================================================================================================
Installing COCKPIT to manage multiple linux servers
Execute below commands on all server you want to manage
yum -y install cockpit
systemctl enable --now cockpit.socket
firewall-cmd --add-service=cockpit
firewall-cmd --add-service=cockpit --permanent

After Cockpit installed successfully, you can access it using a web browser at the following locations.

https://ip-address:9090

=================================================================================================
Useful Equinox OSGi commands
ss (Ex: osgi> ss)
This command can be used to list all the existing bundles in the OSGi environment. Bundle ID, State and Bundle symbolic name of all the bundles are shown.

start <bundle-id> (Ex: osgi> start 16)
This command can be used to start a particular bundle. If it can’t be started, reasons are printed on the console.

stop <bundle-id> (Ex: osgi> stop 16)
This command can be used to stop a particular bundle. If it can’t be stopped, reasons are printed on the console.

b <bundle-id> (Ex: osgi> b 16)
This can be used to print all the meta data related to this bundle. That includes imported packages, exported packages, host bundle, required bundles etc.

diag <bundle-id> (Ex: osgi> diag 16)
This can be used to diagnose a particular bundle. It will show you the list of missing imported packages.

headers <bundle-id> (Ex: osgi> headers 16)
This can be used to list the headers for a particular bundle.

packages <package-name> (Ex: osgi> packages org.wso2.foo)
This can be used to list all the bundles which use the given package.

install file:<file-path> (Ex: osgi> install file:/home/temp/bundle1.jar)
This can be used to install a bundle into running OSGi environment. After installing, use ‘start’ command to activate the bundle.

uninstall <bundle-id> (Ex: osgi> uninstall 16)
This can be used to remove a bundle from the OSGi environment.

active (Ex: osgi> active)
This can be used to list all active bundles in the current instance.

refresh (Ex: osgi> refresh)
This can be used to refresh the system. All the package resolutions are refreshed when this is executed. Always refresh the system after uninstalling a bundle as a best practice.
Note : <bundle-symbolic-name> also can be used instead of <bundle-id> in above commands which uses <bundle-id> as a parameter.
=================================================================================================
CPU Details on centos
cat /proc/cpuinfo
lscpu

=================================================================================================
upgrading from centos 7.3 to 7.4 with epel enabled
Step 1. Try "yum update". 
Fix 1.  If fails due to libgpod then first do "yum downgrade libgpod" then do "yum update"
Fix 2. If failing due to 'ipa' then do "yum remove freeipa" then do "yum update"
=================================================================================================
Create /home Partition manually
Pre-Requisite/Assumption -> you have space left in centos volume group or there is some other physicial volume that you can use to create HOME volume. 

Assuming that centos volume group(vgs will show the remaining space) have 8GB of free space
1. lvcreate /dev/centos/home -L 8G
2. mkfs-ext4 /dev/centos/home
3. add entry in /etc/fstab as below
/dev/mapper/centos-home /home                   ext4     defaults        0 0
4. execute "mount -a"
5. If using SELinux policy, Execute below command to label the /home correctly
 restorecon -R -v /home
=================================================================================================
Superputty Shortcuts
	(User defined)
	Duplicate : ctrl+shift+d
	Next tab : ctrl + right

	CTRL+SHIFT+F8 Toggle command mask mode
	F3 Display or Hide the Menu Bar
	CTRL+S Save current window layout to the active layout

	F2 Open SuperPuTTY Options Dialog
	F11 Switch To/From Fullscreen mode
=================================================================================================
Testing Slowloris attack
	
Install slowhttptest
	yum install slowhttptest
Execute the command against the server(4000 connections)	
	slowhttptest -c 4000 -H -g -o apache_no_mitigation -i 10 -r 200 -t GET -u https://example.com -x 24 -p 3 -l 120
=================================================================================================
Replace all "home/example" to "home/exampleuser" in file exampleclient.properties

 sed -i 's/home\/example\//home\/exampleuser\//g' config.ini
 
================================================================================================= 
Copy equinox_osgiserver folder and exclude listener/* 
 rsync -av equinox_osgiserver --exclude 'equinox_osgiserver/listener/*' example-node1:/home/exampleuser/example_HOME/FileListener/
================================================================================================= 
 Setting hostname on systemd linux env
 hostnamectl set-hostname <hostname>
=================================================================================================
Accessing Hadoop as different user
Allowing other users to add file to hdfs 
 hdfs dfs -mkdir /<user>
 hdfs dfs -chown -R /<user>
 
 
 Now the <user> can add the files in the directory 
 
 Source : http://hadoopinrealworld.com/fixing-org-apache-hadoop-security-accesscontrolexception-permission-denied/
================================================================================================= 
Vagrant commands
#install on centos 7
curl https://releases.hashicorp.com/vagrant/2.1.5/vagrant_2.1.5_x86_64.rpm >> vagrant.rpm
rpm -ivh vagrant.rpm
-------------------------------------------------------------------------------------------------
#Installing ovirt-provider
# Vagrant 1.8+ is must(https://www.vagrantup.com/downloads.html)
# references https://github.com/myoung34/vagrant-ovirt4
# $ yum -y install gcc libcurl-devel libxml2-devel redhat-rpm-config ruby ruby-devel rubygems rubygems-devel
# $ vagrant plugin install vagrant-ovirt4
# $ vagrant up --provider=ovirt4


Vagrant.configure("2") do |config|
  config.vm.box = 'ovirt4'
  config.vm.hostname = "vagrant-test"
  config.vm.box_url = 'https://github.com/myoung34/vagrant-ovirt4/blob/master/example_box/dummy.box?raw=true'

  config.vm.provider :ovirt4 do |ovirt|
    ovirt.url = 'https://ovirt-mgmt.mig.in/ovirt-engine/api'
    ovirt.username = "admin@internal"
    ovirt.password = "sirus177"
    ovirt.insecure = true
    ovirt.debug = true
    ovirt.cluster = 'Apollo'
    ovirt.template = 'vagrant-centos7'
    ovirt.console = 'spice'
    ovirt.memory_size = '1 GiB' #see https://github.com/dominikh/filesize for usage
    ovirt.memory_guaranteed = '256 MB' #see https://github.com/dominikh/filesize for usage
  end
end
-------------------------------------------------------------------------------------------------

	vagrant status
	vagrant up
	vagrant ssh <hostname>

Ansible commands
	ansible --version
	ansible web1 -m ping
	ssh-keyscan web1
	ssh-keyscan lb web1 web2 >> .ssh/known_hosts
	cat .ssh/known_hosts
	ansible all -m ping --ask-pass
	ansible all -m ping
	
established network connections, between our management node, and client machines.
	 ps x
	 netstat -nap |grep EST
	 netstat -ap |grep EST
	 
	ansible-playbook e45-ssh-addkey.yml --ask-pass
	ansible-playbook e45-ssh-addkey.yml

	ansible web1 -m apt -a "name=ntp state=installed" --sudo
	ansible web1 -m copy -a "src=/home/vagrant/files/ntp.conf dest=/etc/ntp.conf mode=644 owner=root group=root" --sudo
	ansible web1 -m service -a "name=ntp state=restarted"ansible all -m shell -a "uptime"
	ansible all -m shell -a "uptime"
	
	
Note: Ansible playbook indent is 2 space. 	
================================================================================================= 
JIRA Commandline command to get userful info.	(first change directory to C:\atlassian-cli-7.8.0)
	jira.bat --action getServerInfo
	
	
Rest Command	
curl -D- -u username:password -X GET -H "Content-Type: application/json" http://jira-host:6565/jira/rest/api/2.0.alpha1/user?username=username	
	
================================================================================================= 	
	
	
Disable IPv6 using sysctl settings (no reboot required)

To verify if IPv6 is enabled or not, execute :
	# ifconfig -a | grep inet6
	
Append below lines in /etc/sysctl.conf:
	net.ipv6.conf.all.disable_ipv6 = 1
	net.ipv6.conf.default.disable_ipv6 = 1
	
To make the settings affective, execute :
	sysctl -p
	
============================= Nginx Setup ===================================================== 	
yum install epel-release
yum install nginx
cd /etc/nginx/
vim nginx.conf
mkdir /etc/ssl/private
chmod 700 /etc/ssl/private
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
vim nginx.conf
systemctl restart nginx
vim /opt/C-DAC/jetty9.1.0.M0/example-jetty.xml
systemctl restart nginx
 systemctl restart example_viewer
systemctl restart nginx

On system from where/ to where you connect to this nginx peer, copy nginx-selfsigned.crt in $JAVA_HOME/jre/lib/security and add to trusted cacerts using below command
keytool -import -alias nginxca -keystore cacerts -file nginx-selfsigned.crt

Check if a module is installed in nginx 
2>&1 nginx -V | tr -- - '\n' | grep _module
================================================================================================= 

disable selinux temoprarity
setenforce 0
=================================================================================================

Linux renew ip command

dhclient -r  //release the current lease
dhclient     //get the new IPs

=================================================================================================
YUM command cheatcode

Get status of updates with severty levels
yum updateinfo summary

To upgrade packages that have security errata
yum --security update



To show a list of all BZs that are fixed for packages you have installed
yum updateinfo list bugzillas


To check updates only
yum check-update

=================================================================================================
Zypper Cheat Sheet

Listing Needed Patches
zypper list-patches or zypper lp

Applying Patches
zypper patch

Checking Patches
zypper patch-check or zypper pchk

Listing All Patches
zypper patches

Getting Information About Patches
zypper patch-info
zypper info -t patch


Packages Updates
zypper list-updates or zypper lu
zypper update or zypper up


Downgrade a package in suse
https://stackoverflow.com/questions/25995813/zypper-update-package-to-the-previous-version-not-the-last


zypper pa -ir SLE-Module-Public-Cloud12-Updates
zypper info cloud-init
zypper in cloud-init=17.1-37.9.6
zypper in --oldpackage cloud-init=17.1-37.9.6
zypper patch-info package:cloud-init
zypper list-patches
=================================================================================================
apt-get cheatcode

Check available version of software for downgrade purpose
sudo apt-cache showpkg vim

install specific version of VIM(2:7.4.1689-3ubuntu1) including dependecies
sudo apt-get install vim=2:7.4.1689-3ubuntu1 vim-common=2:7.4.1689-3ubuntu1 vim-runtime=2:7.4.1689-3ubuntu1

===========================================================================================
Setting teamviewer remotely
On remote machine ssh and install teamviewer 13.
------------- On 64-bit Systems ------------- 
wget https://download.teamviewer.com/download/linux/teamviewer.x86_64.rpm
yum install teamviewer.x86_64.rpm

Set your password first:

	teamviewer --passwd newPassword

And the run

	teamviewer -info

It will show you the TeamViewer ID

Login as TeamViewerID and password as newPassword
===========================================================================================
Check crons running as cron.daily  tasks
run-parts --test /etc/cron.daily

===========================================================================================
Configuring NFS Shared Storage on the oVirt Node
 Procedure

Create the shared directories.

# mkdir -p /export/nfs/data /export/nfs/iso

Adjust the file permissions.

# chown vdsm:kvm /export/nfs/data /export/nfs/iso

Create the NFS export file.

Edit the file /etc/exports with the following content:

# cat /etc/exports
/export/nfs/data *(rw,no_subtree_check,insecure,no_root_squash)
/export/nfs/iso *(rw,no_subtree_check,insecure,no_root_squash)

Set the appropriate permissions to the shared directories:

# chmod 0755 /export/nfs/data/ 
# chmod 0755 /export/nfs/iso/

The directory owner, you, have permission to read, write, and execute, which is designated with the number 7. Other users can read and execute but not write, which is designated with the number 5.
Enable the export directories as active NFS mount points.

# exportfs -a
# systemctl enable nfs-server
# systemctl start nfs-server
# showmount -e localhost

The shared directories are ready to be used as data domains for the data center.

===========================================================================================
Create a user on ovirt-engine

ovirt-aaa-jdbc-tool user add dhirendrap --attribute=firstName=dhirendra --attribute=lastName=pratap
ovirt-aaa-jdbc-tool user password-reset dhirendrap

===========================================================================================
Download a package without wget 
curl http://mirror.centos.org/centos/7/os/x86_64/Packages/telnet-0.17-64.el7.x86_64.rpm > telnet.rpm

===========================================================================================
Network interface issue on ubuntu
Sympotms
	1. `ifconfig` doesn't show any iterface other than lo
	2. `ip addr` shows eth interface but it is down
	3. `ifup eth0` shown the interface does not exists
	4. `ifconfig -a` shows an interface that is down

Solution: the issue may have occured because interface name changed. This can occur when you move your machine from one cloud to other cloud-init
check "/etc/network/interfaces" file and update the name of interface as shown in `ip addr` command
check 

===========================================================================================
Check your initiator name
vi /etc/iscsi/initiatorname.iscsi

Check existing connections
iscsiadm -m session

Discover targets
iscsiadm -m discovery -t st -p 192.168.12.20

Login 
iscsiadm -m node -T iqn.2016-02.local.itzgeek.server:disk1 -p 192.168.12.20 -l

Logout
iscsiadm -m node -T iqn.2016-02.local.itzgeek.server:disk1 -p 192.168.12.20 -u

===========================================================================================
Install Docker on centos
sudo yum update
yum install yum-utils device-mapper-persistent-data lvm2
 yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 yum install docker-ce
 systemctl start docker
 systemctl status docker
  systemctl enable docker
  docker -v
  
Note: If Machine is under a proxy then add following configurations
$ cat /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://192.168.8.5:8080"

 systemctl daemon-reload
 systemctl restart docker

  #install docker for ansible-playable
  docker run -p 80:8080 mmumshad/ansible-playable
  
To persist data across docker container restarts use the below command to map volumes from docker container on the docker host.

  docker run -p 80:8080 -v /data/db:/data/db -v /opt/ansible-projects:/opt/ansible-projects mmumshad/ansible-playable

firewall-cmd --add-service=http --permanent;firewall-cmd --reload 
 
Access http://<hostname>:80  username=admin@example.com pass=admin



docker image rm 0f80a3691d18



==============================================================================

cloud-init install
yum install cloud-init


install ovirt-guest Using YUM via terminal to install the oVirt Guest Tools
	sudo yum install centos-release-ovirt42
	sudo yum install ovirt-guest-agent-common

Starting the service
	sudo systemctl enable --now ovirt-guest-agent.service

==============================================================================	
#cloud-config
bootcmd:
    - echo 'APT::Periodic::Enable "0";' > /etc/apt/apt.conf.d/10cloudinit-disable
    - apt-get -y purge update-notifier-common ubuntu-release-upgrader-core landscape-common unattended-upgrades

==============================================================================	
GREP OR condition
grep 'pattern1\|pattern2' filename