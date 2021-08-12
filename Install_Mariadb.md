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

3. Maria DB 접속 및 데이터베이스 생성
```
sudo mysql                              // password 없이 sudo 인증으로 로그인
create database DATABASE_NAME;
create user 'USER_NAME'@'%' identified by 'USER_PW';  // '%'는 외부접속 허용 내부만 사용할 경우 localhost
grant all privileges on DATABASE_NAME.* to 'USER_NAME'@'%';
flush privileges;
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

5. Mariadb 데이터 파일 위치 변경
```
sudo systemctl stop mysql

sudo mkdir -p /data/mariadb/mysql 
sudo rsync -av /var/lib/mysql /data/mariadb/mysql
sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf

[mysqld]

#
# * Basic Settings
#
user                    = mysql
pid-file                = /run/mysqld/mysqld.pid
socket                  = /run/mysqld/mysqld.sock
port                    = 3306
basedir                 = /usr
datadir                 = /data/mariadb/mysql     // datadir 위치 변경
tmpdir                  = /tmp
lc-messages-dir         = /usr/share/mysql
#skip-external-locking


sudo vi /usr/lib/systemd/system/mariadb.service 
# Prevent accessing /home, /root and /run/user 
#ProtectHome=true 
ProtectHome=false


// 시작
sudo systemctl start mysql

// 만약 Warning: mariadb.service changed on disk. Run 'systemctl daemon-reload' to reload units. 에러 발생 시
sudo systemctl daemon-reload 후 다시 시도.
```




번외 MariaDB 삭제

```
// 삭제
sudo apt purge mariadb-*
sudo apt autoremove
sudo apt purge mysql-common
sudo reboot

// 재설치
sudo apt install mariadb-server --fix-missing --fix-broken
```
