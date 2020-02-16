---
title: >-
  Start e stop automático de instância EC2 AWS utilizando os serviços Lambda e
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
thumbnailImage: ''
coverImage: /images/uploads/structure.png
---
## 

## Reduza custos e economize braço

Você precisa de instâncias aws ligadas por algum período de tempo específico? Deseja reduzir custos com seu ambiente de DEV/QA ou até mesmo em PROD em casos específicos? Então esse processo simples de automação para ligar e desligar sua(s) instância(s) em determinados períodos pode ajudar a reduzir os custos da sua conta asw consideravelmente.\
Então bora lá para configuração das Rules e Functions necessárias para implementar a automação. 

## Criando a regra no IAM

Acesse a console da sua conta no **AWS**\
Selecione a região onde a instancia está localizada\
No exemplo utilizamos a região **US East (N.Virginia)**

![null](/images/uploads/funcao-lambda-step-002.png)

Acesse no painel o **AWS Services**, e pesquise por **IAM**

![null](/images/uploads/funcao-lambda-step-001.png)

Localize **Rules** a esquerda do painel em seguida clique em **Create role**

![null](/images/uploads/funcao-lambda-step-003.png)

Na próxima tela selecione **AWS Service**, escolha o serviço **Lambda**, e na sequência clique em **Next: Permission**

![null](/images/uploads/funcao-lambda-step-02.1.png)

Clique em **Create policy**

![null](/images/uploads/funcao-lambda-step-03.png)

Na tela **Create policy**, clique na guia **JSON**;

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

Acesse o **Services** e pesquise pelo serviço **Lambda**. Clique para acessar o **Dashboard**

![null](/images/uploads/lambda-step-01.png)

Vamos criar as funções que serão responsáveis pelo **start** e **stop** da(s) instância(s). Clique em **Create function**

![null](/images/uploads/lambda-step-02.png)

Siga as etapas abaixo:

![null](/images/uploads/lambda-step-03.png)

![null](/images/uploads/lambda-step-03.1.png)

A seguir será informado que a função foi criada com sucesso

![null](/images/uploads/lambda-step-04.png)

Role a tela para acessar o código da função;

Confirme se as opções **Code entry type**, **python 3.8 **e **Handler** estão devidamente selecionadas. Acesse o script no repositório [git](https://github.com/lazarete/aws-lambda-start-stop-ec2) ([start-instance.py](https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/start-instance.py) ) copie o código e substitua no arquivo **lambda_function.py** conforme a imagem

![null](/images/uploads/lambda-step-05.png)

Rolando um pouco mais a página, faça a edição do** Basic setings**

![null](/images/uploads/lambda-step-07.png)

Salve as alterações

![null](/images/uploads/lambda-step-06.png)

Repita esse procedimento para criar uma regra de **STOP** da instância. Copie o script do repositório git ([stop-instance.py](https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/stop-instance.py)) substitua pelo código no **lambda_function.py.**

> _**Hora de realizar um teste para validar se tudo está funcionando conforme o esperado. Antes de iniciar os testes vamos fazer alguns ajustes lá no EC2, configurando as TAGs diretamente na nossa instância a ser atingida pela regra.**_
>
> >

**Vamos voltar para o Services e acessar o EC2**

![null](/images/uploads/services-ec2.png)

No lado esquerdo do painel, localize e acesse **Instance**

![null](/images/uploads/instances.png)

Configure as **Tags **referente que foram passadas nos scripts python

![null](/images/uploads/add-tags.png)

Retorne ao **Services Lambda**. Selecione a função que deseja testar e em seguida clique em **Actions**, e selecione **Test**

![null](/images/uploads/teste-stop.png)

Na tela **Configure test event**, selecione **Create new test event**.  Digite um nome para o evento em **Event name** e clique em Create

![null](/images/uploads/stop-test.png)

![null](/images/uploads/stop-test-1.png)

A próxima tela retornará uma mensagem de sucesso sobre a criação da função **stopinstance**. Agora execute o teste

![null](/images/uploads/execute-teste.png)

Verifique o log após a sua execução

![null](/images/uploads/log.png)

Agora verifique o status da instância

![null](/images/uploads/stopping.png)

Com as regras e funções devidamente configuradas e testadas, podemos passar para parte final que é a configuração do scheduler, que fará a execução das novas regras e funções de acordo com a necessidade de utilização.

Acesse novamente o menu **Services**, e no campo de busca digite o nome do serviço **CloudWatch**

![null](/images/uploads/service-cloudwatch.png)

Acessando o **CloudWatch**, localize no lado esquerdo **Rules** e clique nele

![null](/images/uploads/rule-cloudwatch.png)

Na próxima tela, clique em **Create rule**

![null](/images/uploads/create-rule-cloudwatch.png)

No **Step 1** de configuração da **Rule** do **CloudWatch** siga as etapas abaixo:

Em **Event Source** marque os campos **Schedule**, **Cron expression** e preencha com a expressão **01 12 ? \* MON-FRI \***

No **Targets** em **Lambda** **functions** selecione o nome da função **startinstance** e clique em **Configure details**

> **_IMPORTANTE_**
>
> **_O CloudWatch usa o padrão de hora UTC-0. Então para configurar o horário considere 3 horas a mais. No exemplo abaixo, vamos iniciar a instância as 09:01Horas, logo a a expressão 01 12._**

![null](/images/uploads/create-rule-cloudwatch-step-1.png)

No **Step 2 Configure rule details**, digite o nome da **Rule definition**. **Name: Start Instance**, **Description**: forneça uma descrição sobre a regra. Verifique se o campo State está marcado como **Enabled** e clique em **Create rule**.

![null](/images/uploads/create-rule-cloudwatch-step-2.png)

A próxima tela informa que a rule foi criada com sucesso.

![null](/images/uploads/rule-cloudwatch-success.png)

Refaça o Step 2 **Configure rule details** para criar a **rule de Stop**.
