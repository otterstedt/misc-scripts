SMTPd with postfix and saslauthd (On a Raspberry Pi)

My use case was that a local ip camera could not use google smtp server, not even with "less secure apps allowed"
Google did not not allow my local postfix server either.
The end result was to setup a local postfix relay smtpd with authentication to gmail smtpd.

I had to add local authentication due to that the ip camera (amcrest) could not connect without credentials due to some firmware bug.

So I did the setup with a postfix smtpd relaying to google, with local authentication using saslauthd

/etc/postfix/main.cf
```
# See /usr/share/postfix/main.cf.dist for a commented, more complete version


# Debian specific:  Specifying a file name will cause the first
# line of that file to be used as the name.  The Debian default
# is /etc/mailname.
#myorigin = /etc/mailname

smtpd_banner = $myhostname ESMTP $mail_name (Raspbian)
biff = no

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h

readme_directory = no

# See http://www.postfix.org/COMPATIBILITY_README.html -- default to 2 on
# fresh installs.
compatibility_level = 2

mynetworks = 192.168.1.0/24 127.0.0.0/8 10.0.0.0/8

relayhost = [smtp.gmail.com]:587

# Enable SASL authentication
smtp_sasl_auth_enable = yes
# Disallow methods that allow anonymous authentication
smtp_sasl_security_options = noanonymous
# Location of sasl_passwd
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
# Enable STARTTLS encryption
smtp_tls_security_level = encrypt
# Location of CA certificates
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt

smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = $myhostname
broken_sasl_auth_clients = yes

smtpd_recipient_restrictions = 
   permit_sasl_authenticated, 
   permit_mynetworks, 
   check_relay_domains
```

/etc/postfix/sasl/smtpd.conf

```
pwcheck_method: saslauthd
#mech_list: PLAIN LOGIN
```

/etc/postfix/master.cf

```
...
smtp      inet  n       -       n       -       -       smtpd
...
```

Add user 'postfix' to group 'sasl' 
```
adduser postfix sasl
```
saslauthd

sudo apt install sasl2-bin
```
warning: SASL authentication failure: cannot connect to saslauthd server: No such file or directory

The warning message tells me that saslauthd can’t be located. The real location is /var/spool/postfix/var/run/saslauthd, but postfix is expecting to find it in /var/run/saslauthd.  Create a symlink as described below and see if that fixes your problem.

sudo ln -s /var/spool/postfix/var/run/saslauthd /var/run
sudo chown root:sasl /var/spool/postfix/var/run/saslauthd

https://nfolamp.wordpress.com/2013/02/04/fixing-postfix-and-saslauthd-cannot-connect-to-saslauthd/

```




Links:
http://postfix.state-of-mind.de/patrick.koetter/smtpauth/smtp_auth_mailclients.html
http://www.postfix.org/SASL_README.html
http://postfix.state-of-mind.de/patrick.koetter/smtpauth/smtp_auth_mailservers.html
https://www.linode.com/docs/email/postfix/configure-postfix-to-send-mail-using-gmail-and-google-apps-on-debian-or-ubuntu/
