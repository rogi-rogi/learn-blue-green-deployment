# blue-green-deployment
블루-그린 배포 전략을 통한 무중단 배포 예시 저장소입니다.<br/>

## Project Setting
배포한 예제 프로젝트 설정은 다음과 같습니다.<br/>

+ 프로그래밍 언어 : Java 17
+ 빌드도구 : Gradle(Groovy DSL)
+ JDK 버전 : Oracle Open JDK 17.0.10
+ 패키징 방식 : JAR

## Configuring Deployment Environments

이 프로젝트는 AWS EC2 인스턴스에서 Docker를 사용하여 무중단 배포를 구현합니다.<br/>
Docker 내부에서 Nginx를 사용한 블루-그린 배포 전략을 구현했으며, Github Action을 통해 CI/CD 파이프라인을 구축해 배포 작업을 자동화했습니다.<br/>

## Target

AWS EC2 프리티어를 활용해 Zero-Spend Budget을 준수하는 블루-그린 무중단 배포를 하는 것이 목표입니다.<br/>

## 가상 메모리 활용

AWS EC2 프리티어는 [t2.micro](https://aws.amazon.com/ko/ec2/instance-types/t2/)를 사용하며, RAM이 1.0GiB로 매우 작습니다.
배포 또는 런타임 중 메모리의 부족으로 인해 시스템이 크래시되거나 성능 저하가 발생하는 경우를 방지하고자, 적절한 가상 메모리를 할당해 메모리 스왑을 활성화하고 이러한 문제를 해결했습니다. 

특히 RAM이 부족한 경우 EC2의 재시작 명령이 즉시 작동하지 않으며, 인스턴스의 중지를 할 경우 탄력적 IP가 고아 상태가 되어 요금이 발생할 수 있습니다.

## Repository Secrets Setting

+ SECRET_FILE : appcliation-secret.yml을 base64로 encode해 저장합니다.
+ DOCKERHUB_USERNAME : DockerHub 계정 이름을 저장합니다.
+ DOCKERHUB_TOKEN : DockerHub 계정 접근 토큰을 저장합니다.
+ DOCKERHUB_REPO_NAME : DockerHub내에 생성된 프로젝트 저장소 이름을 저장합니다.
+ SERVER_USER_NAME : EC2서버의 사용자 이름입니다.
+ LIVE_SERVER_IP : EC2의 공개 IP 또는 탄력적 IP를 저장합니다.
+ SSH_KEY : LIVE_SERVER_IP에 접속하기 위한 키를 저장합니다.

+ nginxserver : EC2 내부의 Docker에서 실행 중인 nginx 컨테이너 이름입니다.
+ blue/green : EC2 내부의 Docker에서 실행 중인 서버 컨테이너의 이름입니다. 배포하고자 하는 서버입니다.
