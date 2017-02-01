# sdj-u2f-nstic
NSTIC-funded U2F project at the School District of Janesville

Project Lead: [Yubico](http://yubico.com)
#
## Table of Contents

1. [Shibboleth IdP Installation](#shibboleth-idp-installation)
2. [Shib IdP -- Active Directory Integration](#shib-idp-active-directory-integration)

## Shibboleth IdP Installation

* Following [HowTo Guide](https://github.com/malavolti) from the Italian Identity Federation for installation on sdj-nstic running ubuntu 14.04. Be sure to use the right how-to guide. If you hover over the github filename, it will show the full name. The file you want includes "Ubuntu Linux LTS 14.04" in its name.
* "openSSL >= 1.0.2", buf Ub 14.04 has 1.01f. Good enough?
* Generate a Self-Signed Certificate 
** per https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs
** and move to /tmp
```
openssl req -newkey rsa:2048 -nodes -keyout jvu2f.key -x509 -days 3659 -out jvu2f.crt
chmod 600 jvu2f.key
mv jv* /tmp
root@test-cache:/home/khazelton# ls -la /tmp/jv*
-rw-r--r-- 1 root root 1602 Dec 18 10:18 /tmp/jvu2f.crt
-rw------- 1 root root 1704 Dec 18 10:18 /tmp/jvu2f.key
root@test-cache:/home/khazelton# openssl x509 -text -noout -in /tmp/jvu2f.crt
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 10770621338274413314 (0x9578ef0abe16bb02)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, ST=Wisconsin, L=Janesville, O=School District of Janesville, OU=Security, CN=jvu2f.janesville.k12.wi.us/emailAddress=cassandra.anderson@janesville.k12.wi.us
        Validity
            Not Before: Dec 18 16:18:31 2016 GMT
            Not After : Dec 25 16:18:31 2026 GMT
        Subject: C=US, ST=Wisconsin, L=Janesville, O=School District of Janesville, OU=Security, CN=jvu2f.janesville.k12.wi.us/emailAddress=cassandra.anderson@janesville.k12.wi.us
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:c2:7a:a5:ff:d0:bb:98:80:37:d7:36:34:9b:c7:
                    4d:82:f3:1c:a1:74:52:ac:e9:82:4f:2c:9f:36:27:
                    a6:40:97:2b:c9:67:58:8e:71:8e:49:a2:c0:a9:88:
                    8f:8c:36:39:16:a4:29:05:10:4c:95:e9:2f:cc:c1:
                    f2:05:14:f4:b1:2a:c9:0f:df:06:89:dd:bd:26:67:
                    38:8e:9f:79:5b:00:98:5c:30:4b:e9:6b:17:58:cd:
                    e5:2b:5f:c1:50:27:28:c2:5c:98:39:c6:66:39:18:
                    10:b8:d3:f2:45:2d:10:b4:97:0a:67:29:63:ca:d5:
                    67:86:d8:3e:e3:2c:39:23:4f:0d:e5:99:04:d5:a3:
                    b1:06:7e:02:59:5a:b3:f7:cb:12:da:a4:2f:b8:50:
                    78:df:38:a0:20:15:f1:f0:f0:69:29:fc:65:ee:a3:
                    cb:d5:1e:9d:fb:d6:0b:22:7d:54:f3:22:14:a7:2f:
                    12:46:2b:4e:09:82:64:46:04:7e:34:7b:ae:87:6b:
                    9a:0a:01:1c:29:29:3e:66:35:51:1f:22:29:a9:3a:
                    ba:c3:77:00:5d:ed:78:35:6d:3d:b9:4f:e9:30:88:
                    73:aa:38:fd:72:ba:92:5a:f6:62:ae:cb:1a:9c:a8:
                    7e:5b:09:58:3c:1d:38:34:b2:3f:9c:13:3e:6d:a8:
                    de:4d
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                5A:28:8C:EC:7C:B6:0F:33:1D:FA:DD:DA:A6:3B:37:2D:94:5A:8D:1A
            X509v3 Authority Key Identifier: 
                keyid:5A:28:8C:EC:7C:B6:0F:33:1D:FA:DD:DA:A6:3B:37:2D:94:5A:8D:1A

            X509v3 Basic Constraints: 
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption
         20:7e:4e:42:7e:42:2d:5f:b9:b8:40:9d:6f:ab:fc:19:54:2d:
         b0:d0:8a:61:5e:81:0a:b4:43:25:e4:fd:4d:c5:01:f2:ed:ba:
         b1:a3:85:df:f8:4e:d0:fb:4d:da:6a:bd:8b:79:84:b8:c7:a2:
         d7:11:68:ee:0b:5b:89:8c:42:a4:1f:8b:90:57:3c:1d:a2:af:
         e3:6e:02:5a:11:7c:55:a5:22:26:bf:93:62:cd:b1:cb:f5:32:
         f5:60:41:e9:ae:8c:45:59:92:bf:c4:74:e4:92:9f:3e:36:4a:
         27:f4:22:1c:27:2b:6b:5a:7b:d6:e2:dd:43:81:c8:0b:b7:27:
         db:71:d0:e2:c1:48:68:dd:bc:7e:71:fc:0f:f0:6b:60:43:21:
         17:27:43:5f:74:40:c6:bd:57:31:4d:78:7e:d1:80:e9:d6:e4:
         04:36:34:6c:73:7d:3d:01:ce:69:7a:4e:ec:ec:ad:c1:6c:f2:
         ba:da:16:6e:3b:ee:56:ad:69:8c:29:1e:30:67:d6:1c:28:75:
         62:a0:91:72:f5:c1:84:6c:9f:61:81:ba:48:ee:c0:ee:f6:fb:
         ce:e7:16:b5:61:b0:8a:4a:94:7a:46:66:0d:b7:4a:71:3f:97:
         a3:1d:12:43:8d:a1:95:20:85:80:e1:96:52:bb:a2:60:b0:89:
         54:e9:c1:47
root@test-cache:/home/khazelton# 
```
#### 10 Install apache portable runtime (APR)
#### 11 Install Tomcat 8
* NOTE: error in url for daemon-tomcat; should be https://github.com/malavolti/HOWTO-Install-and-Configure-Shibboleth-Identity-Provider/blob/master/daemon-tomcat 

#### 12 Create the init.d service for restarting Tomcat 8
#### 13 Ensure that your firewall/firewalls are not blocking the traffic on the port 443 and 8080
#### 14 Open the Tomcat 8 Home Page:

* http://jvu2f:8080 shows the Tomcat 8 Home Page

#### 15 Move the HTTPS Certificate and Key to a new directory /opt/tomcat/ssl
#### 16 Deprecated Approach: Install the Tomcat Native:
* NOTE: Find latest Tomcat Native at https://archive.apache.org/dist/tomcat/tomcat-connectors/native/1.2.10/source/tomcat-native-1.2.10-src.tar.gz
* /usr/local/src/tomcat-native-1.2.10-src/native
* ./configure --with-apr=/usr/local/apr --with-java-home=/usr/lib/jvm/java-8-oracle --with-ssl=yes --prefix=/opt/tomcat --with-ssl=/????

tar -xzvf openssl-1.0.2j.tar.gz...

INSTALL openssl 1.0.2j per /opt/openssl-1.0.2j/INSTALL

  $ ./config
  $ make
  $ make test
  $ make install
  
  /usr/local/ssl/bin/openssl version
  OpenSSL 1.0.2j  26 Sep 2016
  
point /usr/bin/openssl at /usr/local/ssl/bin/openssl

root@test-cache:/etc/alternatives# ls -la /usr/bin/openssl
-rwxr-xr-x 1 root root 513280 Sep 23 07:23 /usr/bin/openssl
root@test-cache:/etc/alternatives# openssl version
OpenSSL 1.0.1f 6 Jan 2014
root@test-cache:/etc/alternatives# mv /usr/bin/openssl /usr/bin/opensslV1.0.1f
root@test-cache:/etc/alternatives# ln -s /usr/local/ssl/bin/openssl /usr/bin/openssl

root@test-cache:/etc/alternatives# which openssl
/usr/bin/openssl
root@test-cache:/etc/alternatives# openssl version
OpenSSL 1.0.2j  26 Sep 2016
root@test-cache:/etc/alternatives# root




Deprecated Approach: Install Tomcat Native

cd /usr/local/src/tomcat-native-1.2.10-src/native
./configure --with-apr=/usr/local/apr --with-java-home=/usr/lib/jvm/java-8-oracle --with-ssl=yes --prefix=/opt/tomcat
 
#### 16a  Install Shib IdP 3.2.1 and Jetty 

...
root@test-cache:/opt/jetty-distribution-9.3.15.v20161220# cat webapps/idp.xml

<Configure class="org.eclipse.jetty.webapp.WebAppContext">
  <Set name="war"><SystemProperty name="idp.home"/>/war/idp.war</Set>
  <Set name="contextPath">/idp</Set>
  <Set name="extractWAR">false</Set>
  <Set name="copyWebDir">false</Set>
  <Set name="copyWebInf">true</Set>
</Configure>
root@test-cache:/opt/jetty-distribution-9.3.15.v20161220# pwd
/opt/jetty-distribution-9.3.15.v20161220
root@test-cache:/opt/jetty-distribution-9.3.15.v20161220# cd /opt/shibboleth-identity-provider-3.2.1
root@test-cache:/opt/shibboleth-identity-provider-3.2.1# ./bin/install.sh
Source (Distribution) Directory: [/opt/shibboleth-identity-provider-3.2.1]

Installation Directory: [/opt/shibboleth-idp]

Hostname: [sdj-nstic.janesville.k12.wi.us]

SAML EntityID: [https://sdj-nstic.janesville.k12.wi.us/idp/shibboleth]

Attribute Scope: [janesville.k12.wi.us]

Backchannel PKCS12 Password: 
Password cannot be zero length
Backchannel PKCS12 Password: 
Re-enter password: 
Cookie Encryption Key Password: 
Re-enter password: 
Warning: /opt/shibboleth-idp/bin does not exist.
Warning: /opt/shibboleth-idp/dist does not exist.
Warning: /opt/shibboleth-idp/doc does not exist.
Warning: /opt/shibboleth-idp/system does not exist.
Warning: /opt/shibboleth-idp/webapp does not exist.
Generating Signing Key, CN = sdj-nstic.janesville.k12.wi.us URI = https://sdj-nstic.janesville.k12.wi.us/idp/shibboleth ...
...done
Creating Encryption Key, CN = sdj-nstic.janesville.k12.wi.us URI = https://sdj-nstic.janesville.k12.wi.us/idp/shibboleth ...
...done
Creating Backchannel keystore, CN = sdj-nstic.janesville.k12.wi.us URI = https://sdj-nstic.janesville.k12.wi.us/idp/shibboleth ...
...done
Creating cookie encryption key files...
...done
Rebuilding /opt/shibboleth-idp/war/idp.war ...
...done

BUILD SUCCESSFUL
Total time: 2 min

#### 16b Configuring Jetty to serve the Shib 3.2.1 IdP webapp per https://wiki.shibboleth.net/confluence/display/IDP30/Jetty93 

***You Are Here***



## Shibboleth IdP -- Active Directory Integration
* See detailed instructions on the Shibboleth wiki, at https://wiki.shibboleth.net/confluence/display/IDP30/LDAPAuthnConfiguration 
