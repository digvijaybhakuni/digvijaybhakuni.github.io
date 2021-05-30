---
layout: blog
title: Install JFrog Artifactory in Ubuntu using deb file
published: true
date: 2019-07-12T09:57:55.633Z
thumbnail: /images/uploads/download_jfrog_artifactory_oss.png
rating: 3
---
This post will deal with install JFrog Artifactory OSS using deb file in Ubuntu.

To download artifactory goto <https://jfrog.com/open-source/> and click on Download Debian

![Download JFrog Artifactory OSS Debian](/images/uploads/download_jfrog_artifactory_oss.png "Download JFrog Artifactory OSS Debian")

If you are on shell you can use wget to get the deb file.

#### To Install 
Open the terminal and navigate to location where you download jfrog-artifactory binary and type following command (Note: your deb file name may differ)

```sh
$ sudo dpkg -i  jfrog-artifactory-oss-6.11.1.deb
```
#### Output 
```
Selecting previously unselected package jfrog-artifactory-oss.
(Reading database ... 84132 files and directories currently installed.)
Preparing to unpack jfrog-artifactory-oss-6.11.1.deb ...
dpkg-query: no packages found matching artifactory
Checking if group artifactory exists...
Group artifactory doesn't exist. Creating ...
Checking if user artifactory exists...
User artifactory doesn't exist. Creating ...
Checking if ARTIFACTORY_HOME exists
Removing tomcat work directory
Unpacking jfrog-artifactory-oss (6.11.1) ...
Setting up jfrog-artifactory-oss (6.11.1) ...
Adding the artifactory service to auto-start... DONE

************ SUCCESS ****************
The Installation of Artifactory has completed successfully.

PLEASE NOTE: It is highly recommended to use Artifactory with conjunction with an external database (MySQL, Oracle, Microsoft SQL Server, PostgreSQL, MariaDB). For details about how to configure the database, refer to https://www.jfrog.com/confluence/display/RTF/Configuring+the+Database

You can activate artifactory with:
> systemctl start artifactory.service

Then check the status with:
> systemctl status artifactory.service
Processing triggers for systemd (239-7ubuntu10.14) ...
```

#### Installation Directory 
By Default the artifactory would be installed in `/opt/jfrog/artifactory/`

#### Configation

#### As Service
To check the status 
```
systemctl status artifactory.service
```

To start the service (this may take some time)
```
systemctl start artifactory.service
```

