# 하드디스크 관리

# 하드디스크 한 개 추가하기

## IDE 장치와 SCSI 장치 구성

메인보드의 IDE 0, IDE 1 슬롯에는 각각 2개의 IDE 장치를 장착할 수 있음. 그래서 IDE 장치는 총 4개를 장착할 수 있음. 이 4개의 장치를 표기할 때는 주로 IDE 0:0, 0:1, 1:0, 1:1 방식으로 표기함.

VMware는 기본적으로 IDE 1:0에 CD/DVD 장치를 장착해줌. IDE 장치(주로 하드디스크)를 추가하려면 나머지 비어 있는 3개의 장치에 장착해야 함.

VMware는 메인보드에 4개의 SCSI 슬롯을 제공해줌. SCSI 0번 슬롯의 경우 SCSI 0:0 ~ SCSI 0:15 (0:7제외)까지 15개 하드디스크를 장착할 수 있음. SCSI 1,2,3번 슬롯도 각각 사용할 수 있으므로 총 4x15=60개의 SCSI 하드디스크를 사용할 수 있음. 가상머신으로 구축한 리눅스 서버는 SCSI 0:0에 2개의 파티션을 사용 중임.

리눅스에서는 처음 장착된 SCSI 하드디스크의 이름을 /dev/sda 라고 부름. 추가로 SCSI 하드디스크가 장착되면 /dev/sdb, /dev/sdc, … 등으로 부름. 파티션은 순차적으로 1, 2, 3, 4를 붙여서 /dev/sda1, /dev/sda2로 부름. 처음 나눴던 2개의 파티션이 여기에 해당됨.

SCSI 하드디스크를 물리적으로는 /dev/sda, /dev/sdb, … 형식으로 부르면 되고, 그 장치에 파티션이 나눠진 것을 논리적으로는 /dev/sda1, /dev/sda2, … , /dev/sdb1, /dev/sdb2, … 형식으로 부르면 됨.

## 하드디스크 추가하기

파티션

하디디스크를 사용하려면 먼저 파티션을 설정해야 함. 파티션은 Primary 파티션과 Extended 파티션 두 가지가 있음. 1개의 하디디스크는 4개의 Primary 파티션까지 설정할 수 있음. 만약 파티션을 5개 이상 설정하고 싶다면 3개의 Primary 파티션과 1개의 Extended 파티션으로 설정한 후에 Extended 파티션을 2개 이상의 Logical 파티션으로 설정해야 함.



VMware를 통해 1GB 용량의 작은 하드디스크 하나를 추가함. 추가한 하드디스크의 이름은 /dev/sdb가 됨. 추가 하드디스크 장치의 물리 이름인 /dev/sdb를 사용하려면 최소한 1개 이상의 파티션으로 나눠야 함. 1개의 파티션으로만 나누면 논리 파티션의 이름은 /dev/sdb1이 됨.

리눅스에서는 이 파티션을 그냥 사용할 수 없으며 반드시 특정한 디렉토리에 마운트 시켜야만 사용할 수 있음.

리눅스에서 하드디스크 1개를 추가할 때 전체 흐름

1. 물리적인 하드디스크 장착 SCSI 0:1 /dev/sdb
2. `fdisk` 명령 입력
3. 파티션 설정 /dev/sdb1
4. 파일시스템 생성 `mkfs.ext4`
5. `mount`
6. /etc/fstab에 등록
7. reboot

하드디스크 1개를 장착해서 사용하기 실습

1. VMware 디스크 추가로 SCSI 0:1 (/dev/sdb)에 1GB 하드디스크 하나를 장착.



하드디스크 추가

1. 추가한 물리 디스크에 1개의 파티션 할당
    1. `ls /dev/sd*` 명령 입력해 현재 상태 확인. sda는 sda1, sda2로 파티션 분할되어 있고 sdb는 현재 파티션이 없음.
    2. `fdisk /dev/sdb` 명령 입력. fdisk는 fixed disk로 파티션 설정 명령어임.
    3. m 누르면 매뉴얼 나옴
    4. n 눌러서 새로운 파티션 분할
    5. p 눌러서 Primary 파티션 선택
    6. 1 눌러서 파티션 번호 1번 선택
    7. First sector 엔터 눌러서 기본값. 2048이 시작 섹터인 이유는 제일 앞의 0~2047 부분은 시스템 성능 향상을 위해 사용하지 않는 부분이기 때문임.
    8. Last sector 엔터 눌러서 기본값
    9. p 눌러서 설정된 내용 확인
    10. w 눌러서 설정 저장
    11. `ls /dev/sd*` 명령 입력해 sdb에 파티션 할당되어 sdb1 생긴 것을 확인.



파티션 할당

1. 파일시스템 ext4 형식으로 생성.
    1.  이 과정이 포맷이라고 생각하면 됨.
    2. `mkfs -t ext4 /dev/sdb1` or `mkfs.ext4 /dev/sdb1` 명령 입력.
    3. 페도라는 ext2, ext3, ext4, xfs 파일시스템을 사용할 수 있음. ext4, xfs가 향상된 파일시스템임.



파일시스템 생성

1. /dev/sdb1을 디렉토리에 마운트.
    1. `mkdir /add1g` 명령 입력해 마운트할 디렉토리 생성.
    2. `cp anaconda-ks.cfg /root/add1g/test1` 명령 입력해 anaconda-ks.cfg 파일을 test1이라는 이름의 파일로 바꿔 마운트할 디렉토리에 복사.
    3. `ls -l /add1g/` 명령 입력해 test1 파일 확인. test1 파일은 기존의 /dev/sda2에 저장된 상태임.
    4. `mount /dev/sdb1 /root/add1g` 명령 입력해 포맷이 완료된 /dev/sdb1 장치를 /root/add1g 디렉토리에 마운트.
    5. `cp anaconda-ks.cfg /root/add1g/test2` 명령 입력해 test2 파일을 /root/add1g 디렉토리에 복사.
    6. `umount /dev/sdb1` 명령 입력해 마운트 해제. → `ls -l /root/add1g` 명령 입력해 test1 파일 그대로 있는지 확인.
    7. 최초 add1g 디렉토리 생성 시 add1g의 변경사항 test1은 /dev/sda2에 기록됨. 기본으로 마운트되어 있기 때문. → /dev/sdb1 생성 후 add1g 디렉토리에 마운트하면 add1g의 새로운 변경사항인 test2는 /dev/sdb1에 기록됨. → 마운트 해제하고 add1g 디렉토리 조회 시 /dev/sda2에 기록된 내용인 test1 확인 가능. → /dev/sdb1을 add2g라는 새로운 디렉토리에 마운트하면 add2g 디렉토리에서 test2 확인 가능.
2. 부팅 시 /dev/sdb1 장치가 항상 /root/add1g에 마운트되어 있도록 설정하기.
    1. `vi /etc/fstab` 명령 입력. fstab은 file system table을 나타냄.
    2. 제일 아랫부분에 “/dev/sdb1 /root/add1g ext4 defaults 0 0” 추가.
    3. 내용의 6개 필드는 ‘장치 이름’, ‘마운트될 디렉토리’, ‘파일 시스템’, ‘속성’, ‘dump 사용 여부’, ‘파일시스템 체크 여부’를 의미함.
    4. 
    5. 수정한 /etc/fstab 파일을 저장한 후 reboot.
    6. `ls -l /root/add1g` 명령 입력해 디렉토리를 확인하면 test2 파일 확인 가능.