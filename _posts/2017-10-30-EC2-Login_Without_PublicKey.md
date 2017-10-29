---
layout: post
title:  " EC2 Login without Public Key "
date:   2017-10-29
excerpt: " Login Using Password "
cate : "post"
tag:
- EC2
---

## Problem

* 아마존 웹 서비스(AWS)에서 생성한 EC2 인스턴스는 기본적으로 생성시 발급한 공개키로만 접근이 가능하다.

* 공개키 접속 방법이 가장 뛰어난 보안성을 제공하지만, 경우에 따라서 Password를 통한 Login이 필요하다.

## How to Login Using Password

1. 우선 Public Key를 이용하여 EC2 Server에 접속한다. 

2. sudo vi /etc/ssh/sshd_config

{% highlight JavaScript %}

sudo vi /etc/ssh/sshd_config

{% endhighlight %}

3. 

{% capture images %}
	/assets/img/posts/ec2_login_1.png
	/assets/img/posts/ec2_login_2.png
{% endcapture %}
{% include gallery images=images caption=" " cols=2 %}


| Option |  Before   |  After  |
|:-------:|:-------:|:-------:| :-------:|
| PermitRootLogin   | Prohibit | yes |
| PasswordAuthentication   | no | yes  |
|=====

4.

{% highlight JavaScript %}

sudo service ssh restart

{% endhighlight %}
