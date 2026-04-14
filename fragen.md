# 1. Technologien

## NAT (Network Address Translation)
- **Unterschied IP-Adressen**: Private IPs sind nur im lokalen Netz gültig, öffentliche IPs sind weltweit eindeutig und im Internet routbar.
- **Private Netzwerk Kontingente (RFC 1918)**:
  - `10.0.0.0` - `10.255.255.255`
  - `172.16.0.0` - `172.31.255.255`
  - `192.168.0.0` - `192.168.255.255`
- **NAT-Table**: Erstellen einer Tabelle mit internen IPs/Ports, die auf die externe Router-IP und selbstgewählte externe Ports gemappt werden.
- **Vor- und Nachteile von privaten IPs**:
  - **Pro**: Sicherheit (von außen ist nicht sichtbar, wer im internen Netzwerk gesendet hat).
  - **Pro**: Weniger IP-Adressen werden verbraucht (hält IPv4 länger aktuell).
  - **Kontra**: IPv4 ist trotzdem begrenzt und nicht unendlich haltbar.

## Subnetting
- **Grundlagen**: Aufbau einer IP-Adresse, Subnetzmasken (SNM) und IP-Klassen.
- **Beispiel NW 170.10.x.x -> 4 Teilnetzwerke (TNW)**:
  - Benötigt Anpassung der Subnetzmaske, um Bits für 4 Netze ($2^2$) zu reservieren.
- **Netzwerkadresse**: Die Adresse `x.x.0.0` (bzw. die erste Adresse im Subnetz) steht für das gesamte Netzwerk und darf keinem Host zugewiesen werden.

## Routing (Statisches & Dynamisches Routing)
- **Grundlagen**: Aufbau eines grundlegenden Netzwerks mit IGs (Interior Gateways) und EGs (Exterior Gateways).
- **Statisches Routing**: Routing-Tables müssen schnell umprogrammiert werden, wenn sich in den IGs etwas ändert.
- **3 Arten von Routing**:
  - **Direct**: Ins Nachbarnetz (Kein Eintrag in der Routing-Table nötig).
  - **Indirect**: Über 2 oder mehr Router (Benötigt zwingend einen Eintrag in der Routing-Table).
  - **Default**: Alles Unbekannte läuft über das EG (Default Gateway), "der wird es wissen".
- **Dynamisches Routing (Alternativen zum statischen)**:
  - **Distance Vector Routing**: Nutzt den Bellman-Ford-Algorithmus (z.B. RIP).
  - **Link State Routing**: Nutzt den Dijkstra-Algorithmus (z.B. OSPF).

## VLAN & WLAN
- **VLAN (Virtual LAN)**:
  - Switch zeichnen und Funktionsweise erklären.
  - Was macht einen Switch VLAN-fähig? (Tagging nach 802.1Q).
  - Was ist ein Layer-3-Switch? (Kombiniert Switch- und Router-Funktionen für Routing zwischen VLANs).
- **WLAN (Notfallfrage) & Leitungscodes**:
  - Wie codiert man eine Bitfolge im Manchester Code? (Takt und Daten in einem Signal, Flankenwechsel in der Bit-Mitte).

# 2. Applikationen

## CDN & Servercluster
- **Webserver**: Aufbau WWW und Server-Software (APACHE, NGINX, IIS). *Hinweis: Auf spezifische Probleme eingehen.*
- **CDN (Content Delivery Network)**: Aufbau und Funktionsweise (geografisch verteilte Server zur Latenzverringerung).
- **Servercluster**: Praktischer Aufbau und Funktionsweise (Hochverfügbarkeit, Lastverteilung).

## DNS (Domain Name System)
- **Aufbau**:
  - **Logisch**: Baumstruktur (Top-Level-Domains etc.).
  - **Physisch**: Verteilte Datenbank.
- **Praxis**: Ein grundlegendes Bind-9 Zone-File schreiben können.

## Mail
- **Systemaufbau**: Skizze des Systems, verwendete Protokolle (SMTP, IMAP, POP3) und Aufbau einer Mail-Adresse.
- **Praxis**: E-Mail via Commandline verschicken (Telnet / Netcat).
- **Alternativen**: Welche Alternativen gibt es zum klassischen Mail-System?

## Telnet, FTP & SSH
- **Telnet**: Aufbau und Zweck (Steuerung externer Geräte). Unsicher, da Klartext. SSH ist der bessere, verschlüsselte Nachfolger.
- **FTP (File Transfer Protocol)**:
  - Nutzt 2 Leitungen: Steuerleitung (abgespecktes Telnet) und Datenleitung.
  - Befehle: `CD` (Change Directory, remote), `LCD` (Local Change Directory, am Client), `PUT` (Upload), `GET` (Download).
  - Sicherheitsproblem: Alles (inklusive Passwörter) kann mitgelesen werden.
  - Lösung: SFTP (FTP getunnelt durch SSH-Steuerleitung).
- **SSH**: Welche Kryptographie wird verwendet? (Asymmetrisch für Schlüsselaustausch, Symmetrisch für Daten).

# 3. Netzwerkverwaltung

## Grenznetze & iptables
- **iptables**: Aufbau von Befehlen, CHAINS (Input, Output, Forward) und POLICIES (Accept, Drop, Reject). Interpretation und Erstellung von Regeln.
- **Grenznetz (Perimeter Network)**: Aufbau und Konfiguration des inneren und äußeren Routers austüfteln.
- **Alternativen**: DMZ (Demilitarized Zone).

## Bastion Host & Überwachungsrouter
- **Architektur**: Aufbau von Bastion Host (BH) + Überwachungsrouter (ÜR).
- **Konfiguration**:
  - Proxy-Konfiguration am Bastion Host.
  - URL-Regex für Filterung.
  - iptables-Konfiguration am Überwachungsrouter.
- **Kompromiss**: Nicht ein Proxy, der alles filtert, sondern Arbeitsteilung (HTTP/HTML über Proxy, E-Mails etc. über iptables am Router gefiltert).
- **Zusammenführung**: Wie arbeiten iptables und Proxy (BH + ÜR) zusammen?

## Domainmodelle und AD/LDAP
- **Grundlagen**: Aufbau von Workgroups (Peer-to-Peer) vs. Domains (Zentral).
- **Domain Controller (DC)**: Fungiert als zentrale Objektdatenbank.
- **Modelle**: (Multiple) Masterdomainmodelle.
- **Active Directory (ADS)**: Aufbau des AD auf dem DC.
- **LDAP**: Protokoll, mit dem auf den Domain Controller (die Objektdatenbank) zugegriffen wird.

# 4. Kryptographie

## VPN (Virtual Private Network)
- **Digitale Signatur**: Funktionsweise (Hash + Private Key).
- **VPN Tunnel bauen**: 2 öffentliche IPs, 2 LANs. Eintragen von ESP (Encapsulating Security Payload) und AH (Authentication Header).
- **Probleme**: Wie kann ein VPN-Tunnel blockiert werden (z.B. Deep Packet Inspection)?

## TOR (The Onion Router)
- **Diffie-Hellman**: Wie funktioniert der Schlüsselaustausch und warum wird er genutzt?
- **Web-Arten**: 3 Arten von Web (Surface, Deep, Dark) und wie man dorthin gelangt.
- **Aufbau**: TOR mit 3 Servern (Entry, Middle, Exit Node) + große Variante.
- **Erfinder**: Naval Research Laboratory (US Navy).
- **Schwächen**: Wie kann man TOR austricksen? (Traffic Analysis / Timing Attacks).

## Blockchain
- **Grundlagen**: Was ist ein kryptographischer Hash?
- **Konsensverfahren**:
  - **Proof of Work**: Wie wird die Blockchain damit betrieben?
  - **Alternativen**: Proof of Stake, Proof of Authority.

## Verschlüsselungsverfahren (AES, RSA, Diffie-Hellman)
- **Symmetrische Verschlüsselung**: Erste Verfahren und zugrundeliegende Mathematik (mit Tabelle erklären).
  - Abgelöst durch AES. 2 Vorteile von AES benennen.
- **Asymmetrische Verschlüsselung**: Was wird benötigt? (Public / Private Key).
- **RSA**: Funktionsweise + bisschen rechnen.
  - Probleme von RSA: Nicht quantensicher (Bleichenbacher / Bleeding Heart Attacke).
  - Heutige Nutzung: Wird hauptsächlich zum Signieren verwendet, weniger für reine Datenverschlüsselung.

# 5. Hacking

> *Hinweis: Hier fehlen noch Teile, diese bei Bedarf ergänzen!*

## Pentesting
- Allgemeine Strategie.
- 5 Phasen des Pentestings (Theoretisch).
- Realisierung der Phasen mit spezifischen Tools.
- *(Killerfrage kommt noch)*

## Social Engineering
- Allgemeine Strategie.
- Typische Vorgehensweisen.
- Praktische Durchführung & Gadgets (z.B. Rubber Ducky).
- *(Killerfrage kommt noch)*

## Passwort-Strategien
- [Inhalte ergänzen]

## Malware
- **Definition**: Was ist Malware?
- **Typen**: Verschiedene Typen erklären (Würmer, Trojaner, Ransomware, Spyware etc.).

## Backup-Strategien
- [Inhalte ergänzen]
