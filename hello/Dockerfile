# FROM 은 맨 앞에 와야하는 이미지 로컬에 없어도 받아온다.

# 이미지 여러개도 사용이 가능하다. (보통 빌드와 파일포함을 분리 하고자 위해서)

# hello.c의 이미지 파일 예시

FROM ubuntu:22.04 AS builder

RUN apt update
RUN apt install -y gcc
WORKDIR /tmp
ADD hello.c ./
RUN GCC hello.c -static -o hello

FROM alpine:3.16.2
WORKDIR /tmp
COPY --from=builder /tmp/hello ./

CMD["/tmp/hello"]
