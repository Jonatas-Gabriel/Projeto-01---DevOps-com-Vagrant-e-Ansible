# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # --- Configurações Comuns para Todas as Máquinas ---
  # Definimos a box (imagem base do sistema operacional)
  config.vm.box = "debian/bookworm64"

  # Especificamos o provedor VirtualBox
  config.vm.provider "virtualbox" do |vb|
    # Desabilita a verificação de Guest Additions (melhora a compatibilidade em alguns cenários)
    vb.guest_additions_disable = true
    # Utiliza linked_clone para economia de espaço em disco e inicialização mais rápida
    vb.linked_clone = true
    # Memória RAM padrão para todas as VMs, a menos que seja sobrescrita por máquina
    vb.memory = "512"
  end

  # Desativa a inserção automática de chaves SSH pelo Vagrant.
  # Manteremos isso, pois o Ansible vai gerenciar as chaves posteriormente.
  config.ssh.insert_key = false


  # --- Configuração do Servidor de Arquivos (arq) ---
  config.vm.define "arq" do |arq|
    # Substitua 'nome1' e 'nome2' pelos primeiros nomes dos integrantes
    arq.vm.hostname = "arq.jantony.jonatas.devops"

    # Substitua 'XX' pelos dois últimos dígitos da matrícula do primeiro integrante
    arq.vm.network "private_network", ip: "192.168.56.124"

    arq.vm.provider "virtualbox" do |vb|
    end
  end


  # --- Configuração do Servidor de Banco de Dados (db) ---
  config.vm.define "db" do |db|
    # Substitua 'nome1' e 'nome2' pelos primeiros nomes dos integrantes
    db.vm.hostname = "db.jantony.jonatas.devops"

    # Endereço IP obtido via DHCP na rede privada
    db.vm.network "private_network", type: "dhcp"

    db.vm.provider "virtualbox" do |vb|
      # A memória já está definida globalmente para 512 MB.
      # vb.memory = "512"
    end
  end


  # --- Configuração do Servidor de Aplicação (app) ---
  config.vm.define "app" do |app|
    # Substitua 'nome1' e 'nome2' pelos primeiros nomes dos integrantes
    app.vm.hostname = "app.jantony.jonatas.devops"

    # Endereço IP obtido via DHCP na rede privada
    app.vm.network "private_network", type: "dhcp"

    app.vm.provider "virtualbox" do |vb|
      # A memória já está definida globalmente para 512 MB.
      # vb.memory = "512"
    end
  end


  # --- Configuração do Host Cliente (cli) ---
  config.vm.define "cli" do |cli|
    # Substitua 'nome1' e 'nome2' pelos primeiros nomes dos integrantes
    cli.vm.hostname = "cli.jantony.jonatas.devops"

    # Endereço IP obtido via DHCP na rede privada
    cli.vm.network "private_network", type: "dhcp"

    cli.vm.provider "virtualbox" do |vb|
      # Sobrescreve a memória RAM padrão para 1024 MB
      vb.memory = "1024"
    end
  end
end
