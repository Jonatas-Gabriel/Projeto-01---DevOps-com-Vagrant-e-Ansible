# Projeto-01---DevOps-com-Vagrant-e-Ansible

Este projeto tem como objetivo desenvolver competências práticas em DevOps e Infraestrutura como Código (IaC) utilizando as ferramentas Vagrant e Ansible. Nele, foi provisionada uma infraestrutura virtual e automatizada, a configuração do sistema operacional e de serviços essenciais em quatro máquinas virtuais, abordando a configuração de SUDO, SSH, NFS, LVM e DHCP em um ambiente Linux.

Integrantes da Equipe:

José Jantony Da Silva Floriano - Matrícula: 20232380024
Jonatas Gabriel Evangelista da Silva - Matrícula: 20232380023
Disciplina: Administração de Sistemas Abertos
Professor(a): Leônidas

Escopo do Projeto
1. Provisionamento da Infraestrutura com Vagrant
O Vagrantfile provisiona quatro máquinas virtuais com as seguintes configurações:

Configurações Comuns:

-------------------------------------

Provider: VirtualBox
Box: debian/bookworm64
Geração de chaves SSH: Desativada
Utilização de clones: linked_clone
Verificação de Guest Additions: Desabilitada

-------------------------------------

Máquinas Virtuais:

Servidor de Arquivos (arq):
Memória RAM: 512 MB
Discos adicionais: 3 discos de 10 GB cada
Endereço IP na rede privada: 192.168.56.124
Hostname: arq.jantony.jonatas.devops

-------------------------------------

Servidor de Banco de Dados (db):
Memória RAM: 512 MB
Endereço IP na rede privada: Obtido via DHCP
Hostname: db.jantony.jonatas.devops

-------------------------------------

Servidor de Aplicação (app):
Memória RAM: 512 MB
Endereço IP na rede privada: Obtido via DHCP
Hostname: app.jantony.jonatas.devops
Host Cliente (cli):
Memória RAM: 1024 MB
Endereço IP na rede privada: Obtido via DHCP
Hostname: cli.jantony.jonatas.devops

--------------------------------------

2. Provisionamento da Infraestrutura com Ansible
Após o provisionamento das VMs, o Ansible configura cada máquina através de playbooks, conforme as especificações abaixo:

Configurações Comuns (Todas as Máquinas):

Atualização do sistema operacional (apt update e apt upgrade).
Instalação e configuração do chrony (servidor NTP) para pool.ntp.br.
Ajuste da zona de tempo para America/Recife.
Criação do grupo ifpb.
Criação dos usuários nome1 e nome2 (logins iguais aos primeiros nomes dos integrantes), incluídos no grupo ifpb.
Configuração do servidor SSH:
Permitir apenas autenticação por chaves públicas.
Bloquear acesso ao usuário root via SSH.
Permitir acesso apenas a usuários pertencentes aos grupos vagrant e ifpb.
Geração e configuração de chaves públicas para os usuários jantony e jonatas.
Mensagem de saudação ao acessar via SSH:
Acesso apenas para pessoas com autorização expressa.
Seu acesso está sendo monitorado !!!
Instalação do cliente NFS.
Configuração do sudo para que usuários do grupo ifpb tenham acesso de root sem senha.

--------------------------------

Configurações Específicas por Máquina:

Servidor de Arquivos (arq):

Servidor DHCP autoritativo:
Nome de domínio: jantony.jonatas.devops.
Servidores DNS: 1.1.1.1 e 8.8.8.8.
Escopo DHCP para a rede 192.168.56.0/24, com faixa de endereços entre 50 e 100, e gateway padrão 192.168.56.1.
Endereços IP fixos configurados via DHCP para db (192.168.56.1YY) e app (192.168.56.1XY).

---------------------------------

LVM:
Criação de um Volume Group (VG) chamado dados utilizando os 3 discos adicionais.
Criação de um Logical Volume (LV) chamado ifpb com 15 GB.
Formatação do LV ifpb com ext4.
Configuração para montagem automática da partição ifpb no diretório /dados ao iniciar o sistema.
Serviço NFS:
Compartilhamento do diretório /dados/nfs com qualquer host da rede 192.168.56.0/24.
Criação do usuário nfs-ifpb (sem shell para segurança).
Permissão de escrita no diretório apenas para o usuário nfs-ifpb.
Mapeamento automático de usuários remotos para nfs-ifpb.
Forçamento de esvaziamento do buffer de memória e gravações imediatas no disco.
Servidor de Banco de Dados (db):

----------------------------------

Instalação do mariadb-server.
Instalação e configuração do autofs para montagem automática do compartilhamento NFS /dados/nfs do servidor arq no diretório /var/nfs.

----------------------------------

Servidor de Aplicação (app):

Instalação e configuração do apache2.
Criação e transferência de um arquivo index.html personalizado (com descrição do projeto, nome e matrícula dos integrantes) para substituir a página padrão do Apache.
Instalação e configuração do autofs para montagem automática do compartilhamento NFS /dados/nfs do servidor arq no diretório /var/nfs.

----------------------------------

Host Cliente (cli):

Instalação dos pacotes firefox-esr e xauth.
Ajuste do servidor SSH para permitir a exportação da interface dos aplicativos X11.
Instalação e configuração do autofs para montagem automática do compartilhamento NFS /dados/nfs do servidor arq no diretório /var/nfs.
Estrutura do Projeto
.
├── Vagrantfile
├── ansible/
│   ├── inventory.ini
│   ├── common.yml
│   ├── arq.yml
│   ├── db.yml
│   ├── app.yml
│   ├── cli.yml
│   ├── roles/
│   │   ├── common/
│   │   ├── nfs_server/
│   │   ├── dhcp_server/
│   │   ├── mariadb/
│   │   ├── apache/
│   │   └── autofs/
│   ├── files/
│   └── templates/
└── README.md

Como Executar o Projeto
Siga os passos abaixo para levantar e configurar o ambiente:

Pré-requisitos:

Instale o VirtualBox
Instale o Vagrant
Instale o Ansible (normalmente via pip install ansible ou gerenciador de pacotes da sua distribuição).
Certifique-se de ter um editor de texto e terminal.
Clonar o Repositório:

Bash

git clone https://docs.github.com/articles/referencing-and-citing-content
cd [nome-do-seu-repositorio]
Configurar o Vagrantfile:

Abra o Vagrantfile e substitua os placeholders:
XX pelo final da matrícula do primeiro integrante.
nome1 e nome2 pelos primeiros nomes dos integrantes.
YY pelo final da matrícula do segundo integrante.
XY pelos últimos dígitos da matrícula de cada integrante (Ex: se um é 123 e outro 456, pode ser 36 ou 25).
Levantar as Máquinas Virtuais:

No diretório raiz do projeto (onde está o Vagrantfile), execute:
Bash

vagrant up
Este comando fará o download da box Debian, clonará as VMs e as inicializará. Pode levar alguns minutos.
Verificar as Máquinas (Opcional):

Você pode verificar o status das VMs com:
Bash

vagrant status
Para acessar uma VM via SSH:
Bash

vagrant ssh [nome_da_vm] # Ex: vagrant ssh arq
Executar os Playbooks do Ansible:

Certifique-se de que o Ansible esteja instalado e que você esteja no diretório raiz do projeto.

O inventory.ini já deve estar configurado para se comunicar com as VMs do Vagrant.

Execute os playbooks na ordem lógica, começando pelos comuns e depois os específicos:

Bash

# Executar configurações comuns para todas as máquinas
ansible-playbook -i ansible/inventory.ini ansible/common.yml

# Executar configurações específicas do servidor de arquivos
ansible-playbook -i ansible/inventory.ini ansible/arq.yml

# Executar configurações específicas do servidor de banco de dados
ansible-playbook -i ansible/inventory.ini ansible/db.yml

# Executar configurações específicas do servidor de aplicação
ansible-playbook -i ansible/inventory.ini ansible/app.yml

# Executar configurações específicas do host cliente
ansible-playbook -i ansible/inventory.ini ansible/cli.yml
Monitore a saída do Ansible para garantir que não haja erros.

Testar as Configurações:

Após a execução dos playbooks, verifique se todas as configurações foram aplicadas corretamente em cada VM (ex: testar SSH, DHCP, NFS, LVM, acessar a página web do Apache, etc.).


Considerações Finais

Este projeto foi fundamental para aprofundar nossos conhecimentos em IaC e DevOps, proporcionando experiência prática com Vagrant para orquestração de ambientes virtuais e Ansible para automação de configurações. O desafio de integrar diversos serviços e garantir a comunicação entre as VMs foi um aprendizado valioso.
