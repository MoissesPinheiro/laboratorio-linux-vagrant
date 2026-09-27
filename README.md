# Laboratório Linux com Vagrant

Laboratório utilizado nos estudos e aulas práticas do **Aprenda Linux BR**.

O objetivo deste projeto é criar um ambiente Linux reproduzível utilizando:

- VirtualBox para executar as máquinas virtuais;
- Vagrant para automatizar a criação e o gerenciamento das máquinas;
- Git para obter os arquivos do laboratório.

> **Importante:** siga este documento na ordem apresentada.
>
> Antes de executar `vagrant up`, vamos instalar e validar todas as ferramentas, verificar a rede do computador e somente depois criar as máquinas virtuais.

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

O `Vagrantfile` deve ser considerado a fonte de configuração deste laboratório.

---

# Requisitos do computador

Esta primeira versão do laboratório foi preparada para:

- Windows;
- arquitetura x86-64 / x64 / AMD64;
- VirtualBox como provedor de virtualização.

Para executar as duas máquinas simultaneamente, recomendamos:

- 8 GB de memória RAM ou mais;
- processador com suporte à virtualização por hardware;
- virtualização habilitada na BIOS/UEFI;
- espaço disponível em disco para armazenar as boxes e máquinas virtuais.

As duas VMs recebem juntas aproximadamente:

```text
4 GB de RAM
4 vCPUs
```

Os 4 GB de RAM são destinados às máquinas virtuais.

O Windows e os demais programas do computador também precisam de memória RAM para funcionar.

---

# 1. Abrir o PowerShell

Abra o **Windows PowerShell** ou o **PowerShell**.

Os comandos deste documento devem ser executados no computador Windows, e não dentro de uma máquina Linux.

---

# 2. Verificar a arquitetura do Windows

Antes de instalar as ferramentas, confirme a arquitetura do sistema.

Execute:

```powershell
Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, OSArchitecture
```

Para este laboratório esperamos uma arquitetura de:

```text
64 bits
```

No site do Vagrant, essa arquitetura normalmente aparece identificada como:

```text
AMD64
```

AMD64 também é utilizado para computadores x86-64 com processadores Intel.

---

# 3. Verificar a virtualização

Também é possível conferir a virtualização pelo:

```text
Gerenciador de Tarefas
→ Desempenho
→ CPU
→ Virtualização
```

Se a virtualização estiver desabilitada, ela deverá ser habilitada na BIOS/UEFI antes de utilizar as máquinas virtuais.

---

# 4. Verificar o WinGet

Vamos utilizar o **WinGet** como método principal de instalação das ferramentas no Windows.

Execute:

```powershell
winget --version
```

Se uma versão for apresentada, podemos continuar.

Exemplo:

```text
v1.x.x
```

Caso o comando `winget` não esteja disponível, utilize os instaladores disponibilizados nos sites oficiais das ferramentas.

---

# 5. Instalar o VirtualBox

Antes da instalação, podemos pesquisar o pacote:

```powershell
winget search VirtualBox
```

Para instalar:

```powershell
winget install --id Oracle.VirtualBox -e --source winget
```

Durante a instalação, o Windows poderá solicitar confirmação de administrador.

Aguarde a conclusão do processo.

---

# 6. Verificar o VirtualBox

Após a instalação, abra o VirtualBox pelo menu Iniciar do Windows.

Confirme que a interface do programa abre normalmente.

Também podemos verificar a versão pelo PowerShell:

```powershell
& "$env:ProgramFiles\Oracle\VirtualBox\VBoxManage.exe" --version
```

Se uma versão for apresentada, o VirtualBox foi localizado corretamente.

Exemplo:

```text
7.x.x
```

> O número exato da versão poderá ser diferente, pois novas versões podem ser publicadas.

---

# 7. Instalar o Vagrant

Pesquise o pacote:

```powershell
winget search Vagrant
```

Instale o Vagrant:

```powershell
winget install --id Hashicorp.Vagrant -e --source winget
```

Aguarde a conclusão da instalação.

Depois da instalação, **feche o PowerShell e abra novamente** para que alterações no PATH sejam reconhecidas.

---

# 8. Verificar o Vagrant

Execute:

```powershell
vagrant --version
```

O terminal deverá apresentar a versão instalada.

Exemplo:

```text
Vagrant 2.x.x
```

O número exato da versão poderá mudar com o tempo.

Se o comando não for reconhecido imediatamente após a instalação, feche e abra novamente o terminal.

---

# 9. Instalar o Git

Antes de instalar o Git, vamos primeiro localizar o pacote no catálogo do WinGet.

Execute:

```powershell
winget search --name Git --source winget
```

Esse comando apenas realiza uma pesquisa.

Nenhum programa é instalado nesta etapa.

O objetivo é descobrir qual é o **ID do pacote** utilizado pelo WinGet para identificar o Git.

O resultado será apresentado em formato de tabela, com informações semelhantes a:

```text
Name    Id       Version    Source
Git     Git.Git  ...        winget
```

Observe principalmente a coluna:

```text
Id
```

No exemplo acima, o ID encontrado foi:

```text
Git.Git
```

Esse valor será utilizado no próximo comando.

Agora monte o comando de instalação utilizando exatamente o ID retornado pela pesquisa.

Formato do comando:

```powershell
winget install --id ID_ENCONTRADO -e --source winget
```

Substitua:

```text
ID_ENCONTRADO
```

pelo valor apresentado na coluna `Id`.

No exemplo anterior, como o ID encontrado foi:

```text
Git.Git
```

o comando ficará:

```powershell
winget install --id Git.Git -e --source winget
```

Nesse comando:

```text
winget install
```

solicita a instalação de um programa.

```text
--id Git.Git
```

informa que o programa deve ser localizado pelo campo `Id`, utilizando o valor `Git.Git`.

```text
-e
```

é a forma curta de `--exact`.

Isso significa que o WinGet deverá encontrar uma correspondência exata para o ID informado.

```text
--source winget
```

informa que a pesquisa e a instalação devem utilizar a fonte chamada `winget`.

Aguarde a conclusão da instalação.

Depois, feche o PowerShell e abra novamente.

# 10. Verificar a instalação do Git

Agora vamos verificar se o Git foi instalado corretamente.

Execute:

```powershell
git --version
```

Se a instalação foi concluída corretamente, o terminal deverá apresentar a versão instalada.

Exemplo:

```text
git version 2.x.x
```

O número exato da versão poderá ser diferente, pois novas versões podem ser disponibilizadas.

Se o comando apresentar a versão do Git, a instalação foi concluída com sucesso.

Neste momento devemos ter:

```text
VirtualBox → instalado
Vagrant    → instalado
Git        → instalado
```

---

# 11. Verificar a rede do computador

Antes de criar as máquinas virtuais, precisamos verificar se a rede utilizada pelo laboratório pode ser usada no computador.

Nosso laboratório utiliza:

```text
192.168.56.0/24
```

Com os endereços:

```text
ubuntu-linux → 192.168.56.10
oracle-linux → 192.168.56.11
```

Primeiro, visualize os endereços IPv4 existentes no Windows:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Sort-Object InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

Também podemos utilizar:

```powershell
ipconfig
```

Agora verifique se existe uma rota utilizando exatamente a rede do laboratório:

```powershell
Get-NetRoute -DestinationPrefix "192.168.56.0/24" -ErrorAction SilentlyContinue
```

Também é possível consultar toda a tabela de rotas:

```powershell
route print
```

---

# 12. Interpretar o resultado da rede

Precisamos descobrir se:

```text
192.168.56.0/24
```

já está sendo utilizada por alguma rede que possa entrar em conflito com o laboratório.

Pode existir uma interface relacionada ao próprio VirtualBox utilizando um endereço como:

```text
192.168.56.1
```

Uma interface Host-Only pertencente ao próprio VirtualBox não deve ser interpretada automaticamente como um problema.

Devemos prestar atenção principalmente se a faixa `192.168.56.0/24` estiver sendo utilizada por outra infraestrutura, como:

- Wi-Fi;
- Ethernet;
- VPN;
- outro software de virtualização;
- outra rede ou rota já configurada no computador.

Se não houver conflito, continue normalmente.

Se houver conflito, anote essa informação.

Ainda não execute `vagrant up`.

Primeiro vamos obter o `Vagrantfile` e alterar a rede.

---

# 13. Escolher onde armazenar o laboratório

No PowerShell, escolha um diretório onde deseja guardar o projeto.

Por exemplo, podemos utilizar o diretório do próprio usuário:

```powershell
cd $HOME
```

Confira o diretório atual:

```powershell
Get-Location
```

---

# 14. Clonar o repositório

Agora utilize o Git para obter os arquivos do laboratório:

```powershell
git clone https://github.com/MoissesPinheiro/laboratorio-linux-vagrant.git
```

O Git deverá criar o diretório:

```text
laboratorio-linux-vagrant
```

> `git clone` não instala o Vagrant e não cria as máquinas virtuais.
>
> Esse comando apenas copia o projeto do GitHub para o computador.

---

# 15. Entrar no diretório do laboratório

Execute:

```powershell
cd laboratorio-linux-vagrant
```

Confirme o diretório:

```powershell
Get-Location
```

Liste os arquivos:

```powershell
Get-ChildItem
```

Devemos encontrar pelo menos:

```text
README.md
Vagrantfile
```

A partir deste ponto, os comandos do Vagrant devem ser executados dentro deste diretório.

---

# 16. Conferir o Vagrantfile

Antes de criar qualquer máquina, visualize o arquivo:

```powershell
Get-Content .\Vagrantfile
```

Devemos encontrar as duas máquinas:

```text
ubuntu-linux
oracle-linux
```

E os endereços:

```text
192.168.56.10
192.168.56.11
```

---

# 17. Somente se houver conflito de rede

Se a verificação realizada anteriormente mostrou conflito com:

```text
192.168.56.0/24
```

não execute `vagrant up` ainda.

Abra o `Vagrantfile`:

```powershell
notepad .\Vagrantfile
```

Escolha outra rede privada que não esteja sendo utilizada no computador.

Por exemplo:

```text
192.168.57.0/24
```

Antes de utilizá-la, também podemos verificar:

```powershell
Get-NetRoute -DestinationPrefix "192.168.57.0/24" -ErrorAction SilentlyContinue
```

Se estiver disponível, os endereços poderiam ser alterados para:

```text
ubuntu-linux → 192.168.57.10
oracle-linux → 192.168.57.11
```

No `Vagrantfile`:

```ruby
ubuntu.vm.network "private_network", ip: "192.168.57.10"
```

e:

```ruby
oracle.vm.network "private_network", ip: "192.168.57.11"
```

Salve o arquivo.

Não altere essa configuração manualmente dentro do VirtualBox ou dentro das máquinas Linux.

A configuração deve continuar declarada no:

```text
Vagrantfile
```

Se não existia conflito com a rede original, não faça nenhuma alteração.

---

# 18. Validar o Vagrantfile

Antes de criar as máquinas virtuais, execute:

```powershell
vagrant validate
```

O Vagrant deverá informar que o arquivo de configuração é válido.

Se for apresentado algum erro, **não execute `vagrant up` ainda**.

Corrija o problema indicado antes de continuar.

---

# 19. Verificar o estado inicial do laboratório

Execute:

```powershell
vagrant status
```

Como ainda não criamos as máquinas, elas não deverão estar em execução.

Esse comando serve para verificar o estado atual conhecido pelo Vagrant.

---

# 20. Criar e iniciar as máquinas virtuais

Agora, com:

```text
VirtualBox validado
Vagrant validado
Git validado
rede verificada
repositório clonado
Vagrantfile validado
```

podemos criar o laboratório.

Execute:

```powershell
vagrant up --provider=virtualbox
```

O Vagrant irá:

```text
ler o Vagrantfile
        ↓
localizar/baixar as boxes necessárias
        ↓
utilizar o VirtualBox
        ↓
criar ubuntu-linux
        ↓
criar oracle-linux
        ↓
configurar recursos
        ↓
configurar redes
        ↓
iniciar as máquinas
```

Na primeira execução esse processo poderá demorar, principalmente porque as boxes poderão precisar ser baixadas.

Não feche o terminal durante o processo.

Aguarde até o Vagrant concluir.

---

# 21. Verificar o estado das máquinas

Após a conclusão:

```powershell
vagrant status
```

Esperamos encontrar as duas máquinas em execução:

```text
ubuntu-linux
oracle-linux
```

---

# 22. Conferir as máquinas no VirtualBox

Abra a interface do VirtualBox.

As máquinas criadas pelo Vagrant deverão aparecer na interface.

Não é necessário criar as máquinas manualmente pelo VirtualBox.

Elas são gerenciadas pelo projeto Vagrant.

---

# 23. Acessar o Ubuntu

No PowerShell, dentro do diretório do projeto:

```powershell
vagrant ssh ubuntu-linux
```

Ao acessar a máquina, confira o hostname:

```bash
hostname
```

Confira o sistema operacional:

```bash
cat /etc/os-release
```

Confira os endereços de rede:

```bash
ip -brief address
```

A máquina deverá possuir o endereço privado configurado para ela:

```text
192.168.56.10
```

Se a faixa foi alterada anteriormente devido a conflito, utilize o endereço definido no seu `Vagrantfile`.

Para sair da máquina:

```bash
exit
```

Você retornará ao PowerShell do Windows.

---

# 24. Acessar o Oracle Linux

Execute:

```powershell
vagrant ssh oracle-linux
```

Confira:

```bash
hostname
```

Depois:

```bash
cat /etc/os-release
```

E:

```bash
ip -brief address
```

A máquina deverá possuir:

```text
192.168.56.11
```

ou o endereço correspondente à rede que você configurou no `Vagrantfile`.

Para retornar ao Windows:

```bash
exit
```

---

# 25. Testar a comunicação pela rede privada

No Windows podemos testar o Ubuntu:

```powershell
Test-Connection 192.168.56.10 -Count 2
```

E o Oracle Linux:

```powershell
Test-Connection 192.168.56.11 -Count 2
```

Caso você tenha alterado a faixa de rede, utilize os novos endereços.

Também podemos testar a comunicação entre as máquinas.

Entre no Ubuntu:

```powershell
vagrant ssh ubuntu-linux
```

E execute:

```bash
ping -c 4 192.168.56.11
```

Depois:

```bash
exit
```

---

# 26. Encerrar o laboratório

Quando terminar os estudos:

```powershell
vagrant halt
```

Depois verifique:

```powershell
vagrant status
```

As máquinas deverão permanecer criadas, porém desligadas.

---

# 27. Iniciar novamente o laboratório

Quando quiser voltar aos estudos, entre novamente no diretório:

```powershell
cd $HOME\laboratorio-linux-vagrant
```

E execute:

```powershell
vagrant up
```

Depois:

```powershell
vagrant status
```

---

# 28. Remover completamente as máquinas virtuais

Este comando deve ser utilizado somente quando você realmente quiser remover as VMs criadas pelo laboratório:

```powershell
vagrant destroy
```

O Vagrant solicitará confirmação.

Também existe:

```powershell
vagrant destroy -f
```

A opção `-f` remove sem solicitar confirmação.

> **Atenção:** `vagrant destroy` remove as máquinas virtuais do laboratório e os dados armazenados nelas.
>
> Ele não deve ser confundido com `vagrant halt`.

---

# Comandos principais do laboratório

| Comando | Função |
|---|---|
| `vagrant validate` | Valida a configuração do `Vagrantfile` |
| `vagrant up` | Cria ou inicia as máquinas |
| `vagrant status` | Mostra o estado das máquinas |
| `vagrant ssh ubuntu-linux` | Acessa o Ubuntu |
| `vagrant ssh oracle-linux` | Acessa o Oracle Linux |
| `vagrant halt` | Desliga as máquinas |
| `vagrant suspend` | Suspende as máquinas |
| `vagrant resume` | Retoma máquinas suspensas |
| `vagrant reload` | Reinicia as máquinas aplicando alterações do `Vagrantfile` |
| `vagrant destroy` | Remove as máquinas virtuais |

---

# Fluxo completo

```text
Verificar Windows e arquitetura
        ↓
Verificar virtualização
        ↓
Verificar WinGet
        ↓
Instalar VirtualBox
        ↓
Validar VirtualBox
        ↓
Instalar Vagrant
        ↓
Validar Vagrant
        ↓
Instalar Git
        ↓
Validar Git
        ↓
Verificar rede 192.168.56.0/24
        ↓
Clonar repositório
        ↓
Entrar no diretório
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

# Objetivo da validação

Se você conseguir seguir este documento desde o início até o final sem precisar de instruções adicionais, o laboratório estará validado para utilização nas aulas do **Aprenda Linux BR**.

Se alguma etapa apresentar comportamento diferente, erro ou instrução pouco clara, registre o ponto onde ocorreu o problema para que a documentação possa ser corrigida e aprimorada.
