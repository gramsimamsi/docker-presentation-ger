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
- Wie kann ich weitermachen?
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
- Beschränken Sichtbarkeit auf...
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
- Beschränken Zugriff auf...
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

## Grundbegriffe!
### Docker Engine, Images, Container

+++


## Docker Engine

@ul
- Das "Programm" Docker
- Verwaltet Docker-Zeug
- Für Windows, MacOS, Linux-Distros
- CLI für manuelles Ausführen
- Beispiel: ```sudo docker images```
@ulend

Note:

Engine = CLI + REST-API + server daemon  
-> remote config möglich!
Achtung: Auf Windows/MacOS nutzt es Hypervisor!  
Was ist ein Image?

+++

## Docker Images

@ul
- Vergleichbar mit .OVF/.OVA-Dateien
- Viele fertig verfügbar
- Jeder kann eigene erstellen oder bestehende anpassen
- mithilfe von *Dockerfiles*
- Gute Dockerfiles sparen Ressourcen!
- Aus Images werden *Container* gestartet
@ulend

Note:

Quasi Pendant zu VM-Vorlagen (nicht snapshots!)  
Repositories, später  
*Base Image* vs *From Scratch*, auch config möglich  
Dockerfiles beschreibens, mehr später  
Philosophie: Images sind vom Fleck weg minimal und "fertig"  
(Alles installed/config, kann loslegen, kein bloat)

+++

## Docker Container

@ul
- Container teilen sich Linux Kernel
- Jeder hat alles individuell:
- Code
- Programme
- Bibliotheken
- Umgebungsvariablen
- Einstellungen
- (Dateistruktur)
- ...
@ulend

Note:

Kernel-teilen mit Containern + Host wenn auf Linux  
Manches wird geteilt (optimierung, egal für Anwenden)  
Manches lässt sich explizit teilen (Ordner, Netzwerk, ...)

+++

## Docker Container continued

@ul
- Isoliertes Stück Software
- Enthält alles was es braucht
- Kann irgendwo laufen
@ulend

Note:

Software-Umgebung ist isoliert und sofort einsatzbereit  
Braucht Docker-Engine und nen Linux-Kernel (Min-Version)

+++

## Images: Woher?

@ul
- von Grund auf selbst bauen (nicht empfohlen!)
- bestehende Images nutzen und anpassen
- Öffentliches Repo: hub.docker.com
- private Repositories ($$$)
@ulend

Note:

Standardmäßig mit nem dockerhub-image beginnen und anpassen  
dockerHub hat verified status  
(poisoned-image problem)  
private repo's mit Docker EE / dockerhub subscription  
ein kostenloses priv repo pro dockerhub acc  
für dev einfach dockerfiles sharen, für prod besser private repo's  

+++

## Exkurs - Designphilosophie!

@ul
- Ein Container hat nur einen Job
- Kann nur genau so viel wie nötig
- Minimaler Schaden bei Absturz
- Kann jederzeit ersetzt werden
@ulend

Note:

Atomates Splitten -> keine Komplikationen  
Minimaler Ressourcen-Overhead  
Stateless!  
Neuen Starten ist schnell & billig
Microservice-Architektur!

+++

## Docker Compose
### Mehrere Container, eine Anwendung

+++

## Docker Compose

@ul
- mehrere Container auf einmal starten  
- Zusammenspiel konfigurieren (docker-compose.yml)
@ulend

Note:

Container mit 'depends' in richtiger reihenfolge hochfahren  
config: Mehrere Container ins selbe Netzwerk,  
Port-Mapping, ENV-VARs, ...  
alles was man händisch mit Docker CLI machen würde, hier automatisch

+++

## Orchestrierung
### jetzt wirds fancy

+++

## Orchestrierung

@ul
- maximale Verfügbarkeit
- Automatische Ressourcen-Anpassung
- Serverupdates ohne downtime
- ...
@ulend

Note:

Achtung: ich weiß hier nur konzepte, keine Praxiserfahrung!  
Verfügbarkeit über Health-Checks und automatisch neue Container  
Autoscaling über Container zu-/abschalten  
Updates und mehr durch Container rollover  
Cloud-Deployment  
...

+++

## Orchestrierung - Recherche-Hilfen

@ul
- Kubernetes (k8s) für Cluster-Management
- Rancher, Minikube für k8s-Einstieg
- Bei eigener HW: DC/OS
- Cloud-Cluster: Amazon EKS

@ulend

Note:

Es gibt Docker swarm, stirbt aber  
k8s schwer im einstieg, alles stirbt sofort  
Rancher = k8s-klicki-bunti  
minikube = locales entwickeln  
EKS = AWS-integriertes k8s  

---

## Wofür brauch ich das?

+++

Container sind **kein Ersatz** für VMs, sondern **eine Alternative**

+++

## Software-Entwicklung

@ul
- Software-Umfeld isolieren
- Systemumfeld lokal mocken
- Build-Prozesse isolieren
- **Programmversionen leicht anpassbar**
@ulend

Note:

Bspw. unterschiedliche Python-Versionen Programme  
Programm nutzt zwei unserer APIs? Lokal im Container ausführen!  
Multistage-Docker, Beispiel später  
Gold wert, vor allem bei Rollbacks/Downgrades!  
In VM-Umfeldern sehr aufwendig  

+++

## Auslieferung

@ul
- Lauffertig auslieferbar
- als Dockerfiles + docker-compose.yaml
- Weniger Datentransfer notwendig
@ulend

Note:
Keine installationsanleitung notwendig  
textdateien sind schnell geschickt  
braucht an sich schon weniger speicher  
bei updates du cache-diffs -> noch weniger!  

+++

## Betrieb

@ul
- schnellere Reaktion möglich  
- weniger downtime, **weniger Kosten**  
- vor allem in der Cloud
@ulend

Note:

Reaktion automatisiert und manuell  
sekunden statt minuten  
Weniger Server-Ressourcen  
Autoscaling -> cloud zahlt nur was gebraucht wird!  

---

## Wie kann ich das benutzen?

+++

## Demo!


Note: 

sudo docker pull ubuntu  
sudo docker images  
sudo docker run ubuntu (nix)  
sudo docker run -it ubuntu  
strg + p + q  
sudo docker ps  
sudo docker attach *id*  
sudo docker kill *id*  
sudo docker rm *id*  

+++

## Beispiel Dockerfile

```
FROM node:10.14.2

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "npm", "start" ]
```

Notes:

Caching erklären!

+++

## Beispiel docker-compose

```
version: '3' # specify docker-compose version

# Define the services/ containers to be run
services:

  frontend:
    restart: always
    build: ./frontend # specify the directory of the Dockerfile
    ports:
      - "4200:4200" # specify port mapping

  backend:
    restart: always
    build: ./backend # specify the directory of the Dockerfile
    ports:
      - "3000:3000" #specify ports mapping
    links:
      - database # link this service to the database service

  database:
    image: mongo # specify image to build container from
    restart: always
    volumes:
      - ./data:/data/db
    ports:
      - "27017:27017" # specify port forwarding
```
Note:

cd ~/workspace/thirstygames_wt/
sudo docker compose up  
codeänderung frontend  
-> nochmal! 

---

## Wie kann ich weitermachen?

+++

## Mit diesen Links!

- docker-curriculum.com
- docs.docker.com
- docs.docker.com/engine/reference/builder/
- docs.docker.com/compose
- hub.docker.com
- github.com/wsargent/docker-cheat-sheet

---

# Fragen?
