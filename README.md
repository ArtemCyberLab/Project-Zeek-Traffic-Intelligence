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
