EC2 서비스 설치 및 서버 초기 셋팅을 위한 문서
하도 많이 삭제해서;;

1. 디스크 마운트
```
> lsblk

loop0         7:0    0  33.3M  1 loop /snap/  
loop1         7:1    0  55.5M  1 loop /snap/   
loop2         7:2    0  70.4M  1 loop /snap/ 
loop3         7:3    0  32.3M  1 loop /snap/ 
nvme2n1     259:0    0 279.4G  0 disk 
nvme1n1     259:1    0 279.4G  0 disk 
nvme0n1     259:2    0     8G  0 disk
└─nvme0n1p1 259:3    0     8G  0 part /

디스크 용량의 경우 300G SSD 2개로 서버를 구성하였고,
lsblk 를 통해 확인해보면 nvme2n1 과 nvme1n1 이 있는걸 확인할 수 있다.
현재는 Linux os에 마운트 되어있지 않은 상황.
```

2. 파티션된 하드디스크 초기화 및 마운트
```
sudo mkfs -t ext4 /dev/nvme1n1
sudo mkfs -t ext4 /dev/nvme2n1

sudo mkdir /data
sudo mkdir /app
sudo chmod 777 /data
sudo chmod 777 /app

sudo mount /dev/nvme1n1 /data
sudo mount /dev/nvme2n1 /app
```

3. 재시작 이후에도 자동으로 마운트가 되도록 설정
```
sudo vi /etc/fstab

/dev/nvme1n1            /data    ext4   defaults,noatime        1 1
/dev/nvme2n1            /app     ext4   defaults,noatime        1 1
```

4. 유저 생성 및 sudo 그룹 부여
```
sudo adduser USER_NAME
sudo usermod -aG sudo USER_NAME
```

5. Timezone 설정
```
sudo timedatectl set-timezone Asia/Seoul
```
