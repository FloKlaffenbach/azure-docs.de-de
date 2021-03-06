---
title: Konfigurieren von Verfügbarkeitsgruppenlistenern und einer Load Balancer-Instanz (Azure-Portal)
description: Eine ausführliche Anleitung zum Erstellen eines Listeners für eine AlwaysOn-Verfügbarkeitsgruppe für SQL Server auf virtuellen Azure-Computern
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/16/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: aefd7a55090da7f55404d6f551ab61268582ff5a
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74039648"
---
# <a name="configure-a-load-balancer-for-an-availability-group-on-azure-sql-server-vms"></a>Konfigurieren einer Load Balancer-Instanz für eine Verfügbarkeitsgruppe in Azure auf Azure SQL Server-VMs
In diesem Artikel erfahren Sie, wie Sie einen Load Balancer für eine SQL Server-AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern erstellen, auf denen Azure Resource Manager ausgeführt wird. Eine Verfügbarkeitsgruppe benötigt einen Load Balancer, wenn sich die SQL Server-Instanzen auf virtuellen Azure-Computern befinden. Der Load-Balancer speichert die IP-Adresse für den Verfügbarkeitsgruppenlistener. Wenn sich eine Verfügbarkeitsgruppe über mehrere Regionen erstreckt, benötigt jede Region einen Load Balancer.

Für diese Aufgabe benötigen Sie eine SQL Server-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern mit Resource Manager bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die Verfügbarkeitsgruppe in Resource Manager automatisch erstellen. Diese Vorlage nimmt Ihnen die Erstellung eines internen Load Balancers ab. 

Alternativ können Sie aber auch eine [Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

Dieser Artikel setzt voraus, dass Ihre Verfügbarkeitsgruppen bereits konfiguriert sind.  

Verwandte Themen:

* [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen auf einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Im Rahmen dieses Artikels erstellen und konfigurieren Sie mithilfe des Azure-Portals einen Load Balancer. Nach Abschluss des Verfahrens konfigurieren Sie den Cluster so, dass er die IP-Adresse aus dem Load Balancer für den Verfügbarkeitsgruppenlistener verwendet.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Erstellen und Konfigurieren des Load Balancers im Azure-Portal
Gehen Sie in diesem Teil der Aufgabe folgendermaßen vor:

1. Erstellen Sie im Azure-Portal den Load Balancer, und konfigurieren Sie die IP-Adresse.
2. Konfigurieren Sie den Back-End-Pool.
3. Erstellen Sie den Test. 
4. Legen Sie die Lastenausgleichsregeln fest.

> [!NOTE]
> Wenn sich die SQL Server-Instanzen in mehreren Ressourcengruppen und Regionen befinden, führen Sie jeden Schritt zweimal durch: einmal in jeder Ressourcengruppe.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>Schritt 1: Erstellen des Load Balancers und Konfigurieren der IP-Adresse
Erstellen Sie zuerst den Load Balancer. 

1. Öffnen Sie im Azure-Portal die Ressourcengruppe mit den virtuellen SQL Server-Computern. 

2. Klicken Sie in der Ressourcengruppe auf **Hinzufügen**.

3. Suchen Sie nach **Load Balancer**, und wählen Sie dann in den Suchergebnissen den **Load Balancer** aus, der von **Microsoft** veröffentlicht wurde.

4. Klicken Sie auf dem Blatt **Load Balancer** auf **Erstellen**.

5. Konfigurieren Sie den Load Balancer unter **Lastenausgleich erstellen** wie folgt:

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für den Load Balancer. Beispiel: **sqlLB**. |
   | **Typ** |**Intern:** Die meisten Implementierungen verwenden einen internen Lastenausgleich, wodurch Anwendungen innerhalb des gleichen virtuellen Netzwerks eine Verbindung mit der Verfügbarkeitsgruppe herstellen können.  </br> **Extern:** Dieser Typ ermöglicht es Anwendungen, über eine öffentliche Internetverbindung eine Verbindung mit der Verfügbarkeitsgruppe herzustellen. |
   | **Virtuelles Netzwerk** |Wählen Sie das virtuelle Netzwerk aus, in dem sich die SQL Server-Instanzen befinden. |
   | **Subnetz** |Wählen Sie das Subnetz aus, in dem sich die SQL Server-Instanzen befinden. |
   | **IP-Adresszuweisung** |**Statisch** |
   | **Private IP-Adresse** |Geben Sie eine verfügbare IP-Adresse aus dem Subnetz an. Verwenden Sie diese IP-Adresse beim Erstellen eines Listeners im Cluster. Die Adresse wird später in einem PowerShell-Skript für die Variable `$ILBIP` verwendet. |
   | **Abonnement** |Dieses Feld wird unter Umständen angezeigt, wenn Sie über mehrere Abonnements verfügen. Wählen Sie das Abonnement aus, mit dem diese Ressource verknüpft werden soll. Hierbei handelt es sich üblicherweise um das gleiche Abonnement wie bei allen Ressourcen für die Verfügbarkeitsgruppe. |
   | **Ressourcengruppe** |Wählen Sie die Ressourcengruppe aus, in der sich die SQL Server-Instanzen befinden. |
   | **Location** |Wählen Sie die Azure-Region aus, in der sich die SQL Server-Instanzen befinden. |

6. Klicken Sie auf **Create**. 

Azure erstellt den Load Balancer. Der Load Balancer gehört zu einem bestimmten Netzwerk, Subnetz, einer bestimmten Ressourcengruppe und einem bestimmten Standort. Überprüfen Sie nach Abschluss des Vorgangs die Einstellungen für den Load Balancer in Azure. 

### <a name="step-2-configure-the-back-end-pool"></a>Schritt 2: Konfigurieren des Back-End-Pools
Im Kontext von Azure wird der Back-End-Adresspool als *Back-End-Pool* bezeichnet. Im vorliegenden Fall enthält der Back-End-Pool die Adressen der beiden SQL Server-Instanzen in Ihrer Verfügbarkeitsgruppe. 

1. Klicken Sie in Ihrer Ressourcengruppe auf den zuvor erstellten Load Balancer. 

2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools**.

3. Klicken Sie im Bereich **Back-End-Pools** auf **Hinzufügen**, um einen Back-End-Adresspool zu erstellen. 

4. Geben Sie im Bereich **Back-End-Pool hinzufügen** unter **Name** einen Namen für den Back-End-Pool ein.

5. Klicken Sie unter **Virtuelle Computer** auf **Virtuellen Computer hinzufügen**. 

6. Klicken Sie unter **Virtuelle Computer auswählen** auf **Verfügbarkeitsgruppe auswählen**, und geben Sie die Verfügbarkeitsgruppe an, der die virtuellen SQL Server-Computer angehören.

7. Nachdem Sie die Verfügbarkeitsgruppe ausgewählt haben, klicken Sie auf **Wählen Sie die virtuellen Computer aus**, wählen Sie die beiden virtuellen Computer aus, auf denen die SQL Server-Instanzen in der Verfügbarkeitsgruppe gehostet werden, und klicken Sie dann auf **Auswählen**. 

8. Klicken Sie auf **OK**, um die Blätter zum **Auswählen der virtuellen Computer** bzw. zum **Hinzufügen des Back-End-Pools** zu schließen. 

Azure aktualisiert die Einstellungen für den Back-End-Adresspool. Die Verfügbarkeitsgruppe verfügt nun über einen Pool mit zwei SQL Server-Instanzen.

### <a name="step-3-create-a-probe"></a>Schritt 3: Erstellen eines Tests
Mit dem Test wird definiert, wie Azure überprüft, welche SQL Server-Instanz gerade für den Verfügbarkeitsgruppenlistener zuständig ist. Azure testet den Dienst auf der Grundlage der IP-Adresse an einem Port, den Sie beim Erstellen des Tests definieren.

1. Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Integritätstests**. 

2. Klicken Sie auf dem Blatt **Integritätstests** auf **Hinzufügen**.

3. Konfigurieren Sie den Test auf dem Blatt **Test hinzufügen** . Verwenden Sie dabei die folgenden Werte:

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für den Test. Beispiel: **SQLAlwaysOnEndPointProbe**. |
   | **Protokoll** |**TCP** |
   | **Port** |Sie können jeden verfügbaren Port verwenden. Beispiel: *59999*. |
   | **Intervall** |*5* |
   | **Fehlerhafter Schwellenwert** |*2* |

4.  Klicken Sie auf **OK**. 

> [!NOTE]
> Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server-Instanzen geöffnet ist. Beide Instanzen benötigen eine eingehende Regel für den verwendeten TCP-Port. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](https://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure erstellt den Test und ermittelt dann mit seiner Hilfe, welche SQL Server-Instanz über den Listener für die Verfügbarkeitsgruppe verfügt.

### <a name="step-4-set-the-load-balancing-rules"></a>Schritt 4: Festlegen der Lastenausgleichsregeln
Mit den Lastenausgleichsregeln wird konfiguriert, wie der Load Balancer Datenverkehr an die SQL Server-Instanzen weiterleitet. Für diesen Load Balancer aktivieren Sie Direct Server Return, da immer nur eine der beiden SQL Server-Instanzen für die Verfügbarkeitsgruppenlistener-Ressource zuständig ist.

1. Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Load balancing rules** (Lastenausgleichsregeln). 

2. Klicken Sie auf dem Blatt **Load balancing rules** (Lastenausgleichsregeln) auf **Hinzufügen**.

3. Konfigurieren Sie auf dem Blatt **Lastenausgleichsregeln hinzufügen** die Lastenausgleichsregeln. Verwenden Sie folgende Einstellungen: 

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für die Lastenausgleichsregeln. Beispiel: **SQLAlwaysOnEndPointListener**. |
   | **Protokoll** |**TCP** |
   | **Port** |*1433* |
   | **Back-End-Port** |*1433*. Dieser Wert wird ignoriert, da diese Regel **Floating IP (Direct Server Return)** verwendet. |
   | **Test** |Verwenden Sie den Namen des Tests, den Sie für diesen Load Balancer erstellt haben. |
   | **Sitzungspersistenz** |**Keine** |
   | **Leerlaufzeitüberschreitung (Minuten)** |*4* |
   | **Floating IP (Direct Server Return)** |**Aktiviert** |

   > [!NOTE]
   > Möglicherweise müssen Sie auf dem Blatt nach unten scrollen, um alle Einstellungen anzuzeigen.
   > 

4. Klicken Sie auf **OK**. 
5. Azure konfiguriert die Lastenausgleichsregel. Damit ist der Load Balancer dafür konfiguriert, Datenverkehr an die SQL Server-Instanz weiterzuleiten, die den Listener für die Verfügbarkeitsgruppe hostet. 

Die Ressourcengruppe verfügt nun also über einen mit beiden SQL Server-Computern verbundenen Load Balancer. Außerdem verfügt der Load Balancer über eine IP-Adresse für den SQL Server-AlwaysOn-Verfügbarkeitsgruppenlistener, sodass beide Computer auf Anforderungen an die Verfügbarkeitsgruppen reagieren können.

> [!NOTE]
> Falls sich Ihre SQL Server-Instanzen in unterschiedlichen Regionen befinden, wiederholen Sie die Schritte in der anderen Region. Jede Region benötigt einen Load Balancer. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren des Clusters, von dem die IP-Adresse des Load Balancers verwendet werden soll
Als Nächstes wird der Listener für den Cluster konfiguriert und online geschaltet. Gehen Sie wie folgt vor: 

1. Erstellen Sie den Verfügbarkeitsgruppenlistener für den Failovercluster. 

2. Schalten Sie den Listener online.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>Schritt 5: Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster
In diesem Schritt erstellen Sie manuell den Verfügbarkeitsgruppenlistener im Failovercluster-Manager und in SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Überprüfen der Konfiguration des Listeners

Wenn die Clusterressourcen und Abhängigkeiten ordnungsgemäß konfiguriert sind, sollten Sie den Listener in SQL Server Management Studio-Umgebungen anzeigen können. Gehen Sie folgendermaßen vor, um den Listenerport festzulegen:

1. Starten Sie SQL Server Management Studio, und stellen Sie dann eine Verbindung mit dem primären Replikat her.

2. Navigieren Sie zu **Hochverfügbarkeit mit AlwaysOn** > **Verfügbarkeitsgruppen** > **Verfügbarkeitsgruppenlistener**.  
    Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. 

3. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie dann auf **Eigenschaften**.

4. Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für $EndpointPort (Standardwert: 1433). Klicken Sie anschließend auf **OK**.

Sie verfügen nun über eine Verfügbarkeitsgruppe auf virtuellen Azure-Computern im Resource Manager-Modus. 

## <a name="test-the-connection-to-the-listener"></a>Testen der Verbindung mit dem Listener
Testen Sie die Verbindung folgendermaßen:

1. Stellen Sie eine RDP-Verbindung mit einer SQL Server-Instanz her, die sich im selben virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Hierbei kann es sich um die andere SQL Server-Instanz im Cluster handeln.

2. Testen Sie die Verbindung mithilfe des **sqlcmd** -Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd** -Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:
   
        sqlcmd -S <listenerName> -E

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Erstellen einer IP-Adresse für eine zusätzliche Verfügbarkeitsgruppe

Jede Verfügbarkeitsgruppe verwendet einen separaten Listener. Jeder Listener ist mit einer eigenen IP-Adresse ausgestattet. Verwenden Sie das gleiche Lastenausgleichsmodul, um die IP-Adresse für zusätzliche Listener zu speichern. Nachdem Sie eine Verfügbarkeitsgruppe erstellt haben, fügen Sie die IP-Adresse dem Load Balancer hinzu, und konfigurieren Sie den Listener.

Um einem Load Balancer über das Azure-Portal eine IP-Adresse hinzuzufügen, gehen Sie folgendermaßen vor:

1. Öffnen Sie im Azure-Portal die Ressourcengruppe, die den Load Balancer enthält, und klicken Sie auf den Load Balancer. 

2. Klicken Sie unter **EINSTELLUNGEN** auf **Front-End-IP-Pool**, und klicken Sie dann auf **Hinzufügen**. 

3. Vergeben Sie unter **Front-End-IP-Adresse hinzufügen** einen Namen für das Front-End. 

4. Stellen Sie sicher, dass das **Virtuelle Netzwerk** und das **Subnetz** mit denen der SQL Server-Instanzen identisch sind.

5. Legen Sie die IP-Adresse für den Listener fest. 
   
   >[!TIP]
   >Sie können die IP-Adresse auf statisch festlegen und eine Adresse eingeben, die derzeit nicht im Subnetz verwendet wird. Alternativ können Sie die IP-Adresse als dynamisch festlegen und den neuen Front-End-IP-Pool speichern. Hierdurch weist das Azure-Portal dem Pool automatisch eine verfügbare IP-Adresse zu. Sie können dann den Front-End-IP-Pool erneut öffnen und die Zuweisung in statisch ändern. 

6. Speichern Sie die IP-Adresse für den Listener. 

7. Fügen Sie einen Integritätstest mit den folgenden Einstellungen hinzu:

   |Einstellung |Wert
   |:-----|:----
   |**Name** |Ein Name zur Identifizierung des Tests.
   |**Protokoll** |TCP
   |**Port** |Ein nicht verwendeter TCP-Port, der auf allen virtuellen Computern verfügbar sein muss. Er kann nicht für andere Zwecke verwendet werden. Nur jeweils ein Listener kann einen Testport verwenden. 
   |**Intervall** |Der Zeitraum zwischen Testversuchen. Verwenden Sie den Standardwert (5).
   |**Fehlerhafter Schwellenwert** |Die Anzahl der aufeinanderfolgenden Vorgänge, bei denen Schwellenwerte fehlschlagen sollten, bevor eine virtuelle Maschine als fehlerhaft eingestuft wird.

8. Klicken Sie zum Speichern des Tests auf **OK**. 

9. Erstellen Sie eine Lastenausgleichsregel. Klicken Sie auf **Lastenausgleichsregeln** und anschließend auf **Hinzufügen**.

10. Konfigurieren Sie die neue Lastenausgleichsregel mit den folgenden Einstellungen:

    |Einstellung |Wert
    |:-----|:----
    |**Name** |Ein Name zur Identifizierung der Lastenausgleichsregel. 
    |**Frontend IP address** (Front-End-IP-Adresse) |Wählen Sie die erstellte IP-Adresse aus. 
    |**Protokoll** |TCP
    |**Port** |Verwenden Sie den Port, den die SQL Server-Instanzen verwenden. Eine Standardinstanz verwendet Port 1433, wenn Sie dies dahingehend geändert haben. 
    |**Back-End-Port** |Verwenden Sie denselben Wert wie **Port**.
    |**Back-End-Pool** |Der Pool, der die virtuellen Computer mit den SQL Server-Instanzen enthält. 
    |**Integritätstest** |Wählen Sie den Test aus, den Sie erstellt haben.
    |**Sitzungspersistenz** |Keine
    |**Leerlaufzeitüberschreitung (Minuten)** |Standard (4)
    |**Floating IP (Direct Server Return)** | Enabled

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>Konfigurieren der Verfügbarkeitsgruppe zur Verwendung der neuen IP-Adresse

Um die Konfiguration des Clusters abzuschließen, wiederholen Sie die Schritte, die Sie bei der ersten Verfügbarkeitsgruppe ausgeführt haben. Konfigurieren Sie daher den [Cluster für die Verwendung der neuen IP-Adresse](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Nachdem Sie eine IP-Adresse für den Listener hinzugefügt haben, konfigurieren Sie die zusätzliche Verfügbarkeitsgruppe folgendermaßen: 

1. Vergewissern Sie sich, dass der Testport für die neue IP-Adresse auf beiden virtuellen SQL Server-Computern geöffnet ist. 

2. [Fügen Sie im Cluster-Manager den Clientzugriffspunkt hinzu](#addcap).

3. [Konfigurieren Sie die IP-Ressource für die Verfügbarkeitsgruppe](#congroup).

   >[!IMPORTANT]
   >Verwenden Sie bei der Erstellung der IP-Adresse die IP-Adresse, die Sie dem Lastenausgleichsmodul hinzugefügt haben.  

4. [Richten Sie die Abhängigkeit der SQL Server-Verfügbarkeitsgruppenressource vom Clientzugriffspunkt ein](#dependencyGroup).

5. [Legen Sie fest, dass die Clientzugriffspunkt-Ressource von der IP-Adresse abhängig ist](#listname).
 
6. [Legen Sie die Clusterparameter in PowerShell fest](#setparam).

Konfigurieren Sie nach der Konfiguration der Verfügbarkeitsgruppe für die Verwendung der neuen IP-Adresse die Verbindung für den Listener. 

## <a name="add-load-balancing-rule-for-distributed-availability-group"></a>Hinzufügen einer Lastenausgleichsregel für eine verteilte Verfügbarkeitsgruppe

Wenn eine Verfügbarkeitsgruppe an einer verteilten Verfügbarkeitsgruppe beteiligt ist, benötigt der Lastenausgleich eine zusätzliche Regel. In dieser Regel wird der vom Listener der verteilten Verfügbarkeitsgruppe verwendete Port gespeichert.

>[!IMPORTANT]
>Dieser Schritt ist nur erforderlich, wenn die Verfügbarkeitsgruppe an einer [verteilten Verfügbarkeitsgruppe](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups) beteiligt ist. 

1. Erstellen Sie auf jedem Server, der an der verteilten Verfügbarkeitsgruppe beteiligt ist, eine eingehende Regel für den Listener-TCP-Port der verteilten Verfügbarkeitsgruppe. In vielen Beispielen in der Dokumentation wird 5022 verwendet. 

1. Klicken Sie im Azure-Portal auf den Lastenausgleich, klicken Sie auf **Lastenausgleichsregeln**, und klicken Sie dann auf **+ Hinzufügen**. 

1. Erstellen Sie die Lastenausgleichsregel mit den folgenden Einstellungen:

   |Einstellung |Wert
   |:-----|:----
   |**Name** |Name zum Identifizieren der Lastenausgleichsregel für die verteilte Verfügbarkeitsgruppe 
   |**Frontend IP address** (Front-End-IP-Adresse) |Verwenden Sie dieselbe Front-End-IP-Adresse wie die Verfügbarkeitsgruppe.
   |**Protokoll** |TCP
   |**Port** |5022 – der Port für den [Endpunktlistener der verteilten Verfügbarkeitsgruppe](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups).</br> Es kann sich um einen beliebigen verfügbaren Port handeln.  
   |**Back-End-Port** | 5022 – verwenden Sie denselben Wert wie bei **Port**.
   |**Back-End-Pool** |Der Pool, der die virtuellen Computer mit den SQL Server-Instanzen enthält. 
   |**Integritätstest** |Wählen Sie den Test aus, den Sie erstellt haben.
   |**Sitzungspersistenz** |Keine
   |**Leerlaufzeitüberschreitung (Minuten)** |Standard (4)
   |**Floating IP (Direct Server Return)** | Enabled

Wiederholen Sie diese Schritte für den Lastenausgleich in den anderen Verfügbarkeitsgruppen, die Teil der verteilten Verfügbarkeitsgruppe sind.

Wenn Sie den Zugriff mit einer Azure-Netzwerksicherheitsgruppe einschränken, stellen Sie sicher, dass die Zulassungsregeln die IP-Adressen des virtuellen SQL Server-Back-End-Computers, die Floating IP-Adressen des Lastenausgleichs für den AG-Listener und die IP-Adresse der Hauptressourcen des Clusters (falls zutreffend) umfassen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren einer SQL Server-AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern in verschiedenen Regionen](virtual-machines-windows-portal-sql-availability-group-dr.md)
