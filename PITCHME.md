# Einführung Docker 

#### MUC, 08.01.2019

---
@snap[north]
## Wer bin ich?
@snapend

@snap[west]
@ul
- Lukas Grams
- Informatikstudent, 7. Semester
- Security-Fokus 
@ulend
@snapend

@snap[east]
@ul
- Container-Erfahrung als Werksstudent
- Mehrere Vorlesungen zum Thema
- Bei weitem **kein Spezialist**
@ulend
@snapend

+++

## Disclaimer

@ul
- Wissen vermitteln > Kompetenz zeigen
- Effizienz > Professionalität
@ulend

---

## Agenda

@ul
- Container - Was ist das, kann man das essen?
- Docker & Friends
- Wofür brauch ich das?
- Wie kann ich das benutzen?
- Praxisbeispiele!
- Wie kann ich weitermachen?
- Tipps
@ulend

Note:

Einführung, Vergleich zu VMs  
Einsatzmöglichkeiten  
Docker & Tools drumrum 
Bedienungsanleitung  
Beispiele  
Recherche-Links  
Best Practices

---

## Container
### Was ist das, kann man das essen?

+++

## Technische Grundlage
@ul
- Zwei Features des Linux-Kernels
- Process namespaces
- Control groups (cgroups)
@ulend

Note:

Linux Container allgemein, nicht nur Docker  
Weiß nicht viel, kurzer Abriss zu beidem

+++

## namespaces
@ul
- Beschränkt Sichtbarkeit auf...
- Prozesse
- Netzwerk
- Dateisystem
- Hostnamen
@ulend

Note:

Prozesse sehen Unterschiedliches
1 Rechner, 2 Prozesse, 2 Hostnamen

+++

## cgroups

@ul
- Beschränkt Zugriff auf...
- Ressourcen (RAM, CPU, ...)
- Geräte (Tastatur, WLAN, ...)
@ulend

Note:

Einschränkung binär oder graduell  
-> Prozess darf nicht auf GPU oder nur X shares

+++

@snap[west]
## Was bringt uns das?

@ul
- verschiedene Prozesse, vollkommen andere Umgebungen
- aber: gemeinsamer Kernel
- "VM-light"
@ulend

@snapend

@snap[east span-20]
![](assets/images/thinking_face.png)
@snapend


Note:

beliebig einstellen wer was sieht  
für docker: alle starten bei null, kochen eigenes süppchen  
optimierung: manche teilen sich noch sachen, nicht wichtig  
sieht ähnlich aus wie VM  

+++

![](assets/images/containers_vms.png)

Note:

Kein Hypervisor mehr  
Keine parallelen OS  
Docker-"Programm" verwaltet

+++

@snap[west]

### VMs
@ul
- etablierte Hypervisor
- "normales System"
- Ressourcen-"Verschwendung"
- Lange Anlaufzeiten
@ulend

@snapend

vs.

@snap[east]

### Container
@ul
- braucht Einarbeitung
- Ressourcensparend
- schneller oben
- wird komplex ab Cluster
@ulend

@snapend

Note:

VMs gibts lange, sehen für Admin aus wie normaler Rechner  
Anlaufzeiten beim Aufsetzen UND Hochfahren  
(selbst mit ansible/chef/puppet)  
höhö  
Cluster = sahnehäubchen, kommt später

+++

## Verschiedene Container

@ul
- FreeBSD: Jails
- Solaris: Zones  
- Linux: **Docker**, Google Containers, LXC,  ...
- Docker = King
@ulend

Note:

Alternativen mit selber Idee, unterschiedlicher Umsetzung  
LXC mehr Prozesse, Docker einer pro Container  
Docker mit Abstand am verbreitetsten  
-> bester Support, mehr Tools drumrum

---

## Docker & Friends

+++

@snap[west]
## Geschichtsstunde!

@ul
- Firma begonnen 2013 als dotCloud
- Rebranding zu Docker Inc.
- Open-Source Projekt mit "Freemium Modell"
- angefangen auf LXC-Basis
- jetzt direkt auf Kernel
@ulend

@snapend


@snap[east span-20]
![](assets/images/docker_logo.png)
@snapend

Note:

Enterprise Maintainer, OS -> cool  
Premium-Features wären Bonus für Production  
(Security, Private Repos, ...)

+++

Image, container, engine
(docker for windows, windows-docker anschneiden)

+++

Docker Repo's, Docker Hub, private Repos

+++

Docker-Compose

+++

Orchestrierung, Kubernetes, Rancher, Swarm, DC/OS, Cloud-Integration

---

## Wofür brauch ich das?

+++

Container sind **kein Ersatz** für VMs, sondern **eine Alternative**

+++

Stell dir vor, VMs...
@ul
- neu-anlegen ginge in Sekunden
- wären sofort arbeitsfähig nach Hochfahren
- fahren in <10 Sekunden hoch
- können jederzeit wegsterben
- vergessen dabei alles seit Start
@ulend

Note:

Neu-Anlegen braucht einmalig download/build, danach beliebig oft "duplizierbar"  
Wird am Anfang so konfiguriert dass er direkt seinen Zweck erfüllt  
"hochfahren" ist hier einfaches Prozess-starten  
vergessen kann umgangen werden, mehr später

---

use cases

---

## Wie kann ich das benutzen?

+++

TODO

---

## Praxisbeispiele!

+++

TODO

---

## Wie kann ich weitermachen?

+++

TODO

---

## Tipps

+++

TODO

---

#Fragen?
