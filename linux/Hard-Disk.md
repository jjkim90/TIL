2. 1. # 하드디스크 관리
    
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
    
       ![Untitled](C:\Users\MIT-김재준\Downloads\Export-32638809-7c54-4167-9a27-e97dd4ceaf18\하드디스크 관리 16ee87d214204310b5fff9682476c746\Untitled.png)
    
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
    
       ![하드디스크 추가](C:\Users\MIT-김재준\Downloads\Export-32638809-7c54-4167-9a27-e97dd4ceaf18\하드디스크 관리 16ee87d214204310b5fff9682476c746\Untitled 1.png)
    
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
    
       ![파티션 할당](C:\Users\MIT-김재준\Downloads\Export-32638809-7c54-4167-9a27-e97dd4ceaf18\하드디스크 관리 16ee87d214204310b5fff9682476c746\Untitled 2.png)
    
       파티션 할당
    
       1. 파일시스템 ext4 형식으로 생성.
          1.  이 과정이 포맷이라고 생각하면 됨.
          2.  `mkfs -t ext4 /dev/sdb1` or `mkfs.ext4 /dev/sdb1` 명령 입력.
          3.  페도라는 ext2, ext3, ext4, xfs 파일시스템을 사용할 수 있음. ext4, xfs가 향상된 파일시스템임.
    
       ![파일시스템 생성](C:\Users\MIT-김재준\Downloads\Export-32638809-7c54-4167-9a27-e97dd4ceaf18\하드디스크 관리 16ee87d214204310b5fff9682476c746\Untitled 3.png)
    
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
    
       # 여러 개의 하드디스크를 하나처럼 사용하기
    
       ## RAID의 정의와 개념
    
       Redundant Array of Inexpensive/Independent Disks의 약자.
    
       RAID는 여러 개의 하드디스크를 하나의 하드디스크처럼 사용하는 방식을 말함. RAID의 종류는 크게 하드웨어 RAID와 소프트웨어 RAID로 나눌 수 있음.
    
       ### 하드웨어 RAID
    
       하드웨어 제조업체에서 여러 개의 하드디스크를 연결한 장비를 만들어서 그 자체를 공급하는 것. 좀 더 안정적이고, 각 제조업체에서 기술 지원을 받을 수 있기에 많이 선호하는 방법. 상당히 고가임.
    
       ### 소프트웨어 RAID
    
       고가 하드웨어 RAID의 대안. 하드디스크만 여러 개 있으면 운영체제에서 지원하는 방식으로 RAID를 구성하는 방법을 말함. 하드웨어 RAID에 비해 신뢰성이나 속도 등이 떨어지지만 훨씬 저가임.
    
       ## RAID 레벨
    
       실무에서 주로 사용되는 방식은 Linear RAID, RAID 0, RAID 1, RAID 5와 RAID 5의 변형인 RAID 6, 그리고 RAID 1과 0의 혼합인 RAID 1+0 등이 있음.
    
       ### 단순 볼륨
    
       하드디스크 하나를 볼륨 1개로 사용하는 방식. 위에서 한 실습이 단순 볼륨임.
    
       ### Linear RAID와 RAID 0
    
       Linear RAID는 1번째 하드디스크가 모두 채워진 후에 2번째 하드디스크를 사용하기 시작함. 각 하드디스크의 용량이 달라도 전체 용량을 문제 없이 사용할 수 있어 공간 효율성이 100%임.
    
       RAID 0은 1번째, 2번째, 3번째 하드디스크에 동시에 저장됨. 일련의 데이터들이 비트 혹은 블록 단위로 순차 회전하면서 저장됨. 이렇게 여러 개의 하드디스크에 동시에 저장되는 방식을 스트라이핑 방식(Stripping)이라고 함. 속도 측면에서 RAID 방식 중 성능이 가장 뛰어남. 하드디스크 개수가 가진 총 용량을 모두 사용하므로 공간 효율성이 아주 좋음. 3개의 하드디스크 중 하나가 고장 날 경우에는 모든 데이터를 잃어버리게 됨. 빠른 성능을 요구하되, 혹시 전부 잃어버려도 큰 문제가 되지 않는 자료를 저장하는 데 적절한 방식임. 동일한 용량으로만 구성할 수 있어 용량이 서로 다른 하드디스크로 구성할 때 용량 낭비가 발생하게 됨.
    
       ### RAID 1
    
       미러링 방식(Mirroring)을 사용함. 2개의 하드디스크 중 하나가 고장 나도 데이터가 손실되지 않음. 이것을 결함 허용을 제공한다(Fault-tolerance)고 표현함. 단점은 실제 계획하는 것보다 2배 큰 저장 공간이 필요하다는 것임. 하드디스크가 고장 나도 없어져서는 안 될 중요한 데이터가 있을 때 사용하면 좋음. 똑같은 데이터를 동시에 양쪽에 저장하기 때문에 저장 속도는 보통임.
    
       ### RAID 5
    
       RAID 1처럼 데이터의 안전성이 어느 정도 보장되면서 RAID 0처럼 공간 효율성도 좋은 방식이 RAID 5 방식임. 최소 3개 이상의 하드디스크 필요. 보통 5개 이상의 하드디스크로 구성함. 하드디스크에 오류가 발생하면 패리티를 이용해서 데이터를 복구할 수 있음.
    
       RAID 5 방식의 장점은 어느 정도의 결함을 허용하면서 저장 공간의 효율도 좋다는 것임. 하드디스크의 개수를 N개라고 하면 N-1만큼의 공간을 사용할 수 있음. 여러 개 하드디스크로 RAID 5를 구성할수록 저장 공간의 효율이 높아짐.
    
       ### RAID 6
    
       RAID 5 방식의 개선으로 2개의 패리티를 사용함. 공간 효율성은 RAID 5보다 약간 낮지만, 2개의 하드디스크가 동시에 고장 나도 데이터에는 이상이 없도록 하는 방식임. 공간 효율은 약간 낮지만 데이터 신뢰도는 더욱 높아지는 효과를 갖음. 패리티 2개 생성해야 하므로 내부적인 쓰기 알고리즘이 복잡해져서 성능은 RAID 5와 비교했을 때 약간 떨어짐.
    
       최소 4개의 하드디스크로 구성해야 함. 하드디스크 N개일 때 N-2 만큼의 용량을 사용할 수 있음.
    
       ### 그 외 RAID를 조합하는 방법
    
       RAID 1+0, RAID 0+1, RAID 1+6 등이 있음.
    
       ## Linear RAID, RAID 0, RAID 1, RAID 5 구현
    
       ### 하드디스크 9개 준비
    
       - VMware 초기화 후 하드디스크 9개 추가. 2기가 1개, 1기가 8개. SCSI 0:1부터 SCSI 0:10까지 생성됨 (SCSI 0:7은 VMware 예약되어 있어 사용할 수 없음).
       - 터미널에서 `ls -l /dev/sd*` 명령 입력. 방금 장착한 SCSI 장치가 /dev 디렉터리에 있는 것을 확인. 원래 Fedora가 설치된 /dev/sda를 제외하고 /dev/sdb ~ /dev/sdj까지 9개의 장치를 확인할 수 있음.
    
       ![하드디스크 9개 준비.](C:\Users\MIT-김재준\Downloads\Export-32638809-7c54-4167-9a27-e97dd4ceaf18\하드디스크 관리 16ee87d214204310b5fff9682476c746\Untitled 4.png)
    
       하드디스크 9개 준비.
    
       ### Linear RAID 구축
    
       1. `fdisk` 명령으로 파티션 설정.
       2. `mdadm —create /dev/md9 —level=linear —raid-devices=2 /dev/sdb1 /dev/sdc1` 명령으로 RAID 생성.
       3. `mdadm —detail —scan` 명령으로 RAID 확인.
       4. `mkfs.ext4 /dev/md9` 명령으로 파티션 장치의 파일 시스템 생성. 포맷.
       5. `mkdir /raidLinear` 명령으로 마운트할 디렉터리 생성. `mount /dev/md9 /raidLinear` 명령으로 마운트. `df` 명령으로 확인.
       6. `vi /etc/fstab` 명령으로 vi 에디터 열어 마지막 부분에 “/dev/md9    /raidLinear    ext4    defaults    0    0” 추가하고 저장.
       7. `mdadm —detail /dev/md9` 명령으로 구축한 Linear RAID 자세히 확인.
    
       ### RAID 0 구축, RAID 1 구축, RAID 5 구축
    
       Linear RAID 구축 방식과 거의 동일함.
    
       `mdadm —create /dev/md? —level=? —raid-devices=? /dev/sd? /dev/sd? …` 물음표 부분만 바꿔주면 됨.
    
       ## Linear RAID, RAID 0, RAID 1, RAID 5에서 문제 발생과 조치 방법
    
       ### Linear RAID, RAID 0, RAID 1, RAID 5의 문제 발생 테스트
    
       1. 테스트를 위해 VMware에서 disk0-2, disk0-4, disk0-6, disk0-8 총 4개의 하드디스크 삭제.
       2. 부팅하면 정상적으로 부팅되지 않고 응급 모드(Emergency mode)로 접속됨.
       3. 접속하기 위해 root 사용자의 비밀번호를 입력하고 Enter.
       4. `ls -l /dev/sd*` 명령으로 장치 이름 확인. /dev/sdj까지 9개였던 것이 /dev/sdf까지 5개만 남았음.
       5. `df` 명령으로 확인해보면 기존의 /raidLinear, /raid0 디렉터리는 보이지 않고, 결함 허용을 하는 /raid1, /raid5만 보임.
       6. `mdadm —detail /dev/md1` 명령으로 RAID 1 장치가 어떻게 작동하는지 상세 확인. 총 2개 중 1개 removed 확인 가능함.
       7. `mdadm —stop /dev/md9` 명령과 `mdadm —stop /dev/md0` 명령으로 결함 허용이 없는 RAID 중지. 
       8. `vi /etc/fstab` 명령으로 fstab을 vi에디터로 열고 md9와 md0 입력된 줄에서 dd 2번 입력하여 삭제. 저장하고 종료. 재부팅하면 정상적으로 X 윈도로 부팅됨.
    
       ### RAID 1, RAID 5의 원상 복구
    
       고장 난 장치를 새로운 하드디스크로 교체
    
       1. VMware에서 하드디스크 2개 추가.
       2. mdadm —detail /dev/md1 명령으로 RAID 1 장치 구성 확인하기. 장치가 /dev/sdd에서 /dev/sdf로 변했음.
       3. `ls -l /dev/sd*` 명령으로 파티션 할당되지 않은 장치 확인. 뒤에 숫자 없는 것.
       4. `fdisk /dev/sdg` 명령으로 파티션 할당. 파일시스템 유형을 Linux raid autodetect 유형으로 지정.
       5. `mdadm /dev/md1 —add /dev/sdg1` 명령으로 RAID 1 재구성. 
       6. `mdadm —detail /dev/md1` 명령으로 RAID 장치 잘 작동되는지 확인.
       7. RAID 5 역시 같은 방식으로 재구성.
       8. `reboot` 명령으로 재부팅하면 정상적으로 부팅됨.
    
       ## 고급 RAID 레벨
    
       ### RAID 6과 RAID 1+0 개념
    
       ### RAID 6과 RAID 1+0의 문제 발생 테스트
    
       # LVM
    
       ## LVM 개념 이해
    
       ## LVM의 구현
    
       # RAID에 Fedora 설치하기
    
       # 사용자별로 공간 할당
    
       ## 쿼터의 개념과 실습