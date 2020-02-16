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
autoThumbnailImage: true
thumbnailImagePosition: top
thumbnailImage: ''
coverImage: ''
---
![](/images/uploads/structure.png)

## Reduza custos e economize braço

Você precisa de instâncias AWS ligadas por algum período de tempo específico? Deseja reduzir custos com seu ambiente de DEV/QA ou até mesmo PROD em casos específicos? Então esse processo simples de automação para ligar e desligar sua(s) instância(s) em determinados períodos pode ajudar a reduzir os custos da sua conta AWS consideravelmente.\
Então bora lá para configuração das Rules e Functions necessárias para implementar a automação. 

## Criando a regra no IAM

1. Acesse a console da sua conta no **AWS**\
   Selecione a região onde a instancia está localizada\
   No exemplo utilizamos a região **US East (N.Virginia)**

![Aws Management Console](/images/uploads/funcao-lambda-step-002.png)

2. Acesse no painel, **Services,** e pesquise por **IAM**

![null](/images/uploads/funcao-lambda-step-001.png)

3. Localize **Rules** a esquerda do painel em seguida clique em **Create role**

![null](/images/uploads/funcao-lambda-step-003.png)

4. Na próxima tela selecione **AWS Service**, escolha o serviço **Lambda**, e na sequência clique em **Next: Permission**

![Identity and Access Management (IAM)](/images/uploads/funcao-lambda-step-02.1.png)

5. Clique em **Create policy**

![null](/images/uploads/funcao-lambda-step-03.png)

6. Na tela **Create policy**, clique na guia **JSON**;

![null](/images/uploads/funcao-lambda-step-006.png)

7. Apague o conteúdo existente, e cole o código disponibilizado no [GitHub](https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/iam-role-start-stop-instance-ec2.json) e m seguida clique em **Review policy.**

![null](/images/uploads/funcao-lambda-step-04.png)

8. Insira um nome e a descrição para sua política, por exemplo: **start-stop-ec2-instance**; em seguida clique em **Create policy**

![null](/images/uploads/funcao-lambda-step-008.png)

![null](/images/uploads/funcao-lambda-step-05.1.png)

9. Ao criar a política, você receberá a notificação a seguir na tela:

![null](/images/uploads/funcao-lambda-step-009.png)

> > **IMPORTANTE**
> >
> > **_No momento em que clicar em Create policy uma nova aba será aberta no navegador para criação da nova policy. Assim que terminar o processo e visualizar a mensagem acima, você deve fechar a aba  Create policy  voltar a aba anterior Create role para continuar o processo._**
>
> Siga as etapas:
>
> 1. faça um refresh
> 2. pesquise pela rule start-stop-ec2-instance
> 3. selecione a regra
> 4. clique em próxima Tags

![null](/images/uploads/funcao-lambda-step-08.png)

10. Na tela **Add tags** **(optional)**, vamos avançar, e trabalharemos as tags logo mais a frente

![null](/images/uploads/funcao-lambda-step-09.png)

11. Na tela de **Create role Review** digite o nome para regra como a do exemplo: **start-stop-ec2-instance**\
    Faça uma breve descrição e clique em **Create role**

![null](/images/uploads/funcao-lambda-step-10.png)

Sucesso! A primeira etapa de criação de regras e políticas no **IAM** foi finalizada. Agora vamos criar as funções no **Lambda**.

## Services Lambda

12. Acesse o **Services** e pesquise pelo serviço **Lambda**. Clique para acessar o **Dashboard**

![null](/images/uploads/lambda-step-01.png)

13. Vamos criar as funções que serão responsáveis pelo **start** e **stop** da(s) instância(s). Clique em **Create function**

![null](/images/uploads/lambda-step-02.png)

14. Siga as etapas abaixo, preenchendo e marcando os campos respectivamente conforme a imagem e finalize clicando em **Create function**

![null](/images/uploads/lambda-step-03.png)

![null](/images/uploads/lambda-step-03.1.png)

A seguir será informado que a função foi criada com sucesso

![null](/images/uploads/lambda-step-04.png)

15. Role a tela para acessar o código da função;

Confirme se as opções **Code entry type**, **python 3.8** e **Handler** estão devidamente selecionadas. Acesse o script no repositório [git](https://github.com/lazarete/aws-lambda-start-stop-ec2) ([start-instance.py](https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/start-instance.py) ) copie o código e substitua no arquivo **lambda_function.py** conforme a imagem

![null](/images/uploads/lambda-step-05.png)

16. Rolando um pouco mais a página, faça a edição do **Basic setings**

![null](/images/uploads/lambda-step-07.png)

17. Salve as alterações

![null](/images/uploads/lambda-step-06.png)

18. Repita esse procedimento para criar uma regra de **STOP** da instância. Copie o script do repositório git ([stop-instance.py](https://github.com/lazarete/aws-lambda-start-stop-ec2/blob/master/stop-instance.py)) substitua pelo código no **lambda_function.py.**

> **_Hora de realizar um teste para validar se tudo está funcionando conforme o esperado. Antes de iniciar os testes vamos fazer alguns ajustes lá no EC2, configurando as TAGs diretamente na nossa instância a ser atingida pela regra._**

19. Vamos voltar para o Services e acessar o EC2

![null](/images/uploads/services-ec2.png)

20. No lado esquerdo do painel, localize e acesse **Instances**

![null](/images/uploads/instances.png)

21. Configure as **Tags** que foram passadas nos scripts python no campo Tags da Instância

![null](/images/uploads/add-tags.png)

## Configurando os testes

22. Retorne ao **Services Lambda**. Selecione a função que deseja testar e em seguida clique em **Actions**, e selecione **Test**

![null](/images/uploads/teste-stop.png)

23. Na tela **Configure test event**, selecione **Create new test event**.  Digite um nome para o evento em **Event name** e clique em Create

![null](/images/uploads/stop-test.png)

![null](/images/uploads/stop-test-1.png)

A próxima tela retornará uma mensagem de sucesso sobre a criação da função **stopinstance**. Agora execute o teste

![null](/images/uploads/execute-teste.png)

24. Verifique o log após a sua execução

![null](/images/uploads/log.png)

25. Agora verifique o status da instância

![null](/images/uploads/stopping.png)

Com as regras e funções devidamente configuradas e testadas, podemos passar para parte final que é a configuração do scheduler, que fará a execução das novas regras e funções de acordo com a necessidade de utilização.

## Configurando rules no CloudWatch

26. Acesse novamente o menu **Services**, e no campo de busca digite o nome do serviço **CloudWatch**

![null](/images/uploads/service-cloudwatch.png)

27. Acessando o **CloudWatch**, localize no lado esquerdo **Rules** e clique nele

![null](/images/uploads/rule-cloudwatch.png)

28. Na próxima tela, clique em **Create rule**

![null](/images/uploads/create-rule-cloudwatch.png)

29. No **Step 1** de configuração da **Rule** do **CloudWatch** siga as etapas abaixo:

Em **Event Source** marque os campos **Schedule**, **Cron expression** e preencha com a expressão **01 12 ? \* MON-FRI \***

_**No exemplo abaixo vamos iniciar a instância as 9 horas da manhã e desligar as 18 da tarde.**_

30. A direita do painel em  **Targets** em **functions** selecione o nome da função **startinstance** e clique em **Configure details**

> **_IMPORTANTE_**
>
> **_O CloudWatch usa o padrão de hora UTC-0. Então para configurar o horário considere 3 horas a mais. No exemplo abaixo, vamos iniciar a instância as 09:01Horas, logo a a expressão 01 12._**

![null](/images/uploads/create-rule-cloudwatch-step-1.png)

31. No **Step 2 Configure rule details**, digite o nome da **Rule definition**. **Name: Startinstance**, em **Description**: forneça uma descrição sobre a regra. Verifique se o campo **State** está marcado como **Enabled** e clique em **Create rule**.

![null](/images/uploads/create-rule-cloudwatch-step-2.png)

A próxima tela informa que a rule foi criada com sucesso.

![null](/images/uploads/rule-cloudwatch-success.png)

32. Refaça o **Step 2 Configure rule details** para criar a **rule de Stop**.

## **Referências:**

\
[Schedule AWS Lambda Functions Using CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/RunLambdaSchedule.html)\
[CloudWatch Events Event Examples From Supported Services](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html)\
[Adding Stop Actions to Amazon CloudWatch Alarms](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/UsingAlarmActions.html#AddingStopActions)

## 

##
