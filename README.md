1. Protect the operating system layer
Hosting and server deployment aspects

Place the database server and web server on separate physical machines, and ideally have the database server run by itself to prevent server issues caused by vulnerabilities in other applications or services.

System Security Maintenance Aspects

Install antivirus software, a firewall, and all recommended patches and updates. The firewall should filter traffic to the MySQL server and implement entry blocking.

Disable all unnecessary services; the fewer services, the better.

II. User and Account Management
Password Settings Related

Set a password for all MySQL accounts to make it difficult to connect using anonymous accounts, as client programs may not always recognize the user if they know the database name but not the password, and may specify other usernames to connect to the database.

Use the setpassword statement to modify the user's password. First, log in to the database system with mysql -uroot, then execute mysql update mysql.user  set password=password('newpwd'), and finally run flush privileges.

Root User Related

Do not use the root user to run the MySQL server, as root is the default administrative user and is easily targeted by attackers. You can rename the root user in the MySQL console using the mysql RENAME USER root TO new_username; command, and change the password using the mysql SET PASSWORD FOR 'username'@'%hostname'= PASSWORD('newpassword'); command.

Account Number Management Aspect

Reduce the number of administrator accounts, create accounts only for those who truly need them, and keep the number of accounts as low as possible. Regularly check and clean up unnecessary, unused, and anonymous accounts, as the more administrator accounts there are, the greater the risk.

Strengthen password aspects for all users

In addition to the administrator account, it is necessary to check the usernames and passwords of all other users, and reset the passwords of accounts with low security strength when necessary.

III. Authority Management Aspect
Reasonable Grant of Authority

Each user should be granted appropriate permissions to ensure the database operates normally, but non-administrative users should not be granted file/advanced/program permissions, as file permissions allow users to write files anywhere in the file system, and program permissions allow users to view server activity, terminate client connections, and even change server operations.

Restrict or disable the display database privilege, which can be used to collect database information and is easily exploited by attackers to steal data and prepare for further attacks. You can add skip - show - database to the /etc/my.cnf configuration file in the MySQL database (add to the my.ini file on Windows systems), or only grant this privilege to those who truly need it.

Even administrators should not be given all permissions in the same account. Lower the administrator account's access to data and check the permissions of other users to ensure they are appropriate.

IV. Database Components and Data Aspects
Risk Component Handling Aspect

Disable the LOAD DATA LOCAL INFILE directive, which may allow users to read local files or even access files on other operating systems, thus helping attackers gather information and exploit vulnerabilities to invade the database.

Delete the test database. The default test database has security risks and is accessible to anonymous users. You can clear it using the mysql DROP database test; instruction.

The historical files of the MySQL server are useless after a successful installation and contain sensitive information (such as passwords) that can pose significant security risks if obtained by attackers. You can handle them using cat /dev/null.

Restricting Access to Data

Set up so that no users other than the root user are allowed to access the u (the description here is not very clear, it is speculated to be a specific database object, etc., listed according to search results) in the main database mysql.

5. Network Access
Remote Access Restrictions

Avoid accessing the MySQL database from the Internet, ensuring that only specific hosts have access privileges. If remote access is used, ensure that only defined hosts can access the server through TCP wrappers, iptables, or firewall software/hardware. You can add the skip-networking parameter in the [mysqld] section of my.cnf or my.ini to restrict opening network sockets (still allowing local connections), or add bind-address = 127.0.0.1 to force MySQL to listen only on the local machine. If users connect to the server from their own machines or to a web server on other machines, consider granting limited access permissions, such as mysql GRANT SELECT, INSERT ON mydb.* TO 'someuser'@'somehost';.

For most users, there is no need to access a MySQL server through an unsafe open network. You can configure a firewall or hardware to limit hosts, or force MySQL to listen only on localhost if remote access is required, using an SSH tunnel.

6. Backup Related
Regularly back up the database

Any system can suffer from a disaster, and businesses should make the backup process part of their server's daily routine so that they can quickly recover from disasters when bad situations occur.
