---
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/21/2020
ms.author: dapine
ms.openlocfilehash: 9b4317064196c4ea3d761fd1a0bd43a764054fe6
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2020
ms.locfileid: "77563308"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Spracherkennung oder benutzerdefinierte Spracherkennung

Der Container bietet websocketbasierte Abfrageendpunkt-APIs, auf die über das [Speech SDK](../index.yml) zugegriffen wird. Standardmäßig verwendet das Speech SDK Online-Speech-Dienste. Um den Container verwenden zu können, müssen Sie die Initialisierungsmethode ändern.

> [!TIP]
> Wenn Sie das Sprach-SDK mit Containern verwenden, müssen Sie nicht die Azure Speech-Ressource [Abonnementschlüssel oder Authentifizierungsbearertoken](../rest-speech-to-text.md#authentication) bereitstellen.

Weitere Informationen finden Sie in den folgenden Beispielen.

# <a name="c"></a>[C#](#tab/csharp)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

in den folgenden Aufruf mit dem [Containerhost](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet):

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```
# <a name="python"></a>[Python](#tab/python)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

in den folgenden Aufruf mit dem [Containerhost](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python):

```python
speech_config = speechsdk.SpeechConfig(
    host="ws://localhost:5000")
```

***
