---
layout: post
title: "쿠버네티스란"
subtitle: 컨테이너 운영 자동화와 컨테이너 오케스트레이션을 수행하는 대표적인 도구 쿠버네티스에 대해 알아봅니다.
author: 이원모
categories: [DevOps, Kubernetes]
tags: [Kubernetes, 쿠버네티스, DevOps, 컨테이너 오케스트레이션]
---

쿠버네티스(Kubernetes)는 컨테이너화된 애플리케이션의 배포, 확장 및 관리를 자동화하기 위한 오픈소스 플랫폼입니다. 구글이 처음 개발하고 현재는 CNCF(Cloud Native Computing Foundation)에서 관리하고 있는 프로젝트이며, 클라우드 인프라를 추상화하여 애플리케이션이 클라우드, 온프레미스 등 다양한 환경에서 원활하게 운영될 수 있도록 도와줍니다.

## 필요성
---
컨테이너는 애플리케이션과 그에 필요한 모든 라이브러리 및 종속성을 패키징하여 다른 환경에서도 동일하게 실행될 수 있도록 돕는 기술입니다. 도커(Docker)가 대표적인 컨테이너 기술로, 컨테이너를 통해 애플리케이션의 배포와 운영이 더욱 유연해졌습니다.

하지만, 여러 개의 컨테이너로 구성된 대규모 애플리케이션을 관리하기 위해서는 컨테이너 오케스트레이션이 필요합니다. 쿠버네티스는 이러한 컨테이너 오케스트레이션을 담당하여, 수천 개의 컨테이너를 효과적으로 배포하고 관리할 수 있게 해줍니다.

<br>

## 아키텍처
---
쿠버네티스는 분산된 애플리케이션의 관리와 확장을 지원하는 모듈형 아키텍처를 가지고 있습니다. 이를 이해하기 위해 주요 구성 요소들을 살펴보겠습니다.

![쿠버네티스 아키텍처](/assets/images/posts/이원모/20240820/Screenshot_1.png "쿠버네티스 아키텍처"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">출처: [https://kubernetes.io/docs/concepts/architecture/]</div>

### 마스터 노드(Master Node)
마스터 노드는 컨트롤 플레인이라고도 불리며, 클러스터의 전체적인 상태를 관리하고 각종 명령을 처리하는 중앙 컨트롤러 역할을 합니다.

- API 서버 (API Server): 쿠버네티스의 진입점으로, 사용자와 내부 구성 요소의 요청을 처리합니다.
- 컨트롤러 매니저 (Controller Manager): 클러스터의 상태를 유지하기 위해 다양한 컨트롤러들을 관리합니다.
- 스케줄러 (Scheduler): 새로운 컨테이너(파드)를 실행할 적절한 노드를 선택합니다.
- ETCD: 클러스터의 상태 정보를 저장하는 고가용성 키-값 데이터 저장소입니다.

### 워커 노드(Worker Node)
워커 노드는 실제로 애플리케이션이 실행되는 곳으로, 각 워커 노드에는 다음과 같은 구성 요소가 포함됩니다.

- Kubelet: API 서버와 통신하여 워커 노드의 상태를 관리하고, 파드가 올바르게 동작하도록 조정합니다.
- Kube-proxy: 네트워크 트래픽을 적절한 파드로 라우팅하는 네트워크 프록시입니다.
- 컨테이너 런타임: 컨테이너를 실제로 실행하는 소프트웨어로, 도커나 containerd 등이 사용됩니다.

<br>

## 주요 구성 요소
---
쿠버네티스는 다양한 구성 요소를 통해 클라우드 네이티브 애플리케이션을 관리합니다. 여기서 몇 가지 중요한 요소들을 살펴보겠습니다.

### 파드(Pod)
파드는 쿠버네티스에서 생성되고 관리되는 가장 작은 배포 단위입니다. 파드는 하나 이상의 컨테이너로 구성되며, 이 컨테이너들은 동일한 네트워크 네임스페이스를 공유하여 서로 통신할 수 있습니다. 일반적으로 하나의 파드는 하나의 애플리케이션 인스턴스를 나타내며, 필요에 따라 여러 개의 파드로 확장할 수 있습니다.

### 서비스(Service)
서비스는 파드의 집합을 외부 네트워크에 노출시키는 추상화된 개념입니다. 서비스는 클러스터 내에서 고정된 IP 주소를 제공하며, 로드 밸런싱을 통해 여러 파드에 요청을 분산시킵니다. 이를 통해 클라이언트는 파드의 개별 IP 주소를 알 필요 없이 서비스의 IP만으로 접근할 수 있습니다.

### 디플로이먼트(Deployment)
디플로이먼트는 애플리케이션의 선언적 업데이트를 관리하는 쿠버네티스 오브젝트입니다. 이를 사용해 파드의 배포, 업데이트, 롤백, 확장 등의 작업을 자동으로 수행할 수 있습니다. 디플로이먼트는 애플리케이션의 원활한 운영을 위해 롤링 업데이트 및 롤백 기능을 지원합니다.

### 컨피그맵(ConfigMap)
애플리케이션 설정 데이터를 관리하는 객체로, 환경 변수, 명령줄 인수 또는 구성 파일로 설정을 주입할 수 있습니다.

### 시크릿(Secret)
암호화된 데이터를 관리하는 객체로, 암호, API 키 등 민감한 정보를 안전하게 저장하고 전달할 수 있습니다.

<br>

## 장단점
---
### 장점
- 자동화된 운영

  쿠버네티스는 배포, 확장, 복구 등의 작업을 자동화하여 운영의 복잡성을 크게 줄입니다.

- 확장성

  수천 개의 노드와 수십만 개의 파드를 안정적으로 관리할 수 있는 높은 확장성을 제공합니다.

- 유연성

  다양한 인프라 환경에서 동일한 애플리케이션을 운영할 수 있어 클라우드 공급업체 종속을 피할 수 있습니다.

- 커뮤니티 지원

  쿠버네티스는 활발한 오픈소스 커뮤니티와 생태계를 가지고 있어, 다양한 플러그인과 도구들을 활용할 수 있습니다.

### 단점
- 복잡성

  강력한 기능을 제공하는 만큼, 쿠버네티스의 학습 곡선은 가파릅니다. 특히 초기 설정 및 클러스터 운영에 대한 깊은 이해가 필요합니다.

- 관리 오버헤드

  클러스터의 관리와 모니터링을 위해 추가적인 관리 도구와 리소스가 필요할 수 있습니다. 잘못된 설정이나 관리 소홀로 인해 문제가 발생할 수 있습니다.

<br>

## 사용 사례
---
쿠버네티스는 다양한 환경에서 활용될 수 있으며, 특히 클라우드 네이티브 애플리케이션에 적합합니다. 몇 가지 대표적인 사용 사례를 살펴보겠습니다.

- 마이크로서비스 아키텍처

  마이크로서비스는 서로 독립적인 서비스로 구성된 애플리케이션 아키텍처입니다. 쿠버네티스는 마이크로서비스를 효과적으로 관리할 수 있는 플랫폼을 제공합니다. 각 마이크로서비스를 별도의 파드로 배포하고, 이를 서비스로 연결하여 관리함으로써 마이크로서비스 간의 통신을 원활하게 유지할 수 있습니다.

- 하이브리드 클라우드 및 멀티 클라우드

  쿠버네티스는 다양한 클라우드 제공업체와 온프레미스 환경을 아우르는 하이브리드 및 멀티 클라우드 전략을 지원합니다. 이를 통해 기업은 특정 클라우드 제공업체에 종속되지 않고, 최적의 비용과 성능을 제공하는 환경을 선택할 수 있습니다.

- CI/CD 파이프라인 통합

  쿠버네티스는 CI/CD 파이프라인과의 통합을 통해 애플리케이션의 지속적 배포를 자동화할 수 있습니다. 코드 변경 사항이 커밋될 때마다 자동으로 테스트, 빌드, 배포가 이루어지며, 롤링 업데이트를 통해 무중단 배포가 가능합니다.