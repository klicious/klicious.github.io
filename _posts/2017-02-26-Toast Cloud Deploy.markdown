---
layout: post
title:  "[토스트루키 캠프 4기] Toast Cloud Deploy"
date:   2017-02-26 10:23:15 +0900
categories: jekyll update
---

# [토스트루키 캠프 4기] Toast Cloud Deploy

## Toast Cloud Deploy

### SSH / SSH-KEY

### Jenkins-cli 는 무었이고 어디에 있나? 어떻게 작동하나? 무엇을 할 수 있나?

### Jenkins-cli build 에서 Build parameter ??

### Build 매개변수

### Custom Alert 에서 Dooray Messenger Hook을 걸었을 때 Notification은 오는데 Content가 비어서 오는 이유는?

### Tomcat conf-> *server.xml* 에서 "autoDeploy"가 의미하는 바는?
```html
<Host autoDeploy="true">
```

### 서버에 Deploy를 했더니 *PAGE 404* 를 반환한다 어떻게 해결해야 하나?

 1. 현재 가동중인 `Port` 들을 확인하여 어떤 `Port`로 `Apache` or `Tomcat` 이 실행중인지 확인한다.

 ```sh
netstat -nap | grep LISTEN | grep tcp
 ```

 2. `Apache` 와 `Tomcat` 의 `conf` 를(설정파일을) 살펴본다.

 Apache Configuration file

 ```sh
 vim ~/apps/apache/conf/httpd.conf
 ```

 Tomcat Configuration file

 ```sh
 vim ~/apps/tomcat/conf/server.xml
 ```

 3. 설정파일에서 알수있는 default `apache` / `tomcat` 포트에 뭐가 연결되어있는지 확인한다. 나같은 경우는 설정파일을 따라 `80/8080` 에 뭐가 연결되어있는지 확인했다. 제대로 `apache` `tomcat` 이 연결되어있다면 pass

 ```sh
 netstat -na | grep 80
 ```

 4. 설정 파일을 살펴보았는데도 문제점을 찾지 못했으면  이제는 `log` 파일을 뒤져야 한다. `Tomcat log` 부터 살펴보자

 ```sh
 tail -f logs/apache/access.log.20170221
 ```

 5. 별다른 에러사항을 아직도 찾지 못했다면 `Tomcat log` 를 살펴보자.

 ```sh
 tail -f logs/tomcat/catalina.out
 ```

  [Tomcat Log](/assets/img/for_post/질문리스트/toast_cloud/tomcat_log.png)

  아하! 찾았다! `properties` 파일을 가져오는데 문제가 생겼나 보다. 이런식으로 이제 `properties` 에 관해 어떤에러가 있었는지 자세히 찾아보고 고치면 된다.
