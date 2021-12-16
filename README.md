# kubernetes_on_router

BSN – Lernmaterial
Cloud
-	Cloud-Typen mit Vor- / Nachteile
  1. Privat-Cloud
      - Vorteile:
        - Ressourcen sind flexibel und können nach Bedarf genutzt werden
        - Unbeschränkte Bandbreite
        - Ressourcen sind vom Speicherort unabhängig
        - Ressourcen können nach Bedarf herauf- oder hinunter skaliert werden
        - Sicher durch lokalen Standort
      -	Nachteile:
        -	Nur erreichbar durch eine lokale Verbindung
        -	Verwaltungsaufwand
        -	Wenn interne Infrastruktur ausfällt, ist die Privat-Cloud auch nicht mehr verfügbar
  2. Public-Cloud
      -	Vorteile
        - Geringe Vorlaufkosten
        - Zuverlässig
        - Skalierbarkeit und Flexibilität (Schnelle Änderung der auf- und ab Skalierung und die Ressourcen stehen meisten binnen weniger Minuten zur Verfügung)
        - (Meistens) Zugang zu fortschrittlicher Technologie
      -	Nachteile
        -	Mögliche Sicherheit Probleme
        -	Mangelnde Anpassung (Durch mehrere Mieter kann nur ein gewisser Grad an Bedürfnissen angepasst werden)
  3.	Hybrid-Cloud
        - Je nach Anwendungszweck bringt dieser Cloud-Typ verschiedene Vor- und Nachteile mit sich. Im generellen werden bei dieser Art die Vorteile der Privat- und Public-Cloud           kombiniert und können zusammen genutzt werden.
#

- CaaS (Cloud as a Service) (Pyramide starten unten mit  1. IaaS, 2. PaaS, 3. SaaS)
  1. Iaas (Infrastructure as a Service)
    -	Microsoft Azure
    -	Amazon Web Service
    -	Google Cloud
  2. PaaS (Platform as a Service)
    -	OpenShift
    -	OpenTelekom Cloud
    -	IBN Cloud
  3. SaaS (Software as a Service)
    -	Drop Box
    -	Google G Suit
    -	Box

-	Kubernetes
  -	Architektur
 ![image](https://user-images.githubusercontent.com/93722657/146398330-2150cce0-e500-40f9-8706-5d595c0fabbe.png)

 

API-Server (Programschnittstelle):  
-	Hauptkomponente, die APIs für alle anderen Hauptkomponenten bereitstellt 
-	Eine API (Application Programming Interface) ist ein Satz von Befehlen, Funktionen, Protokollen und Objekten, die Programmierer verwenden können, um eine Software zu erstellen oder mit einem externen System zu interagieren. 
-	API: Ist der Code, welcher den Zugang zum Server regelt und die Kommunikation ermöglicht. In unserem Fall Kubectl oder das Dashboard 

Controller-manager: 
-	verantwortlich für die Knotenverwaltung (Erkennung des Ausfalls eines Knotens), die Pod-Replikation und die Erstellung von Endpunkten 
 
Scheduler: 
-	Der Scheduler entscheidet welche Pods in welche Nodes gepackt werden. 
 
Etcd: 
-	Key/Value-Speicher für interne Clusterdaten 
-	Key/Value = Ein Schlüssel hat einen bestimmten Wert. 
 
Kubelet: 
-	Verarbeitet jegliche Kommunikation zwischen Master und Node.  
-	Kubelet ist auf jedem Knotenserver vorhanden und diese sind mit dem Master verbunden 
-	Er stellt sicher, dass Container in einem Pod ausgeführt werden. 
-	Die PodSpecs werden mit YAML oder JSON Dateien dargestellt. 
-	Der Primärknoten kann die Knoten im API-Server registrieren via Hostnamen oder speziellen Logiken. 
 
cAdvisor: 
-	Analysiert den Ressourcenverbrauch und Performance-Charakteristiken von laufenden Containern 
-	Bietet Monitoring-Features 
 
Pod: 
-	Gruppe von einem oder mehreren Containern 
-	Hauptkomponente fürs Deployment 
-	Alle Container teilen sich die IP-Adresse des Pods 
 
 
Netzwerk: 
-	Managed das Netzwerk 
-	Flannel: 
-	Flannel, ein von CoreOS entwickeltes Projekt, ist vielleicht das einfachste und beliebteste CNI-Plugin (Containernetworking-Plugin) auf dem Markt. Es ist eines der ausgereiftesten Beispiele für eine Networking Fabric für Container-Orchestrierungssysteme, die eine bessere Vernetzung zwischen Containern und Hosts ermöglichen soll. Als sich das CNI-Konzept durchsetzte, war ein CNI-Plugin für Flannel ein früher Einstieg. 
 


Kube-Proxy: 
-	Kümmert sich um Netzwerkeinstellungen und um die Kommunikation zwischen Pods, Nodes und externen Netzwerken. 
-	Die Nutzer können über den Kube-Proxy auf die Pods zugreifen. 
-	Kube-Proxys können über verschiedene Port-Forwarding Protokolle erreicht werden 
  -	TCP [Transmission Control Protocol] 
  -	UDP [User Datagram Protocol] 
  -	SCTP [Stream Control Transmission Protocol] 


 
Nutzer: 
1. User: 
   - Hat nur Zugriff auf die Pods und keinen Zugriff auf die Nodes 
2. Admin: 
   - Hat Zugriff auf sämtliche Pods und Nodes 
 

# Vernetzung
- Cluster IP: Eine virtuelle Cluster-IP-Adresse ist eine gemeinsame IP-Adresse für alle Knoten im Cluster. Es wird verwendet, um den gesamten allgemeinen Netzwerkverkehr vom oder zum Cluster zu verarbeiten. Cluster IP gibt dir ein Service in deinem Cluster welches andere App in deinem Cluster zugreifen können (nur innerhalb des Clusters ansprechbar). 
  
 
- Node Port: Macht den Dienst auf der IP jedes Nodes an einem statischen Port (dem NodePort) verfügbar. Ein Cluster IP-Dienst, an den der Node Port-Dienst weiterleitet, wird automatisch erstellt. Ein NodePort Service ist das höchst primitivste Weg, um einen Externen Verkehr direkt zu deinem Service zu einzufügen. Nobelort, so wie der Name es schon sagt, öffnet einen speziellen Port auf allen Nodes (den VMs), und jeder Verkehr welches an diesem Port sein soll wird weitergeleitet zum Service.  
  
 
- Load Balancer: Ein Load-Balancer ist ein Gerät, das als Reverse-Proxy fungiert und den Netzwerk- oder Anwendungsdatenverkehr auf eine Anzahl von Servern verteilt. Load-Balancer werden eingesetzt, um die Kapazität (für gleichzeitige Benutzer) und Zuverlässigkeit von Anwendungen zu erhöhen. Node Port- und Cluster IP-Dienste, an die der externe Load Balancer weiterleitet, werden automatisch erstellt. 
  
- Ingress / Ingress Controller: ist eine Brücke, die zur Steuerung interner und externer Kommunikation dient. 

- Kubernetes Ingress ist eine Sammlung von Routingregeln, die steuern, wie externe Benutzer auf Dienste zugreifen, die in einem Kubernetes-Cluster ausgeführt werden. Der Datenverkehr wird durch Regel verwaltet. Diese Regel können das Protokoll bestimmen, also z.B. Http oder Https. Der Controller muss manuell gestartet werden. Für die Produktivumgebung ist es sinnvoll Ingress oder auch Load Balancer zu benutzen. 
 


