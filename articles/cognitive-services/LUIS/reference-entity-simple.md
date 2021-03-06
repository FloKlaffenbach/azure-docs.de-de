---
title: Entität vom Typ „Simple“ – LUIS
titleSuffix: Azure Cognitive Services
description: Eine einfache Entität beschreibt ein einzelnes Konzept aus dem durch maschinelles Lernen erworbenen Kontext. Fügen Sie eine Auszugsliste hinzu, wenn Sie eine einfache Entität verwenden, um die Ergebnisse zu verbessern.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 8b92aa6057c81ec9442372c5b85918cb92196d61
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894761"
---
# <a name="simple-entity"></a>Entität vom Typ „Simple“

Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernens erworben wurde. Da es sich bei einfachen Entitäten normalerweise um Namen handelt, z.B. Unternehmensnamen, Produktnamen oder andere Namen, sollten Sie wie folgt vorgehen: Fügen Sie eine [Liste mit Ausdrücken](luis-concept-feature.md) hinzu, wenn Sie eine einfache Entität verwenden, um das Signal für die verwendeten Namen zu verstärken.

**Diese Entität ist gut geeignet, wenn Folgendes gilt**:

* Die Daten sind nicht einheitlich formatiert, aber weisen auf denselben Sachverhalt hin.

![Entität vom Typ „Simple“](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>JSON-Beispiel

`Bob Jones wants 3 meatball pho`

In der vorherigen Äußerung wird `Bob Jones` als die einfache Entität `Customer` bezeichnet.

Die vom Endpunkt zurückgegebenen Daten enthalten den Namen der Entität, den in der Äußerung ermittelten Text, den Speicherort des erkannten Texts und die Bewertung:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)

Dies ist der JSON-Code, wenn `verbose=false` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Entität vom Typ „Simple“|`Customer`|`bob jones`|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informieren Sie sich über die Mustersyntax.](reference-pattern-syntax.md)