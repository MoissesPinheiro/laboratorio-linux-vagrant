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
Microsoft Windows 10 Pro
10.0.19045
64 bits
```

Observe principalmente a arquitetura apresentada pelo sistema.

Durante o download das ferramentas, escolha sempre a opção disponibilizada oficialmente para o seu sistema operacional e para a sua arquitetura.

Termos como:

```text
64 bits
x64
AMD64
ARM64
```

podem aparecer nas páginas de download.

Não escolha uma arquitetura diferente da suportada pelo seu computador.

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

Também podemos verificar a versão utilizando o PowerShell:

```powershell
& "$env:ProgramFiles\Oracle\VirtualBox\VBoxManage.exe" --version
```

Se uma versão for apresentada, o VirtualBox foi localizado nesse caminho.

Exemplo:

```text
7.x.x
```

O número exato poderá ser diferente.

## Se o VirtualBox não estiver instalado

Acesse a página oficial de downloads do VirtualBox:

```text
virtualbox.org
```

Procure a seção de downloads.

Escolha o instalador correspondente ao sistema operacional e à arquitetura suportada pelo seu computador.

Faça o download e execute o instalador.

Conclua a instalação seguindo as instruções apresentadas pelo próprio instalador.

Depois da instalação, abra novamente o VirtualBox e confirme se o programa inicia normalmente.

Volte ao PowerShell e verifique novamente:

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

Se uma versão for apresentada, o Vagrant já está instalado.

Exemplo:

```text
Vagrant 2.x.x
```

O número exato poderá ser diferente.

## Se o Vagrant não estiver instalado

Acesse a página oficial do Vagrant:

```text
developer.hashicorp.com/vagrant
```

Entre na seção de instalação/download.

Escolha o pacote correspondente ao seu sistema operacional e à arquitetura disponibilizada oficialmente para o seu computador.

Faça o download e execute o instalador correspondente.
...

> **Arquitetura:** em computadores Windows de 64 bits, selecione a opção `AMD64`.  
> A opção `i686` corresponde à arquitetura x86 de 32 bits.
...

Depois da instalação, feche o PowerShell e abra novamente.

Execute:

```powershell
vagrant --version
```

Se uma versão for apresentada, a instalação foi reconhecida pelo sistema.

Somente prossiga quando esse comando estiver funcionando.

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

Acesse o site oficial do Git:

```text
git-scm.com
```

Entre na área de downloads.

Escolha o instalador correspondente ao seu sistema operacional e à arquitetura suportada pelo computador.

Faça o download e execute o instalador.

Depois da instalação, feche o PowerShell e abra novamente.

Execute:

```powershell
git --version
```

Se a versão do Git for apresentada, a instalação foi concluída e reconhecida pelo sistema.

---

# 7. Confirmar as ferramentas

Antes de continuar, precisamos ter:

```text
VirtualBox → instalado e funcionando
Vagrant    → instalado e funcionando
Git        → instalado e funcionando
```

No PowerShell, podemos confirmar novamente:

```powershell
vagrant --version
```

e:

```powershell
git --version
```

Se essas verificações funcionarem e o VirtualBox abrir normalmente, podemos continuar.

---

# 8. Verificar a rede do computador

Antes de criar as máquinas virtuais, precisamos verificar a rede utilizada pelo laboratório.

O laboratório utiliza a rede privada:

```text
192.168.56.0/24
```

As máquinas serão configuradas inicialmente com:

```text
ubuntu-linux → 192.168.56.10
oracle-linux → 192.168.56.11
```

No PowerShell, execute:

```powershell
ipconfig
```

Observe as interfaces de rede existentes no computador.

Depois execute:

```powershell
route print
```

Observe se já existe alguma rede ou rota utilizando:

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

Encontrar um endereço `192.168.56.x` não significa automaticamente que exista um problema.

O próprio VirtualBox pode possuir uma interface de rede privada relacionada ao seu ambiente de virtualização.

Devemos investigar principalmente se a rede:

```text
192.168.56.0/24
```

já estiver sendo utilizada por outra infraestrutura, por exemplo:

- Wi-Fi;
- Ethernet;
- VPN;
- outro software de virtualização;
- outra rede configurada no computador.

Se não houver conflito, continue normalmente.

Se houver conflito, anote essa informação.

Mais adiante, depois de obtermos o `Vagrantfile`, vamos alterar a rede antes de executar `vagrant up`.

---

# 10. Obter o laboratório no GitHub

Agora que o computador está preparado, vamos obter os arquivos do laboratório.

Abra no GitHub o repositório:

```text
MoissesPinheiro/laboratorio-linux-vagrant
```

Na página do repositório:

```text
Code
→ HTTPS
→ Copy URL
```

Copie o endereço apresentado pelo GitHub.

---

# 11. Escolher onde armazenar o laboratório

No PowerShell, navegue até o diretório onde deseja armazenar o projeto.

Por exemplo, para utilizar o diretório do seu usuário:

```powershell
cd $HOME
```

Confira o local atual:

```powershell
Get-Location
```

---

# 12. Clonar o repositório

Utilize o endereço copiado anteriormente no GitHub.

Formato:

```powershell
git clone URL_COPIADA_DO_GITHUB
```

O comando `git clone` não instala o VirtualBox, o Vagrant ou as máquinas virtuais.

Ele apenas copia os arquivos do projeto armazenados no GitHub para o computador.

Após a clonagem, deverá existir um diretório chamado:

```text
laboratorio-linux-vagrant
```

---

# 13. Entrar no diretório do projeto

Execute:

```powershell
cd laboratorio-linux-vagrant
```

Confira:

```powershell
Get-Location
```

Depois liste os arquivos:

```powershell
Get-ChildItem
```

Devemos encontrar pelo menos:

```text
README.md
Vagrantfile
```

A partir deste ponto, os comandos do Vagrant relacionados a este laboratório deverão ser executados dentro desse diretório.

---

# 14. Conferir o Vagrantfile

Antes de criar qualquer máquina virtual, visualize o conteúdo do arquivo:

```powershell
Get-Content .\Vagrantfile
```

Devemos encontrar as definições das máquinas:

```text
ubuntu-linux
oracle-linux
```

Também encontraremos os endereços:

```text
192.168.56.10
192.168.56.11
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

Os endereços poderiam ficar:

```text
ubuntu-linux → 192.168.57.10
oracle-linux → 192.168.57.11
```

As linhas correspondentes do `Vagrantfile` deverão ser alteradas para a nova rede.

Salve o arquivo.

> Não altere essa configuração manualmente dentro das máquinas Linux ou diretamente na interface do VirtualBox.
>
> Neste laboratório, o `Vagrantfile` deve permanecer como a fonte de configuração das máquinas.

Se não houver conflito com a rede original, nenhuma alteração será necessária.

---

# 16. Validar o Vagrantfile

Antes de criar as máquinas, execute:

```powershell
vagrant validate
```

Esse comando verifica a configuração do `Vagrantfile`.

Se o Vagrant informar que a configuração é válida, podemos continuar.

Se algum erro for apresentado, não execute `vagrant up` ainda.

Primeiro corrija o problema indicado.

---

# 17. Verificar o estado inicial

Execute:

```powershell
vagrant status
```

Como as máquinas ainda não foram criadas neste computador, elas não deverão aparecer em execução.

Esse comando permite verificar o estado atual do ambiente conhecido pelo Vagrant.

---

# 18. Criar e iniciar o laboratório

Agora podemos criar as máquinas virtuais.

Execute:

```powershell
vagrant up --provider=virtualbox
```

O Vagrant utilizará as definições presentes no `Vagrantfile`.

Durante o primeiro `vagrant up`, ele poderá precisar obter as imagens base necessárias para criar as máquinas.

Depois utilizará o VirtualBox para criar e configurar:

```text
ubuntu-linux
oracle-linux
```

A primeira execução poderá demorar alguns minutos.

Aguarde até o processo terminar.

---

# 19. Verificar o estado das máquinas

Após a conclusão do `vagrant up`, execute:

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

As máquinas criadas pelo Vagrant deverão aparecer na interface.

Não é necessário criar essas máquinas manualmente no VirtualBox.

Neste laboratório, o Vagrant utiliza o VirtualBox como provedor para criar e gerenciar as máquinas definidas no `Vagrantfile`.

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

Verifique as interfaces e endereços:

```bash
ip -brief address
```

Na configuração original do laboratório, esperamos encontrar o endereço:

```text
192.168.56.10
```

Se você alterou a rede anteriormente, utilize como referência o endereço configurado no seu `Vagrantfile`.

Para sair da máquina e voltar ao PowerShell:

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

Confira a rede:

```bash
ip -brief address
```

Na configuração original esperamos:

```text
192.168.56.11
```

Para retornar ao Windows:

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

Caso tenha alterado a rede do laboratório, substitua pelos endereços configurados no seu `Vagrantfile`.

---

# 24. Testar a comunicação entre as máquinas

Entre no Ubuntu:

```powershell
vagrant ssh ubuntu-linux
```

Dentro do Ubuntu, teste o Oracle Linux:

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

Depois verifique:

```powershell
vagrant status
```

As máquinas continuarão existentes, porém desligadas.

---

# 26. Iniciar novamente

Em outro momento, abra o PowerShell e entre novamente no diretório do projeto.

Exemplo:

```powershell
cd $HOME\laboratorio-linux-vagrant
```

Se você armazenou o projeto em outro local, utilize o caminho correspondente.

Depois execute:

```powershell
vagrant up
```

E confira:

```powershell
vagrant status
```

---

# 27. Suspender e retomar as máquinas

Para suspender:

```powershell
vagrant suspend
```

Para retomar:

```powershell
vagrant resume
```

---

# 28. Aplicar alterações do Vagrantfile

Se uma máquina já tiver sido criada e você modificar uma configuração no `Vagrantfile`, poderá ser necessário recarregá-la.

Execute:

```powershell
vagrant reload
```

---

# 29. Remover as máquinas do laboratório

Quando realmente quiser excluir as máquinas virtuais criadas pelo Vagrant:

```powershell
vagrant destroy
```

O Vagrant solicitará confirmação.

Também existe:

```powershell
vagrant destroy -f
```

A opção `-f` executa a remoção sem solicitar confirmação.

> **Atenção:** `vagrant destroy` remove as máquinas virtuais e os dados armazenados nelas.
>
> Não confunda `destroy` com `halt`.

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
| `vagrant reload` | Reinicia as máquinas e aplica alterações do `Vagrantfile` |
| `vagrant destroy` | Remove as máquinas virtuais |

---

# Fluxo resumido

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
Abrir repositório no GitHub
        ↓
Copiar URL do repositório
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
vagrant ssh ubuntu-linux
        ↓
Validar Ubuntu
        ↓
vagrant ssh oracle-linux
        ↓
Validar Oracle Linux
        ↓
Testar comunicação
        ↓
vagrant halt
```

---

# Objetivo concluído

Ao finalizar todas as etapas, teremos o seguinte laboratório em funcionamento:

```text
Windows
   │
   ├── VirtualBox
   │
   └── Vagrant
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
