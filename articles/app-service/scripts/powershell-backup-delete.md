---
title: 'PowerShell: Löschen eines App-Backups'
description: Hier erfahren Sie, wie Sie mit Azure PowerShell die Bereitstellung und Verwaltung von App Service automatisieren. In diesem Beispiel wird gezeigt, wie Sie eine App-Sicherung löschen.
author: msangapu-msft
tags: azure-service-management
ms.assetid: ebcadb49-755d-4202-a5eb-f211827a9168
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: b8d3175b4266e132d94f6f2dde163cb60fb11396
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74685797"
---
# <a name="delete-a-backup-for-a-web-using-azure-powershell"></a>Löschen einer Sicherung für eine Web-App mit Azure PowerShell

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und anschließend eine einmalige Sicherung für sie. 

Um dieses Skript auszuführen, benötigen Sie eine vorhandene Sicherung für eine Web-App. Wie Sie eine Sicherung erstellen, erfahren Sie unter [Backup up a web app](powershell-backup-onetime.md) (Sichern einer Web-app) oder [Create a scheduled backup for a web app](powershell-backup-scheduled.md) (Erstellen einer geplanten Sicherung für eine Web-App).

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-delete/backup-delete.ps1?highlight=1-2,11 "Delete a backup for a web app")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Ruft eine Liste der Sicherungen für eine Web-App ab. |
| [Remove-AzWebAppBackup](/powershell/module/az.websites/remove-azwebappbackup) | Entfernt die angegebene Sicherung einer Web-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
