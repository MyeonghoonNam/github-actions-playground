# Github Actions

빌드, 테스트 및 배포 파이프라인을 자동화 할 수 있는 CI/CD 플랫폼이다.
리포지토리에 대한 Pull Request나 병합된 Pull Request를 프로덕션에 배포할 수 있는데 이를 워크플로우(workflow)로 구현하여 관리할 수 있다.
구현한 워크플로우를 실행할 수 있는 가상 머신을 제공해주므로 개발자 입장에서 유연하게 통합환경을 구성할 수 있다.

# Github Actions 구성요소

크게 아래와 같이 5가지 요소로 구성되며 리포지토리에서 특정 작업이 발생할 때 트리거 되도록 구성할 수 있다. 워크플로우에는 순차 또는 병렬적 동작이 가능한 하나 이상의 작업이 포함되어야 한다.
각 작업들은 자체 가상 머신 러너 내부에서 실행된다.

![image](https://github.com/user-attachments/assets/f5f7ab53-f724-4e8c-afd1-b88825371f30)

## Workflows

워크플로는 하나 이상의 작업을 실행하는 구성 가능한 자동화된 프로세스이다.
워크플로는 리포지토리에 체크인된 YAML 파일로 정의되며 리포지토리의 이벤트에 의해 트리거될 때 실행되거나 수동으로 또는 정의된 일정에 따라 트리거될 수 있습니다.
워크플로는 리포지토리의 `.github/workflows` 디렉터리에 정의되며 리포지토리에는 각각 다른 작업 집합을 수행할 수 있는 여러 워크플로가 있을 수 있다.
예를 들어 풀 리퀘스트를 빌드하고 테스트하는 워크플로, 릴리스가 만들어질 때마다 애플리케이션을 배포하는 워크플로, 누군가 새 이슈를 열 때마다 레이블을 추가하는 또 다른 워크플로를 가질 수 있다.

## Events

이벤트는 워크플로 실행을 트리거하는 리포지토리의 특정 활동이다.
예를 들어 누군가 풀 리퀘스트를 만들거나, 이슈를 열거나, 리포지토리에 커밋을 푸시할 때 GitHub에서 활동이 시작될 수 있다.
[Events that trigger workflows](https://docs.github.com/ko/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#external-events-repository_dispatch)

## Jobs

작업은 동일한 러너에서 실행되는 워크플로우의 단계 집합이다. 각 단계는 실행될 작업이며, 단계는 순서대로 실행되며 서로 종속된다. 각 단계는 동일한 러너에서 실행되므로 한 단계에서 다른 단계로 데이터를 공유할 수 있다. 예를 들어 애플리케이션을 빌드하는 단계 다음에 빌드된 애플리케이션을 테스트하는 단계를 가질 수 있다.

```yml
// jobs 예시
jobs:
  // 첫 번째 작업
  build:
    steps:
      - name:
        run : 실행할 명령어
      - name:
        run : 실행항 명령어

  // 두 번째 작업
  test:
    steps:
      - name:
        run : 실행할 명령어
      - name:
        run : 실행할 명령어
```

작업의 종속성을 다른 작업과 구성할 수 있으며, 기본적으로 작업은 종속성이 없으며 서로 병렬로 실행된다. 한 작업이 다른 작업에 종속된 경우 종속된 작업이 완료될 때까지 기다렸다가 실행할 수 있다.

## Actions

Actions는 복잡하지만 자주 반복되는 작업들을 처리할 때 유용하다.
가상 머신의 러너에서 Github의 리포지토리를 가져온다거나, 빌드 환경에 적합한 환경을 설정하는 등의 작업을 수월하게 해준다.
직접 action을 만들어 사용할 수 도 있고, [Github Marketplace](https://github.com/marketplace?type=actions)에서 제공하는 공용 action을 불러와 사용할 수 도 있다.

```yml
jobs:
  // 첫 번째 작업
  build:
    steps:
      - name: Check out Repository
        // 아래와 같이 actions를 import 할 수 있다.
        uses: actions/checkout@v3
```

## Runners

러너는 워크플로우가 트리거될 때 워크플로우를 실행하는 서버로 가상 머신 환경을 구축하는 인스턴스이다.
다양한 운영체제를 제공하며 각각의 러너는 한 번에 하나의 작업을 실행할 수 있다. jobs의 개별 job 수 만큼 러너가 존재한다.

## 회고

github actions의 기본적인 이론을 학습하였는데, 기초적인 사용법을 토대로 내가 원하는 워크플로우들을 만들어 다양한 자동화 시스템을 구축해봐야겠다.

먼저 현재 npm에 배포중인 패키지에 대해서 기존에 프로젝트를 업데이트하고 npm 사이트에 매번 publish 하는 과정이 번거롭다고 느껴졌는데 이번 기회를 토대로 가장 먼저 npm CI/CD 프로세스를 원하는 기능이 동작하는 방향으로 구현해보고 발생하는 문제점에 대해 개선해보아야겠다.

#### 참고

- [Github Actions Docs](https://docs.github.com/ko/actions/about-github-actions/understanding-github-actions)
