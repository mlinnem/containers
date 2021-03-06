---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Tutorial: usando as políticas de rede do Calico para bloquear o tráfego
{: #policy_tutorial}

Por padrão, os serviços NodePort, LoadBalancer e Ingresso do Kubernetes disponibilizam o seu app em todas as interfaces de rede de cluster pública e privada. A política padrão do Calico `allow-node-port-dnat` permite o tráfego recebido de serviços NodePort, LoadBalancer e Ingresso para os pods de app que esses serviços expõem. O Kubernetes usa a conversão de endereço de rede de destino (DNAT) para encaminhar as solicitações de serviço para os pods corretos.

No entanto, por razões de segurança, pode ser necessário permitir o tráfego para os serviços de rede somente de determinados endereços IP de origem. É possível usar as [políticas pré-DNAT do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") ](https://docs.projectcalico.org/v3.1/getting-started/bare-metal/policy/pre-dnat) para incluir na lista de desbloqueio ou lista de bloqueio o tráfego de/para determinados endereços IP. As políticas pré-DNAT evitam que o tráfego especificado atinja seus apps porque elas são aplicadas antes que o Kubernetes use DNAT regular para encaminhar tráfego para os pods. Ao criar políticas pré-DNAT do Calico, você escolhe se deseja incluir na lista de desbloqueio ou lista de bloqueio os endereços IP de origem. Para a maioria dos cenários, a lista de aplicativos confiáveis fornece a configuração mais segura porque todo o tráfego está bloqueado, exceto o tráfego de endereços IP de origem conhecidos e permitidos. A listagem negra é geralmente útil somente em cenários, tais como, evitar um ataque de um pequeno conjunto de endereços IP.

Nesse cenário, você executa a função de um administrador de rede para uma firma PR e observa algum tráfego incomum atingindo seus apps. As lições neste tutorial orientam passo a passo a criação de um aplicativo de servidor da web de amostra, expondo o app usando um serviço LoadBalancer e protegendo o app de tráfego incomum indesejado com as políticas do Calico de lista de desbloqueio e lista de bloqueio.

## Objetivos

- Aprenda a bloquear todo o tráfego recebido para todos os NodePorts criando uma política Pré-DNAT de alta ordem.
- Aprenda a permitir que os endereços IP de origem incluídos na lista de desbloqueio acessem o IP público e a porta do LoadBalancer criando uma política pré-DNAT de baixa ordem. As políticas de ordem inferior substituem as políticas de ordem mais alta.
- Aprenda a bloquear endereços IP de origem incluídos na lista de bloqueio do acesso ao IP público e porta do LoadBalancer criando uma política pré-DNAT de baixa ordem.

## Tempo Necessário
1 hora

## Público
Este tutorial é destinado a desenvolvedores de software e administradores de rede que desejam gerenciar o tráfego de rede para um app.

## Pré-requisitos

- [Crie um cluster da versão 1.10](cs_clusters.html#clusters_ui) ou [atualize um cluster existente para a versão 1.10](cs_versions.html#cs_v110). Um cluster do Kubernetes versão 1.10 ou mais recente é necessário para usar a CLI do Calico 3.1.1 e a sintaxe de política do Calico v3 neste tutorial.
- [Destinar sua CLI para o cluster](cs_cli_install.html#cs_cli_configure).
- [ Instale e configure a CLI do Calico ](cs_network_policy.html#1.10_install).

<br />


## Lição 1: implementar um app e expô-lo usando um LoadBalancer
{: #lesson1}

A primeira lição mostra como seu app é exposto por meio de múltiplos endereços IP e portas e onde o tráfego público está chegando em seu cluster.
{: shortdesc}

Inicie implementando um app de servidor da web de amostra para usar em todo o tutorial. O servidor da web `echoserver` mostra dados sobre a conexão que está sendo feita com o cluster por meio do cliente e permite que você teste o acesso ao cluster da firma PR. Em seguida, exponha o app criando um serviço LoadBalancer. Um serviço do LoadBalancer torna o seu app disponível sobre o endereço IP do serviço do LoadBalancer e as NodePorts dos nós do trabalhador.

A imagem a seguir mostra como o app de servidor da web será exposto à Internet pelo NodePort público e o LoadBalancer público no término da Lição 1:

<img src="images/cs_tutorial_policies_Lesson1.png" width="450" alt="No término da Lição 1, o app de servidor da web é exposto à Internet pelo NodePort público e o LoadBalancer público." style="width:450px; border-style: none"/>

1. Crie um namespace de teste chamado `pr-firm` para usar neste tutorial.
    ```
    kubectl create ns pr-firm
    ```
    {: pre}

2. Implemente o app do servidor da web de amostra. Quando uma conexão é feita com o app de servidor da web, o app responde com os cabeçalhos de HTTP que ele recebeu na conexão.
    ```
    kubectl run webserver -n pr-firm --image=k8s.gcr.io/echoserver:1.10 --replicas=3
    ```
    {: pre}

3. Verifique se os pods de app de servidor da web têm um **STATUS** de `Running`.
    ```
    kubectl get pods -n pr-firm -o wide
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAME                         READY     STATUS    RESTARTS   AGE       IP               NODE
    webserver-855556f688-6dbsn   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-76rkp   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-xd849   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    ```
    {: screen}

4. Para expor o app à Internet pública, crie um arquivo de configuração de serviço do LoadBalancer chamado `webserver.yaml` em um editor de texto.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: webserver
      name: webserver-lb
      namespace: pr-firm
    spec:
      externalTrafficPolicy: Cluster
      ports:
      - name: webserver-port
        port: 80
        protocol: TCP
        targetPort: 8080
      selector:
        run: webserver
      type: LoadBalancer
    ```
    {: codeblock}

5. Implemente o LoadBalancer.
    ```
    kubectl apply -f filepath/webserver.yaml
    ```
    {: pre}

6. Verifique se é possível acessar publicamente o app exposto pelo LoadBalancer por meio de seu computador.

    1. Obtenha o endereço público **EXTERNAL-IP** do LoadBalancer.
        ```
        kubectl get svc -n pr-firm -o wide
        ```
        {: pre}

        Saída de exemplo:
        ```
        NAME CLUSTER-IP EXTERNAL-IP PORT(S) AGE SELECTOR webserver-lb 172.21.xxx.xxx 169.xx.xxx.xxx 80:31024/TCP 2m run=webserver
        ```
        {: screen}

    2. Crie um arquivo de texto de folha de dicas e copie o IP do LoadBalancer para o arquivo de texto. A folha de dicas o ajudará a usar mais rapidamente os valores em lições posteriores.

    3. Verifique se é possível acessar publicamente o IP externo para o LoadBalancer.
        ```
        curl --connect-timeout 10 <loadbalancer_IP>:80
        ```
        {: pre}

        A saída de exemplo a seguir confirma que o LoadBalancer expõe seu app no endereço IP público do LoadBalancer `169.1.1.1`. O pod de app `webserver-855556f688-76rkp` recebeu a solicitação de curl:
        ```
        Hostname: webserver-855556f688-76rkp
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://169.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=169.1.1.1
            user-agent=curl/7.54.0
        Request Body:
            -no body in request-
        ```
        {: screen}

6. Verifique se é possível acessar publicamente o app exposto pelo NodePort de seu computador. Um serviço do LoadBalancer torna o seu app disponível sobre o endereço IP do serviço do LoadBalancer e as NodePorts dos nós do trabalhador.

    1. Obtenha o NodePort que o LoadBalancer designou aos nós do trabalhador. O NodePort está no intervalo 30000 - 32767.
        ```
        kubectl get svc -n pr-firm -o wide
        ```
        {: pre}

        Na saída de exemplo a seguir, o NodePort é `31024`:
        ```
        NAME CLUSTER-IP EXTERNAL-IP PORT(S) AGE SELECTOR webserver-lb 172.21.xxx.xxx 169.xx.xxx.xxx 80:31024/TCP 2m run=webserver
        ```
        {: screen}  

    2. Obtenha o endereço **IP público** de um nó do trabalhador.
        ```
        ibmcloud ks workers <cluster_name>
        ```
        {: pre}

        Saída de exemplo:
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.176.48.67   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w2   169.xx.xxx.xxx   10.176.48.79   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w3   169.xx.xxx.xxx   10.176.48.78   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        ```
        {: screen}

    3. Copie o IP público do nó do trabalhador e o NodePort em sua folha de dicas de texto para usar em lições posteriores.

    4. Verifique se é possível acessar o endereço IP público do nó do trabalhador por meio do NodePort.
        ```
        curl  --connect-timeout 10 <worker_IP>:<NodePort>
        ```
        {: pre}

        A saída de exemplo a seguir confirma que a solicitação para seu app veio por meio do endereço IP privado `10.1.1.1` para o nó do trabalhador e o NodePort `31024`. O pod de app `webserver-855556f688-xd849` recebeu a solicitação de curl:
        ```
        Hostname: webserver-855556f688-xd849
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://10.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=10.1.1.1:31024
            user-agent=curl/7.60.0
        Request Body:
            -no body in request-
        ```
        {: screen}

Neste momento, seu app é exposto por meio de múltiplos endereços IP e portas. A maioria desses IPs é interna para o cluster e pode ser acessada somente por meio da rede privada. Somente o NodePort público e a porta do LoadBalancer público são expostos à Internet pública.

Em seguida, é possível iniciar a criação e aplicação de políticas do Calico para bloquear o tráfego público.

## Lição 2: bloquear todo o tráfego recebido para todos os NodePorts
{: #lesson2}

Para proteger o cluster da firma PR, deve-se bloquear o acesso público para o serviço LoadBalancer e os NodePorts que estão expondo seu app. Comece bloqueando o acesso aos NodePorts. A imagem a seguir mostra como o tráfego será permitido para o LoadBalancer, mas não para os NodePorts no término da Lição 2:

<img src="images/cs_tutorial_policies_Lesson2.png" width="450" alt="No término da Lição 2, o app de servidor da web é exposto à Internet somente pelo LoadBalancer público." style="width:450px; border-style: none"/>

1. Em um editor de texto, crie uma política pré-DNAT de alta ordem chamada `deny-nodeports.yaml` para negar o tráfego TCP e UDP recebido de qualquer IP de origem para todos os NodePorts.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-nodeports
    spec:
      applyOnForward: true
      ingress:
      - action: Deny destination: ports:
          - 30000:32767
            protocol: TCP
            source: {}
      - action: Deny destination: ports:
          - 30000:32767 protocol: UDP source: {} preDNAT: true selector: ibm.role=='worker_public' order: 1100 types:
      - Entrada
    ```
    {: codeblock}

2. Aplique a política.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml
      ```
      {: pre}

    - Windows e S.O. X:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml -- config=filepath/calicoctl.cfg
      ```
      {: pre}
  Saída de exemplo:
  ```
  Recurso (s) de 1 'GlobalNetworkPolicy' aplicado com sucesso
  ```
  {: screen}

3. Usando os valores de sua folha de dicas, verifique se não é possível acessar publicamente o endereço IP público do nó do trabalhador e o NodePort.
    ```
    curl  --connect-timeout 10 <worker_IP>:<NodePort>
    ```
    {: pre}

    A conexão atinge o tempo limite porque a política do Calico que você criou está bloqueando o tráfego para NodePorts.
    ```
    curl: (28) Tempo de conexão esgotado após 10016 milissegundos
    ```
    {: screen}

4. Mude o externalTrafficPolicy do LoadBalancer que você criou na lição anterior de `Cluster` para `Local`. `Local` garante que o IP de origem do sistema seja preservado ao fazer curl do IP externo do LoadBalancer na próxima etapa.
    ```
    kubectl patch svc -n pr-firm webserver -p '{"spec":{"externalTrafficPolicy":"Local"}}'
    ```
    {: pre}

5. Usando o valor de sua folha de dicas, verifique se ainda é possível acessar publicamente o endereço IP externo do LoadBalancer.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

    Saída de exemplo:
    ```
    Hostname: webserver-855556f688-76rkp
    Pod Information:
        -no pod information available-
    Server values:
        server_version=nginx: 1.13.3 - lua: 10008
    Request Information:
        client_address=1.1.1.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://<loadbalancer_IP>:8080/
    Request Headers:
        accept=*/*
        host=<loadbalancer_IP>
        user-agent=curl/7.54.0
    Request Body:
        -no body in request-
    ```
    {: screen}
    Na seção `Request Information` da saída, observe que o endereço IP de origem é, por exemplo, `client_address=1.1.1.1`. O endereço IP de origem é o IP público do sistema que você está usando para executar curl. Caso contrário, se você estiver se conectando à Internet por meio de um proxy ou uma VPN, o proxy ou a VPN poderá estar obscurecendo o endereço IP real do seu sistema. Em qualquer dos casos, o LoadBalancer vê o endereço IP de origem do seu sistema como o endereço IP do cliente.

6. Copie o endereço IP de origem do seu sistema (`client_address=1.1.1.1` na saída de etapa anterior) em sua folha de dicas para usar em lições posteriores.

Ótimo! Neste momento, seu app é exposto à Internet pública somente por meio da porta do LoadBalancer público. O tráfego para os NodePorts públicos está bloqueado. Você bloqueou parcialmente seu cluster contra tráfego indesejado.

Em seguida, é possível criar e aplicar políticas do Calico para incluir na lista de desbloqueio o tráfego de certos IPs de origem.

## Lição 3: permitir o tráfego recebido de um IP incluído na lista de desbloqueio para o LoadBalancer
{: #lesson3}

Agora você decide bloquear completamente o tráfego para o cluster da firma PR e testar o acesso incluindo na lista de desbloqueio o endereço IP do seu próprio computador.
{: shortdesc}

Primeiro, além dos NodePorts, deve-se bloquear todo o tráfego recebido para o LoadBalancer que está expondo o app. Em seguida, é possível criar uma política que inclua na lista de desbloqueio o endereço IP do sistema. No término da Lição 3, todo o tráfego para os NodePorts e LoadBalancer públicos será bloqueado e somente o tráfego de seu IP do sistema incluído na lista de desbloqueio será permitido:
<img src="images/cs_tutorial_policies_L3.png" width="600" alt="O app de servidor da web é exposto pelo LoadBalancer público somente para seu IP do sistema." style="width:600px; border-style: none"/>

1. Em um editor de texto, crie uma política pré-DNAT de alta ordem chamada `deny-lb-port-80.yaml` para negar todo o tráfego TCP e UDP recebido de qualquer IP de origem para o endereço IP e porta do LoadBalancer. Substitua `<loadbalancer_IP>` pelo endereço IP público do LoadBalancer de sua folha de dicas.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-lb-port-80
    spec:
      applyOnForward: true
      ingress:
      - ação: negar destino: redes:
          - <loadbalancer_IP>/32 portas:
          - 80
        protocol: TCP
        source: {}
      - ação: negar destino: redes:
          - <loadbalancer_IP>/32 portas:
          - 80
        protocol: UDP
        source: {}
      preDNAT: true
      selector: ibm.role=='worker_public'
      order: 1100
      types:
      - Entrada
    ```
    {: codeblock}

2. Aplique a política.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml
      ```
      {: pre}

    - Windows e S.O. X:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml -- config=filepath/calicoctl.cfg
      ```
      {: pre}

3. Usando o valor de sua folha de dicas, verifique se agora não é possível acessar o endereço IP do LoadBalancer público. A conexão atinge o tempo limite porque a política do Calico que você criou está bloqueando o tráfego para o LoadBalancer.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

4. Em um editor de texto, crie uma política pré-DNAT de baixa ordem chamada `whitelist.yaml` para permitir o tráfego do IP de seu sistema para o endereço IP e porta do LoadBalancer. Usando os valores de sua folha de dicas, substitua `<loadbalancer_IP>` com o endereço IP público do LoadBalancer e `<client_address>` com o endereço IP público do IP de origem do seu sistema.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: whitelist
    spec:
      applyOnForward: true
      ingress:
      - action: Allow
        destination:
          nets:
          - <loadbalancer_IP>/32 portas:
          - 80 protocol: TCP source: nets:
          - <client_address>/32 preDNAT: seletor true: ibm.role=='worker_public' pedido: 500 tipos:
      - Entrada
    ```
    {: codeblock}

5. Aplique a política.
    - Linux:

      ```
      calicoctl apply -f filepath/whitelist.yaml
      ```
      {: pre}

    - Windows e S.O. X:

      ```
      calicoctl apply -f filepath/whitelist.yaml -- config=filepath/calicoctl.cfg
      ```
      {: pre}
  O endereço IP do seu sistema está agora incluído na lista de desbloqueio.

6. Usando o valor de sua folha de dicas, verifique se agora é possível acessar o endereço IP do LoadBalancer público.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

7. Se você tiver acesso a outro sistema que tenha um endereço IP diferente, tente acessar o LoadBalancer por meio desse sistema.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    A conexão atinge o tempo limite porque o endereço IP desse sistema não está incluído na lista de desbloqueio.

Neste momento, todo o tráfego para os NodePorts e LoadBalancer públicos está bloqueado. Somente o tráfego de seu IP do sistema incluído na lista de desbloqueio é permitido.

## Lição 4: negar o tráfego recebido de IPs incluídos na lista de bloqueio para o LoadBalancer
{: #lesson4}

Na lição anterior, você bloqueou todo o tráfego e incluiu na lista de desbloqueio somente alguns IPs. Esse cenário funciona bem para propósitos de teste quando você deseja limitar o acesso a somente alguns endereços IP de origem controlada. No entanto, a firma PR tem apps que precisam estar amplamente disponíveis para o público. É necessário certificar-se de que todo o tráfego seja permitido, exceto o tráfego incomum que você está vendo de alguns endereços IP. A listagem negra é útil em um cenário como este porque pode ajudar a evitar um ataque de um pequeno conjunto de endereços IP.

Nesta lição, você testará a listagem negra bloqueando o tráfego do endereço IP de origem de seu próprio sistema. No término da Lição 4, todo o tráfego para os NodePorts públicos será bloqueado e todo o tráfego para o LoadBalancer público será permitido. Somente o tráfego de seu IP do sistema incluído na lista de bloqueio para o LoadBalancer será bloqueado:
<img src="images/cs_tutorial_policies_L4.png" width="600" alt="O app de servidor da web é exposto pelo LoadBalancer público à Internet. Somente o tráfego de seu IP do sistema é bloqueado." style="width:600px; border-style: none"/>

1. Limpe as políticas de lista de desbloqueio que você criou na lição anterior.
    ```
    calicoctl delete GlobalNetworkPolicy deny-lb-port-80
    ```
    {: pre}
    ```
    calicoctl delete GlobalNetworkPolicy whitelist
    ```
    {: pre}
    Agora, todo o tráfego TCP e UDP recebido de qualquer IP de origem para o endereço IP e porta do LoadBalancer é permitido novamente.

2. Para negar todo o tráfego TCP e UDP recebido do endereço IP de origem do seu sistema para o endereço IP e porta do LoadBalancer, crie uma política pré-DNAT de baixa ordem chamada `deny-lb-port-80.yaml` em um editor de texto. Usando os valores de sua folha de dicas, substitua `<loadbalancer_IP>` com o endereço IP público do LoadBalancer e `<client_address>` com o endereço IP público do IP de origem do seu sistema.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: blacklist
    spec:
      applyOnForward: true
      ingress:
      - ação: negar destino: redes:
          - <loadbalancer_IP>/32 portas:
          - 80 protocol: TCP source: nets:
          - < client_address> / 32
      - ação: negar destino: redes:
          - <loadbalancer_IP>/32 portas:
          - 80
        protocol: UDP
        source:
          nets:
          - <client_address>/32 preDNAT: seletor true: ibm.role=='worker_public' pedido: 500 tipos:
      - Entrada
    ```
    {: codeblock}

3. Aplique a política.
    - Linux:

      ```
      calicoctl apply -f filepath/blacklist.yaml
      ```
      {: pre}

    - Windows e S.O. X:

      ```
      calicoctl apply -f filepath/blacklist.yaml -- config=filepath/calicoctl.cfg
      ```
      {: pre}
  O endereço IP do seu sistema está agora incluído na lista de bloqueio.

4. Usando o valor de sua folha de dicas, verifique em seu sistema se não é possível acessar o IP do LoadBalancer porque o IP do seu sistema está incluído na lista de bloqueio.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    Neste momento, todo o tráfego para os NodePorts públicos está bloqueado e todo o tráfego para o LoadBalancer público é permitido. Somente o tráfego de seu IP do sistema incluído na lista de bloqueio para o LoadBalancer está bloqueado.

5. Para limpar essa política de lista de bloqueio:
    ```
    delete calicoctl delete GlobalNetworkPolicy blacklist
    ```
    {: pre}

Bom trabalho! Você controlou com êxito o tráfego em seu app usando as políticas pré-DNAT do Calico para incluir na lista de desbloqueio e lista de bloqueio os IPs de origem.

## O que Vem a Seguir?
{: #whats_next}

* Leia mais sobre [como controlar o tráfego com políticas de rede](cs_network_policy.html).
* Para obter mais políticas de rede do Calico de exemplo que controlam o tráfego para e por meio do seu cluster, é possível efetuar check-out da [demo de política de estrelas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/stars-policy/) e da [política de rede avançada ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy).
