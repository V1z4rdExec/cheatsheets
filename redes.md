#  Redes & Portas — Cheatsheet

Portas, protocolos e conceitos de rede para segurança e infraestrutura.

---

##  Portas essenciais

| Porta | Serviço | Protocolo |
|---|---|---|
| 20/21 | FTP | TCP |
| 22 | SSH / SFTP | TCP |
| 23 | Telnet (inseguro) | TCP |
| 25 | SMTP (envio de e-mail) | TCP |
| 53 | DNS | UDP/TCP |
| 67/68 | DHCP | UDP |
| 80 | HTTP | TCP |
| 110 | POP3 | TCP |
| 123 | NTP | UDP |
| 143 | IMAP | TCP |
| 161/162 | SNMP | UDP |
| 389 | LDAP | TCP |
| 443 | HTTPS | TCP |
| 445 | SMB | TCP |
| 636 | LDAPS | TCP |
| 3389 | RDP | TCP |
| 5060/5061 | SIP (VoIP) | UDP/TCP |

**Pares normal → seguro:** HTTP 80 → HTTPS 443 · LDAP 389 → LDAPS 636 · FTP 21 → SFTP 22

**E-mail:** SMTP **envia** (25) · POP3 **recebe e baixa** (110) · IMAP **recebe e mantém** (143)

---

##  Modelo OSI

| # | Camada | Função | Exemplos |
|---|---|---|---|
| 7 | Aplicação | Interface do app | HTTP, DNS, FTP |
| 6 | Apresentação | Cripto, formato | TLS, JPEG |
| 5 | Sessão | Gerencia sessões | — |
| 4 | Transporte | Entrega fim-a-fim | TCP, UDP |
| 3 | Rede | Endereço lógico | IP, **roteador** |
| 2 | Enlace | Endereço físico | MAC, **switch** |
| 1 | Física | Bits no meio | cabo, sinal |

> **Roteador = L3 (IP)** · **Switch = L2 (MAC)** · **Switch L3** roteia entre VLANs

**TCP** = confiável, com handshake (web, e-mail) · **UDP** = rápido, sem garantia (voz, vídeo, DNS)

---

##  IP privado (RFC 1918)

| Classe | Range |
|---|---|
| A | `10.0.0.0` – `10.255.255.255` |
| B | `172.16.0.0` – `172.31.255.255` |
| C | `192.168.0.0` – `192.168.255.255` |

> IP privado não roteia na internet — precisa de **NAT**. `169.254.x.x` (APIPA) = DHCP falhou.

---

##  DHCP — processo DORA

**D**iscover → **O**ffer → **R**equest → **A**ck · portas 67 (server) / 68 (client)

---

##  DNS — registros

| Registro | Função |
|---|---|
| A | Nome → IPv4 |
| AAAA | Nome → IPv6 |
| CNAME | Apelido (alias) |
| MX | Servidor de e-mail |
| PTR | IP → Nome (reverso) |
| NS | Servidor de nomes |

> Porta 53. AD depende de DNS (registros SRV localizam os controladores).

---

##  VLAN

Segmenta a rede física em redes lógicas isoladas (cada uma = domínio de broadcast separado).

- **802.1Q** — padrão que etiqueta (tag) os quadros
- **Access** — porta de uma VLAN (untagged, para o dispositivo final)
- **Trunk** — carrega várias VLANs entre switches (tagged)

> VLANs diferentes **só se comunicam via roteador ou switch L3** (inter-VLAN routing) — o trunk só transporta, não roteia.

---

##  VPN

Túnel criptografado sobre a internet pública.

| Tipo | Uso |
|---|---|
| **Site-to-Site** | Conecta duas redes/filiais |
| **Client-to-Site** | Conecta um usuário à rede (home office) |

**Protocolos:** IPsec (camada 3) · SSL/TLS (usa 443) · WireGuard · OpenVPN

---

##  NAT

Traduz IP privado ↔ público. **PAT/NAT Overload** = vários internos por 1 IP público, diferenciados por porta.
