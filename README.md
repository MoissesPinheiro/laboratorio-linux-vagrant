# Laboratório Linux com Vagrant

Ambiente de laboratório utilizado nos estudos e aulas práticas do **Aprenda Linux BR**.

O objetivo deste projeto é criar um laboratório Linux com duas máquinas virtuais utilizando:

- **VirtualBox** para executar as máquinas virtuais;
- **Vagrant** para automatizar a criação e o gerenciamento das máquinas;
- **Git** para obter os arquivos do laboratório hospedados no GitHub.

> **Importante:** siga este documento na ordem apresentada.
>
> Primeiro vamos verificar o computador e preparar as ferramentas. Somente depois vamos obter o projeto e criar as máquinas virtuais.

---

# Estrutura do laboratório

O laboratório possui duas máquinas virtuais:

| Máquina | Sistema Operacional | vCPUs | Memória RAM | IP privado |
|---|---|---:|---:|---|
| `ubuntu-linux` | Ubuntu 24.04 LTS | 2 | 2048 MB | `192.168.56.10` |
| `oracle-linux` | Oracle Linux 9 | 2 | 2048 MB | `192.168.56.11` |

As configurações das máquinas estão declaradas no arquivo:

```text
Vagrantfile
```

O `Vagrantfile` é a fonte de configuração deste laboratório.

---

# Recursos do computador

Cada máquina virtual está configurada com:

```text
2 vCPUs
2048 MB de RAM
```

Com as duas máquinas em execução, teremos aproximadamente:

```text
4 GB de RAM destinados às VMs
4 vCPUs configuradas entre as duas VMs
```

Esses recursos não incluem o consumo do próprio Windows e dos demais programas em execução.

Para trabalhar com maior estabilidade, recomendamos:

- 8 GB de memória RAM ou mais;
- processador com suporte à virtualização por hardware;
- virtualização habilitada na BIOS/UEFI;
- espaço disponível em disco para as máquinas virtuais e arquivos utilizados pelo laboratório.

> **Observação:** computadores com poucos recursos podem apresentar lentidão ao executar as duas máquinas simultaneamente.

---

# Sequência de preparação do laboratório

A partir deste ponto, siga as etapas na ordem apresentada.

---

# 1. Abrir o PowerShell

Abra o **PowerShell** no Windows.

Os comandos de preparação do laboratório serão executados no computador host.

---

# 2. Verificar o Windows e a arquitetura

Execute:

```powershell
Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, OSArchitecture
```

O comando apresenta informações como:

```text
Caption
Version
OSArchitecture
```

Exemplo:

```text
Caption                   Version     OSArchitecture
-------                   -------     --------------
Microsoft Windows 10 Pro  10.0.19045  64 bits
```

Observe principalmente a arquitetura apresentada pelo sistema.

Durante o download das ferramentas, escolha sempre a opção disponibilizada oficialmente para o seu sistema operacional e para a sua arquitetura.

Alguns termos que podem aparecer nas páginas de download são:

```text
x64
AMD64
i686
ARM64
```

Para computadores Windows x86 de 64 bits, a opção normalmente utilizada é:

```text
AMD64
```

O termo `AMD64` também é utilizado em computadores com processadores Intel compatíveis com a arquitetura x86-64.

A opção:

```text
i686
```

corresponde à arquitetura x86 de 32 bits.

A opção:

```text
ARM64
```

é destinada a computadores baseados em arquitetura ARM de 64 bits.

Escolha sempre o pacote correspondente à arquitetura do seu computador.

---

# 3. Verificar a virtualização

Antes de utilizar o VirtualBox, confirme se a virtualização por hardware está habilitada.

No Windows, abra:

```text
Gerenciador de Tarefas
→ Desempenho
→ CPU
```

Procure a informação:

```text
Virtualização
```

O resultado esperado é:

```text
Virtualização: Habilitada
```

Se estiver desabilitada, será necessário habilitar a virtualização na BIOS/UEFI antes de prosseguir.

---

# 4. Verificar o VirtualBox

Antes de instalar qualquer programa, vamos verificar se o VirtualBox já está instalado.

Procure por:

```text
Oracle VirtualBox
```

no menu Iniciar do Windows.

Se o programa estiver instalado, abra-o e confirme se a interface inicia normalmente.

Também podemos verificar a versão pelo PowerShell:

```powershell
& "$env:ProgramFiles\Oracle\VirtualBox\VBoxManage.exe" --version
```

Se uma versão for apresentada, o VirtualBox foi localizado corretamente.

Exemplo:

```text
7.x.x
```

O número exato poderá ser diferente.

## Se o VirtualBox não estiver instalado

Acesse a página oficial:

[Download do VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Escolha o instalador correspondente ao sistema operacional e à arquitetura suportada pelo seu computador.

Faça o download e execute o instalador.

Conclua a instalação seguindo as instruções apresentadas pelo próprio instalador.

Depois da instalação, abra o VirtualBox e confirme se o programa inicia normalmente.

Volte ao PowerShell e execute novamente:

```powershell
& "$env:ProgramFiles\Oracle\VirtualBox\VBoxManage.exe" --version
```

Somente prossiga quando o VirtualBox estiver instalado e funcionando.

---

# 5. Verificar o Vagrant

Agora vamos verificar se o Vagrant já está instalado.

No PowerShell, execute:

```powershell
vagrant --version
```

Se uma versão for apresentada, o Vagrant já está instalado e reconhecido pelo sistema.

Exemplo:

```text
Vagrant 2.x.x
```

O número exato poderá ser diferente.

## Se o Vagrant não estiver instalado

Acesse a página oficial:

[Instalação do Vagrant](https://developer.hashicorp.com/vagrant/install)

Escolha:

```text
Windows
```

e depois selecione a arquitetura correspondente ao seu computador.

Para um Windows x86 de 64 bits, normalmente utilizaremos:

```text
AMD64
```

A opção:

```text
i686
```

corresponde à versão x86 de 32 bits.

Faça o download do instalador adequado e execute-o.

Depois da instalação, feche o PowerShell e abra novamente.

Execute:

```powershell
vagrant --version
```

Se uma versão for apresentada, a instalação foi reconhecida pelo sistema.

Somente prossiga quando o comando `vagrant` estiver funcionando.

---

# 6. Verificar o Git

Agora vamos verificar se o Git já está instalado.

Execute:

```powershell
git --version
```

Se uma versão for apresentada, o Git já está instalado.

Exemplo:

```text
git version 2.x.x
```

O número exato poderá ser diferente.

## Se o Git não estiver instalado

Acesse a página oficial:

[Download do Git](https://git-scm.com/downloads)

Escolha o instalador correspondente ao sistema operacional e à arquitetura suportada pelo seu computador.

Faça o download e execute o instalador.

Depois da instalação, feche o PowerShell e abra novamente.

Execute:

```powershell
git --version
```

Se uma versão for apresentada, o Git foi instalado e reconhecido corretamente.

---

# 7. Confirmar as ferramentas

Antes de continuar, precisamos ter:

```text
VirtualBox → instalado e funcionando
Vagrant    → instalado e funcionando
Git        → instalado e funcionando
```

Podemos confirmar novamente o Vagrant:

```powershell
vagrant --version
```

E o Git:

```powershell
git --version
```

Também confirme que o VirtualBox abre normalmente.

Com as três ferramentas funcionando, podemos seguir para a verificação da rede.

---

# 8. Verificar a rede do computador

Antes de criar as máquinas virtuais, precisamos verificar a rede utilizada pelo laboratório.

Nosso laboratório utiliza:

```text
192.168.56.0/24
```

As máquinas serão configuradas inicialmente com:

```text
ubuntu-linux → 192.168.56.10
oracle-linux → 192.168.56.11
```

Primeiro execute:

```powershell
ipconfig
```

Observe os endereços IPv4 e as interfaces existentes no computador.

Também podemos utilizar:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Format-Table InterfaceAlias,IPAddress,PrefixLength
```

Observe principalmente:

```text
IPAddress
PrefixLength
```

Por exemplo, uma rede:

```text
192.168.18.0/24
```

é diferente da rede utilizada pelo laboratório:

```text
192.168.56.0/24
```

Nesse exemplo, não existe conflito entre as duas redes.

Agora consulte a tabela de rotas:

```powershell
route print
```

Procure por uma rota utilizando:

```text
192.168.56.0
```

com máscara:

```text
255.255.255.0
```

Isso corresponde à rede:

```text
192.168.56.0/24
```

---

# 9. Interpretar a verificação da rede

Encontrar um endereço:

```text
192.168.56.x
```

não significa automaticamente que existe um problema.

O próprio VirtualBox pode possuir uma interface Host-Only relacionada ao ambiente de virtualização utilizando essa faixa.

Devemos investigar principalmente se:

```text
192.168.56.0/24
```

estiver sendo utilizada por outra infraestrutura, por exemplo:

- rede Wi-Fi;
- rede Ethernet;
- VPN;
- outro software de virtualização;
- outra rede ou rota configurada no computador.

Se não houver conflito, continue normalmente.

Se houver conflito, anote essa informação.

Ainda não altere nada no VirtualBox.

Depois de obtermos o `Vagrantfile`, poderemos alterar a rede diretamente nele antes de executar `vagrant up`.

---

# 10. Obter o laboratório no GitHub

Agora que o computador está preparado, vamos acessar o repositório onde estão armazenados os arquivos do laboratório.

Acesse:

[Laboratório Linux com Vagrant](https://github.com/MoissesPinheiro/laboratorio-linux-vagrant)

Esse endereço abre a página do projeto no GitHub.

Na página do repositório, clique em:

```text
Code
```

Depois selecione:

```text
HTTPS
```

e copie o endereço apresentado pelo GitHub.

O endereço utilizado para clonagem deverá ser:

```text
https://github.com/MoissesPinheiro/laboratorio-linux-vagrant.git
```

Esse endereço será utilizado posteriormente com o comando:

```text
git clone
```

> **Importante:** nesta etapa ainda não estamos clonando o repositório.
>
> Estamos apenas acessando a página do projeto e copiando o endereço HTTPS que será utilizado pelo Git.

---

# 11. Escolher onde armazenar o laboratório

Agora escolha em qual diretório do computador deseja armazenar o projeto.

Por exemplo, podemos utilizar o diretório do usuário atual:

```powershell
cd $HOME
```

Confira em qual diretório você está:

```powershell
Get-Location
```

O projeto será criado dentro do diretório escolhido.

---

# 12. Clonar o repositório

Agora vamos utilizar o Git para copiar o projeto do GitHub para o computador.

Execute:

```powershell
git clone https://github.com/MoissesPinheiro/laboratorio-linux-vagrant.git
```

O comando:

```text
git clone
```

não instala o VirtualBox, não instala o Vagrant e não cria máquinas virtuais.

Ele apenas copia para o computador os arquivos armazenados no repositório do GitHub.

Após a clonagem, deverá ser criado o diretório:

```text
laboratorio-linux-vagrant
```

---

# 13. Entrar no diretório do laboratório

Execute:

```powershell
cd laboratorio-linux-vagrant
```

Confira onde você está:

```powershell
Get-Location
```

Agora liste os arquivos:

```powershell
Get-ChildItem
```

Devemos encontrar pelo menos:

```text
README.md
Vagrantfile
```

A partir deste momento, os comandos do Vagrant relacionados a este laboratório deverão ser executados dentro do diretório onde está localizado o `Vagrantfile`.

---

# 14. Conferir o Vagrantfile

Antes de criar qualquer máquina virtual, visualize o conteúdo do arquivo:

```powershell
Get-Content .\Vagrantfile
```

Devemos encontrar as definições das duas máquinas:

```text
ubuntu-linux
oracle-linux
```

Também encontraremos os endereços configurados:

```text
192.168.56.10
192.168.56.11
```

E os recursos definidos para cada máquina:

```text
2 vCPUs
2048 MB de RAM
```

---

# 15. Caso exista conflito de rede

Se a verificação realizada anteriormente mostrou conflito com:

```text
192.168.56.0/24
```

não execute `vagrant up` ainda.

Abra o arquivo:

```powershell
notepad .\Vagrantfile
```

Escolha outra rede privada que não esteja sendo utilizada no computador.

Por exemplo:

```text
192.168.57.0/24
```

Os endereços poderiam ser alterados para:

```text
ubuntu-linux → 192.168.57.10
oracle-linux → 192.168.57.11
```

No `Vagrantfile`, as linhas correspondentes seriam alteradas para:

```ruby
ubuntu.vm.network "private_network", ip: "192.168.57.10"
```

e:

```ruby
oracle.vm.network "private_network", ip: "192.168.57.11"
```

Salve o arquivo.

> Não é recomendado alterar manualmente essa configuração diretamente dentro do VirtualBox ou dentro das máquinas Linux.
>
> Neste laboratório, o `Vagrantfile` deve permanecer como a fonte de configuração das máquinas.

Se não existir conflito com a rede original:

```text
192.168.56.0/24
```

não faça nenhuma alteração.

---

# 16. Validar o Vagrantfile

Antes de criar as máquinas, execute:

```powershell
vagrant validate
```

Esse comando verifica se a configuração do `Vagrantfile` pode ser interpretada pelo Vagrant.

Se a configuração for válida, o Vagrant deverá informar que não encontrou problemas.

Se algum erro for apresentado, não execute `vagrant up` ainda.

Primeiro corrija o problema indicado.

---

# 17. Verificar o estado inicial do laboratório

Execute:

```powershell
vagrant status
```

Como as máquinas ainda não foram criadas neste computador, elas não deverão estar em execução.

Esse comando permite consultar o estado das máquinas conhecidas pelo projeto Vagrant.

---

# 18. Criar e iniciar o laboratório

Agora podemos criar as máquinas virtuais.

Execute:

```powershell
vagrant up --provider=virtualbox
```

O Vagrant irá ler o arquivo:

```text
Vagrantfile
```

e utilizará o VirtualBox como provedor de virtualização.

Durante a primeira execução, o Vagrant poderá precisar baixar as boxes utilizadas como base para as máquinas virtuais.

Depois ele realizará a criação e configuração de:

```text
ubuntu-linux
oracle-linux
```

A primeira execução poderá levar alguns minutos, principalmente durante o download das boxes.

Aguarde até o processo terminar.

---

# 19. Verificar o estado das máquinas

Depois que o `vagrant up` terminar, execute:

```powershell
vagrant status
```

Esperamos encontrar:

```text
ubuntu-linux
oracle-linux
```

em execução.

---

# 20. Conferir as máquinas no VirtualBox

Abra o VirtualBox.

As máquinas criadas pelo Vagrant deverão aparecer na interface do VirtualBox.

Não é necessário criar essas máquinas manualmente.

A relação é:

```text
Vagrantfile
      ↓
Vagrant
      ↓
VirtualBox
      ↓
Máquinas virtuais
```

O Vagrant lê a configuração definida no projeto e utiliza o VirtualBox para criar e controlar as máquinas virtuais.

---

# 21. Acessar o Ubuntu

No PowerShell, dentro do diretório do projeto, execute:

```powershell
vagrant ssh ubuntu-linux
```

Depois de entrar na máquina, verifique o hostname:

```bash
hostname
```

Verifique o sistema operacional:

```bash
cat /etc/os-release
```

Verifique as interfaces e endereços IP:

```bash
ip -brief address
```

Na configuração original do laboratório esperamos encontrar:

```text
192.168.56.10
```

Se você alterou a rede devido a algum conflito, utilize como referência o endereço definido no seu `Vagrantfile`.

Para sair da máquina e retornar ao PowerShell:

```bash
exit
```

---

# 22. Acessar o Oracle Linux

Execute:

```powershell
vagrant ssh oracle-linux
```

Confira o hostname:

```bash
hostname
```

Confira o sistema operacional:

```bash
cat /etc/os-release
```

Confira as interfaces:

```bash
ip -brief address
```

Na configuração original esperamos encontrar:

```text
192.168.56.11
```

Para sair:

```bash
exit
```

---

# 23. Testar a comunicação do Windows com as máquinas

No PowerShell, teste o Ubuntu:

```powershell
Test-Connection 192.168.56.10 -Count 2
```

Depois teste o Oracle Linux:

```powershell
Test-Connection 192.168.56.11 -Count 2
```

Caso tenha alterado a rede do laboratório, substitua esses endereços pelos IPs definidos no seu `Vagrantfile`.

---

# 24. Testar a comunicação entre as máquinas

Entre no Ubuntu:

```powershell
vagrant ssh ubuntu-linux
```

Dentro do Ubuntu, teste a comunicação com o Oracle Linux:

```bash
ping -c 4 192.168.56.11
```

Se a rede foi alterada, utilize o endereço correspondente ao Oracle Linux.

Depois saia:

```bash
exit
```

---

# 25. Desligar o laboratório

Quando terminar os estudos, execute:

```powershell
vagrant halt
```

Depois confira:

```powershell
vagrant status
```

As máquinas continuarão existentes no VirtualBox, porém desligadas.

---

# 26. Iniciar novamente o laboratório

Quando quiser voltar aos estudos, abra o PowerShell e entre novamente no diretório do projeto.

Exemplo:

```powershell
cd $HOME\laboratorio-linux-vagrant
```

Se você armazenou o projeto em outro local, utilize o caminho correspondente.

Depois execute:

```powershell
vagrant up
```

Confira:

```powershell
vagrant status
```

---

# 27. Suspender e retomar as máquinas

Para suspender as máquinas:

```powershell
vagrant suspend
```

Para retomar:

```powershell
vagrant resume
```

---

# 28. Aplicar alterações do Vagrantfile

Se as máquinas já tiverem sido criadas e uma configuração do `Vagrantfile` for alterada, poderá ser necessário recarregar o ambiente.

Execute:

```powershell
vagrant reload
```

Esse comando reinicia as máquinas e permite ao Vagrant aplicar configurações alteradas.

---

# 29. Remover as máquinas do laboratório

Quando realmente quiser excluir as máquinas virtuais criadas pelo projeto:

```powershell
vagrant destroy
```

O Vagrant solicitará confirmação antes da remoção.

Também existe:

```powershell
vagrant destroy -f
```

A opção:

```text
-f
```

executa a remoção sem solicitar confirmação.

> **Atenção:** `vagrant destroy` remove as máquinas virtuais e os dados armazenados nelas.
>
> Não confunda `destroy` com `halt`.
>
> `halt` apenas desliga as máquinas.
>
> `destroy` remove as máquinas.

---

# Comandos principais

| Comando | Função |
|---|---|
| `vagrant validate` | Valida a configuração do `Vagrantfile` |
| `vagrant status` | Mostra o estado das máquinas |
| `vagrant up` | Cria ou inicia as máquinas |
| `vagrant ssh ubuntu-linux` | Acessa o Ubuntu via SSH |
| `vagrant ssh oracle-linux` | Acessa o Oracle Linux via SSH |
| `vagrant halt` | Desliga as máquinas |
| `vagrant suspend` | Suspende as máquinas |
| `vagrant resume` | Retoma as máquinas suspensas |
| `vagrant reload` | Reinicia as máquinas aplicando alterações do `Vagrantfile` |
| `vagrant destroy` | Remove as máquinas virtuais |

---

# Fluxo completo do laboratório

```text
Verificar Windows e arquitetura
        ↓
Verificar virtualização
        ↓
Verificar VirtualBox
        ↓
Instalar pelo site oficial, se necessário
        ↓
Validar VirtualBox
        ↓
Verificar Vagrant
        ↓
Instalar pelo site oficial, se necessário
        ↓
Validar Vagrant
        ↓
Verificar Git
        ↓
Instalar pelo site oficial, se necessário
        ↓
Validar Git
        ↓
Verificar rede 192.168.56.0/24
        ↓
Acessar o repositório no GitHub
        ↓
Copiar URL HTTPS
        ↓
Escolher diretório local
        ↓
git clone
        ↓
cd laboratorio-linux-vagrant
        ↓
Conferir Vagrantfile
        ↓
Resolver eventual conflito de rede
        ↓
vagrant validate
        ↓
vagrant status
        ↓
vagrant up --provider=virtualbox
        ↓
vagrant status
        ↓
Conferir máquinas no VirtualBox
        ↓
vagrant ssh ubuntu-linux
        ↓
Validar Ubuntu
        ↓
vagrant ssh oracle-linux
        ↓
Validar Oracle Linux
        ↓
Testar comunicação de rede
        ↓
vagrant halt
```

---

# Objetivo concluído

Ao finalizar todas as etapas, teremos o seguinte laboratório:

```text
Windows
   │
   ├── Git
   │
   ├── Vagrant
   │
   └── VirtualBox
          │
          ├── ubuntu-linux
          │      Ubuntu 24.04 LTS
          │      2 vCPUs
          │      2048 MB RAM
          │      192.168.56.10
          │
          └── oracle-linux
                 Oracle Linux 9
                 2 vCPUs
                 2048 MB RAM
                 192.168.56.11
```

O laboratório estará pronto para ser utilizado nas aulas e práticas do **Aprenda Linux BR**.
