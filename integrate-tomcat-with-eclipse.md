# Integrate Tomcat with Eclipse Java EE 

##### All of steps below are specific for Macbook users.

**1. install tomcat**

use homebrew on Macbook `brew install tomcat`

try start tomcat from
`/usr/local/opt/tomcat/bin/catalina start`
to validate installation succeed.

##### trouble shooting:
`error stating SHA256 mismatch`

run:
```
brew cleanup
brew update
```
before running `brew install tomcat`


**2. install latest j2ee eclipse (need to support the tomcat version)**

**3. Locate Servers tab (usually at the bottom section)**
```
click "click this link to create a new server..."
click the version you will link to. 
enter the installation directory.
```

##### trouble shooting:
I installed v8.5, so I chose Tomcat v8.0 Server, and entered my installation     directory:
`/usr/local/opt/tomcat/libexec`  (CATALINA_HOME)
However this caused an error:
`Apache Tomcat installation at this directory is version 8.5.0. A Tomcat 8.0     installation is expected.`

The reason for this error is eclipse WTP(web tools platform) checks version     info from catalina.jar; if the version info from ServerInfo.properties          doesn't match it will throw the error. But this check is actually redundant.

To solve this you have to patch catalina.jar, follow the steps here:

(make sure tomcat is stopped before doing the steps)
1) cd /usr/local/opt/tomcat/libexec/lib
2) mkdir catalina
3) cd catalina
4) unzip ../catalina.jar
5) subl org/apache/catalina/util/ServerInfo.properties
Change server.info version numbers in this file to be 8.0.0 instead of 8.5.x
6) jar uf ../catalina.jar org/apache/catalina/util/ServerInfo.properties
7) cd ..
8) rm -rf catalina

**4. now you can see the tomcat server under Servers tab**

double click on it and check
1) HTTP ports (default 8080)
2) Under Server locations choose "Use tomcat installation" (give server path        as `/usr/local/opt/tomcat/libexec`)

**5. now you can right click and start the server** 
    
Check see it is running at http://localhost:8080/
