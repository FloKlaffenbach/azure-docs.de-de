---
title: Wiederherstellen gelöschter Apps
description: Erfahren Sie, wie Sie eine gelöschte App in Azure App Service wiederherstellen. Bleiben Sie beim versehentlichen Löschern einer App gelassen.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: c7d778a0afca4b3552976526d58a2cb2efe12161
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75689618"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Wiederherstellen einer gelöschten App Service-App mithilfe von PowerShell

Wenn Sie Ihre App versehentlich in Azure App Service gelöscht haben, können Sie sie mithilfe der Befehle im [Azure PowerShell-Modul](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0) wiederherstellen.

> [!NOTE]
> Gelöschte Apps werden 30 Tage nach dem ersten Löschvorgang aus dem System gelöscht. Nachdem eine App gelöscht wurde, kann sie nicht wiederhergestellt werden.
>

## <a name="re-register-app-service-resource-provider"></a>Erneutes Registrieren eines App Service-Ressourcenanbieters
Einige Kunden stoßen möglicherweise auf ein Problem, bei dem das Abrufen der Liste gelöschter Apps fehlschlägt. Führen Sie zum Beheben dieses Problems den folgenden Befehl aus:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Auflisten gelöschter Apps

Zum Aufrufen der Auflistung gelöschter Apps können Sie `Get-AzDeletedWebApp` verwenden.

Für Details zu einer bestimmten gelöschten App können Sie Folgendes verwenden:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Die ausführlichen Informationen enthalten folgende Angaben:

- **DeletedSiteId**: Eindeutiger Bezeichner für die App, der für Szenarien verwendet wird, in denen mehrere Apps mit gleichem Namen gelöscht wurden
- **SubscriptionID**: Abonnement, das die gelöschte Ressource enthält
- **Standort**: Speicherort der ursprünglichen App
- **ResourceGroupName**: Name der ursprünglichen Ressourcengruppe
- **Name**: Name der ursprünglichen App
- **Slot**: Name des Slots
- **DeletionTime**: Zeitpunkt, an dem die App gelöscht wurde  

## <a name="restore-deleted-app"></a>Wiederherstellen einer gelöschten App

Wenn Sie die wiederherzustellende App identifiziert haben, können Sie sie mithilfe von `Restore-AzDeletedWebApp` wiederherstellen.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Eingaben für den Befehl:

- **Ressourcengruppe**: Zielressourcengruppe, in der die App wiederhergestellt wird
- **Name**: Name für die App, muss global eindeutig sein
- **TargetAppServicePlanName**: App Service-Plan, der mit der App verknüpft ist

Standardmäßig wird von `Restore-AzDeletedWebApp` sowohl die Konfiguration der App als auch deren Inhalt wiederhergestellt. Wenn lediglich der Inhalt wiederhergestellt werden soll, verwenden Sie das Flag `-RestoreContentOnly` mit diesem Cmdlet.

> [!NOTE]
> Falls die App in einer App Service-Umgebung gehostet und anschließend daraus gelöscht wurde, kann sie nur wiederhergestellt werden, wenn die entsprechende App Service-Umgebung noch vorhanden sind.
>

Die vollständige Cmdlet-Referenz finden Sie hier: [Restore-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
