# Laboratório Linux com Vagrant

Ambiente de laboratório utilizado nos estudos e aulas práticas do **Aprenda Linux BR**.

Este projeto utiliza o **Vagrant** para automatizar a criação e o gerenciamento das máquinas virtuais, utilizando o **VirtualBox** como provedor de virtualização.

## Dependências

Antes de utilizar este laboratório, é necessário ter instalado no computador:

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads)
- [Git](https://git-scm.com/downloads)

Também é necessário que a virtualização por hardware esteja habilitada no computador.

## Laboratório

As máquinas virtuais utilizadas neste ambiente são definidas no arquivo `Vagrantfile`.

| Máquina | Sistema Operacional | vCPUs | Memória RAM | IP privado |
|---|---|---:|---:|---|
| `ubuntu-linux` | Ubuntu 24.04 LTS | 2 | 2048 MB | `192.168.56.10` |
| `oracle-linux` | Oracle Linux 9 | 2 | 2048 MB | `192.168.56.11` |

Cada máquina virtual é criada com:

- 2 vCPUs;
- 2048 MB de memória RAM;
- uma interface de rede privada com IP fixo;
- VirtualBox como provedor de virtualização.

Além da rede privada configurada no `Vagrantfile`, o Vagrant utiliza a interface de rede padrão necessária para o funcionamento e gerenciamento das máquinas virtuais.

## Recursos do computador host

Ao iniciar as duas máquinas simultaneamente, aproximadamente **4 GB de memória RAM serão destinados às máquinas virtuais**.

Esse valor não inclui a memória utilizada pelo próprio sistema operacional do computador, navegador, terminal e outros programas.

Por esse motivo, para executar o laboratório completo com maior estabilidade, recomendamos:

- **8 GB de memória RAM ou mais**;
- processador com suporte à virtualização por hardware;
- virtualização habilitada na BIOS/UEFI;
- espaço disponível em disco para armazenar as imagens e máquinas virtuais.

Cada VM recebe 2 vCPUs. Isso não significa que sejam necessários dois núcleos físicos exclusivos para cada máquina, pois o VirtualBox utiliza os processadores lógicos disponíveis no computador host.

> **Observação:** computadores com poucos recursos podem apresentar lentidão ao executar as duas máquinas simultaneamente.

## Obtendo o laboratório

Para obter os arquivos deste laboratório, clone este repositório utilizando o Git:

```bash
git clone https://github.com/MoissesPinheiro/laboratorio-linux-vagrant.git
```

Após o download, acesse o diretório do projeto:

```bash
cd laboratorio-linux-vagrant
```

## Iniciando o laboratório

Dentro do diretório do projeto, execute:

```bash
vagrant up
```

O Vagrant irá ler o arquivo `Vagrantfile` e solicitar ao VirtualBox a criação e inicialização das máquinas virtuais.

Na primeira execução, o processo pode levar alguns minutos, pois as imagens base utilizadas pelas máquinas podem precisar ser baixadas.

## Verificando o estado das máquinas

Para verificar o estado das máquinas virtuais:

```bash
vagrant status
```

## Acessando as máquinas

Para acessar a máquina Ubuntu:

```bash
vagrant ssh ubuntu-linux
```

Para acessar a máquina Oracle Linux:

```bash
vagrant ssh oracle-linux
```

## Comandos principais

| Comando | Função |
|---|---|
| `vagrant up` | Cria e inicia as máquinas virtuais |
| `vagrant status` | Mostra o estado das máquinas |
| `vagrant ssh ubuntu-linux` | Acessa a máquina Ubuntu via SSH |
| `vagrant ssh oracle-linux` | Acessa a máquina Oracle Linux via SSH |
| `vagrant halt` | Desliga as máquinas virtuais |
| `vagrant suspend` | Suspende as máquinas virtuais |
| `vagrant resume` | Retoma as máquinas suspensas |
| `vagrant destroy` | Remove as máquinas virtuais criadas |

## Compatibilidade

Esta primeira versão do laboratório está sendo preparada e testada para:

- Windows;
- arquitetura x86-64;
- VirtualBox como provedor de virtualização.

O suporte a outras plataformas e arquiteturas poderá ser adicionado posteriormente.
