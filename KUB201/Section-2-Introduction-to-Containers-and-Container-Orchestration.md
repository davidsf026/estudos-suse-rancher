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
