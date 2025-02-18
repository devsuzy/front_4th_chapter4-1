# 항해 플러스 프론트엔드 4기 과제: Chapter4-1 성능 최적화 (1)

## 1. 개요

- AWS 인프라를 세팅하여 프론트엔드 프로젝트 배포 파이프라인을 구성합니다.
- GitHub Actions을 통해 자동화된 배포 파이프라인을 구축합니다.
- Amazon CloudFront를 활용하여 사용자에게 빠르고 안정적인 웹 서비스를 제공하는 CDN 서비스를 구축합니다.

## 2. 배포 프로세스

![배포 프로세스 drawio](https://github.com/user-attachments/assets/98553018-0cc3-4175-806c-1d487dd94cde)

## 3. 주요 링크
- S3 버킷 웹사이트 엔드포인트: http://suzysfirstawsbucket.s3-website-ap-southeast-2.amazonaws.com
- CloudFrount 배포 도메인 이름: https://d2z9bqn431wjha.cloudfront.net

## 4. 주요 개념

### 1) GitHub Actions과 CI/CD 도구
- GitHub Actions는 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD(지속적인 통합 및 지속적인 업데이트) 플랫폼입니다.
- CI(Continuous Integration)는 공유 레포지토리에 코드를 자주 커밋해야 하는 소프트웨어 방식입니다. 코드를 커밋하면 오류를 더 빨리 감지하고 개발자가 오류의 원인을 찾을 때 디버그해야 하는 코드의 양이 줄어듭니다. 이는 코드를 작성하는 데 더 많은 시간을 사용하고 오류를 디버그하거나 병합 충돌을 해결하는 데 더 적은 시간을 사용할 수 있는 개발자에게 유용합니다.
- CD(Continuous Deployment)는 자동화를 사용하여 소프트웨어 업데이트를 게시하고 배포하는 방법입니다. 일반적인 CD 프로세스의 일부로 코드는 배포 전에 자동으로 빌드되고 테스트됩니다.
- GitHub Actions은 CI/CD를 실행할 수 있는 워크플로우를 제공합니다. 워크플로우는 레포지토리의 코드를 빌드하고 배포하기 전에 테스트를 실행할 수 있습니다.
- 워크플로우는 자동화 프로세스입니다. 워크플로우는 레포지토리의 `.github/workflows` 디렉터리에 YAML 파일에서 정의되며, 다양한 이벤트에 대한 자동화 작업을 설정할 수 있습니다.

### 2) S3와 스토리지
- Amazon Simple Storage Service(S3)는 AWS에서 제공하는 객체 스토리지 서비스입니다.
- 다양한 파일 형식의 데이터를 저장할 수 있고, 높은 가용성과 내구성으로 설계되어 정보를 안전하게 보호할 수 있습니다.
- 서버 측 처리 또는 데이터베이스 기능이 필요하지 않은 웹사이트인 정적 웹사이트를 호스팅하는데 사용할 수 있습니다. 이는 높은 트래픽 볼륨을 처리하고 효율적인 비용으로 확장 가능합니다.

### 3) CloudFront와 CDN
- 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전세계 고객에게 안전하게 전송하는 AWS의 고속 콘텐츠 전송 네트워크(CDN) 서비스입니다.
- CDN(Content Delivery Network or Content Distribution Network)은 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는 시스템입니다.
- S3에 저장된 파일을 전 세계에 분산된 엣지 로케이션에 캐싱하여 유저에게 빠르게 콘텐츠를 제공합니다.
- 경로 패턴으로 URL에 따라 정적/동적 컨텐츠를 분기처리하여 제공합니다.

### 4) 캐시 무효화(Cache Invalidation)
- 캐시 정책(TTL)에 따라 일정 시간이 지나면 캐싱 내용이 업데이트 되겠지만, 만일 급한 버그 수정과 같은 상황이라면 마냥 기다리기엔 치명적인 결과가 일어날 수 있습니다.
- 따라서 무효화 기능을 통해 캐싱된 콘텐츠를 업데이트 하여, 수정된 파일이 캐시에 바로 반영해 가장 최신의 콘텐츠를 받을 수 있도록 보장해줍니다.
- Github Actions의 워크플로우 디렉토리 내 `deployment.yml` 파일에 명령어 추가 또는 AWS의 명령 도구인 CLI를 사용하여 create-invalidation 명령어로 CloudFront 캐시를 무효화를 자동으로 수행하게끔 설정합니다.
```
$ aws cloudfront create-invalidation \
> --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }} \
> --paths "/*" 
```

### 5) Repository secret과 환경변수
- secret은 레포지토리 또는 개인 계정용 Github Codespaces 설정에서 만든 암호화된 환경변수 입니다.
- Github Actions의 워크플로우 안에 api키, 비밀번호 등 보안이 중요한 환경변수들은 secrets 기능을 사용하여 관리합니다.

## 5. GitHub Actions 워크플로우

## 6. CDN 성능 최적화 분석
