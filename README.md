# SETUP CLUSTER MONGODB REPLICATION 3 NODE USE KEY SECURITY

# Add hosts 3 node mongodb
![Screenshot 2024-01-08 at 00 50 36](https://github.com/sysadminzz/mongodb/assets/152803356/a4e69313-544d-4db5-9775-8fbbfc71e54d)


1. Add repo & install mongodb


vi /etc/yum.repos.d/mongodb-org-4.4.repo


yum install mongodb-org
mongo --version

2. Add disk Data mongodb
mkdir -p /data/mongodb
chown mongod:mongod /data/mongodb/

3. Add Key Security
mkdir /etc/mongodb/
openssl rand -base64 741 >/etc/mongodb/mongodb.key
chmod 600 /etc/mongodb/mongodb.key
chown -R mongod:mongod /etc/mongodb/

=> copy key to 2 node #
scp /etc/mongodb/mongodb.key user@node2:~/
mkdir /etc/mongodb/
mv /home/vagrant/mongodb.key /etc/mongodb/
chown -R mongod:mongod  /etc/mongodb
chmod 600 /etc/mongodb/mongodb.key

4. Config mongodb
vi /etc/mongod.conf
vi /usr/lib/systemd/system/mongodb.service
=> systemctl daemon-reload
systemctl enable mongod.service
systemctl start mongod.service
systemctl status mongod.service

5. Create user & db mongo cluster
mongo --host 127.0.0.1 --port 27017
rs.initiate() 
use admin 
db.createUser({
  user: "admin",
  pwd: "mongoPassDemo", 
  roles: [{ role: "root", db: "admin" }]
})

=> Config Mongo Shell by user admin:
mongo --host node1:27017 --username admin --password  --authenticationDatabase admin
Add Node:
rs.status() 
rs.add("node2:27017")
rs.add("node3:27017")
rs.status() 







