# 🐧 Linux — Cheatsheet

Comandos e conceitos essenciais de administração Linux para infraestrutura e segurança.

---

##  Estrutura de diretórios (FHS)

| Diretório | Conteúdo |
|---|---|
| `/etc` | Arquivos de configuração |
| `/var/log` | Logs do sistema |
| `/home` | Diretórios dos usuários |
| `/root` | Home do usuário root (**não** é `/home/root`) |
| `/bin`, `/sbin` | Binários essenciais (`/sbin` = administração) |
| `/tmp` | Temporários (sticky bit) |
| `/proc` | Info virtual de processos/kernel |
| `/dev` | Dispositivos (`/dev/sda`) |

---

##  Permissões

Leitura de `ls -l`: `-rwxr-xr--` → tipo · dono · grupo · outros

| Valor | Permissão | Octal |
|---|---|---|
| `r` | leitura | 4 |
| `w` | escrita | 2 |
| `x` | execução | 1 |

**Combinações comuns:** `755` (rwxr-xr-x) · `644` (rw-r--r--) · `600` (chaves SSH) · `777` (inseguro)

```bash
chmod 755 script.sh        # octal
chmod u+x script.sh        # simbólico: dono ganha execução
chmod -R 755 /var/www      # recursivo
chown user:grupo arquivo   # muda dono e grupo
```

**Especiais:** SUID (`4`) roda com permissão do dono · SGID (`2`) herda grupo · Sticky (`1`) só dono apaga

> Em **diretório**: `r`=listar, `w`=criar/apagar, `x`=entrar (cd)

---

##  Processos e serviços

```bash
ps aux                  # todos os processos
top                     # tempo real
kill -15 PID            # SIGTERM (encerra educadamente)
kill -9 PID             # SIGKILL (força)

systemctl status ssh    # estado do serviço
systemctl start ssh     # inicia agora
systemctl enable ssh    # habilita no boot (NÃO inicia agora)
systemctl enable --now ssh   # boot + agora
journalctl -u ssh -f    # logs em tempo real
```

> **Pegadinha:** `start` ≠ `enable`. `start` = agora; `enable` = no boot.

---

##  Usuários

| Arquivo | Conteúdo |
|---|---|
| `/etc/passwd` | Cadastro de usuários (público) |
| `/etc/shadow` | Hashes de senha (só root) |
| `/etc/group` | Grupos |

```bash
useradd -m -s /bin/bash user   # -m cria o home
passwd user                     # define senha
usermod -aG grupo user          # -aG adiciona (sem -a, SUBSTITUI!)
id user                         # UID, GID, grupos
```

> UID 0 = **root**, sempre.

---

##  Busca e texto

```bash
grep -i "erro" arquivo     # -i ignora maiúsc/minúsc
grep -r "senha" /etc       # -r recursivo
grep -v "debug" arquivo    # -v inverte (linhas que NÃO têm)
find / -name "*.conf"      # busca por nome
find / -perm -4000         # arquivos SUID (auditoria)
tail -f /var/log/syslog    # acompanha log em tempo real
```

> `grep` busca **dentro** de arquivos · `find` busca **os arquivos**

---

##  Pacotes

| Debian/Ubuntu | RHEL/CentOS |
|---|---|
| `apt update` (atualiza lista) | `yum/dnf update` |
| `apt upgrade` (atualiza pacotes) | `yum upgrade` |
| `apt install pacote` | `yum install pacote` |
| `.deb` / `dpkg` | `.rpm` / `rpm` |

> `apt update` só atualiza a **lista**; quem atualiza os programas é o `upgrade`.

---

##  Compactação

```bash
tar -czvf backup.tar.gz /etc   # Criar (c), gzip (z)
tar -xzvf backup.tar.gz        # eXtrair
tar -tzvf backup.tar.gz        # lisTar sem extrair
```
