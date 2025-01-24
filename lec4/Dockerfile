# WORKDIR, COPY
# 이미지 생성 : docker build -t test_ub:4.0 ./lec4
# 컨테이너 생성 및 구동 : docker run -it --name test_ub40 test_ub:4.0
# -it만 적용하여서, 백그라운드에서 가동 X, 1회성 사용, 별도로 가동 명령 수행 필요(1회성)
# 진입 후 => ls /home/ubuntu/app/b -al => 확인
# ENV는 Docker compose 수행시 처리
FROM ubuntu

# 워킹 디렉토리 지정
WORKDIR /home/ubuntu/app
# 이후 ADD, COPY, CMD, ... 모두 영향을 받는다

# 카피 -> 소스나 리소스를 이미지 상에 특정 위치에 카피
# 현재 디렉토리상의 모든 내용을 작업디렉토리 하위로 모두 카피
COPY . .
