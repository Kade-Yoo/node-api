# AWS CodePipeline + Jenkins Batch
## Deploy 구성
- AWS CodeCommit -> AWS CodeBuild -> AWS CodeDeploy -> New Instance(EC2)
- CodeDeploy에서 New Instance(EC2)로 배포하는 과정 중 Symbolink를 이용한 무중단 배포를 구성해야 한다

## Jenkins Batch 구성
- Schedule(Job) -> Batch(Job)
- Schedule : 일정 주기별로 수행하는 Schedule이다.
- Batch : start_date ~ end_date 동안 배치를 돌릴 수 있도록 parameter를 구성한다.
- 또한, 기존 배포되었던 Schedule에 영향을 미치지 않아야 한다.