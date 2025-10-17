# Configuração do Ambiente

## Pré-requisitos

### Software Necessário
- VirtualBox 6.0 ou superior
- Kali Linux (ISO)
- Metasploitable 2 (OVA)
- DVWA (Damn Vulnerable Web Application)

### Recursos do Sistema
- RAM: Mínimo 4GB (recomendado 8GB)
- Espaço em disco: 20GB livres
- Processador: Dual-core ou superior

## Configuração das Máquinas Virtuais

### 1. Kali Linux

#### Configurações Básicas
- **Nome**: Kali-Linux-BruteForce
- **Tipo**: Linux
- **Versão**: Debian (64-bit)
- **RAM**: 2048 MB
- **Disco**: 15 GB (VDI, Dinamicamente alocado)

#### Configuração de Rede
- **Adaptador 1**: Host-only Adapter
- **Nome**: vboxnet0
- **IP**: 192.168.56.101 (configurado automaticamente)

#### Instalação
1. Baixar ISO do Kali Linux
2. Criar nova VM no VirtualBox
3. Configurar as especificações acima
4. Instalar o sistema operacional
5. Atualizar o sistema: `sudo apt update && sudo apt upgrade`

### 2. Metasploitable 2

#### Configurações Básicas
- **Nome**: Metasploitable2
- **Tipo**: Linux
- **Versão**: Ubuntu (64-bit)
- **RAM**: 1024 MB
- **Disco**: 4 GB (usar arquivo OVA)

#### Configuração de Rede
- **Adaptador 1**: Host-only Adapter
- **Nome**: vboxnet0
- **IP**: 192.168.56.102 (configurado automaticamente)

#### Instalação
1. Baixar OVA do Metasploitable 2
2. Importar no VirtualBox (File > Import Appliance)
3. Configurar rede como Host-only Adapter

### 3. DVWA

#### Instalação no Kali Linux
```bash
# Instalar dependências
sudo apt update
sudo apt install apache2 mysql-server php php-mysql php-gd libapache2-mod-php

# Baixar DVWA
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo chown -R www-data:www-data DVWA
sudo chmod -R 755 DVWA

# Configurar banco de dados
sudo mysql -u root -p
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'p@ssw0rd';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;

# Configurar DVWA
cd DVWA/config
sudo cp config.inc.php.dist config.inc.php
sudo nano config.inc.php
```

## Verificação de Conectividade

### Teste de Ping
```bash
# Do Kali Linux para Metasploitable
ping 192.168.56.102

# Do Metasploitable para Kali Linux
ping 192.168.56.101
```

### Scan de Portas
```bash
# Scan básico
nmap -sS 192.168.56.102

# Scan de portas específicas
nmap -p 21,22,80,139,445 192.168.56.102

# Detecção de serviços
nmap -sV 192.168.56.102
```

## Instalação de Ferramentas

### Medusa
```bash
# Instalar Medusa
sudo apt install medusa

# Verificar instalação
medusa -V

# Listar módulos disponíveis
medusa -d
```

### Nmap
```bash
# Instalar Nmap
sudo apt install nmap

# Verificar instalação
nmap --version
```

### Enum4linux
```bash
# Instalar enum4linux
sudo apt install enum4linux

# Verificar instalação
enum4linux
```

## Configuração de Wordlists

### Criar Wordlists Personalizadas
```bash
# Criar diretório para wordlists
mkdir -p wordlists

# Wordlist básica
echo -e "admin\npassword\n123456\nroot\ntest" > wordlists/basic_passwords.txt

# Wordlist para FTP
echo -e "admin\npassword\n123456\nroot\ntest\nftp\nanonymous" > wordlists/ftp_passwords.txt

# Wordlist para DVWA
echo -e "admin\npassword\n123456\nletmein\nqwerty" > wordlists/dvwa_passwords.txt
```

## Troubleshooting

### Problemas Comuns

#### VM não consegue se comunicar
- Verificar se ambas as VMs estão na mesma rede Host-only
- Verificar se o adaptador vboxnet0 está ativo
- Testar ping entre as VMs

#### Serviços não respondem
- Verificar se os serviços estão rodando no Metasploitable
- Verificar firewall (desabilitar se necessário)
- Verificar logs de erro

#### DVWA não carrega
- Verificar se Apache está rodando: `sudo systemctl status apache2`
- Verificar se MySQL está rodando: `sudo systemctl status mysql`
- Verificar permissões dos arquivos DVWA
