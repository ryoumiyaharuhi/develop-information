[mysql给root开启远程访问权限](https://www.cnblogs.com/goxcheer/p/8797377.html)
```
mysql -u root -p
use mysql；
select User,authentication_string,Host from user;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Hong4321!';
flush privileges;
```