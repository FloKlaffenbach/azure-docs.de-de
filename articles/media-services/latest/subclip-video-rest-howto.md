---
title: Erstellen von Subclips für ein Video beim Codieren mit Azure Media Services-REST
description: In diesem Artikel wird beschrieben, wie Sie beim Codieren mit Azure Media Services mithilfe von REST Subclips für ein Video erstellen
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: c39aded55fe36cb130459a4f6f119f872b1adbc4
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514322"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Erstellen von Subclips für ein Video beim Codieren mit Media Services: REST

Sie können mithilfe eines [Auftrags](https://docs.microsoft.com/rest/api/media/jobs) Videos beim Codieren zuschneiden oder Subclips für diese erstellen. Dies funktioniert mit jeder [Transformation](https://docs.microsoft.com/rest/api/media/transforms), die entweder mit den [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)-Voreinstellungen oder mit den [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)-Voreinstellungen erstellt wird. 

Mithilfe des in diesem Artikel verwendeten REST-Beispiels wird ein Auftrag erstellt, der ein Video zuschneidet, sobald er einen Codierungsauftrag übermittelt. 

## <a name="prerequisites"></a>Voraussetzungen

Für die in diesem Thema beschriebenen Schritte ist Folgendes erforderlich:

- [Erstellen eines Azure Media Services-Kontos](create-account-cli-how-to.md)
- [Konfigurieren Sie Postman für Azure Media Services-REST-API-Aufrufe](media-rest-apis-with-postman.md).
    
    Befolgen Sie dabei unbedingt den letzten Schritt im Thema [Abrufen von Azure AD-Token](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Erstellen Sie eine Transformation und ein Ausgabeobjekt. Informationen zum Erstellen einer Transformation und von Ausgabeobjekten finden Sie im Tutorial [Codieren einer Remotedatei anhand einer URL und Streamen des Videos über REST](stream-files-tutorial-with-rest.md).
- Lesen Sie sich den Artikel [Encoding concept (Codierungskonzept)](encoding-concept.md) durch.

## <a name="create-a-subclipping-job"></a>Erstellen eines Auftrags zum Erstellen von Subclips

1. Klicken Sie in der heruntergeladenen Postman Collection auf **Transforms and Jobs** (Transformationen und Aufträge) -> **Create Job with Sub Clipping** (Auftrag mit Subclips erstellen).
    
    Die **PUT**-Anforderung sieht wie folgt aus:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Aktualisieren Sie den Wert der Umgebungsvariablen „transformName“ durch den Namen Ihrer Transformation. 
1. Klicken Sie auf die Registerkarte **Body** (Text), und aktualisieren Sie das Element „myOutputAsset“ durch den Namen Ihres Ausgabeobjekts.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. Klicken Sie auf **Senden**.

    Dann wird Ihnen die **Antwort** mit den Informationen zum erstellten und übermittelten Auftrag und seinem Status angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

[Codieren mit einer benutzerdefinierten Transformation](custom-preset-rest-howto.md) 
