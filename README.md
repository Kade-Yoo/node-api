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
```
[2022-09-30T14:17:32.654584 #3980]  INFO -- : Starting Ruby version check.
[2022-09-30T14:17:32.655009 #3980] ERROR -- : Current running Ruby version for root is 3.0.2, but Ruby version 2.x needs to be installed.
[2022-09-30T14:17:32.655217 #3980] ERROR -- : If you already have the proper Ruby version installed, please either create a symlink to /usr/bin/ruby2.x,
[2022-09-30T14:17:32.655399 #3980] ERROR -- : or run this install script with right interpreter. Otherwise please install Ruby 2.x for root user.
[2022-09-30T14:17:32.655576 #3980] ERROR -- : You can get more information by running the script with --help option.
[2022-09-30T14:17:59.082945 #3984]  INFO -- : Starting Ruby version check.
```
- apt-get ruby 삭제
- /usr/bin/env: ‘ruby’: No such file or directory
- 소프트 링크 지정
```sudo ln -s /home/ubuntu/.rbenv/shims/ruby /usr/bin/ruby```

CodeDeploy-agent install Error
```
dpkg-query: package 'codedeploy-agent' is not installed and no information is available
Use dpkg --info (= dpkg-deb --info) to examine archive files.
I, [2022-10-10T12:26:32.271473 #124514]  INFO -- : Running version No running version
I, [2022-10-10T12:26:32.271971 #124514]  INFO -- : Downloading package from bucket aws-codedeploy-ap-northeast-2 and key releases/codedeploy-agent_1.4.0-2218_all.deb...
I, [2022-10-10T12:26:32.272222 #124514]  INFO -- : Endpoint: https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/releases/codedeploy-agent_1.4.0-2218_all.deb
I, [2022-10-10T12:26:32.486143 #124514]  INFO -- : Executing `/usr/bin/gdebi -n -o Dpkg::Options::=--force-confdef -o Dkpg::Options::=--force-confold /tmp/codedeploy-agent_1.4.0-2218_all.tmp-20221010-124514-42l3qe.deb`...
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Reading state information... Done
This package is uninstallable
Dependency is not satisfiable: ruby2.0|ruby2.1|ruby2.2|ruby2.3|ruby2.4|ruby2.5|ruby2.6|ruby2.7
E [2022-10-10T12:26:33.347877 #124514] ERROR -- : Error installing /tmp/codedeploy-agent_1.4.0-2218_all.tmp-20221010-124514-42l3qe.deb.
```
- 버그로 발생한 오류로 아래와 같이 스크립트 안에서 실행을 해주면 된다고 함
- 스크립트 안에서 실행하는 방법이 기존에 하던 방식이 아니라 동작을 시키지 못했음..
- 일단 버그라는 것은 알았으니 실행시키는 방법을 찾아보고 해결을 해보자
```
dpkg -i #{tmp_path}
```