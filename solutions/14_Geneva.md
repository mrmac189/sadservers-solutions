https://proxy.sadservers.com/03b8e0e48128b5f97/

There's an Nginx web server running on this machine, configured to serve a simple "Hello, World!" page over HTTPS. However, the SSL certificate is expired.

Create a new SSL certificate for the Nginx web server with the same Issuer and Subject (same domain and company information).



```bash
find / -name *.crt 2> /dev/null | grep nginx
# /etc/nginx/ssl/nginx.crt
cd /etc/nginx/ssl
openssl verify nginx.crt
openssl x509 -in /etc/nginx/ssl/nginx.crt -text | head -20
# Not Before: Sep 17 22:34:18 2024 GMT
# Not After : Sep 18 22:34:18 2023 GMT
# CN = localhost, O = Acme, OU = IT Department, L = Geneva, ST = Geneva, C = CH
# RSA Public-Key: (2048 bit)
sudo mv nginx.crt nginx.crt.old && sudo mv nginx.key nginx.key.old
```
```bash
sudo openssl req -x509 -newkey rsa:2048 -keyout nginx.key -out nginx.crt -days 365 -nodes -subj "/CN=localhost/O=Acme/OU=IT Department/L=Geneva/ST=Geneva/C=CH"
sudo systemctl restart nginx
```
```bash
echo | openssl s_client -connect localhost:443 2>/dev/null | openssl x509 -noout -dates
echo | openssl s_client -connect localhost:443 2>/dev/null | openssl x509 -noout -subject
```