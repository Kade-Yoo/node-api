# AWS CodePipeline + Jenkins Batch
## Deploy 구성
- AWS CodeCommit -> AWS CodeBuild -> AWS CodeDeploy -> New Instance(EC2)
- CodeDeploy에서 New Instance(EC2)로 배포하는 과정 중 Symbolink를 이용한 무중단 배포를 구성해야 한다
- EC2 생성
  - iam 역할 추가
  - EC2 시작하기
  - key pair생성
  - ssh 연결
- CodeDeploy 구성
  - aws cli 설치
  - ruby 2.x 버전 설치
    - 3.x버전으로 설치하여 2.x버전으로 돌아가야 한다...
  - aws codedeploy agent 설치

## Jenkins Batch 구성
- Schedule(Job) -> Batch(Job)
- Schedule : 일정 주기별로 수행하는 Schedule이다.
- Batch : start_date ~ end_date 동안 배치를 돌릴 수 있도록 parameter를 구성한다.
- 또한, 기존 배포되었던 Schedule에 영향을 미치지 않아야 한다.

### AWS CodeCommit Test
- AWS CodeCommit 복제 완료 
- 하지만 동기화 실패... -> 방법을 찾아야함 일단 이게 급한게 아니니까 CodeBuild로 넘어가자