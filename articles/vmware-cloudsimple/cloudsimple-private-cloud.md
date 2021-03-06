---
title: 'Azure VMware Solutions (AVS): Private AVS-Clouds'
description: Hier erfahren Sie mehr über private AVS-Clouds und AVS-Konzepte.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2688edf281a6d8bc3d61e8e294c920f115f0f3f6
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024945"
---
# <a name="avs-private-cloud-overview"></a>Übersicht über private AVS-Clouds

AVS transformiert und erweitert VMware-Workloads in Minuten in öffentliche Clouds. Mithilfe des AVS-Diensts können Sie VMware nativ in einer Azure-Bare-Metal-Infrastruktur bereitstellen. Ihre Bereitstellung befindet sich an Azure-Standorten und ist vollständig in den Rest der Azure-Cloud integriert.

Die AVS-Lösung bietet vollständige VMware-Betriebskontinuität. Diese Lösung eröffnet Ihnen die folgenden Vorteile der öffentlichen Cloud:

* Elastizität
* Innovation
* Effizienz

Mit AVS profitieren Sie von einem Cloudnutzungsmodell, mit dem Ihre Gesamtkosten verringert werden können. CloudSimple bietet außerdem bedarfsgesteuerte Bereitstellung, nutzungsbasierte Bezahlung und Kapazitätsoptimierung.

AVS ist vollständig kompatibel mit Folgendem:

* Vorhandene Tools
* Fähigkeiten
* Prozesse

Dank dieser Kompatibilität können Ihre Teams, Workloads in der Azure-Cloud verwalten, ohne die folgenden Arten von Richtlinien zu beeinträchtigen:

* Netzwerk
* Sicherheit  
* Schutz von Daten  
* Audit

AVS verwaltet die Infrastruktur und alle erforderlichen Netzwerk- und Verwaltungsdienste. Mit dem AVS-Dienst kann sich Ihr Team auf Folgendes konzentrieren:

* Geschäftswert
* Bereitstellung von Anwendungen
* Geschäftskontinuität
* Support
* Durchsetzung von Richtlinien

## <a name="avs-private-cloud-environment-overview"></a>Übersicht über Umgebungen für private AVS-Clouds

Eine private AVS-Cloud ist ein isolierter VMware-Stapel, der Folgendes unterstützt:

* ESXi-Hosts
* vCenter
* vSAN
* NSX

Private AVS-Clouds werden über das AVS-Portal verwaltet. Sie verfügen über einen eigenen vCenter-Server in einer eigenen Verwaltungsdomäne.

Der Stapel wird auf folgenden Komponenten ausgeführt:

* Dedizierte Knoten
* Isolierte Bare-Metal-Hardwareknoten

Benutzer nutzen den Stapel über systemeigene VMware-Tools, wozu folgende Tools gehören:

* vCenter
* NSX Manager

Sie können dedizierte Knoten in Azure-Standorten bereitstellen. Anschließend können Sie diese mit Azure und AVS verwalten. Eine private AVS-Cloud besteht aus einem oder mehreren vSphere-Clustern, und jeder Cluster enthält 3 bis 16 Knoten.

Sie können eine private AVS-Cloud mit erworbenen Knoten mit nutzungsbasierter Bezahlung oder mit reservierten, dedizierten Knoten erstellen.

Sie können die private AVS-Cloud über die folgenden Verbindungen mit Ihrer lokalen Umgebung und dem Azure-Netzwerk verbinden:

* Sicher
* Privates VPN
* Azure ExpressRoute

Die Umgebung einer privaten AVS-Cloud ist so konzipiert, dass sie keinen Single Point of Failure aufweist:

* ESXi-Cluster sind mit vSphere Hochverfügbarkeit konfiguriert und in der Größe so ausgelegt, dass sie mindestens einen Ersatzknoten für Resilienz haben.
* vSAN stellt redundanten primären Speicher bereit. vSAN erfordert mindestens drei Knoten, um Schutz vor einem Ausfall eines einzelnen Knotens zu bieten. Sie können vSAN so konfigurieren, dass es eine höhere Resilienz für größere Cluster bietet.
* Sie können vCenter-, PSC- und NSX Manager-VMs mit RAID-10-Speicherrichtlinien konfigurieren, um sie vor Speicherausfällen zu schützen. vSphere-Hochverfügbarkeit schützt vor Knoten- und Netzausfällen.

## <a name="scenarios-for-deploying-an-avs-private-cloud"></a>Szenarien für die Bereitstellung einer privaten AVS-Cloud

Im Folgenden finden Sie einige Anwendungsfälle für die Bereitstellung einer privaten AVS-Cloud.

### <a name="data-center-retirement-or-migration"></a>Außerbetriebnahme oder Migration eines Rechenzentrums

* Buchen Sie zusätzliche Kapazität, wenn Sie die Grenzen Ihres vorhandenen Rechenzentrums erreichen oder Hardware aktualisieren.
* Fügen Sie erforderliche Kapazität in der Cloud hinzu, und eliminieren Sie die Probleme, die beim Verwalten von Hardwareaktualisierungen auftreten können.
* Verringern Sie das Risiko und die Kosten von Cloudmigrationen im Vergleich zu zeitaufwändigen Konvertierungen oder Umstrukturierungen.
* Verwenden Sie vertraute VMware-Tools und Fähigkeiten, um Cloudmigrationen zu beschleunigen. Verwenden Sie in der Cloud Azure-Dienste, um Ihre Anwendungen in Ihrem Tempo zu modernisieren.

### <a name="expand-on-demand"></a>Bedarfsgesteuerte Erweiterung

* Erweitern Sie in die Cloud, um unerwartete Anforderungen erfüllen zu können, z. B. neue Entwicklungsumgebungen oder saisonale Kapazitätsspitzen.
* Erstellen Sie neue Kapazität nach Bedarf, und behalten Sie diese nur so lange, wie sie benötigt wird.
* Verringern Sie Ihre Vorabinvestitionen, beschleunigen Sie die Geschwindigkeit der Bereitstellung, und verringern Sie die Komplexität mit derselben Architektur und denselben Richtlinien sowohl lokal als auch in der Cloud.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Notfallwiederherstellung und virtuelle Desktops in der Azure-Cloud

* Richten Sie Remotezugriff auf Daten, Apps und Desktops in der Azure-Cloud ein. Über Verbindungen mit hoher Bandbreite können Sie Daten schnell hoch- bzw. herunterladen, um nach Vorfällen eine Wiederherstellung vorzunehmen. Netzwerke mit niedriger Wartezeit bieten Ihnen schnelle Reaktionszeiten, die Benutzer von einer Desktop-App erwarten.

* Replizieren Sie Ihre gesamten Richtlinien und Netzwerke in der Cloud über das AVS-Portal und vertraute VMware-Tools. Die Replikation verringert den Aufwand und das Risiko beim Erstellen und Verwalten von Notfallwiederherstellungs- und VDI-Implementierungen.

### <a name="high-performance-applications-and-databases"></a>Leistungsstarke Anwendungen und Datenbanken

* Führen Sie Ihre anspruchsvollsten Workloads mit der hyperkonvergenten Architektur von AVS aus.
* Führen Sie Oracle, Microsoft SQL Server, Middleware-Systeme und leistungsstarke NoSQL-Datenbanken aus.
* Erleben Sie die Cloud als Ihr eigenes Rechenzentrum mit äußerst schnellen 25-Gbit/s-Netzwerkverbindungen. Hochgeschwindigkeitsverbindungen ermöglichen es Ihnen, hybride Apps auszuführen, die sich über lokale, VMware in Azure- und private Azure-Workloads erstrecken, ohne dass die Leistung beeinträchtigt wird.

### <a name="true-hybrid"></a>Eine echte Hybridlösung

* Vereinheitlichen Sie DevOps über VMware- und Azure-Dienste hinweg.
* Optimieren Sie die VMware-Verwaltung für Azure-Dienste und -Lösungen, die auf alle Workloads angewendet werden können.
* Greifen Sie auf Dienste für öffentliche Clouds zu, ohne Ihr Rechenzentrum erweitern oder Ihre Anwendungen neu strukturieren zu müssen.
* Zentralisieren Sie Identitäten, Zugriffssteuerungsrichtlinien, Protokollierung und Überwachung für VMware-Anwendungen in Azure.

## <a name="limits"></a>Einschränkungen

In der folgenden Tabelle sind die Knotenbeschränkungen für Ressourcen einer privaten AVS-Cloud aufgeführt:

| Resource | Begrenzung |
|----------|-------|
| Mindestanzahl von Knoten zum Erstellen einer privaten AVS-Cloud | 3 |
| Maximale Anzahl von Knoten in einem Cluster in einer privaten AVS-Cloud | 16 |
| Maximale Anzahl von Knoten in einer privaten AVS-Cloud | 64 |
| Mindestanzahl von Knoten in einem neuem Cluster | 3 |

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen einer privaten AVS-Cloud](create-private-cloud.md).
* Informieren Sie sich über das [Konfigurieren einer privaten AVS-Cloudumgebung](quickstart-create-private-cloud.md).
