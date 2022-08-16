# Comandos Básicos do Kubernetes

## O comando *kubectl*:

- É o principal comando para se interagir com o Kubernetes.
- O arquivo de configuração padrão fica em: ~/.kube/config
- Sintaxe: kubectl VERB RESOURCE [OPTIONS]

# Sobre Namespaces

- É uma abstração do Kubernetes para sustentar vários clusters virtuais no mesmo cluster físico.
- Organiza os objetos de um cluster de uma forma para dividir os recursos do cluster.
- Os objetos de um cluster podem ser os mesmos desde que estejam em namespaces diferentes.
- Nem todo recurso do Kubernetes está dentro de um namespace, exemplos:
	- Node
	- PV
- Default Namespaces:
	- **default:** 
	- **kube-system:** Objetos criados pelo próprio Kubernetes e workloads de cluster.
	- **kube-public:** 
	- **kube-node-lease:** 

# Sobre Manifestos de Kubernetes

- São arquivos que descrevem como o Kubernetes deve configurar objetos ou até mesmo o próprio cluster.
- Mais simples do que escrever cada instrução manualmente via API	ou com o comando kubectl.
- São escritos em YAML.
- Fácil de integrar no source control (GitHub).

## A Estrutura de um Manifesto de Kubernetes

Define o tipo do objeto que está sendo criado.

    apiVersion:
    kind:

Metadados do objeto que está sendo criado, por exemplo, labels.

    Metadata:
      labels:
 
Detalhes do objeto que está sendo criado.

    spec:
      selector:
      
      template:

## Boas Práticas Para Manifestos de Kubernetes

- Um manifesto faz o deploy de apenas um componente.
- Especifique a versão da imagem do container.
- Sempre use um Deployment (mesmo que seja apenas 1 pod).
- Defina e use variáveis de ambiente.

# Deployment Multi-Pod

## Configurações de Deployment Multi-Pod

- **ReplicaSet:** É o recurso responsável por manter um número pré-determinado de réplicas de um pod, em execução.
	- Aplicações stateful são aquelas executadas com base no contexto das transações anteriores, ou seja precisam de persistência.
- **StatefulSet:**
	- O StatefulSet define para cada pod (vindos da mesma spec) um identificador de persistência, logo, seus dados não irão se misturar.
- **DaemonSet:** Para definir em quais nodes determinada aplicação irá rodar.
- **Deployment:**
	- Provê atualizações declarativas para pods e ReplicaSets
	- Deployments normalmente fazem o deploy de ReplicaSets automaticamente.
	- Tem a habilidade de usar simple e rolling updates.
	- Podem ser Stateful e Stateless.
	- Casos de uso do Deployment:
		- Fazer o rollout de um ReplicaSet.
		- Declarar um novo estado dos pods a partir da alteração do "PodTemplateSpec".
		- Fazer o rollback de um Deployment anterior.
		- Escalar o Deployment.
		- Usar o status de um Deployment como métrica para ver se o rollout está sendo executado com sucesso.

# Usando o Recurso Deployments

- Um recurso de Deployment é CRIA ou ATUALIZA Pods e ReplicaSets, esse é declarativo.
- Estratégias de Deployment (atualização):
	- Rolling Update
	- Recreation Update
- Quando um Deployment é apagado todos os recursos relacionados a eles também serão apagados.
- Para deletar um Pod de um Deployment você precisará apagar ou alterar o Deployment, pois, se apenas o Pod for apagado ele será recriado.

# Configurar Rede Para Aplicações no Kubernetes

## Serviço

- Serviços são recurso que permitem que apps sejam acessíveis pela internet, para usuários ou até mesmo para outros aplicativos.
- Um serviço encontra um Pod através de uma label.
- Provê uma interface de rede estável para as aplicações.

### Tipos de Serviço:

#### ClusterIP

- A aplicação só pode ser acessada DENTRO do cluster.
- Nem toda aplicação precisa de ser acessada fora do cluster. Um exemplo são bancos de dado, muitas das vezes esse só precisa ser acessível para a aplicação que está dentro cluster.
- Todo Pod tem o seu próprio IP interno e por padrão esse não fica disponível para fora do cluster.

#### NodePort

- A aplicação pode ser acessada de FORA do cluster.
- As aplicações expostas por esse tipo de serviço são acessíveis a partir de uma porta reservada para a mesma: `http://IP-ou-FQDN:PORTA`
- A porta é acessível em todos os nodes.
- É preciso definir qual porta está sendo escutada no container (port) e qual porta será exposta no IP/FQDN do cluster (nodePort).
- Se o nodePort for definido como 0, será definida uma porta disponível aleatória.
- Por padrão as portas disponíveis para nodePort são de 30000 à 32767.

#### LoadBalancer

- É mais usado na cloud, mas é possível sim, instalar um LoadBalancer em um cluster bare metal.
- O LoadBalancer não vem nativo no Kubernetes, é necessário instalar softwares de terceiros como o Metallb.
- Nesse caso o serviço vai bater no LoadBalancer e o resto é com ele.

# Variáveis de Ambiente

- Podem ser definidas quando um pod é deployado.
- São comumente usadas para configurar a aplicação (as possíveis variáveis de ambiente de configuração normalmente ficam descritas na documentação da imagem).
- As variáveis ficam na seção `env` do YAML.

# ConfigMaps

- É outra forma de ter variáveis de ambiente no seu Pod.
- É definida fora do manifesto, pois é um objeto do Kubernetes, logo, permite ter mais flexibilidade na definição das variáveis de ambiente.
- Para chamar um ConfigMap na hora de definir a variáveis de ambiente do Pod deve-se seguir a seguinte estrutura:

	    envFrom:
	      - configMapRef:
	          name: my-configmap

# Secrets

- Ao definir via YAML, devem ser codificadas em base64.
- Secrets podem ser armazenados em Chave/Valor ou em arquivos.

# Segregação de Recursos (Labels, Selectors, Taints e etc.)

Labels e Selector são declarados na MESMA SEÇÃO, a `Labels` (na minha opinião eles são basicamente a mesma coisa.)

## Labels

- Metadata para objetos que geralmente representam identidade.
- Podem ser definidos diretamente nos manifestos ou manualmente.

## Selectors

- Tem por função segregar requisições para determinados objetos, por exemplo: Somente os pods com o selector "escalável" terão 8 réplicas.

## NodeSelectors

- Labels que são aplicadas em Nodes.
- Segregar requisições para rodar em determinados Nodes, podendo ser duas formas, por atração e por rejeição
	- Rejeição: Pod NÃO será deployado em Nodes com a label "teste".
	- Atração: Pod SÓ será deployado em Nodes com a label "teste".
- Para segregar um recurso para determinado node, deve-se seguir a seguinte sintaxe do recurso a ser deployado.

		spec:
		  nodeSelector:
		    key: value

## Taints

- Exemplo:

	    kubectl taint nodes worker01 frutafavorita=maracuja:NoSchedule
	    
Esse comando diz que objetos que NÃO tiverem essa label NÃO poderão ser schedulados.

## Tolerations

- Exemplo (YAML de um Pod):

	    spec:
	      tolerations:
	      - key: "frutafavorita"
	        operator: "Equal"
			value: "maracuja"
			effect: "NoSchedule"
			
O Pod só poderá ser Deployado em nodes que tiverem uma taint com esses parâmetros.