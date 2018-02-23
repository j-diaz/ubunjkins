
# Guide for installing Jenkins in Ubuntu

1. Install openjdk8 (Do not use OracleJDK)
```bash
sudo apt-get install openjdk-8-jrk
```
2. Verify java installation
```bash
java -version
```
Should yield something similar to:
3. Install Jenkins
```bash
sudo apt-get install jenkins
```
Should automatically start it. if not run:
```bash
systemctl status jenkins.servce
````
Output:
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
Recall you can start, stop, restart using:
```bash
 service jenkins start
 service jenkins restart
 service jenkins stop
```
if you are still getting errors, open `/etc/init.d/jenkins` and verify the ```$PATH`` variable points to the value listed by `whereis java` command. You may run into issues if you used the OracleJDK instead of OpenJDK.






