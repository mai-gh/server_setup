pacman -S nginx nginx-core certbot python3-certbot-nginx


vim /etc/nginx/sites-enabled/default

edit root
edit servername

root /var/www/html;
server_name example.com www.example.com;


check configuration file with
nginx -t && nginx -s reload

make sure domains are set up dorrectly with http. i just made 2 A records to my ip

one with no name, the other with WWW as the name

certbot --nginx -d example.com -d www.example.com

now to set up auto renewal with systemd

```
# /etc/systemd/system/certbot-renewal.service

[Unit]
Description=Certbot Renewal

[Service]
Type=oneshot
ExecStart=/usr/bin/certbot renew --quiet --agree-tos
ExecStartPost=/bin/systemctl reload nginx.service
```

```
# /etc/systemd/system/certbot-renewal.timer

[Unit]
Description=Timer for Certbot Renewal

[Timer]
OnBootSec=300
OnUnitActiveSec=1w

[Install]
WantedBy=multi-user.target
```


systemctl start certbot-renewal.timer
systemctl enable certbot-renewal.timer

Check Status
$ systemctl status certbot-renewal.timer
$ journalctl -u certbot-renewal.service
