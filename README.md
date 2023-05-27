docker 프로세스 스탑, 프로세스 종료, 이미지 삭제 한 번에 하는 법(linux, Git Bash 이용)

docker stop $(docker ps -q)

docker rm $(docker ps -a -q)

docker rmi -f $(docker images -q)

<br><br>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32aa08ad-b4b1-4cb7-ad04-fff3fc04aabb/Untitled.png)

<br><br>

docker run -d —name myubuntu ubuntu - (run: 실행, -d: 백 그라운드, -name: 이름 설정, ubuntu 다운로드와 동시에 실행)

docker run -dit —name myubuntu ubuntu - (-d뒤에 it를 붙임 i는 interaction, t는 터미널 안꺼지고 계속 실행되어 있음)

docker run -dit ubuntu bash도 됨
<br><br>

docker attach (container_id) - (해당 컨테이너에 접속)

docker exec -it (container_id) bash - ( httpd와 같은 서버에 접속하기 위해 exec -it 를 붙여서 bash로 실행) - 실행중인 컨테이너에 커맨드를 변경해서 접속할 수 있음

httpd는 while로 돌고 있어서 안꺼지지만 ubuntu는 os여서 실행할게 없어서 그냥 꺼짐

<br><br>

---

웹 서버와 OS 사용

1. os

docker run -dit ubuntu bash

docker attach 컨테이너ID

2. while process (httpd)

docker run -d -p 8080:80 httpd

docker exec -it 컨테이너ID bash

docker run -dit -p 8080:80 httpd - 이렇게 해도 됨

---
<br><br>


Volume 설정

docker run -d -p 8080:80 -v C:/users/HanYoonSoo/Desktop/docker:/usr/local/apache2/htdocs httpd

(앞에쓴 경로로 /usr/local/… 경로를 할당하는걸로 이해하면 될 듯)

<br><br>

ubuntu 실행된 상태로 빠져나오기(백그라운드에 남아있음)

ctrl + p + q

<br><br>
docker hub에 업로드

docker commit 컨테이너ID (도커 허브 이름)/(레포 이름):(태그)

docker push (도커 허브 이름)/(레포 이름):(태그)

hub에 올린거 다운받아서 실행

주소 복사한 뒤 docker run -dit hanyoonsoo/vim-ubuntu:1.0 같이 실행

<br><br>

docker 파일 만들어서 실행(예시)

1. Dockerfile이라는 확장자 없는 파일을 만든다.
    
    파일 내용(예시)
    
    FROM httpd
    
    COPY ./docker /usr/local/apache2/htdocs
    
    CMD [”httpd-foreground”]
    
2. 파일을 이미지로 빌드
    
    docker build -t webserver(image 이름) ./(현재 폴더에서 Dockerfile을 알아서 찾아줌)
    
<br><br>
파일을 이용하여 index.html을 연동하는 방법은 volume과 달리 직접 연동이되는 것이 아닌 파일만 복사이다.
