---
layout: post
title: 쿠버네티스 container 생성전에 세팅하고 싶은게 있는데
subtitle: K8S initContainer
categories: Kubernetes
tags: [K8s, initContainer]
date: 2025-05-24
---
원래는 aws ec2 를 사용하다 ncp로 넘어오게되면서 k8s로 구성을 변경했다.

게임 서버다 보니 아이템이나 스테이지 등등등 csv로 데이터들이 svn에 저장되어있었는데 

ec2에선 그냥 업데이트 스크립트를 돌리면 끝이였는데

k8s에선 어떻게 해야할까 고민하다

container 생성할때 pod에 저장하고 실행하면 되지 않을까?

했지만 문제가 있었다 명령어를 어떻게 하든 두개를 읽지 못하는 것이였다.

command에서 svn에서 check in 해서 데이터 가져와 하고 npm run 하자~
하면 svn 데이터 가져오고 자꾸 퇴근하는 것이다..
실행은 2개를 하지 못한다.

그래서 찾아보다 initContainer에서 svn스크립트 실행하고 템프 볼륨 마운트해서 저장하고 main container에서 마운트된 temp 볼륨을 가져와서 pod에 마운트 시켜줬다.

여기서 따라온 장점도 있었다.

pod 갯수만큼 svn 업데이트 할필요없고 initContainer에서 한번만 생성해서 마운트 해주면 끝!

세팅 방법보단 문제 해결 한 경험을? 네 

