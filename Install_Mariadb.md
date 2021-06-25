Ubuntu 20.04에서 MariaDB 설치
Version : 10.3.29-MariaDB-0ubuntu0.20.04.1

1. Repository 업데이트
```
sudo apt update && sudo apt-get -y upgrade

필요한 라이브러리 및 설치된 라이브러리를 최신버전에 맞게 다운받을 수 있게.
```

2. Install Maria DB
```
sudo apt-get install -y mariadb-server
```

3. Maria DB 접속
```
sudo mysql                            // password 없이 sudo 인증으로 로그인
set password = password('1234');      // 패스워드 초기화
flush privileges;                    

// exit 이후 재로그인
mysql -u root -p                      // 기존의 방식으로 로그인
```

4. Maria DB Port 및 IP 접속
```
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf

[mysqld]

#
# * Basic Settings
#
user                    = mysql
pid-file                = /run/mysqld/mysqld.pid
socket                  = /run/mysqld/mysqld.sock
port                    = 3306
basedir                 = /usr
datadir                 = /var/lib/mysql
tmpdir                  = /tmp
lc-messages-dir         = /usr/share/mysql
#skip-external-locking

# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0

port 부분 주석 해제 및 bind-address를 0.0.0.0 으로 변경

sudo systemctl restart mysqld
```
