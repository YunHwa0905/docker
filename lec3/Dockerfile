# RUN, CMD, EXPOSE 테스트
# 실제는 nginx 이미지 사용
# 여기서는 실습상 깨끗한 리눅스에 일일이 세팅하는 연습

# 1. 베이스 이미지
FROM ubuntu

# 2. 패키지 최신 업데이트
RUN apt-get -y update && apt-get -y upgrade

# 3. nginx 설치
RUN apt-get -y install nginx

# 4. 웹 서버 -> 포트 설정 80(외부):80(내부)
EXPOSE 80

# 5. 리액트 빌드낸 결과물 COPY OR ADD -> 생략

# 6. 컨테이너가 만들어지고, 실행될 때 nginx 가동되길 원한다
CMD ["nginx","-g","daemon off;"]