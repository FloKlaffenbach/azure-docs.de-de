---
title: Zentrales Hochskalieren von Features und Funktionen
description: Hier erfahren Sie, wie Sie eine App in Azure App Service zentral hochskalieren. Profitieren Sie von mehr CPU, Speicher, Speicherplatz und zusätzlichen Features.
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.topic: article
ms.date: 08/19/2019
ms.custom: seodec18
ms.openlocfilehash: dfd9297e80836978b8a185df3fc4659676383802
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75659898"
---
# <a name="scale-up-an-app-in-azure-app-service"></a>Zentrales Hochskalieren einer App in Azure App Service

In diesem Artikel wird die Vorgehensweise beim Skalieren Ihrer App in Azure App Service beschrieben. Es gibt zwei Workflows für die Skalierung: zentrales Hochskalieren und horizontales Hochskalieren. Dieser Artikel befasst sich mit dem Workflow zum zentralen Hochskalieren.

* [Zentrales Hochskalieren](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Bei dieser Methode erhalten Sie mehr CPUs, mehr Arbeitsspeicher und mehr Speicherplatz sowie zusätzliche Features wie dedizierte virtuelle Computer, benutzerdefinierte Domänen und Zertifikate, Stagingslots, automatische Skalierung und mehr. Zum zentralen Hochskalieren muss der Tarif des App Service-Plans geändert werden, zu dem die App gehört.
* [Horizontales Skalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Bei dieser Methode wird die Anzahl von VM-Instanzen erhöht, die Ihre App ausführen.
  Die Instanzenanzahl kann je nach Tarif auf bis zu 30 erhöht werden. [App Service-Umgebungen](environment/intro.md) im Tarif **Isolated** erhöhen die Anzahl der Instanzen beim horizontalen Hochskalieren weiter auf 100. Weitere Informationen zum horizontalen Hochskalieren finden Sie unter [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md) (Manuelles oder automatisches Skalieren der Instanzanzahl). Dort erfahren Sie, wie Sie die automatische Skalierung verwenden, d.h. das automatische Skalieren der Instanzenzahl basierend auf vordefinierten Regeln und Zeitplänen.

Diese Skalierungseinstellungen werden innerhalb von Sekunden angewendet und wirken sich auf alle Apps im [App Service-Plan](../app-service/overview-hosting-plans.md)aus.
Dazu müssen Sie weder Ihren Code ändern noch Ihre Anwendung erneut bereitstellen.

Informationen zur Preisgestaltung und zu den Funktionen der einzelnen App Service-Pläne finden Sie unter [App Service Preise](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Bevor Sie einen App Service-Plan aus dem Tarif **Free** entfernen, müssen Sie zunächst die [Ausgabenlimits](https://azure.microsoft.com/pricing/spending-limits/) für Ihr Azure-Abonnement aufheben. Informationen zum Anzeigen oder Ändern von Optionen für Ihr Microsoft Azure App Service-Abonnement finden Sie unter [Microsoft Azure-Abonnements][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Zentrales Hochskalieren Ihres Tarifs

> [!NOTE]
> Informationen zum zentralen Hochskalieren auf den Tarif **PremiumV2** finden Sie unter [Konfigurieren der PremiumV2-Ebene für App Service](app-service-configure-premium-tier.md).
>

1. Öffnen Sie das [Azure-Portal][portal] in Ihrem Browser.

1. Wählen Sie auf Seite „App Service“ der App im linken Menü **Zentral hochskalieren (App Service-Plan)** aus.
   
3. Wählen Sie Ihren Tarif, und klicken Sie auf **Übernehmen**. Wählen Sie die verschiedenen Kategorien (z. B. **Produktion**) aus, und **sehen Sie sich auch weitere Optionen an**, um weitere Tarife einzublenden.
   
    ![Navigieren Sie, um Ihre Azure-App zentral hochzuskalieren.][ChooseWHP]

    Nach Abschluss des Vorgangs sehen Sie ein Popupfenster mit einem grünen Erfolgshäkchen.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Skalieren zugehöriger Ressourcen
Wenn Ihre App von anderen Diensten abhängt, beispielsweise von Azure SQL-Datenbank oder Azure Storage, können Sie diese Ressourcen gesondert zentral hochskalieren. Diese Ressourcen werden vom App Service-Plan nicht verwaltet.

1. Klicken Sie auf der Seite **Übersicht** Ihrer App auf den Link **Ressourcengruppe**.
   
    ![Zentrales Hochskalieren der zugehörigen Ressourcen Ihrer Azure-App](./media/web-sites-scale/RGEssentialsLink.png)

2. Wählen Sie im Bereich **Zusammenfassung** der Seite **Ressourcengruppe** eine Ressource aus, die Sie skalieren möchten. Der folgende Screenshot zeigt eine SQL-Datenbank-Ressource.
   
    ![Navigieren zur Seite „Ressourcengruppe“, um Ihre Azure-App zentral hochzuskalieren](./media/web-sites-scale/ResourceGroup.png)

    Informationen zum zentralen Hochskalieren der betreffenden Ressource finden Sie in der Dokumentation zum jeweiligen Ressourcentyp. Informationen zum zentralen Hochskalieren einer einzelnen SQL-Datenbank finden Sie unter [Skalieren von Einzeldatenbankressourcen in Azure SQL-Datenbank](../sql-database/sql-database-single-database-scale.md). Informationen zum zentralen Hochskalieren einer Azure Database for MySQL-Ressource finden Sie unter [Skalieren von MySQL-Ressourcen](../mysql/concepts-pricing-tiers.md#scale-resources).

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Vergleichen von Tarifen

Ausführliche Informationen, z.B. VM-Größen für jeden Tarif, finden Sie unter [App Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service).

Eine Tabelle der Dienstbeschränkungen, Kontingente und Einschränkungen sowie unterstützte Features in jedem Tarif finden Sie unter [App Service-Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="more-resources"></a>Weitere Ressourcen

[Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md)  
[Konfigurieren des PremiumV2-Tarifs für App Service](app-service-configure-premium-tier.md)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
