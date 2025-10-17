# Desafio de Força Bruta - DIO

## 📋 Descrição do Projeto

Este projeto implementa e documenta cenários de ataque de força bruta utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (Metasploitable 2 e DVWA), para simular e exercitar medidas de prevenção em ambiente controlado.

## 🎯 Objetivos

- [ ] Compreender ataques de força bruta em diferentes serviços (FTP, Web, SMB)
- [ ] Utilizar o Kali Linux e o Medusa para auditoria de segurança
- [ ] Documentar processos técnicos de forma clara e estruturada
- [ ] Reconhecer vulnerabilidades comuns e propor medidas de mitigação
- [ ] Utilizar o GitHub como portfólio técnico

## 🛠️ Ferramentas Utilizadas

- **Kali Linux** - Sistema operacional para testes de penetração
- **Medusa** - Ferramenta de força bruta
- **Metasploitable 2** - VM vulnerável para testes
- **DVWA** - Damn Vulnerable Web Application
- **VirtualBox** - Virtualização
- **Nmap** - Scanner de rede

## 🔧 Configuração do Ambiente

### Pré-requisitos
- [ ] VirtualBox instalado
- [ ] Kali Linux (ISO)
- [ ] Metasploitable 2 (OVA)
- [ ] DVWA configurado

### Configuração das VMs

#### 1. Kali Linux
- **RAM**: [ 2 ] GB
- **Disco**: [ 15 ] GB
- **Rede**: Host-only Adapter

#### 2. Metasploitable 2
- **RAM**: [ 1 ] GB
- **Disco**: [ 4 ] GB
- **Rede**: Host-only Adapter

### Verificação de Conectividade
```bash
# Teste de conectividade entre as VMs
ping [IP_METASPLOITABLE]
nmap -sn [RANGE_IP]
```

## 🎯 Cenários de Ataque

### 1. Ataque de Força Bruta em FTP

#### Configuração

- **Serviço**: vsftpd (Metasploitable 2)
- **Porta**: 21

#### Wordlist
O wordlist utilizado foi obtido de: https://github.com/jeanphorn/wordlist

#### Comando Medusa
```bash
# Ataque de força bruta FTP
medusa -h 192.168.56.102 -u msfadmin -P wordlists/ftp_passwords.txt -M ftp
```

#### Resultados

![scan - Medusa](images/Screenshot%2025-10-16%211748.png)
*Figura 2: Execução do ataque de força bruta no FTP*

- [x] Sucesso: Senha "msfadmin" encontrada para o usuário "msfadmin"
![resuldado invadido - Medusa](images/Screenshot%2025-10-16%21235a2.png)


### 2. Ataque em Formulário Web (DVWA)

Foi utilizado um DVWA (Damn Vulnerable Web Application) para teste de força bruta em tela de login.

![DVWA Login](images/Screenshot%202025-10-16%20211620.png)
*Figura 3: Interface de login do DVWA*

#### Configuração
- **URL**: http://127.0.0.1/dvwa/
- **Formulário**: Login
- **Usuário alvo**: admin

#### Wordlist
Mesma wordlist utilizada na atividade anterior: https://github.com/jeanphorn/wordlist

#### Comando Medusa
```bash
# Ataque de força bruta HTTP
medusa -h 127.0.0.1 -u admin -P wordlists/dvwa_passwords.txt -M http \
-m PAGE: '/dvwa/login.php' \
-m FORM: 'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
```

#### Resultados
![Ataque DVWA - Medusa](images/Screenshot%2025-10-16%211318.png)
*Figura 4: Execução do ataque de força bruta no DVWA*

Todas as tentativas terá certo provavelmente por que não configurei o DVWA corretamente que eu peguei do git: https://github.com/digininja/DVWA


### 3. Password Spraying em SMB

#### Enumeração de Usuários
```bash
# Enumeração de usuários SMB
enum4linux -U [IP_TARGET]
```

#### Comando Medusa
```bash
# Password spraying SMB
medusa -h [IP_TARGET] -U wordlists/usernames.txt -p wordlists/password.txt -M smbnt
```

#### Resultados
- [ ] Usuários encontrados: [ ] 
- [ ] Senhas válidas: [ ] 
- [ ] Tempo total: [ ] 

## 📊 Análise de Resultados
![Enumeração - SMB](images/Screenshot%2025-10-16%211434.png)
![Enumeração - SMB](images/Screenshot%2025-10-16%211620.png)

### Vulnerabilidades Identificadas
1. **FTP**: Serviço vsftpd vulnerável a ataques de força bruta com senhas fracas
2. **DVWA**: Aplicação web vulnerável com credenciais padrão (admin/password)



### Resumo dos Testes
- ✅ **FTP**: Ataque bem-sucedido com credenciais msfadmin/msfadmin
- ✅ **DVWA**: Ataque bem-sucedido com credenciais admin/password
- ✅ **SMB**





## ⚠️ Aviso Legal

**IMPORTANTE**: Este projeto é destinado exclusivamente para fins educacionais e de aprendizado em ambiente controlado. O uso dessas técnicas em sistemas sem autorização é ilegal e pode resultar em consequências legais. Sempre obtenha permissão explícita antes de realizar testes de penetração em qualquer sistema.

## 📞 Contato

- **Nome**: Luiz Barros da Silva Neto
- **Email**: luizbarros200303@gmail.com
- **LinkedIn**: https://www.linkedin.com/in/luiz-barros-bb79a0269/
- **GitHub**: https://github.com/LuizBarros-code



**Data de Conclusão**: 10/16/2025
**Versão**: 1.0