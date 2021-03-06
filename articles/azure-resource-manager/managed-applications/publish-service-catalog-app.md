---
title: Veröffentlichen einer verwalteten Dienstkatalog-App
description: Erfahren Sie, wie Sie eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist.
author: tfitzmac
ms.topic: tutorial
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: e756617a700d258078e84a3fa11c8aceb6f4dd88
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903262"
---
# <a name="create-and-publish-a-managed-application-definition"></a>Erstellen und Veröffentlichen einer Definition für die verwaltete Anwendung

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Sie können [verwaltete Azure-Anwendungen](overview.md) erstellen und veröffentlichen, die für Mitglieder Ihrer Organisation vorgesehen sind. Eine IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Standards der Organisation erfüllen. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Um eine verwaltete Anwendung im Azure-Dienstkatalog zu veröffentlichen, gehen Sie folgendermaßen vor:

* Erstellen Sie eine Vorlage, die definiert, welche Ressourcen mit der verwalteten Anwendung bereitgestellt werden.
* Definieren Sie die Elemente der Benutzeroberfläche für das Portal, wenn Sie die verwaltete Anwendung bereitstellen.
* Erstellen Sie ein ZIP-Paket, das die erforderlichen Vorlagendateien enthält.
* Entscheiden Sie, welche Benutzer, Gruppen oder Anwendungen Zugriff auf die Ressourcengruppe im Benutzerabonnement benötigen.
* Erstellen Sie die verwaltete Anwendungsdefinition, die auf das ZIP-Paket verweist und Zugriff auf die Identität anfordert.

Für diesen Artikel verfügt die verwaltete Anwendung nur über ein Speicherkonto. Es soll die Schritte zum Veröffentlichen einer verwalteten Anwendung veranschaulichen. Vollständige Beispiele finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).

Für die PowerShell-Beispiele in diesem Artikel wird mindestens die Azure PowerShell-Version 6.2 benötigt. [Aktualisieren Sie Ihre Version](/powershell/azure/install-Az-ps), sofern erforderlich.

## <a name="create-the-resource-template"></a>Erstellen der Ressourcenvorlage

Jede Definition einer verwalteten Anwendung enthält eine Datei namens **mainTemplate.json**. Darin definieren Sie die bereitzustellenden Azure-Ressourcen. Die Vorlage unterscheidet sich nicht von anderen regulären Resource Manager-Vorlagen.

Erstellen Sie eine Datei mit dem Namen **mainTemplate.json**. Beim Namen wird die Groß- und Kleinschreibung berücksichtigt.

Fügen Sie Ihrer Datei den folgenden JSON-Code hinzu. Er definiert die Parameter für das Erstellen eines Speicherkontos und legt die Eigenschaften des Speicherkontos fest.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

Speichern Sie die Datei „mainTemplate.json“.

## <a name="defining-your-create-experience-using-createuidefinitionjson"></a>Definieren Ihrer Erstellungsoberfläche mithilfe von „CreateUiDefinition.json“

Als Herausgeber definieren Sie Ihre Erstellungsoberfläche mithilfe der Datei **createUiDefinition.json**. Mit dieser Datei wird die Oberfläche für Benutzer generiert, die verwaltete Anwendungen erstellen. Sie definieren mithilfe von [Steuerelementen](create-uidefinition-elements.md) (etwa Dropdownmenüs, Textfeldern und Kennwortfeldern), wie Benutzer Werte für die einzelnen Parameter eingeben.

Erstellen Sie eine Datei mit dem Namen **createUiDefinition.json**. (Bei diesem Namen muss die Groß-/Kleinschreibung beachtet werden.)

Fügen Sie der Datei den folgenden JSON-Startercode hinzu, und speichern Sie sie:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Weitere Informationen finden Sie unter [Erstellen einer Benutzeroberfläche im Azure-Portal für die verwaltete Anwendung](create-uidefinition-overview.md).

## <a name="package-the-files"></a>Erstellen eines Pakets aus den Dateien

Fügen Sie die beiden Dateien einer ZIP-Datei mit dem Namen „app.zip“ hinzu. Die zwei Dateien müssen sich auf der Stammebene der ZIP-Datei befinden. Wenn Sie sie in einem Ordner speichern, erhalten Sie beim Erstellen der Definition der verwalteten Anwendung eine Fehlermeldung mit dem Hinweis, dass die erforderlichen Dateien nicht vorhanden sind. 

Laden Sie das Paket an einen zugänglichen Speicherort hoch, wo es verwendet werden kann. 

```powershell
New-AzResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Erstellen einer Azure Active Directory-Benutzergruppe oder -Anwendung

Der zweite Schritt besteht darin, eine Benutzergruppe oder Anwendung für das Verwalten der Ressourcen für den Kunden zu erstellen. Diese Benutzergruppe oder Anwendung verfügt über Berechtigungen für die verwaltete Ressourcengruppe, die durch die zugewiesene Rolle festgelegt sind. Es kann sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle (Role-Based Access Control, RBAC) wie „Besitzer“ oder „Mitwirkender“ handeln. Sie können einem einzelnen Benutzer auch Berechtigungen zum Verwalten der Ressourcen zuweisen, in der Regel weisen Sie diese Berechtigungen jedoch einer Benutzergruppe zu. Weitere Informationen zum Erstellen einer neuen Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Sie benötigen zum Verwalten der Ressourcen die Objekt-ID der Benutzergruppe. 

```powershell
$groupID=(Get-AzADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>Abrufen der Rollendefinitions-ID

Als Nächstes benötigen Sie die Rollendefinitions-ID der integrierten RBAC-Rolle, der Sie Zugriff auf den Benutzer, die Benutzergruppe oder Anwendung gewähren möchten. In der Regel verwenden Sie die Rolle „Besitzer, „Mitwirkender“ oder „Leser“. Der folgende Befehl zeigt, wie die Rollendefinitions-ID für die Rolle „Besitzer“ abgerufen wird:

```powershell
$ownerID=(Get-AzRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Erstellen der Definition für die verwaltete Anwendung

Falls Sie noch nicht über eine Ressourcengruppe zum Speichern der Definition Ihrer verwalteten Anwendung verfügen, erstellen Sie jetzt eine:

```powershell
New-AzResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Erstellen Sie nun die Definitionsressource für die verwaltete Anwendung.

```powershell
$blob = Get-AzStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="bring-your-own-storage-for-the-managed-application-definition"></a>Bereitstellen eines eigenen Speichers (Bring Your Own Storage) für die Definition der verwalteten Anwendung
Sie können die Definition der verwalteten Anwendung in einem von Ihnen während der Erstellung angegebenen Speicherkonto speichern. Dadurch können sein Speicherort und der Zugriff gemäß Ihrer gesetzlichen Anforderungen vollständig von Ihnen verwaltet werden.

> [!NOTE]
> Die Bereitstellung eines eigenen Speichers wird nur bei Bereitstellungen der Definition der verwalteten Anwendung über ARM-Vorlagen oder REST-APIs unterstützt.

### <a name="select-your-storage-account"></a>Wählen Sie Ihr Speicherkonto aus.
Sie müssen [ein Speicherkonto erstellen](../../storage/common/storage-account-create.md), in dem die Definition der verwalteten Anwendung zur Verwendung mit dem Dienstkatalog gespeichert wird.

Kopieren Sie die Ressourcen-ID des Speicherkontos. Sie wird später bei der Bereitstellung der Definition verwendet.

### <a name="set-the-role-assignment-for-appliance-resource-provider-in-your-storage-account"></a>Festlegen der Rollenzuweisung für „Applianceressourcenanbieter“ in Ihrem Speicherkonto
Bevor die Definition der verwalteten Anwendung in Ihrem Speicherkonto bereitgestellt werden kann, müssen Sie der Rolle **Applianceressourcenanbieter** Berechtigungen vom Typ „Mitwirkender“ zuweisen, damit die Definitionsdateien in den Container Ihres Speicherkontos geschrieben werden können.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für das Speicherkonto anzuzeigen. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.
1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** die Rolle **Mitwirkender** aus. 
1. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus.
1. Suchen Sie unter **Auswählen** nach der Rolle **Applianceressourcenanbieter**, und wählen Sie sie aus.
1. Speichern Sie die Rollenzuweisung.

### <a name="deploy-the-managed-application-definition-with-an-arm-template"></a>Bereitstellen der Definition der verwalteten Anwendung mit einer ARM-Vorlage 

Stellen Sie mithilfe der folgenden ARM-Vorlage Ihre gepackte verwaltete Anwendung als neue Definition der verwalteten Anwendung im Dienstkatalog bereit, dessen Definitionsdateien in Ihrem eigenen Speicherkonto gespeichert und verwaltet werden:
   
```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "applicationName": {
            "type": "string",
            "metadata": {
                "description": "Managed Application name"
            }
        },
        "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
        "definitionStorageResourceID": {
            "type": "string",
            "metadata": {
                "description": "Storage account resource ID for where you're storing your definition"
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located."
            }
        }
    },
    "variables": {
        "lockLevel": "None",
        "description": "Sample Managed application definition",
        "displayName": "Sample Managed application definition",
        "managedApplicationDefinitionName": "[parameters('applicationName')]",
        "packageFileUri": "[parameters('_artifactsLocation')]",
        "defLocation": "[parameters('definitionStorageResourceID')]",
        "managedResourceGroupId": "[concat(subscription().id,'/resourceGroups/', concat(parameters('applicationName'),'_managed'))]",
        "applicationDefinitionResourceId": "[resourceId('Microsoft.Solutions/applicationDefinitions',variables('managedApplicationDefinitionName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/applicationDefinitions",
            "apiVersion": "2019-07-01",
            "name": "[variables('managedApplicationDefinitionName')]",
            "location": "[parameters('location')]",
            "properties": {
                "lockLevel": "[variables('lockLevel')]",
                "description": "[variables('description')]",
                "displayName": "[variables('displayName')]",
                "packageFileUri": "[variables('packageFileUri')]",
                "storageAccountId": "[variables('defLocation')]"
            }
        }
    ],
    "outputs": {}
}
```

Wir haben den applicationDefintion-Eigenschaften eine neue Eigenschaft namens **storageAccountId** hinzugefügt und die ID des Speicherkontos, in dem die Definition gespeichert werden soll, als Wert dafür angegeben:

Sie können überprüfen, ob die Anwendungsdefinitionsdateien in Ihrem bereitgestellten Speicherkonto in einem Container mit dem Namen **applicationdefinitions** gespeichert sind.

> [!NOTE]
> Um für mehr Sicherheit zu sorgen, können Sie eine Definition für verwaltete Anwendungen erstellen und in einem [Azure-Speicherkontoblob mit aktivierter Verschlüsselung](../../storage/common/storage-service-encryption.md) speichern. Der Definitionsinhalt wird mithilfe der Verschlüsselungsoptionen des Speicherkontos verschlüsselt. Nur Benutzer mit Berechtigungen für die Datei können die Definition im Dienstkatalog anzeigen.

### <a name="make-sure-users-can-see-your-definition"></a>Sicherstellen, dass Benutzer Ihre Definition sehen können

Sie haben zwar Zugriff auf die Definition der verwalteten Anwendung, möchten aber sicherstellen, dass andere Benutzer in Ihrer Organisation darauf zugreifen können. Erteilen Sie ihnen mindestens die Leserrolle für die Definition. Diese Zugriffsebene haben sie möglicherweise vom Abonnement oder von der Ressourcengruppe geerbt. Informationen zum Überprüfen, wer Zugriff auf die Definition hat, sowie zum Hinzufügen von Benutzern oder Gruppen finden Sie unter [Manage access using RBAC and the Azure portal](../../role-based-access-control/role-assignments-portal.md) (Verwalten des Zugriffs mithilfe von RBAC und Azure-Portal).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Veröffentlichen der verwalteten Anwendung im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](publish-marketplace-app.md).
* Informationen zum Bereitstellen einer Instanz der verwalteten Anwendung finden Sie unter [Deploy service catalog app through Azure portal](deploy-service-catalog-quickstart.md) (Bereitstellen einer Dienstkatalog-App über das Azure-Portal).
