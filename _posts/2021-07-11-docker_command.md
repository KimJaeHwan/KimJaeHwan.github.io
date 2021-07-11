---
layout: single
title: "docker 이미지 관리"

categories: ['docker']
---



# docker 기본 명령어


## docker 이미지 관리 

******



### docker 이미지 검색

------

- docker는 [dockerhub](https://hub.docker.com/)에서 각종 이미지들을 관리하고 있으며 해당 사이트에서 원하는 이미지를 가져올 수 있다.
- <code># docker search &lt;image Name&gt;</code>



### docker  이미지 설치 

------

- <code># docker pull &lt;image Name&gt;</code>
- <code># docker search &lt;image Name&gt;:<tag></code> 



### 설치된 docker 이미지 확인

------

- <code># docker images </code>



### 설치된 docker 이미지 제거

------

이미지 제거시 해당 이미지로 run한 모든 컨테이너들이 삭제 되어있어야한다.

- <code># docker rmi &lt;image ID&gt;</code>



### docker container 시작

------

docker container를 사용하기 위해서는 사용하고자하는 docker image와 그에 맞는 적절한 옵션을 넣어주어야 한다.

- <code># docker run &lt;image ID&gt; &lt;option&gt; </code>

#### Ex)

- <code># docker run -it ubuntu /bin/bash</code>
  - ubuntu이미지를 /bin/bash 터미널로 시작한다.
- <code># docker run -d -p 88:80 owncloud </code>
  - owncloud 이미지를 백그라운드(-d 옵션)로 88번포트( -p 옵션)를 사용하여 시작한다.



### docker 이미지 저장

------

현재 가지고 있는 이미지를 .tar형식으로 저장하여 공유및 관리할수 있다.

- <code># docker save -o &lt;image.tar&gt; &lt;image ID&gt;</code>



### docker 이미지 로드

------

save명령어로 저장된 tar파일들을 다시 docker 이미지로 로드할 수 있다.

- <code># docker load -i &lt;image.tar&gt;</code>



## docker 컨테이너  관리

------

### docker 컨테이너 확인

------

- 실행중인 컨테이너만 확인
  - <code># docker ps </code>
  - <code># docker container ls </code>
- 모든 컨테이너 확인 
  - <code># docker ps -a </code>
  - <code># docker container ls -a </code>

### docker 컨테이너 시작

------

- <code># docker start &lt;container ID&gt; </code>
- <code># docker start &lt;container Name&gt; </code>

### docker 컨테이너 종료

------

- <code># docker stop &lt;container ID&gt;</code>
- <code># docker stop &lt;container Name&gt;</code>



### docker 컨테이너 이미지화

------

현재 실행중인 컨테이너 또는 실행중이지 않은 컨테이너 들을 현재 상태그래도 docker 이미지로 만들 수 있으며, 이렇게 만들어진 이미지로 다른 사용자들과 공유 할 수 있다. 

tag에는 자신이 원하는 버전을 작성할 수 있으며 tag를 사용하지 않는 경우  기본적으로 latest로 붙게 된다.

- <code># docker commit &lt;container ID&gt; &lt;image Name&gt;:<tag></code> 

  

## docker 컨테이너, 이미지 모두 삭제하기 

------

- docker를 사용하다보면 불필요한 컨테이너와 이미지들이 쌓이기 마련이다. 한번에 정리하기 위해 아래와 같은 명령어를 사용하면 유용하다.

1. 모든 도커 컨테이너 삭제
   - <span># docker stop $(docker ps -a -q)</span>
   - <span># docker rm $(docker ps -a -q)</span>
2. 모든 도커 이미지 삭제
   - <span># docker rmi $(docker images -q) </span>



## docker  이미지 업로드 및 배포

------

docker hub에 자신이 만든 이미지를 업로드하여 공유할 수 있다.



### docker hub로그인

------

docker hub에 업로드를 하기 위해서는 계정이 필요하며 이는 [dockerhub홈페이지](https://hub.docker.com/)에서 진행할 수 있다.

- <code># docker login</code>



### docker 이미지 업로드

------

docker 이미지를 업로드하기 위해서는 push명령어를 사용하며 이떄 로그인한 자신 ID를  넣어주어야 정상적으로 업로드된다.

- <code># docker image push &lt;account&gt;/&lt;image Name&gt;</code>
