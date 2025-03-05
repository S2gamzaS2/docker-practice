#### service-a 에서 service-b 호출

#### PORT
- service-a 18080:8080
- service-b 18081:8080

<br>

#### Docker 사용
```bash
# 1. 네트워크 생성
docker network create my-network

# 2. 프로젝트 빌드 (Gradle 빌드 도구를 사용하여 프로젝트 정리 후, JAR 파일 생성)
./gradlew clean bootJar

# 3. 이미지 생성 (각 디렉토리에서)
docker build -t img-service-a .
docker build -t img-service-b .

# 4. 컨테이너 생성
docker run -d --name service-a \
 --network my-network \
 -p 18080:8080 \
 img-service-a

docker run -d --name service-b \
 --network my-network \
 -p 18081:8080 \
 img-service-b
```

http://localhost:18080/hi
접속해서 service-a가 service-b를 호출하는지 확인

<br>

#### Docker Compose 사용
```bash
# 1. 기존 컨테이너 모두 중지 후 삭제
docker stop 컨테이너아이디
docker rm 컨테이너아이디

# 중지를 안하고 바로 삭제 - 위험
docker rm -f 컨테이너아이디

# 2. docker-compose.yml파일 생성
vi docker-compose.yml

# 3. Docker Compose 실행
docker compose up -d

```
http://localhost:18080/hi
접속해서 service-a가 service-b를 호출하는지 확인

    💡 Docker Compose를 사용하여 서비스를 실행하면, Docker Compose는 기본적으로 새 브릿지네트워크를 생성함
       네트워크 이름: directoryname_default
