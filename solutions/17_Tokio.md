curl 127.0.0.1:80
nc -zv localhost 80
ss -tulpn
systemctl status apache2
# OK 
# firewall?
ufw status
# inactive
iptables -S
iptables -D INPUT -p tcp -m tcp --dport 80 -j DROP
cat /var/log/apache2/error.log
# file permissions deny server access: /var/www/html/index.html
chmod 0744 /var/www/html/index.html
curl 127.0.0.1:80
# hello sadserver