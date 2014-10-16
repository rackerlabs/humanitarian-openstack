Welcome to the OpenStack Humanitarian workshop!
===============================================

Purpose
-------
The purpose of this tutorial is to deploy a webservice using libcloud and openstack.

Installing pre-requisites
-------------------------

+ Install python 2.7 on Ubuntu (if you are using Mac, Windows, or other flavor of Linux, skip this step):
```
sudo apt-get install build-essentialsudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev
libgdbm-dev libc6-dev libbz2-dev
wget http://python.org/ftp/python/2.7.5/Python-2.7.5.tgz
tar -xvf Python-2.7.5.tgz
cd Python-2.7.5
./configure
make
make install
apt-get install git
```
+ Install pip:
```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```

Running the libcloud code
-------------------------

+ Install and run virtual environment:
```
pip install virtualenv
virtualenv venv
source venv/bin/activate
```
+ Get code for the application from GitHub:
```
git clone https://github.com/rackerlabs/humanitarian-openstack
```
+ Make sure you are in the humanitarian-openstack/participants/code directory:
```
cd humanitarian-openstack/participants/code
```
+ Install requirements:
```
pip install -r requirements.txt
```
+ Create a 'libcloud.conf' file by coyping configuration template and paying attention to the name:
```
cp libcloud.conf.template libcloud.conf
```
+ Use your favorite editor to edit the configuration file. Set the "identity" and "credential" values with the values your workshop assistant gives you. The credential value is an API Key available on the Rackspace Cloud control panel by clicking Account: username > Account Settings, then clicking Show next to API Key.
```
vi libcloud.conf
```
+ Edit case-1.py file. Rename "server-1", "server-2", "db", "haproxy" with your name, for example, "Susan-server1", "Susan-db", "Susan-haproxy".
```
vi case-1.py
```
+ Insert the following two lines under the imports section:
```
import libcloud.security
libcloud.security.VERIFY_SSL_CERT = False
```
+ Finally, execute the code:
```
python case-1.py
```

You see some INFO messages returned to you with timestamps.

Once the script completes, you get a load balancer IP address that you can point your browser to so you can see the app running.


Connecting to launched servers
------------------------------

You can use SSH to connect directly to one of the web servers if you want to investigate further. First, set the permissions on the key file:
```
chmod 600 keys/humanitarian
```

Next, use ssh with the key file you modified above to connect to the server as the root user with the IP address you see in the control panel for the web server:
```
ssh -i keys/humanitarian root@<IP address from control panel>
```

You get an authenticity request like this, type 'yes' to continue connecting:
```
$ ssh -i keys/humanitarian root@104.130.139.44
The authenticity of host '104.130.139.44 (104.130.139.44)' can't be established.
RSA key fingerprint is 3d:66:26:0d:78:f4:69:c4:e9:01:e5:ac:d2:32:15:87.
Are you sure you want to continue connecting (yes/no)? yes
```


Troubleshooting
---------------

If you immediately get a credential error, double-check the settings in 'libcloud.conf'. 

If one of your servers fails to build, you just have to try again. Sometimes switching the region setting in the libcloud .deploy files will help as some data centers could be more available than others at any given time. The available regions are IAD, DFW, and ORD in North America. These are best for the workshop as the latency across the oceans would slow you down.

If you have issues while installing paramiko using pip (in Mac or Linux), build it from the source.
	- Download and untar the file (.tar.gz) in this link - https://pypi.python.org/pypi/paramiko/1.15.1
	- In the paramiko-1.15.1 directory, run:
	  ```
	  sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future easy_install ./
	  ```


If you get error installing paramiko (pycrypto error) try  "aptitude install python-dev"

