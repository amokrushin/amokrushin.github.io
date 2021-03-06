# Mail

## Postfix

```
sudo hostname example.com
```

Edit `/etc/hosts`
```
127.0.0.1       localhost example.com
...
```

1. [How to Install and Configure Postfix as a Send-Only SMTP Server on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-16-04)
2. [How To use an SPF Record to Prevent Spoofing & Improve E-mail Reliability](https://www.digitalocean.com/community/tutorials/how-to-use-an-spf-record-to-prevent-spoofing-improve-e-mail-reliability)
3. [How To Install and Configure DKIM with Postfix on Debian Wheezy](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-dkim-with-postfix-on-debian-wheezy)

#### Прочее
* https://habr.com/company/ruvds/blog/325356/
* https://habr.com/post/261861/
* https://www.alexeykopytko.com/2014/postfix-opendkim/
* https://pages.returnpath.com/email-sending-best-practices.html
* [Внедрение DMARC для защиты корпоративного домена от спуфинга](https://habr.com/ru/company/mailru/blog/315778/)
* [Загадки и мифы SPF](https://habr.com/ru/company/mailru/blog/338700/)

```
Все еще сложнее. В идеале, если у вас есть зона, например:
example.com. IN MX 10 mail.example.com.
example.com. IN A 1.2.3.4
mail.example.com. IN A 1.2.3.5
www.example.com. IN A 1.2.3.4
Сервер mail.example.com имеет каноническое имя mail.example.com и сконфигурирован использовать его в команде HELO. Тогда нужны следующие записи:

; основная SPF-запись
example.com. IN TXT "v=spf1 mx a ~all"
; SPF-запись для HELO
mail.example.com. IN TXT "v=spf1 a -all"
; SPF-запись экранирующая www.example.com (при условии что это имя не каноническое и не используется в HELO)
www.example.com. IN TXT "v=spf1 -all"
; экранирующий SPF-wildcard
*.example.com. IN TXT "v=spf1 -all"
```


## Postmaster

* [mail.ru](https://postmaster.mail.ru/)
* [yandex.ru](https://postoffice.yandex.ru/)
* [google.com](https://postmaster.google.com/)


## Test

* http://dkimvalidator.com/
* http://www.mail-tester.com/

## Monitoring

* https://sendersupport.olc.protection.outlook.com/snds/

## Troubleshooting

* https://blog.stickleback.dk/getting-off-hotmails-blocklist/
* http://ipremoval.sms.symantec.com/lookup/
* https://support.microsoft.com/en-us/getsupport?oaspworkflow=start_1.0.0.0&wfname=capsub&productkey=edfsmsbl3&locale=en-us&ccsid=635611717755428181
* https://www.noomle.com/unfortunately-messages-werent-sent-please-contact-your-internet-service-provider/

