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

O Vagrant irá ler o arquivo `Vagrantfile` e utilizar o VirtualBox para criar e iniciar as máquinas virtuais.

Na primeira execução, o processo pode levar alguns minutos, pois as boxes utilizadas como base para as máquinas virtuais podem precisar ser baixadas.

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

Para sair da sessão SSH e retornar ao terminal do Windows:

```bash
exit
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
| `vagrant reload` | Reinicia as máquinas e aplica alterações do Vagrantfile |
| `vagrant destroy` | Remove as máquinas virtuais criadas |

## Compatibilidade

Esta primeira versão do laboratório está sendo preparada e testada para:

- Windows;
- arquitetura x86-64;
- VirtualBox como provedor de virtualização.

O suporte a outras plataformas e arquiteturas poderá ser adicionado posteriormente.

## Importante sobre a rede do laboratório

Este laboratório utiliza a rede privada:

```text
192.168.56.0/24
```

As máquinas virtuais são configuradas com os seguintes endereços:

- `ubuntu-linux`: `192.168.56.10`
- `oracle-linux`: `192.168.56.11`

Antes de executar o comando `vagrant up` pela primeira vez, é recomendado verificar as interfaces e rotas existentes no computador host.

No Windows, abra o PowerShell e execute:

```powershell
ipconfig
```

Em seguida:

```powershell
route print
```

Procure por interfaces ou rotas que estejam utilizando a rede:

```text
192.168.56.0/24
```

ou, na tabela de rotas do Windows:

```text
Destino de rede: 192.168.56.0
Máscara:         255.255.255.0
```

### Atenção ao adaptador do próprio VirtualBox

Após a instalação do VirtualBox, poderá existir no Windows um adaptador de rede Host-Only pertencente ao próprio VirtualBox.

Por exemplo:

```text
VirtualBox Host-Only Ethernet Adapter
IPv4: 192.168.56.1
```

A presença desse adaptador não significa necessariamente que exista um conflito. Ele faz parte da infraestrutura de rede privada utilizada pelo VirtualBox.

O problema deve ser investigado quando a mesma faixa `192.168.56.0/24` estiver sendo utilizada por outra rede não relacionada ao laboratório, como:

- rede Wi-Fi;
- rede Ethernet;
- VPN;
- outro software de virtualização;
- outra rota configurada no computador.

### Caso exista conflito

Se a rede `192.168.56.0/24` estiver sendo utilizada por outro ambiente, altere os endereços diretamente no arquivo `Vagrantfile`.

Por exemplo:

```text
ubuntu-linux  → 192.168.57.10
oracle-linux  → 192.168.57.11
```

No `Vagrantfile`, as linhas ficariam:

```ruby
ubuntu.vm.network "private_network", ip: "192.168.57.10"
```

e:

```ruby
oracle.vm.network "private_network", ip: "192.168.57.11"
```

Se as máquinas ainda não tiverem sido criadas, salve o arquivo e execute normalmente:

```bash
vagrant up
```

Se as máquinas já tiverem sido criadas e o endereço for alterado posteriormente, salve o `Vagrantfile` e execute:

```bash
vagrant reload
```

Não é recomendado alterar manualmente essa configuração diretamente no VirtualBox ou dentro das máquinas virtuais. O `Vagrantfile` deve permanecer como a fonte de configuração do laboratório.

# Sequência de execução do laboratório

Esta sequência pode ser utilizada como roteiro para preparar o ambiente do zero.

## 1. Instalar o VirtualBox

Faça o download do VirtualBox pelo site oficial e realize a instalação no Windows.

Após a instalação, abra o VirtualBox para verificar se o programa inicia normalmente.

## 2. Instalar o Vagrant

Faça o download do Vagrant pelo site oficial e realize a instalação.

Após a instalação, feche e abra novamente o PowerShell.

Verifique a instalação:

```powershell
vagrant --version
```

O terminal deverá apresentar a versão instalada do Vagrant.

## 3. Instalar o Git

Faça o download do Git pelo site oficial e realize a instalação.

Depois, feche e abra novamente o PowerShell.

Verifique a instalação:

```powershell
git --version
```

O terminal deverá apresentar a versão instalada do Git.

## 4. Clonar o laboratório

Escolha no computador o diretório onde deseja armazenar o laboratório.

No PowerShell, execute:

```powershell
git clone https://github.com/MoissesPinheiro/laboratorio-linux-vagrant.git
```

O Git criará o diretório:

```text
laboratorio-linux-vagrant
```

## 5. Acessar o diretório do laboratório

Execute:

```powershell
cd laboratorio-linux-vagrant
```

A partir deste ponto, os comandos do Vagrant devem ser executados dentro do diretório onde está localizado o `Vagrantfile`.

## 6. Verificar a rede do computador

Antes do primeiro `vagrant up`, execute:

```powershell
ipconfig
```

Depois:

```powershell
route print
```

Verifique se existe algum conflito com a rede:

```text
192.168.56.0/24
```

Se aparecer apenas uma interface Host-Only pertencente ao próprio VirtualBox utilizando essa faixa, isso pode fazer parte da configuração normal do ambiente.

Se a mesma rede estiver sendo utilizada por Wi-Fi, Ethernet, VPN ou outro ambiente não relacionado ao laboratório, altere os endereços no `Vagrantfile` antes de continuar.

## 7. Criar e iniciar o laboratório

Com a rede verificada, execute:

```powershell
vagrant up
```

Na primeira execução, o Vagrant poderá baixar as boxes necessárias.

Em seguida, ele utilizará o VirtualBox para criar e iniciar:

```text
ubuntu-linux
oracle-linux
```

Aguarde a conclusão do processo.

## 8. Verificar o estado das máquinas

Execute:

```powershell
vagrant status
```

As máquinas deverão aparecer em execução.

## 9. Acessar o Ubuntu

Execute:

```powershell
vagrant ssh ubuntu-linux
```

Após acessar a máquina, você estará dentro do sistema Ubuntu.

Para retornar ao Windows:

```bash
exit
```

## 10. Acessar o Oracle Linux

Execute:

```powershell
vagrant ssh oracle-linux
```

Para retornar ao Windows:

```bash
exit
```

## 11. Encerrar o laboratório

Quando terminar os estudos, desligue as máquinas virtuais com:

```powershell
vagrant halt
```

As máquinas continuarão existentes no VirtualBox e poderão ser iniciadas novamente posteriormente com:

```powershell
vagrant up
```

## Fluxo resumido

```text
Instalar VirtualBox
        ↓
Instalar Vagrant
        ↓
vagrant --version
        ↓
Instalar Git
        ↓
git --version
        ↓
git clone
        ↓
cd laboratorio-linux-vagrant
        ↓
ipconfig
        ↓
route print
        ↓
Verificar 192.168.56.0/24
        ↓
vagrant up
        ↓
vagrant status
        ↓
vagrant ssh ubuntu-linux
        ↓
vagrant ssh oracle-linux
        ↓
vagrant halt
```

Se todas essas etapas forem concluídas com sucesso, o laboratório estará preparado para as aulas e práticas do **Aprenda Linux BR**.
