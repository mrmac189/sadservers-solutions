sudo -u postgres psql -c "insert into persons(name) values ('jane smith');" -d dt
systemctl list-units | grep post
systemctl status postgres@14main
# no space left on device
df -h 
# /opt/pgdata is full
rm /opt/pgdata/*.bak
# better move somewhere