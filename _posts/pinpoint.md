---
title: "Ansible 사용해보기"
date: 2023-06-30 21:13:16 +0900
categories:
  - blog
tags:
  - Ansible
 
---


## Ansible 사용해보는 이유

친구는 서버를 50개가량 관리하는데 일일히 제어가 너무 힘들어서 Ansible을 도입하였다고 들었고 아주 만족한다고 전달받았다.
궁금해서 나도 한번 기본적인 것만 한번 해보기


우선 나는 Ansible은 Nginx의 대체제인줄 알고 있었는데 전혀 다른 역할을 하고 있었다.
Ansible 과 Nginx의 차이점은 다음과 같다.

- Ansible
오픈소스 IT 자동화 도구. 소프트웨어의 구성 관리, 애플리케이션 배포, 태스크 자동화 등을 원격으로 처리 가능
복잡한 작업과 프로세스를 단순화하는데 도움.

- Nginx
웹 서버, 리버스 프록시, 로드 밸런서, 메일 프록시 등의 기능을 제공하는 소프트웨어.
Nginx는 특히 동시 연결을 처리하는 데 탁월하며, 그 때문에 고성능 웹 애플리케이션을 제공하는데 매우 효과적

요약하자면,

Ansible은 시스템 관리 및 작업 자동화에 중점을 두고 있다.
Nginx는 웹 서비스 제공에 중점을 두고 있다.

## 일단 해보자

Ansible 설치를 위해서 별도의 VM 구성은 귀찮으니까
윈도우 로컬에 설치되어있는 WSL을 이용해볼까한다.

ubuntu 기준이고 ansible 설치 시작
```
sudo apt update
sudo apt install ansible -y
```

설치확인은 version 명령어로 확인해본다.
```
ansible --version
```


- Ansible Ad-hoc
playbook이라고하는  Ansible에서 사용되는 설정, 배포, 그리고 일련의 작업들을 자동화하는 정의된 형식의 파일을 사용하지 않고
command-line에서 직접 Ansible 모듈을 호출해서 사용해보려고한다.

간단하게 ping을 실험해보자

※테스트
```
ansible localhost -m ping -c local
```

※결과
```
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
localhost가 온라인 상태임을 확인하였다.


'ansible localhost -m ping -c local' 
명렁어를 풀어보면 "Ansible을 사용하여 localhost에 'ping' 명령을 직접 실행하라"이다.
기본적으로 Ansible이 정상적으로 작동하는지 확인하는 데 사용할 수 있는 간단한 테스트 명령어
ansible-playbook playbook.yml


- Ansible Playbook

```
---
- name: Test Playbook
  hosts: localhost
  tasks:
    - name: Check if host is online
      ping:
```
위에서 사용한 Ad-hoc 명령어를 Playbook으로 사용해보았다.


실행 명령어는 다음과 같다.
```
ansible-playbook test-playbook.yml
```

결과는 성공
![pm2 monit](/assets/images/ansible-1.png)


다음 실습은 Playbook을 사용하여 Ansible의 장점 중 하나인 설정관리를 통한 일관적인 서버구성 실험을 해봐야겠다.