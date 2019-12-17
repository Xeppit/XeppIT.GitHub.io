---
layout: post
title: A guide to installing and setting up SQL Server 2017 for use within your programs.
excerpt: "A guide to installing and setting up SQL Server 2017 for use within your programs."
categories: [Database]
tags: [SQL Server 2017, Set Up, Guide]
modified: 2019-12-17
comments: true
---

## Description
This document will outline the steps needed to install a local instance of Microsoft SQL Server and set up a database with users.

## Goals
* Install SQL Server 2017
* Install SSMS to manager the SQL server
* Create a Database
* Create a user with read write access to the new database using SQL Server authentication (username & password)
* Connect to the new db using a standard connection string.

## Perquisites
* [SQL Server 2017 Express](https://www.microsoft.com/en-gb/sql-server/sql-server-downloads)
* [SQL Server Management Studio (SSMS) ](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)

## Setup

### Installing SQL Server and SSMS

- Download SQL Server 2017 Express from the link above & start the install file, you will be prompted with the following modal. Select Basic Install.

![Basic Setup Modal]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-1.png)

- Once the install is complete you will be prompted with the following modal, from here click the "Install SSMS" button, download the installer and start the installation.

![Install Finished Modal]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-2.png)


- Once the installation has complete restart your computer & start up SSMS, you will be prompted with the modal shown below. It should automagicaly get all your DbServer details so if you click on connect it will connect to your database instance.

![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-3.png)

### Changing authentication method
- Right click on the root node and select "Properties"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-4.png)

- Select "Security" and then change Server authentication to "SQL Server and Windows Authentication mode" and click "OK"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-5.png)

### Adding a new database
- Right-click on the "Databases" folder and click "Add Database" from the context menu.
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-6.png)

- Enter a database name and then click "OK"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-7.png)

### Adding a User
- Expand the "Security" folder
- Right click on the "Logins" folder
- Select "New Login" from the context menu.

![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-8.png)

- Enter a login name.
- Select "SQL Server authentication"
- Enter a password
- Click "OK"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-9.png)

### Mapping the login to the database

- Right click on the new user and select properties
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-10.png)

- Select the page "User Mapping" from the left panel
- Select the new database from the "Users mapped to this login" table
- Select "db_owner" from the "Database role membership for: xxx" table
- Click "OK"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-11.png)

### Restart Server
- Right click on the server instance on the left and restart the server (Picture to come)
- Once restarted make sure all setting have taken especially the "authentication method"

### Test the new user
- Disconnect from the Server
- Reconnect using the new Username and Password
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-12.png)

### Enable TCP/IP Connections <-- Not Sure this bit is needed check next time I Use this guide!!

- Start up "SQL Server Configuration Manager"
- Select "Protocols for SQLEXPRESS" on the left menu
- Right click on "TCP/IP" and select "Enable"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-13.png)

- Right click on "TCP/IP" and select "Properties"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-14.png)

- Modify the "IPAII" to Match the following and click "Apply" and "OK"
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-15.png)

- Click on "SQL Server Services" on the left
- Right click on the server instance and click restart
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-16.png)

### Finished

- You should now be able to connect to the database using c# etc.
- There does seem to be a small time delay after restarting the service no longer than 1 minute

- Rider Config
![SSMS Startup]({{ site.url }}/img/2019-12-17-2019-12-17-installing-sqlserver-and-set-up-17.png)

{% highlight xml %}
Data Source=DESKTOP-53V8BKJ\SQLEXPRESS;User ID=Nathan;Password=********;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False
{% endhighlight %}

## References
[Ryadel blog post](https://www.ryadel.com/en/sql-server-2017-express-edition-install-setup-configure-how-to-tutorial-guide/)


## What's Next
TBC .Net Core WebAPI With EFCore ORM and SQL Server