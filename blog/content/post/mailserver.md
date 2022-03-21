---
title: "Mailserver"
date: 2022-03-21T16:55:44+08:00
---
# 尝试搭建邮件服务
![mail](https://poste.io/schema.svg)

## 搬运安装过程

Downloading and running Poste.io

There are two versions of product, PRO and FREE image.

PRO VERSION
```
poste.io/mailserver # (from https://poste.io docker server)
```
FREE VERSION
```
analogic/poste.io # (from https://hub.docker.com)
```
Both versions share same data directory structure - the only one difference when running PRO version is that you will login to our private docker repository.

PRO VERSION
$ docker login -u "username" -p "password" https://poste.io
$ docker run \
    --net=host \
    -e TZ=Europe/Prague \
    -v /your-data-dir/data:/data \
    --name "mailserver" \
    -h "mail.example.com" \
    -t poste.io/mailserver
FREE VERSION
You will be using image from public Docker hub.

$ docker run \
    --net=host \
    -e TZ=Europe/Prague \
    -v /your-data-dir/data:/data \
    --name "mailserver" \
    -h "mail.example.com" \
    -t analogic/poste.io
Docker arguments explained

--net=host (recomended) mailserver will use host network stack (see https://docs.docker.com/network/host/)
in this mode host's firewall will work correctly
connection source IP is not hidden by userland-proxy
ipv6 working correctly
network schemes explanation
Ports which are opened by poste.io:

Port number	Purpose
25	SMTP - mostly processing incoming mails
80	HTTP - redirect to https (see options) and authentication for Let's encrypt service
110	POP3 - standard protocol for accessing mailbox, STARTTLS is required before client auth
143	IMAP - standard protocol for accessing mailbox, STARTTLS is required before client auth
443	HTTPS - access to administration or webmail client
465	SMTPS - Legacy SMTPs port
587	MSA - SMTP port used primarily for email clients after STARTTLS and auth
993	IMAPS - alternative port for IMAP encrypted since connection
995	POP3S - encrypted POP3 since connections
4190	Sieve - remote sieve settings
-e TZ=Europe/Prague Set timezone for correct datetime

-v /your-data-dir/data:/data Mounts data directory from host system. User database, emails, logs, all will end up in this directory for easy backup.

--name "mailserver" Run poste.io as container with defined name

-h "mail.example.com" Hostname for your mailserver

-t analogic/poste.io Image name, differs for PRO and FREE version

Optional arguments

-e "HTTPS=OFF" To disable all redirects to encrypted HTTP, its useful when you are using some kind of reverse proxy (place this argument before image name!)

-e "HTTP_PORT=8080" Custom HTTP port. Please note that you must handle Let's encrypt requests at port 80, so if you are using reverse proxy setup you need to forward /.well-known/ folder to this port

-e "HTTPS_PORT=4433" Custom HTTPS port.

-e "DISABLE_CLAMAV=TRUE" To disable all ClamAV, it is useful for low mem usage.

-e "DISABLE_RSPAMD=TRUE" To disable all Rspamd, it is useful for low mem usage.

-e "DISABLE_ROUNDCUBE=TRUE" To disable Roundcube webmail.

-p 4190:4190 When you are going to use clients with ability to manage Sieve filters externally, you need also publish port 4190