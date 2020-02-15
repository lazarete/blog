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
autoThumbnailImage: false
thumbnailImagePosition: top
thumbnailImage: /images/uploads/structure.png
coverImage: ''
---
## Reduza custos e economize braço

Você precisa de instâncias aws ligadas por algum periodo de tempo expecifico? Deseja reduzir custos com seu ambiente de DEV/QA ou até mesmo em PROD em casos especificos? Então esse processo simples de automação para ligar e desligar sua(s) instância(s) em determinados periodos pode ajudar a reduzir os custos da sua conta asw consideravelmente.\
Então bora lá para configuração das Rules e Functions necessárais para implementar a automação. 

## Criando a regra no IAM

Acesse a console da sua conta no AWS\
Selecione a região onde a instancia está localizada\
No exemplo utilizamos a região **US East (N.Virginia)**

![null](/images/uploads/funcao-lambda-step-002.png)

Acesse no painel o **AWS Services**, e pesquise por **IAM**

![null](/images/uploads/funcao-lambda-step-001.png)

Localize **Rules** a esquerda do painel em seguida em clique **Create role**

![null](/images/uploads/funcao-lambda-step-003.png)

Na próxima tela selecione **AWS Service**, escolha o serviço **Lambda**, e na sequência clique em **Next: Permission**

![null](/images/uploads/funcao-lambda-step-02.1.png)

Clique em **Create policy**

![null](/images/uploads/funcao-lambda-step-03.png)

Na tela de **Create policy**, clique na guia **JSON**;

![null](/images/uploads/funcao-lambda-step-006.png)

Apague o conteúdo existente, e cole o código disponibilizado no [GitHub](https://github.com/lazarete/aws-lambda-start-stop-ec2)\
**url direta do .json:** https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/iam-role-start-stop-instance-ec2.json

![null](/images/uploads/funcao-lambda-step-04.png)

Clique em **Review policy**, insira um nome e a descrição para sua política, por exemplo: **start-stop-ec2-instance**; em seguida clique em **Create role**

![null](/images/uploads/funcao-lambda-step-008.png)

![null](/images/uploads/funcao-lambda-step-05.1.png)

Ao criar a política, você receberá a notificação a seguir na tela:

![null](/images/uploads/funcao-lambda-step-009.png)

> > **IMPORTANTE**
> >
> > _No momento em que clicar em Create policy uma nova aba será aberta no navegador para criação da nova policy. Assim que terminar o processo e visualizar a mensagem acima, você deve fechar a aba  Create policy  voltar a aba anterior Create role para continuar o processo._
>
> Siga as etapas:
>
> 1. faça um refresh
> 2. pesquise pela rule start-stop-ec2-instance
> 3. selecione a regra
> 4. clique em próxima Tags

![null](/images/uploads/funcao-lambda-step-08.png)

Na tela **Add tags** **(optional)**, vamos avançar, e trabalharemos as tags logo mais a frente

![null](/images/uploads/funcao-lambda-step-09.png)

Na tela de **Create role Review** digite o nome para regra como a do exemplo: **start-stop-ec2-instance**\
Faça uma breve descrição e clique em **Create role**

![null](/images/uploads/funcao-lambda-step-10.png)



Sucesso! A primeira etapa de criação de regras e políticas no **IAM** foi finalizada. Agora vamos criar as funções no **Lambda**.
