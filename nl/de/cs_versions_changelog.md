---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Änderungsprotokoll der Version
{: #changelog}

Zeigen Sie Informationen zu Versionsänderungen für Hauptversions-, Nebenversions- und Patchaktualisierungen an, die für Ihre {{site.data.keyword.containerlong}}-Kubernetes-Cluster verfügbar sind. Diese Änderungen umfassen Updates für Kubernetes und {{site.data.keyword.Bluemix_notm}} Provider-Komponenten.
{:shortdesc}

IBM wendet Patch-Level-Updates auf Ihren Master automatisch an, aber Sie müssen den Patch für [Ihre Workerknoten aktualisieren](cs_cluster_update.html#worker_node). Für den Masterknoten und die Workerknoten müssen Sie Aktualisierungen für [Haupt- und Nebenversionen](cs_versions.html#update_types) anwenden. Prüfen Sie einmal im Monat auf verfügbare Updates. Sind Aktualisierungen verfügbar, werden Sie benachrichtigt, wenn Sie Informationen zum Masterknoten und den Workerknoten in der GUI beispielsweise mit den folgenden Befehlen anzeigen: `ibmcloud ks clusters`, `cluster-get`, `workers` oder `worker-get`.

Eine Zusammenfassung der Migrationsaktionen finden Sie unter [Kubernetes-Versionen](cs_versions.html).
{: tip}

Weitere Informationen zu Änderungen seit der vorherigen Version finden Sie in den Änderungsprotokollen.
-  Version 1.10, [Änderungsprotokoll](#110_changelog).
-  Version 1.9, [Änderungsprotokoll](#19_changelog).
-  Version 1.8, [Änderungsprotokoll](#18_changelog).
-  [Archiv](#changelog_archive) der Änderungsprotokolle veralteter oder nicht unterstützter Versionen.

## Version 1.10, Änderungsprotokoll
{: #110_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für 1.10.5_1517, veröffentlicht am 27. Juli 2018
{: #1105_1517}

<table summary="Seit Version 1.10.3_1514 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1514</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 3.1.1</td>
<td>Version 3.1.3</td>
<td>Werfen Sie einen Blick auf die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.3-85</td>
<td>Version 1.10.5-118</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.5 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>IBM File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.3</td>
<td>Version 1.10.5</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.10.3_1514, veröffentlicht 3. Juli 2018
{: #1103_1514}

<table summary="Seit Version 1.10.3_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.10.3_1513, veröffentlicht 21. Juni 2018
{: #1103_1513}

<table summary="Seit Version 1.10.3_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.3_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.3_1512, veröffentlicht 12. Juni 2018
{: #1103_1512}

<table summary="Seit Version 1.10.1_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.10.1</td>
<td>Version 1.10.3</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option `--enable-admission-plugins` wurde `PodSecurityPolicy` für den Kubernetes-API-Server des Clusters hinzugefügt und das Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](cs_psp.html).</td>
</tr>
<tr>
<td>Kubelet-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Es wurde die Option `--authentication-token-webhook` aktiviert, um das Trägertoken und das Servicekontotoken der API zur Authentifizierung am HTTPS-Endpunkt `kubelet` zu unterstützen.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.10.1-52</td>
<td>Version 1.10.3-85</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10.3 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`livenessProbe` wurde der `vpn`-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich `kube-system` ausgeführt wird.</td>
</tr>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Workerknoten-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



### Änderungsprotokoll für Workerknoten-Fixpack 1.10.1_1510, veröffentlicht am 18. Mai 2018
{: #1101_1510}

<table summary="Seit Version 1.10.1_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.10.1_1509, veröffentlicht am 16. Mai 2018
{: #1101_1509}

<table summary="Seit Version 1.10.1_1508 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1508</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.1_1508, veröffentlicht am 1. Mai 2018
{: #1101_1508}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 2.6.5</td>
<td>Version 3.1.1</td>
<td>Werfen Sie einen Blick auf die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>Version 1.5.0</td>
<td>Version 1.5.2</td>
<td>Werfen Sie einen Blick auf die Kubernetes Heapster-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.7</td>
<td>Version 1.10.1</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>StorageObjectInUseProtection</code> wurde zur Option <code>--enable-admission-plugins</code> für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Werfen Sie einen Blick auf die Kubernetes-DNS-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.7-102</td>
<td>Version 1.10.1-52</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10 aktualisiert.</td>
</tr>
<tr>
<td>GPU-Unterstützung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Unterstützung für [Container-Workloads der Graphics Processing Unit (GPU)](cs_app.html#gpu_app) ist jetzt für die Terminierung und Ausführung verfügbar. Eine Liste der verfügbaren GPU-Maschinentypen finden Sie unter [Hardware für Workerknoten](cs_clusters.html#shared_dedicated_node). Weitere Informationen finden Sie in der Kubernetes-Dokumentation zum [Terminieren von GPUs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Version 1.9, Änderungsprotokoll
{: #19_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für 1.9.9_1520, veröffentlicht 27. Juli 2018
{: #199_1520}

<table summary="Seit Version 1.9.8_1517 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1517</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.8-141</td>
<td>Version 1.9.9-167</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.9 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>IBM File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.8</td>
<td>Version 1.9.9</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.8_1517, veröffentlicht am 3. Juli 2018
{: #198_1517}

<table summary="Seit Version 1.9.8_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.9.8_1516, veröffentlicht am 21. Juni 2018
{: #198_1516}

<table summary="Seit Version 1.9.8_1515 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.8_1515</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.8_1515, veröffentlicht 19. Juni 2018
{: #198_1515}

<table summary="Seit Version 1.9.7_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.7</td>
<td>Version 1.9.8</td>
<td>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option '--admission-control' wurde 'PodSecurityPolicy' für den Kubernetes API-Server des Clusters hinzugefügt und das Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](cs_psp.html).</td>
</tr>
<tr>
<td>IBM Cloud-Provider</td>
<td>Version 1.9.7-102</td>
<td>Version 1.9.8-141</td>
<td>Wurde für die Unterstützung von Kubernetes 1.9.8 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>livenessProbe</code> wurde der <code>vpn</code>-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich <code>kube-system</code> ausgeführt wird.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.9.7_1513, veröffentlicht am 11. Juni 2018
{: #197_1513}

<table summary="Seit Version 1.9.7_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Worker-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.9.7_1512, veröffentlicht am 18. Mai 2018
{: #197_1512}

<table summary="Seit Version 1.9.7_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.9.7_1511, veröffentlicht am 16. Mai 2018
{: #197_1511}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.7_1510, veröffentlicht am 30. April 2018
{: #197_1510}

<table summary="Seit Version 1.9.3_1506 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.3_1506</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.3</td>
<td>Version 1.9.7	</td>
<td><p>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Anmerkung</strong>: `secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden nun als schreibgeschützte Datenträger angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, die das System möglicherweise automatisch zurücksetzt. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`admissionregistration.k8s.io/v1alpha1=true` wurde zur Option `--runtime-config` für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.3-71</td>
<td>Version 1.9.7-102</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

## Version 1.8, Änderungsprotokoll
{: #18_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für 1.8.15_1518, veröffentlicht 27. Juli 2018
{: #1815_1518}

<table summary="Seit Version 1.8.13_1516 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1516</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.8.13-176</td>
<td>Version 1.8.15-204</td>
<td>Wurde für die Unterstützung von Kubernetes 1.8.15 aktualisiert. Darüber hinaus enthalten `create failure`-Ereignisse des LoadBalancer-Service nun alle Fehler portierbarer Teilnetze.</td>
</tr>
<tr>
<td>IBM File Storage-Plug-in</td>
<td>320</td>
<td>334</td>
<td>Das Zeitlimit bei der Erstellung persistenter Datenträger wurde von 15 auf 30 Minuten erhöht. Der Standardabrechnungstyp wurde in 'Stündlich' (`hourly`) geändert. Es wurden Mountoptionen zu den vordefinierten Speicherklassen hinzugefügt. In der NFS-Dateispeicherinstanz in Ihrem Konto der IBM Cloud-Infrastruktur (SoftLayer) wurde das Format des Felds **Anmerkungen** in JSON geändert und der Kubernetes-Namensbereich hinzugefügt, in dem der persistente Datenträger bereitgestellt ist. Für die Unterstützung von Mehrzonenclustern wurden persistenten Datenträgern Zonen- und Regionsbezeichnungen hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.8.13</td>
<td>Version 1.8.15</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>An den Netzeinstellungen für Workerknoten wurden kleinere Verbesserungen vorgenommen, um Netzarbeitslasten mit hoher Leistung zu unterstützen.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die `vpn`-Bereitstellung des OpenVPN-Clients, die im Namensbereich `kube-system` ausgeführt wird, wird nun von `addon-manager` von Kubernetes verwaltet.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten-Fixpack 1.8.13_1516, veröffentlicht am 3. Juli 2018
{: #1813_1516}

<table summary="Seit Version 1.8.13_1515 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1515</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernel</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Optimiertes `sysctl` für Netzauslastungen mit hoher Leistung.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.8.13_1515, veröffentlicht am 21. Juni 2018
{: #1813_1515}

<table summary="Seit Version 1.8.13_1514 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.13_1514</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Docker</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Bei nicht verschlüsselten Maschinentypen wird die sekundäre Platte bereinigt, indem Sie beim erneuten Laden oder Aktualisieren des Workerknotens ein neues Dateisystem abrufen.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.8.13_1514, veröffentlicht 19. Juni 2018
{: #1813_1514}

<table summary="Seit Version 1.8.11_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.8.11</td>
<td>Version 1.8.13</td>
<td>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Der Option '--admission-control' wurde 'PodSecurityPolicy' für den Kubernetes API-Server des Clusters hinzugefügt und das Cluster wurde für die Unterstützung von Sicherheitsrichtlinien für Pods konfiguriert. Weitere Informationen finden Sie unter [Sicherheitsrichtlinien für Pods konfigurieren](cs_psp.html).</td>
</tr>
<tr>
<td>IBM Cloud-Provider</td>
<td>Version 1.8.11-126</td>
<td>Version 1.8.13-176</td>
<td>Wurde für die Unterstützung von Kubernetes 1.8.13 aktualisiert.</td>
</tr>
<tr>
<td>OpenVPN-Client</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>livenessProbe</code> wurde der <code>vpn</code>-Bereitstellung des OpenVPN-Clients hinzugefügt, die im Namensbereich <code>kube-system</code> ausgeführt wird.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten-Fixpack 1.8.11_1512, veröffentlicht am 11. Juni 2018
{: #1811_1512}

<table summary="Seit Version 1.8.11_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Worker-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1511, veröffentlicht am 18. Mai 2018
{: #1811_1511}

<table summary="Seit Version 1.8.11_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1510, veröffentlicht am 16. Mai 2018
{: #1811_1510}

<table summary="Seit Version 1.8.11_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.8.11_1509, veröffentlicht am 19. April 2018
{: #1811_1509}

<table summary="Seit Version 1.8.8_1507 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.8_1507</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td><p>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Anmerkung</strong>: `secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden nun als schreibgeschützte Datenträger angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, die das System möglicherweise automatisch zurücksetzt. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<tr>
<td>Containerimage anhalten</td>
<td>3.0</td>
<td>3.1</td>
<td>Entfernt geerbte verwaiste Zombie-Prozesse.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

## Archiv
{: #changelog_archive}

### Version 1.7, Änderungsprotokoll (nicht unterstützt)
{: #17_changelog}

Überprüfen Sie die folgenden Änderungen.

#### Änderungsprotokoll für Workerknoten-Fixpack 1.7.16_1514, veröffentlicht am 11. Juni 2018
{: #1716_1514}

<table summary="Seit Version 1.7.16_1513 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1513</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kernelaktualisierung</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>Neue Worker-Images mit Kernelaktualisierung für [CVE-2018-3639 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1513, veröffentlicht am 18. Mai 2018
{: #1716_1513}

<table summary="Seit Version 1.7.16_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1512, veröffentlicht am 16. Mai 2018
{: #1716_1512}

<table summary="Seit Version 1.7.16_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für 1.7.16_1511, veröffentlicht am 19. April 2018
{: #1716_1511}

<table summary="Seit Version 1.7.4_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.4_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td><p>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</p><p><strong>Anmerkung</strong>: `secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden nun als schreibgeschützte Datenträger angehängt. Bisher konnten Apps Daten in diese Datenträger schreiben, die das System möglicherweise automatisch zurücksetzt. Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</p></td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>
