# RUN은 설정한 이미지 위에 명령함. 실행 내역의 이미지의 히스트리에 기록

RUN apt install -y nginx
RUN echo "Hello Docker" > /tmp/hello
RUN curl -sSL https://go.dev/dl/go1.19.2.src.tar.gz | tar -v -C /usr/local -xz
RUN git clone https://github.com/docker/docker.git


# 셸없이 실행
RUN ["apt", "install", "-y", "nginx"]
RUN ["/user/local/bin/hello", "--help"]

# CMD는 컨테이너가 시작되었을 때 docker start or run 실행 시 Dockerfile에서 한번만 사용이 가능하다.
CMD touch /home/hello/hello.txt

# 셸없이 실행
CMD ["redis-server"]
CMD ["mysqld", "--datadir=/var/lib/mysql", "--user=mysql"]

# ENTRYPOINT랑 같이 있으면 매개변수만 전달 해준다.
ENTRYPOINT ["echo"]
CMD ["hello"]

# ENTRYPOINT는 컨테이너 시작되었을 때 docker start or run 실행 시 명령 실행 Dockerfile에서 단 한번만 사용 가능
ENTRYPOINT touch /home/hello/hello.txt

# 셸없이 실행
ENTRYPOINT ["/home/hello/hello.sh"]
ENTRYPOINT ["/home/hello/hello.sh", "--hello=1", "--world=2"]

# CMD와의 차이점은 docker run에서 추가 명령어가 있으면 CMD는 무시되고, ENTRYPOINT는 무시되지 않는다.

# EXPOSE 는 호스트와 연결할 포트 번호를 설정한다. docker run 명령어의 --expose와 같음
# 포트를 외부에 노출하려면 docker run 명령어의 -p or -P 옵션을 사용한다.
# 두 개 이상 저장 가능
EXPOSE 80
EXPOSE 443

EXPOSE 80 443


# ENV는 환경 변수를 설정한다. RUN, CMD, ENTRYPOINT에 적용한다.
# docker run 명령어의 -e or --env와 같으며 여러번 사용 가능하다.
ENV GOPATH /go
ENV PATH /go/bin:$PATH

ENV HELLO 1234
CMD echo $HELLO

# ADD 는 파일에 이미지를 추가한다.
# ADD 복사할 파일 경로 이미지에 파일이 위치할 경로 순
# 파일 경로에 디렉토리 입력 시 디렉토리가 복사된다.
# 파일 경로에 RUL 설정가능하다.
# 위치할 경로의 마지막에  / 가 있으면 디렉토리가 생성되고 파일은 그 아래에 복사된다.
# 로컬에 있는 압축파일은 해제 후 tar를 풍러 추가된다. 단, 인터넷 경로면 tar이 그대로 추가된다.
# 파일이 위치할 경로는 반드시 절대 경로다.
# 현재 디렉토리를 추가할 때 .dockerignore 파일에 설정한 파일과 디렉토리는 제외된다.
ADD hello-entrypoint.sh /entrypoint.sh

# 파일 URL 압축 해제하여 추가하는 예시
FROM ubuntu:latest
RUN apt update
RUN apt install -y curl
RUN curl http://zlib.net/zlib-1.2.8.tar.gz | tar -xz

FROM ubuntu:latest
RUN apt update
RUN apt install -y wget
RUN wget http://zlib.net/zlib-1.2.8.tar.gz -O - | tar -xz

# tar.* 계열의 압축을 해제하는 방법
# ubuntu에서는 bzip2가 설치되어 있고, xz는 설치 X xz-utils 패키지를 설치한다.
RUN curl http://example.com/hello.tar | tar -x
RUN curl http://example.com/hello.tar.gz | tar -xz
RUN curl http://example.com/hello.tar.bz2 | tar -xj
RUN curl http://example.com/hello.tar.xz | tar -xJ

# ADD 예시
ADD hello-dir /hello-dir
ADD zlib-1.2.8.tar.gz /
ADD hello.zip /
ADD http://example.com/hello.txt /hello.txt
ADD *.txt /root/

# COPY는 파일을 이미지에 추가한다. 압축파일을 추가 할 때  압축을 해제하지 않고, URL도 추가 가능하다.
# 복사할 파일 경로 이미지에서 파일이 위치할 경로순
# 절대경로는 사용하지 않는다.
# 나머지는 ADD와 사용이 같다.
COPY hello-entrypoint.sh /entrypoint.sh
COPY hello-dir /hello-dir
COPY zlib-1.2.8.tar.gz /zlib-1.2.8.tar.gz
COPY *.txt /root/

# VOLUME은 디렉토리의 내용을 컨테이너에 저장하지 않고, 호스트에 저장하도록 설정한다.
# 특정 디렉토리와 연결하려면 docker run 명령의 -v 옵션을 사용
VOLUME /data
VOLUME ["/data", "/var/log/hello"]

# USER는 명령을 사용할 사용자 계정을 설정한다. RUN, CMD, ENTRYPOINT가 적용된다.
# 중간에 변경이 가능하다.
USER nobody
RUN touch /tmp/hello.txt

USER root
RUN touch /hello.txt
ENTRYPOINT /hello-entrypoint.sh

# WORKDIR은 RUN, CMD, ENTRYPOINT의 명령이 실행 될 디렉토리를 설정한다.
# 중간에 다른 디렉토리를 설정 가능하다.
# 상대경로도 지정이 가능한데, 먼저 설정한 경로를 기준으로 디렉토리 변경한다 default = /
WORKDIR /var/www
WORKDIR /root
RUN touch hello.txt

WORKDIR /tmp
RUN touch hello.txt

WORKDIR var
WORKDIR www

RUN touch hello.txt

# ONBUILD는 생성한 이미지를 기반으로 다른 이미자가 생성 되었을 때 명령을 실행한다.
# 다음번에 이미지가 FROM으로 사용 될 때 명령을 예약하는 기능이라고 할수 있다.
# docker inspect 명령으로 이미지의 ONBUILD 설정이 가능하다.
# 설정은 상속되지 않고 바로 아래 자식만 해당된다.
ONBUILD RUN touch /hello.txt
ONBUILD ADD world.txt /world.txt

FROM ubuntu:latest
ONBUILD RUN touch /hello.txt
