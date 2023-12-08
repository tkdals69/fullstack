# k8s-team-self-project

![architecture](https://github.com/moonstar0331/fullstack-app/assets/79830859/6743b464-d4f9-4eb4-8206-9ddf7cbfb365)


위의 아키텍처와 같이 React, NodeJS, MySQL로 이루어진 풀스택 어플리케이션에 대한 CI/CD 파이프라인과 Prometheus, Grafana를 통한 모니터링 시스템, 로깅 시스템을 구축해보는 프로젝트입니다.
프로젝트에 대한 진행과정과 트러블 슈팅은 **기술 블로그**를 통해서 확인하실 수 있습니다.

[해결과정 및 트러블슈팅](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%86%8C%EA%B0%9C)

[Kubernetes Manifest GitHub Repository](https://github.com/moonstar0331/fullstack-app-k8s-manifest)

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

**First! GKE 환경에서 풀스택 어플리케이션 배포**

이번 프로젝트는 GCP에서 VM 인스턴스 3개를 만들어서 직접 쿠버네티스 환경을 구축하는 것이 아닌

간편하게 **GKE 클러스터**를 만들어서 프로젝트 환경을 구축합니다.

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 -GKE 환경에 배포하기](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-GKE-%ED%99%98%EA%B2%BD%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)

---

**Second! CI/CD 파이프라인 구축**

CI/CD 파이프라인은 어플리케이션 소스 코드 GitHub 리포지토리, Kubernetes GitHub 리포지토리, Jenkins, Docker Hub를 사용하여 구축합니다.

CI/CD 파이프라인 실행 과정은 다음과 같이 진행됩니다.

1. 어플리케이션 소스 코드를 **GitHub 리포지토리에 푸시**하면 **Jenkins 서버에 Webhook 요청**을 보냅니다.

2. Jenkinsfile을 작성하여 푸시된 코드를 통하여 **Docker Image를 빌드**하고 빌드된 이미지를 도커 허브에 Push 합니다.
   
    - (+) Jenkins 서버는 빌드된 이미지의 태그 값으로 쿠버네티스 메니페스트가 저장되어 있는 **GitHub 리포지토리를 수정**해줍니다.
    - [Kubernetes Manifest GitHub Repository](https://github.com/moonstar0331/fullstack-app-k8s-manifest)

3. **ArgoCD**가 GitHub의 변경된 메니페스트를 감지하고 **메니페스트와 Cluster의 Sync**를 맞춥니다.

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 - CI/CD 파이프라인 구축:1](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-CICD-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8-%EA%B5%AC%EC%B6%951)

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 - CI/CD 파이프라인 구축:2](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-CICD-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8-%EA%B5%AC%EC%B6%952)

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 - CI/CD 파이프라인 구축:3](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-CICD-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8-%EA%B5%AC%EC%B6%953)

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
📌 해당 대시보드의 패널을 개발합니다.

- 클러스터 전체에 떠있는 Pod 개수 추적
- 클러스터 전체에서 각 Pod의 Memory, Cpu 상태
- Unhealty 상태에 있는 Pod 개수 추적
- 기타 추가적으로 개발 하고 싶은 것이 있으면 개발하면 됩니다.
```

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 - 모니터링 시스템 구축](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95)

---

**Last! 로깅 시스템 구축**

Grafana Loki를 사용하여 Logging 시스템을 구축하고 알림을 받기 위해 임계값 설정

[[K8S] CI/CD Pipeline, 모니터링, 로깅 시스템 구축 프로젝트 - 로깅 시스템 구축](https://velog.io/@mdev97/K8S-CICD-Pipeline-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A1%9C%EA%B9%85-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95)
