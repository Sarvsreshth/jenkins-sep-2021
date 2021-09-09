### Building CentOS custom image to create centos ansible nodes
```
cd ~/jenkins-sep-2021
git pull
cd Day4/centos-ansible
cp ~/.ssh/id_rsa.pub authorized_keys
docker build -t tetktutor/ansible-centos-node .
```

### List and check the CentOS custom image
```
docker images
```
The expected output is
<pre>
[jegan@localhost Day4]$ <b>docker images</b>
REPOSITORY                                       TAG       IMAGE ID       CREATED          SIZE
<b>
tektutor/ansible-centos-node                     latest    56491a566f81   10 minutes ago   259MB
tektutor/ansible-ubuntu-node                     latest    e6a97031776d   20 hours ago     220MB
</b>
tektutor/spring-ms                               1.0       d05e6c1660df   45 hours ago     487MB
releases-docker.jfrog.io/jfrog/artifactory-oss   latest    60c5d817a8d1   3 days ago       980MB
ubuntu                                           16.04     b6f507652425   9 days ago       135MB
hello-world                                      latest    d1165f221234   6 months ago     13.3kB
centos                                           8         300e315adb2f   9 months ago     209MB
openjdk                                          12        e1e07dfba89c   2 years ago      470MB
</pre>

### Create centos1 and centos2 containers
```
cd ~
docker run -d --name centos1 --hostname centos1 -p 2003:22 -p 8003:80 tektutor/ansible-centos-node
docker run -d --name centos2 --hostname centos2 -p 2004:22 -p 8004:80 tektutor/ansible-centos-node
```
### List and check the centos containers
```
docker ps --filter="name=ubuntu*|centos*"
```
The expected output is
<pre>
[jegan@localhost Day4]$ <b>docker ps --filter="name=ubuntu*|centos*"</b>
CONTAINER ID   IMAGE                                 COMMAND               CREATED          STATUS          PORTS     <b>                                                                     NAMES
fc30df7adc40   tektutor/ansible-centos-node:latest   "/usr/sbin/sshd -D"   9 minutes ago    Up 9 minutes    0.0.0.0:2004->22/tcp, :::2004->22/tcp, 0.0.0.0:8004->80/tcp, :::8004->80/tcp   centos2
94309427724d   tektutor/ansible-centos-node:latest   "/usr/sbin/sshd -D"   10 minutes ago   Up 10 minutes   0.0.0.0:2003->22/tcp, :::2003->22/tcp, 0.0.0.0:8003->80/tcp, :::8003->80/tcp   centos1
</b>
260aaad1df35   tektutor/ansible-ubuntu-node          "/usr/sbin/sshd -D"   19 hours ago     Up 23 minutes   0.0.0.0:2002->22/tcp, :::2002->22/tcp, 0.0.0.0:8002->80/tcp, :::8002->80/tcp   ubuntu2
6b516c130ba4   tektutor/ansible-ubuntu-node          "/usr/sbin/sshd -D"   19 hours ago     Up 23 minutes   0.0.0.0:2001->22/tcp, :::2001->22/tcp, 0.0.0.0:8001->80/tcp, :::8001->80/tcp   ubuntu1

</pre>

### Ansible ping on centos and ubuntu ansible nodes
```
cd ~/jenkins-sep-2021
git pull
cd Day4
ansible all -m ping
```
