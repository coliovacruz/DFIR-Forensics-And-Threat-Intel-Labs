# 🔬 DFIR & Cyber Threat Intelligence (CTI) — Real Incident Analysis

![Type](https://img.shields.io/badge/Focus-DFIR%20%26%20CTI-blue)
![Zeek](https://img.shields.io/badge/Tool-Zeek%20%2F%20Bro-green)
![Autopsy](https://img.shields.io/badge/Tool-Autopsy%204.19-orange)
![Malware](https://img.shields.io/badge/Threat-Emotet%20Trojan-red)

## 📌 Visão Geral
Este repositório reúne dois estudos de caso práticos e aprofundados desenvolvidos durante especialização em Cibersegurança, cobrindo o ciclo completo de resposta a incidentes: **Análise de Tráfego Cibernético (CTI)** e **Forense Computacional em Endpoint (Disco)**.

---

## 🛠️ Casos Analisados

### 1. 🛡️ CTI & Network Traffic Analysis — Infecção por Emotet (PCAP)
* **Objetivo:** Investigar o comprometimento da rede da empresa *Good Money Financial* através da análise de logs gerados pelo framework **Zeek** a partir de um arquivo PCAP.
* **Ferramentas:** `Zeek`, `zeek-cut`, `Foremost`, `Wireshark`, `VirusTotal`.
* **Descoberta Principal:** Identificação de um vetor de ataque em múltiplos estágios iniciado por um documento malicioso do Microsoft Word (`2018_11Details_zur_Transaktion.doc`), download do *payload* do trojan bancário **Emotet** (`6169583.exe`) e mapeamento da infraestrutura de servidores de Comando e Controle (C2).
* **Entregáveis no Repo:**
  * Linha do tempo exata da infecção.
  * Tabela de Indicadores de Comprometimento (IoCs: IPs, Hashes e URLs).
  * Recomendações operacionais de Contenção, Erradicação e *Hardening*.

---

### 2. 🔍 Forense Computacional de Disco — Caso Greg Schardt ("Mr. Evil")
* **Objetivo:** Exame pericial em imagem de disco formato E01 (`Dell Latitude CPi`) apreendida em investigação de crimes cibernéticos.
* **Ferramentas:** `Autopsy 4.19.3`, `Registry Explorer`, `Notepad++`.
* **Achados Periciais:**
  * Validação de integridade da imagem via hash MD5 (`aee4fcd9301c03b3b054623ca261959a`).
  * Identificação da conta com privilégios elevados `Mr. Evil` (SID S-1-5-21-...) criada logo após a instalação do sistema.
  * Mapeamento de arsenal ofensivo instalado (*Cain & Abel*, *WinPcap*, *Ethereal*, *Look@LAN*, *Network Stumbler*).
  * Análise de artefatos de navegação (*MSN Search*, *whatismyip*, pesquisas de técnicas de invasão).
  * Recuperação de binários deletados da Lixeira (`RECYCLER`: `DC1.exe` a `DC4.exe`).
  * Detecção de dispositivo USB conectado momentos antes do último logon.
* **Entregáveis no Repo:**
  * *Timeline* cronológica completa de ações do atacante.
  * Mapeamento do Registro do Windows (SAM, SYSTEM, WINLOGON).

---

## 🚀 Metodologia & Comandos de Destaque

### Extração de Logs com Zeek (`zeek-cut`)
```bash
# Processar arquivo pcap gerando logs de eventos
zeek -C -r 2018-CTF-from-malware-traffic-analysis.net-2-of-2.pcap

# Filtrar host e MAC de IP alvo no log DHCP
cat dhcp.log | zeek-cut ts client_addr mac host_name | grep "172.17.1.129"

# Identificar requisições HTTP para arquivos maliciosos de Word/Executáveis
cat files.log | zeek-cut ts rx_hosts tx_hosts mime_type filename md5 | grep -E "msword|dosexec"

### Carving de Executáveis via Foremost

```bash
foremost -t exe -i 2018-CTF-from-malware-traffic-analysis.net-2-of-2.pcap -o pasta_de_saida/

📋 Indicadores de Comprometimento (IoCs) — Resumo
Tipo                             Indicador / Valor                               Descrição
Host Comprometido                  172.17.1.129 / Nalyvaiko-PC               MAC: 00:1e:67:4a:d7:5c
Dropador InicialHash MD5 / 2018_11Details_zur_Transaktion.doc                Trojan Word / Documento Malicioso
Payload PrincipalHash        57c9851f707e... / 6169583.exe                   Malware Emotet
Servidores                    C224.206.17.102:8080, 67.43.253.189:8080, 86.98.71.86:7080 Comunicação periódica de C2

👤 AutorMarco Aurélio Cruz (CLVCRUZ)Especialista em Cibersegurança, DFIR e Inteligência

LinkedIn | GitHub
