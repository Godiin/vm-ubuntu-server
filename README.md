Projeto: Criação e Configuração de VM Linux (Ubuntu Server)

📌 Visão Geral
Este projeto documenta a criação e configuração de uma máquina virtual Linux (Ubuntu Server) com foco em ambiente de estudos, laboratório e boas práticas de administração de sistemas.

O objetivo é preparar uma VM funcional com:
  * Usuários configurados
  * Endereço IP fixo
  * Acesso remoto via SSH

🎯 Objetivos do Projeto
  * Criar uma VM Linux do zero
  * Configurar usuários de forma segura
  * Definir IP fixo
  * Habilitar e testar acesso SSH

🧩 Escopo
Inclui:
  * Instalação do Ubuntu Server
  * Configuração básica de rede
  * Criação e gerenciamento de usuários
  * Acesso remoto via SSH

Não inclui (por enquanto):
  * Firewall avançado
  * Hardening de segurança
  * Monitoramento
  * Serviços (Web, Banco, etc.)

🛠️ Tecnologias Utilizadas
  * Sistema Operacional: Ubuntu Server
  * Virtualização: VirtualBox / VMware (ou outro)
  * Acesso remoto: OpenSSH
  * Ambiente: Linux

🏗️ Arquitetura do Ambiente
  * Host físico executando o hypervisor
  * VM Ubuntu Server em modo Bridge ou NAT
  * Acesso remoto via SSH a partir da rede local

🚀 Etapas do Projeto
1️⃣ Criação da Máquina Virtual
  * Criação de nova VM no hypervisor

  * Definição de:
    * CPU
    * Memória RAM
    * Disco
    * Anexação da ISO do Ubuntu Server
    * Instalação padrão do sistema

2️⃣ Criação e Gerenciamento de Usuários
    * Criação de usuário principal
    * Definição de senha segura
    * Inclusão em grupos administrativos (sudo)

Exemplo de comando:

'''bash
adduser usuario
usermod -aG sudo usuario

obs.: Deixei um usuário para com permissão sudo e outro sem.

3️⃣ Configuração de IP Fixo

'''md
## 🌐 Configuração de IP (Modo Temporário)

Neste projeto, o endereço IP foi configurado de forma **temporária**, utilizando o comando `ifconfig`, com fins de **teste e estudo**.

### Identificação da interface de rede
'''bash
ip a

🌐 Configuração de Endereço IP
Neste projeto, a configuração de rede foi realizada de duas formas:
1. Configuração temporária via ifconfig (para testes)
2. Configuração persistente via Netplan (definitiva)

Opção 1: Configuração Temporária (ifconfig)
Utilizada para testes rápidos em laboratório.

Identificação da interface de rede
'''bash
ip a

Configuração do IP
'''bash
sudo ifconfig enp0s3 192.168.1.100 netmask 255.255.255.0

Configuração do gateway
'''bash
sudo route add default gw 192.168.1.1

⚠️ Observação:
Essa configuração é perdida após reiniciar a VM.

🔹 Opção 2: Configuração Persistente (Netplan)
Utilizada como configuração definitiva do sistema.

Acessar o diretório do Netplan
'''bash
cd /etc/netplan

Editar o arquivo de configuração
'''bash
sudo nano 50-cloud-init.yaml

Configuração aplicada
'''yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4

Aplicar as configurações
'''bash
sudo netplan apply

Verificação do IP
'''bash
ip a

4️⃣ Ativação do SSH
  * Instalação do serviço OpenSSH
  * Inicialização do serviço
  * Liberação de acesso remoto
'''bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl status sshd

Teste de acesso:
'''bash
ssh usuario@192.168.1.100

🔄 Fluxo de Uso
  * Usuário inicia a VM
  * VM recebe IP fixo configurado
  * Administrador acessa via SSH
  * Gerenciamento é feito remotamente

⚠️ Desafios Encontrados
  * Identificação correta da interface de rede
  * Configuração inicial do Netplan
  * Garantir acesso remoto sem perder conectividade

✅ Soluções Aplicadas
  * Uso de IP fixo para evitar perda de acesso
  * Testes locais antes de acesso remoto
  * Configuração mínima para evitar falhas

📈 Resultados Obtidos
  * VM Linux totalmente funcional
  * Acesso remoto estável
  * Ambiente pronto para estudos, labs e serviços

📚 Aprendizados
  * Administração básica de Linux Server
  * Configuração de rede em ambiente virtualizado
  * Importância da documentação técnica
  * Fundamentos essenciais para NOC / SOC / SysAdmin

🛣️ Próximos Passos
  * Configurar firewall (UFW)
  * Criar chaves SSH
  * Desativar login root via SSH
  * Implementar monitoramento
  * Usar essa VM como base de um Home Lab

📎 Conclusão

Este projeto serve como base sólida para ambientes Linux em produção ou laboratório, reforçando conceitos fundamentais de redes, sistemas operacionais e acesso remoto seguro.

👤 Autor

Ygor Silva
📍 Estudante e profissional em redes, Linux e cibersegurança
🔗 GitHub: ([ygor_silvaw](https://github.com/ygor_silvaw))
