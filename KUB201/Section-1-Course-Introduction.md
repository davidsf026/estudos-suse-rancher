# Entendendo os Conceitos de Container

## Artefatos de Um Container Environment:

- **Container:** Um OS minimizado rodando em cima de um OS host.
- **Imagem:** O esqueleto de um container.
- **Registry:** Lugar onde são armazenadas as imagens de containers.
- **Container Engine:** O motor rodar e gerenciar os containers.

## Comparação VM/Container/Chroot

### Isolamento:

- **Virtual Machine:** Isolamento total.
- **Container:** Isolamendo do filesystem, dos processos e da rede.
- **Chroot:** Isolamendo apenas do filesystem.

### Portabilidade:

- **Virtual Machine:** Completamente portátil.
- **Container:** Completamente portátil.
- **Chroot:** Não portátil.

### Contém:

- **Virtual Machine:** Contém um OS completo e o seu próprio kernel.
- **Container:** Contém um OS completo mas acessa o kernel do host. Mas na maioria das vezes não é necessário ter o OS completo, pode-se deixar apenas o que aplicação precisa para rodar (bibliotecas e aplicações).
- **Chroot:** Qualquer coisa que se desejar.

### Velocidade:

- **Virtual Machine:** Relativamente rápido para iniciar. Sua velocidade de execução é PRÓXIMA a do host.
- **Container:** Inicialização MUITO rápida. Sua velocidade de execução é MESMA a do host.
- **Chroot:** Inicialização MUITO rápida. Sua velocidade de execução é MESMA a do host.

## A Respeito de Imagens de Containers

- Uma imagem é a base para um container.
- Uma imagem vira container a partir da adição de uma camada de rw (read & write) na imagem do container.
- Quando um container entra em execução, esse não copia a imagem:
	- O que acontece é apenas a adição de uma camada de RW.
	- Vários containers podem ser instanciados de uma única imagem ao mesmo tempo.
- Imagens de containers são armazenadas em registries.

## A Respeito de Containers Engines:

- Fazendo um parelo com VM's, é um "Hypervisor".
- Provê uma interface de rede
- Separa os containers uns dos outros.
- Provê uma forma de containers persistirem dados.

# Entendendo a Arquitetura de Microserviços

- Os microserviços se comunicam por meio de API's. Geralmente cada microserviço tem uma API própria e essa se comunica com uma API Gateway.
- A arquitetura de microserviços não é exclusiva para containers, mas se dá muito bem com os containers.
- Benefícios da arquitetura de microserviços:
	- Leva a modularização para outro nível.
	- As aplicações ficam mais fáceis de serem desenvolvidas, testadas e entendidas.
	- Produzir e entregar um produto com mais rapidez e qualidade.
	
# Entendendo a Arquitetura de Kubernetes:

## **O que é Kubernetes:**

É uma ferramenta (framework) de gerenciamento de cargas de trabalho e serviços de containers, em outras palavras é um orquestrador de containers.
	
## Para que server o Kubernetes?
	
- Deploy de aplicações.
- Gerenciamento de aplicações.
- Permitir o acesso à aplicações.
- Fazer o escalamento de aplicações (permitir a alta disponibilidade).
- Agendamento de containers.
	
## Kubernetes é "Developer Friendly":

- Faz a abstração da infraestrutura em API's consumíveis.
- Permite que os developers gerenciem aplicações e não máquinas.

## Infraestrutura do Cluster Kubernetes:

### Controller Node:

- Também conhecido como Control Plane.
- Normalmente roda as workloads de funcionamento do cluster:
	- kube-apiserver
	- kube-controller-manager
	- kube-scheduller
	- etcd
- Normalmente não roda as workloads de usuário.

### Worker Node:

- Roda as workloads de usuário.
- Normalmente roda:
	- kubelet
	- kube-proxy
	- User workloads (pods)

## Infraestrutura de Aplicações no Kubernetes

- **Container:** Um invólucro SELADO que carrega uma aplicação.
- **Pod:** Um pequeno grupo de um ou mais containers que são altamente relacionáveis. É a menor unidade de gerenciamento do Kubernetes.
- **Controller:** Um constante loop que mantém componentes em determinado estado, relacionados a pod e containers, há dois tipos de controllers, Deployment e ReplicaSet.
- **Service:** Recurso que direciona o tráfego de rede para os containers da aplicação, dentro e fora do cluster.
- **Namespace:** Uma separação lógica de grupos de recursos (isolamento).

![Hierarquia da Infraestrutura de Aplicações no Kubernetes](https://raw.githubusercontent.com/davidsf026/estudos-suse-rancher/master/KUB201/IMG/Hierarquia%20da%20Infraestrutura%20de%20Aplica%C3%A7%C3%B5es%20no%20Kubernetes.png)

# Ofertas do SUSE Rancher Kubernetes

## RKE (Rancher Kubernetes Engine)

- Distribuição certificada pelo CNCF (Cloud Native Computing Foundation).
- Baseado no Kubernetes vanilla, mas com um suporte 24x7.
- Instalação Simplificada
- Fácil, seguro, e atomic upgrade.
- Usa o Docker como container engine.

## RKE 2 (RKE Government)

- Distribuição certificada pelo CNCF (Cloud Native Computing Foundation) construída para agências do governo.
- Tem FIPS-enabled (Federal Information Processing Standards)
- Usa o Containerd como container engine.

## K3S

- Distribuição lightweight do Kubernetes e certificada pelo CNCF (Cloud Native Computing Foundation).
- Perfeito para deploys em IOT, Edge, CI e ARM.
- É empacotado em único binário para reduzir as dependências e simplificar o processo de instalação e update.

## Rancher

- NÃO é uma distribuição do Kubernetes.
- Plataforma de gerenciamento de Kubernetes.
- Pode gerenciar clusters de Kubernetes localmente e na nuvem.
- Permite gerenciar qualquer distribuição do Kubernetes.
- Provê operações de cluster simplificadas e gerenciamento de usuários, políticas e segurança.
