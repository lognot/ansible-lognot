ansible-lognot
=========

This role downloads [lognot](http://lognot.io) and installs it.

Requirements
------------

Java 8 or above should be already installed on the target machine.

Role Variables
--------------
    url_archive_zip - zip file location. URL or local file path to the archive.
    destination_folder - destination folder on target machine. Default: user log file.
    mail_recipients - comma separated list of email addresses to send email notifications to
    mail_host - smt email server hostname
    mail_port - smtp port
    mail_username - smtp username
    mail_password - password for the user above
    files - list of log file items (below)
      - key - an identifier for this log file.
        path - logfile absolute path. Supports date patterns like %d{yyyyMMdd}.
        period - number of seconds to repeat the lookup.
        regEx - regular expression to apply on log messages.


Playbook
----------------

Create a playbook like the one below. Call it lognot.yml
```yaml
- hosts: all
  roles:
    - ansible-lognot
  vars:
    mail_recipients: admin@example.com, devops@example.com
    mail_host: mail.example.com
    mail_port: 25
    mail_username: mail@example.com
    mail_password: thisismypassword
    files:
      - key: server
        path: /opt/server/log/console-%d{yyyyMMdd}.log
        period: 300
        regEx: .*ERROR.*|.*SEVERE.*
        
      - key: tomcat
        path: /var/log/apache/error.log
        period: 600
        regEx: error                                          
```
You also need a hosts file
```
[server]
172.23.10.1
```
To run this role using the playbook above, run:
```bash
$ ansible-playbook -i hosts lognot.yml
```