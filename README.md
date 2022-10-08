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

### Error
Invalid input: when using CodePipeline both sourceType, and artifactType must be set to: CODEPIPELINE
- CodeBuild 등록 실패 해결 방안 아직 찾지 못함
- Github Repository에 내 개인키 연결 완료
Ruby Version Error  
`` 
[2022-09-30T14:17:32.654584 #3980]  INFO -- : Starting Ruby version check.
[2022-09-30T14:17:32.655009 #3980] ERROR -- : Current running Ruby version for root is 3.0.2, but Ruby version 2.x needs to be installed.
[2022-09-30T14:17:32.655217 #3980] ERROR -- : If you already have the proper Ruby version installed, please either create a symlink to /usr/bin/ruby2.x,
[2022-09-30T14:17:32.655399 #3980] ERROR -- : or run this install script with right interpreter. Otherwise please install Ruby 2.x for root user.
[2022-09-30T14:17:32.655576 #3980] ERROR -- : You can get more information by running the script with --help option.
[2022-09-30T14:17:59.082945 #3984]  INFO -- : Starting Ruby version check.
``
- apt-get ruby 삭제
- /usr/bin/env: ‘ruby’: No such file or directory
- 소프트 링크 지정
``sudo ln -s /home/ubuntu/.rbenv/shims/ruby /usr/bin/ruby``
Error installing /tmp/codedeploy-agent_1.1.0-4_all.tmp-20221008-95705-19ynbt0.deb
- 