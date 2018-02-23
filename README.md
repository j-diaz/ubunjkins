


# Guide for installing Jenkins in Ubuntu

### Install Openjdk8 (Do not use OracleJDK)
```bash
sudo apt-get update
sudo apt-get install openjdk-8-jrk
```
###  Verify java installation 
```bash
 java -version
 ```
*Output*:
```bash
root@ubuntu-s-1vcpu-1gb-nyc1-jenkins:~# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-8u151-b12-0ubuntu0.16.04.2-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```
###  Install Jenkins
Before you can install Jenkins you need to add the jenkins repository
```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
```
Afterwards, add repository https://pkg.jenkins.io/debian binary/ as an entry in `/etc/apt/sources.list` file.
```bash
vim /etc/apt/sources.list
SHIFT+G
$
i
ENTER
ENTER
#Jenkins Packages
deb deb https://pkg.jenkins.io/debian binary/
:wq
```
The entry should look similar to this block.
```bash
...
deb-src http://security.ubuntu.com/ubuntu xenial-security multiverse

## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu xenial partner
# deb-src http://archive.canonical.com/ubuntu xenial partner

# Jenkins Packages
deb https://pkg.jenkins.io/debian binary/

```
Now you can proceed to running the install command

```bash
sudo apt-get install jenkins
```
*Should automatically start it. if not run the command below to check whats up.*
```bash
systemctl status jenkins.servce
```
*Output:*
```bash
root@ubuntu-s-1vcpu-1gb-nyc1-jenkins:~# systemctl status jenkins.service
● jenkins.service - LSB: Start Jenkins at boot time
   Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
   Active: active (exited) since Fri 2018-02-23 04:13:25 UTC; 32min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 20109 ExecStart=/etc/init.d/jenkins start (code=exited, status=0/SUCCESS)

Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins systemd[1]: Starting LSB: Start Jenkins at boot time...
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins jenkins[20109]: Found JAVA_VERISON=18
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins jenkins[20109]: Correct java version found
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins jenkins[20109]:  * Starting Jenkins Automation Server jenkins
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins su[20142]: Successful su for jenkins by root
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins su[20142]: + ??? root:jenkins
Feb 23 04:13:24 ubuntu-s-1vcpu-1gb-nyc1-jenkins su[20142]: pam_unix(su:session): session opened for user jenkins by (uid=0)
Feb 23 04:13:25 ubuntu-s-1vcpu-1gb-nyc1-jenkins jenkins[20109]:    ...done.
Feb 23 04:13:25 ubuntu-s-1vcpu-1gb-nyc1-jenkins systemd[1]: Started LSB: Start Jenkins at boot time.
```
### Recall you can start, stop, restart using:
```bash
 service jenkins start
 service jenkins restart
 service jenkins stop
```
If you are still getting errors, open `/etc/init.d/jenkins` and verify the `$PATH` variable points to the value listed by `whereis java` command. You may run into issues if you used the OracleJDK instead of OpenJDK. You may need to run: ` systemctl daemon-reload` after you fix your changes.

### Paranoid process check
If you want to be absolutely sure its running and the above was not enough evidence. You can always run:
```bash
ps -ef | grep java
```
*Output:*
```bash
jenkins  20155     1  0 04:13 ?        00:00:00 /usr/bin/daemon --name=jenkins --inherit --env=JENKINS_HOME=/var/lib/jenkins --output=/var/log/jenkins/jenkins.log --pidfile=/var/run/jenkins/jenkins.pid -- /usr/bin/java -Djava.awt.headless=true -jar /usr/share/jenkins/jenkins.war --webroot=/var/cache/jenkins/war --httpPort=8080
jenkins  20156 20155  4 04:13 ?        00:02:03 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/jenkins/jenkins.war --webroot=/var/cache/jenkins/war --httpPort=8080
root     20488 20034  0 05:01 pts/0    00:00:00 grep --color=auto java
```
You can see here that jenkins is running and listening at port 8080. Visit `http://<server-ip-address>:8080` and jenkins should prompt for initial install.


Enjoy!