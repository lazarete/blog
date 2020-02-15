---
title: >-
  Start e stop de instância EC2 AWS automático utilizando os serviços Lambda e
  Cloudwatch
date: '2020-02-15T06:26:09-03:00'
categories:
  - aws
tags:
  - lambda
  - cloudwatch
keywords:
  - instance
  - aws
  - ec2
  - start
  - stop
  - lambda
  - cloudwatch
autoThumbnailImage: true
thumbnailImagePosition: left
thumbnailImage: /images/uploads/structure.png
coverImage: ''
---
## Reduza custos e economize braço

Você precisa de instâncias aws ligadas por algum periodo de tempo expecifico? Deseja reduzir custos com seu ambiente de DEV/QA ou até mesmo em PROD em casos especificos? Então esse processo simples de automação para ligar e desligar sua(s) instância(s) em determinados periodos pode ajudar a reduzir os custos da sua conta asw consideravelmente.\
Então bora lá para configuração das Rules e Functions necessárais para implementar a automação. 



## Criando a regra no IAM

Acesse a console da sua conta no AWS\
Selecione a região onde a instancia está localizada\
No exemplo utilizamos a região US East (N.Virginia)

![](/images/uploads/funcao-lambda-step-001.png)
