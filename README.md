
위의 아키텍처와 같이 React, NodeJS, MySQL로 이루어진 풀스택 어플리케이션에 대한 CI/CD 파이프라인과 Prometheus, Grafana를 통한 모니터링 시스템, 로깅 시스템을 구축해보는 프로젝트입니다
## 📝 Summary

1. 풀스택 어플리케이션 GKE 환경에서 배포
2. CI/CD 파이프라인 구축
3. 모니터링 시스템 구축
4. 로깅 시스템 구축

<br>

## 💻 Development Environment
- React 16.13.1
- Node.js 1.18.3
- Docker 24.0.6
- MySQL:8
- GitHub
- Jenkins
- ArgoCD
- Kubernetes
- Prometheus
- Grafana (+Loki)

<br>

## ⭐️ 단계별 목표

** 자동화 된 어플리케이션 배포 

EKS 클러스터를 사용하여 프로젝트 환경을 구축하여 쿠버네티스를 사용해 자동화 하는 환경을 구축

---

CI/CD 파이프라인은 어플리케이션 소스 코드 GitHub 리포지토리, Kubernetes GitHub 리포지토리, Jenkins, Docker Hub를 사용하여 구축합니다.

CI/CD 파이프라인 실행 과정은 다음과 같이 진행됩니다.

1. 어플리케이션 소스 코드를 **GitHub 리포지토리에 푸시**하면 **Jenkins 서버에 Webhook 요청**을 보냅니다.

2. Jenkinsfile을 작성하여 푸시된 코드를 통하여 **Docker Image를 빌드**하고 빌드된 이미지를 도커 허브에 Push 합니다.
   
    - (+) Jenkins 서버는 빌드된 이미지의 태그 값으로 쿠버네티스 메니페스트가 저장되어 있는 **GitHub 리포지토리를 수정**해줍니다.

3. **ArgoCD**가 GitHub의 변경된 메니페스트를 감지하고 **메니페스트와 Cluster의 Sync**를 맞춥니다.


---

**Third! 모니터링 시스템 구축**
모니터링 시스템은 Prometheus와 Grafana를 사용해 구축합니다.

모니터링 시스템을 구축하기 위한 진행 계획은 다음과 같습니다.

**쿠버네티스 환경에서 Prometheus와 Grafana 구축 및 Traffic 매트릭 생성**

**첫 번째 단계**: Prometheus와 Grafana 설치 및 구성

- Prometheus와 Grafana를 쿠버네티스 클러스터에 배포합니다.

- Prometheus 설정 파일을 수정하여 수집 대상을 정의합니다. (WAS자체에서 따로 Exporter 설정을 하지 않습니다)

**두 번째 단계**: Prometheus API를 활용하여 수집기 개발

- Prometheus API를 사용하여 WAS에서 오고가는 Network Packet을 크롤링합니다.
- 그리고 알맞게 DBMS에 하루 단위로 Insert하는 Job을 개발합니다.

**세 번째 단계**: Grafana 대시보드 구성

- Grafana를 열어 새로운 대시보드를 생성합니다.

- 대시보드에 생성한 Traffic 매트릭을 시각화하기 위한 패널을 추가합니다.

그래프 또는 표 등을 사용하여 Traffic의 추이를 표시합니다.
```

