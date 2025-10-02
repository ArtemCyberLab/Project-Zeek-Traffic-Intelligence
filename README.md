Project goal: analyze network captures (PCAPs) using Zeek to detect HTTP and FTP events and to extract DHCP host information.

Summary of actions performed (Part 1):

HTTP signature for password detection
I wrote a Zeek signature http-password.sig that monitors TCP traffic on port 80 and searches the payload for the string "password". I ran Zeek against the HTTP capture with that signature and extracted the source IP and source port from the generated signature events.

Source IP observed: 10.10.57.178
Source port observed: 38712
Packet counts for the related connection: 11 (sent) and 9 (received)

FTP signatures for brute-force detection
I created ftp-bruteforce.sig with two signatures: one to detect FTP USER commands and another to detect server responses indicating incorrect logins (530 Login incorrect). After running Zeek with these signatures on the FTP capture, I counted events and matches in the logs.

Unique events in the notice log: 1413
Matches for the FTP brute-force signature: 1410

DHCP hostname and domain extraction
I used a small Zeek script to extract DHCP host names and domains from DHCP traffic. This produced multiple host names (for example: student01-PC, vinlap01, a set of JDT* and m30-sqdesk hosts) and revealed domain names seen in the captures, including astaro_vineyard and jaalam.net.

Conclusions from Part 1:

Custom Zeek signatures reliably detected cleartext password occurrences in HTTP traffic and FTP brute-force activity in the FTP capture.
DHCP analysis provided a useful inventory of host names and domains present in the captures, which helps map devices and contextualize other findings.
All steps were executed manually using Zeek and standard command-line tools; the commands and signatures are reproducible and can be applied to additional captures.

Next steps (planned for Part 2): continue with scripted Zeek analysis to count connection events and signature hits, aggregate findings, and produce an expanded report that includes extracted files, hashes, and any IOC correlations.

(PART-2)
Goal: continue pcap analysis to extract artifacts (new connections, signature hits, intel/file-framework results, cleartext credentials steps).

Environment: working in ~/Desktop/Exercise-Files (folders: TASK-7/101,201,202, TASK-8, TASK-9).

Key actions & results

Counted new connections: zeek ... 103.zeek → 87 "New Connection Found!".
FTP signature search: grep "administrator" signatures.log → 731 occurrences; specific signature #FTP-Admin → 0 hits.
FTP brute‑force detector: system script → notice.log lines → 11 notices.
Intelligence framework on case1.pcap: second match in HTTP::IN_HOST_HEADER.
HTTP download: detected /knr.exe.
MD5 of downloaded .exe: cc28e40b46237ab6d5282199ef78c464.
Extracted text file contained "Microsoft NCSI".
zeek-sniffpass module missing (failed to run); recommendation: install via zkg install zeek-sniffpass (or copy it) to extract cleartext credentials.

Logs produced: conn.log, http.log, files.log, signatures.log, notice.log, extract_files/*, etc.






****************************************************************************************************************************************************************************************************************
Objetivo do projeto: analisar capturas de rede (PCAPs) com Zeek para detectar eventos HTTP e FTP e extrair informações de hosts via DHCP.

Resumo das ações realizadas (Parte 1):

Assinatura HTTP para detecção de senhas
Eu escrevi uma assinatura Zeek chamada http-password.sig que monitora tráfego TCP na porta 80 e busca no payload a string "password". Executei o Zeek com essa assinatura contra a captura HTTP e extraí o IP de origem e a porta de origem a partir dos eventos gerados pela assinatura.

IP de origem observado: 10.10.57.178
Porta de origem observada: 38712
Contagem de pacotes para a conexão relacionada: 11 (enviados) e 9 (recebidos)

Assinaturas FTP para detecção de brute-force
Criei ftp-bruteforce.sig com duas assinaturas: uma para detectar comandos FTP USER e outra para detectar respostas do servidor indicando logins incorretos (530 Login incorrect). Após executar o Zeek com essas assinaturas na captura FTP, contei eventos e ocorrências nos logs.

Eventos únicos no notice.log: 1413
Correspondências para a assinatura de brute-force FTP: 1410

Extração de hostnames e domínios via DHCP
Utilizei um script Zeek simples para extrair nomes de hosts e domínios do tráfego DHCP. Isso gerou vários nomes de host (por exemplo: student01-PC, vinlap01, várias máquinas JDT* e m30-sqdesk) e revelou domínios vistos nas capturas, incluindo astaro_vineyard e jaalam.net.

Conclusões da Parte 1:

Assinaturas Zeek customizadas detectaram de forma confiável ocorrências de senhas em texto claro no tráfego HTTP e atividade de brute-force no tráfego FTP.
A análise DHCP forneceu um inventário útil de nomes de hosts e domínios presentes nas capturas, ajudando a mapear dispositivos e contextualizar outras evidências.
Todos os passos foram executados manualmente usando Zeek e ferramentas de linha de comando; os comandos e assinaturas são reproduzíveis e podem ser aplicados a outras capturas.

Próximos passos (planejados para a Parte 2): continuar com análises Zeek por script para contar eventos de conexão e hits de assinaturas, agregar os achados e produzir um relatório expandido que inclua arquivos extraídos, hashes e correlações de IOCs.

PARTE-2 
Objetivo: continuar a análise de pcaps para extrair artefatos (novas conexões, hits de assinatura, resultados de frameworks de inteligência/arquivos, passos para credenciais em texto claro).

Ambiente: trabalhando em ~/Desktop/Exercise-Files (pastas: TASK-7/101,201,202, TASK-8, TASK-9).

Principais ações e resultados

Contagem de novas conexões: zeek ... 103.zeek → 87 "New Connection Found!".
Busca de assinatura FTP: grep "administrator" signatures.log → 731 ocorrências; assinatura específica #FTP-Admin → 0 hits.
Detector de brute‑force FTP: script do sistema → linhas de notice.log → 11 avisos.
Framework de inteligência em case1.pcap: segundo match em HTTP::IN_HOST_HEADER.
Download HTTP detectado: /knr.exe.
MD5 do .exe baixado: cc28e40b46237ab6d5282199ef78c464.
Arquivo de texto extraído contém "Microsoft NCSI".
Módulo zeek-sniffpass ausente (falha ao rodar); recomendação: instalar via zkg install zeek-sniffpass (ou copiar) para extrair credenciais em texto claro.

Logs produzidos: conn.log, http.log, files.log, signatures.log, notice.log, extract_files/*, etc.


