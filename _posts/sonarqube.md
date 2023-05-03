---
title: "Puppeteer로 PDF 생성 시 빈 화면 문제 해결하기"
date: 2023-04-22 17:13:16 +0900
categories:
  - blog
tags:
  - puppeteer
 
---


cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-X.Y.Z.zip
sudo unzip sonarqube-X.Y.Z.zip
sudo mv sonarqube-X.Y.Z sonarqube


sudo groupadd sonar
sudo useradd -g sonar -s /bin/false -d /opt/sonarqube sonar
sudo chown -R sonar:sonar /opt/sonarqube

sudo su - sonar -s /bin/bash -c "/opt/sonarqube/bin/linux-x86-64/sonar.sh start"

./gradlew sonarqube