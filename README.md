
# 🛡️ Cyber Security - Guia Prático de Configuração e Testes (Debian Linux)

Este guia contém instruções práticas para configuração de rede, permissões de arquivos, servidor web, e testes com cURL e SSH em ambiente Linux Debian. Ideal para laboratórios e ambientes de estudo.

---

## 📦 Primeiros Passos

### Acesso inicial:
```bash
Usuário: root  
Senha: fiap
```

### Comandos úteis:
```bash
cd /              # Vai para o diretório raiz
ip -br -c a       # Exibe IP de forma resumida e colorida
init 0            # Desliga a máquina
```

---

## 🌐 Configuração de Rede

Editar interfaces:
```bash
nano /etc/network/interfaces
```

Exemplo de configuração:
```bash
# enp0s8 - BRIDGE (DHCP)
allow-hotplug enp0s8
iface enp0s8 inet dhcp

# enp0s9 - INTERNA (Estática)
allow-hotplug enp0s9
iface enp0s9 inet static
address 192.168.15.2/24
```

Ativar/desativar interfaces:
```bash
ifup enp0s8
ifdown enp0s8

ifup enp0s9
ifdown enp0s9
```

---

## 📁 Criação e Permissões de Arquivos

### Criar arquivos e pastas:
```bash
cd /home/aluno
mkdir pasta_teste
cd pasta_teste
touch arquivo_teste.txt
nano arquivo_teste.txt
```

### Permissões (CHMOD):
- Dono: X | Grupo: Y | Outros: Z (ex: `chmod 700`)
- Valores: `4` (r), `2` (w), `1` (x)

Exemplos:
```bash
chmod 700 pasta_teste                      # Apenas o dono acessa
chmod 711 pasta_teste                      # Outros podem ver a pasta, não acessar arquivos
chmod 600 pasta_teste/arquivo_teste.txt    # Somente o dono pode ler/escrever
```

---

## 🌍 Subir Servidor HTTP (Python)

Rodar como root:
```bash
python3 -m http.server 1234
```

Rodar como outro usuário:
```bash
su - aluno -c "python3 -m http.server 1234 --bind 0.0.0.0"
```

---

## 🧰 Gerenciamento do Apache2

### Instalação:
```bash
apt update
apt install apache2 net-tools
```

### Controle de serviço:
```bash
service apache2 start | stop | restart | status
```

### Arquivos de configuração:
```bash
nano /etc/apache2/apache2.conf
nano /etc/apache2/conf-enabled/security.conf
nano /etc/apache2/ports.conf
```

### Diretório padrão:
```bash
cd /var/www/html
rm index.html
```

---

## 📡 Comandos de Rede

Verificar IP e conexões:
```bash
ip -br -c a
netstat -nltp
ss -nltp
```

Verificar usuários:
```bash
cat /etc/passwd
```

Gerenciar usuários:
```bash
adduser nome
passwd nome
userdel -r nome
```

---

## 🧪 Testes com cURL

1. Ver conteúdo:
   ```bash
   curl http://<ip-da-vm>
   ```

2. Somente cabeçalhos:
   ```bash
   curl -I http://<ip-da-vm>
   ```

3. Definir user-agent:
   ```bash
   curl -A "Aluno" http://<ip-da-vm>/agent/
   ```

4. POST simulado:
   ```bash
   curl -X POST -d "user=teste" http://<ip-da-vm>/posttest/
   ```

5. Autenticação HTTP:
   ```bash
   echo -n "usuario:senha" | base64
   curl -H "Authorization: Basic <hash>" http://<ip-da-vm>/authtest/
   ```

6. Acessar arquivo oculto:
   ```bash
   curl http://<ip-da-vm>/oculto/dica.txt
   ```

---

## 🔐 SSH - Acesso Remoto Seguro

### Configurar servidor SSH:
```bash
nano /etc/ssh/sshd_config
```

Parâmetros recomendados:
```
PermitRootLogin yes
Port 22    # Ou outra porta customizada
```

### Reiniciar serviço:
```bash
service ssh restart
```

### Conectar via SSH:
```bash
ssh aluno@<ip-da-maquina>
ssh aluno@<ip-da-maquina> -p <porta>
```

### Redirecionamento de porta (túnel):
```bash
ssh -L 8080:localhost:1234 aluno@192.168.10.20 -p 2222
```

---

## 📄 Resumo de Comandos

| Tarefa | Comando |
|--------|---------|
| Criar usuário | `adduser nome` |
| Criar pasta | `mkdir nome_da_pasta` |
| Criar arquivo | `touch nome.txt` |
| Editar arquivo | `nano nome.txt` |
| Mudar permissões | `chmod 700 nome` |
| Rodar servidor | `python3 -m http.server 1234` |
| Conectar SSH | `ssh usuario@ip` |
| Ver IPs | `ip -br -c a` |

---

## 🔗 Links úteis

- [Site Oficial Debian](https://www.debian.org/index.pt.html)
- [Guia CP1 Notion](https://www.notion.so/CP1-1b8933153f90806b8e11dd9f8c9adf84?pvs=21)

---

> **Atenção**: Este guia é destinado a ambientes de laboratório. Para ambientes de produção, siga as melhores práticas de segurança.
