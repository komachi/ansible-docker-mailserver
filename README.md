# ansible-docker-mailserver

This playbook deploy [docker-mailserver](https://github.com/tomav/docker-mailserver) to your Debian GNU/Linux box. It's complete solution you need to self-host your own mailserver. I use it myself.

## Variables

```
docker_mailserver_domainname: "example.com"
docker_mailserver_hostname: "mail"
email: "example@example.com"
email_password: "changeme"
certbot_certs:
  - email: "{{ email }}"
    domains:
      - "{{ docker_mailserver_hostname }}.{{ docker_mailserver_domainname }}"
```

## DNS settings

You need to create some DNS records first.

1. Create `A` and `AAAA` records which points to your server.
2. Create DMARC record: type `TXT`, name `_dmarc`, value `v=DMARC1; p=none`
3. Create SPF record: type `TXT`, name `@`, value: `v=spf1 mx -all`
4. Create MX record: type `MX`, name `@`, value: yours server hostname.

After that, you can run playbook. There is another one DNS record you should create: DKIM. Find it's content in `/etc/docker-mailserver/opendkim/keys/{{ domainname }}/{{ hostname }}.txt`

Also you should create reverse DNS record pointing to your mailserver domain.