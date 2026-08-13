#  Nmap — Cheatsheet

Scanner de portas e auditoria de rede. Reconhecimento e mapeamento de alvos.

---

##  Sintaxe base

```bash
nmap [opções] [alvo]
nmap 192.168.1.10          # host único
nmap 192.168.1.0/24        # rede inteira
nmap 192.168.1.10-50       # faixa
```

---

##  Flags principais

| Flag | Nome | O que faz |
|---|---|---|
| `-sS` | SYN / stealth | Handshake incompleto, rápido e discreto (padrão root) |
| `-sT` | TCP connect | Handshake completo (fica em log) |
| `-sU` | UDP scan | Varre portas UDP (DNS 53, SNMP 161) |
| `-sV` | Version | Descobre serviço e **versão** |
| `-O` | OS detection | Identifica o **sistema operacional** |
| `-A` | Agressivo | Combo: -O + -sV + scripts + traceroute |
| `-p` | Portas | `-p 80` · `-p 1-1000` · `-p-` (todas) |
| `-Pn` | No ping | Assume host vivo (pula descoberta ICMP) |
| `-sn` | Ping scan | Só descobre hosts vivos, **sem** escanear portas |
| `-T0`–`-T5` | Timing | 0=lento/furtivo, 5=rápido (T3 padrão) |
| `-sC` | Scripts | NSE (Nmap Scripting Engine) |
| `-oN`/`-oX` | Output | Salva em texto / XML |

---

##  Estados de porta

| Estado | Significado |
|---|---|
| **open** | Serviço escutando e respondendo |
| **closed** | Acessível, mas nada escutando (respondeu RST) |
| **filtered** | Firewall bloqueando (sem resposta) |

> **closed responde** (sem serviço) · **filtered não responde** (algo filtra — provável firewall)

---

##  Distinções 

- **`-sS` vs `-sT`** → SYN não completa o handshake (furtivo, root); connect completa (logado)
- **`-sV` vs `-O`** → versão do **serviço** vs **sistema operacional**
- **`-sn`** → só lista hosts vivos, sem tocar em portas

---

##  Exemplos práticos

```bash
nmap -sS -p 22,80,443 10.0.0.5     # SYN scan em 3 portas
nmap -sV -O 10.0.0.5               # serviços + SO
nmap -sn 10.0.0.0/24              # quem está vivo na rede
nmap -A -T4 10.0.0.5              # agressivo e rápido
nmap -sU -p 161 10.0.0.5          # SNMP aberto? (UDP)
nmap -Pn -p 3389 10.0.0.5         # RDP ignorando ping bloqueado
nmap -p- 10.0.0.5                 # todas as 65535 portas
```
