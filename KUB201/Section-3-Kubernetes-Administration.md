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
- **StatefulSet:**
	- Aplicações stateful são aquelas executadas com base no contexto das transações anteriores, ou seja precisam de persistência.
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
