---
title: Benutzeroberflächenelement „InfoBox“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.InfoBox“ für das Azure-Portal beschrieben. Es wird zum Hinzufügen von Text oder Warnungen beim Bereitstellen einer verwalteten Anwendung verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 6d1e4a84904ef7022d9ce85803bf10285bf0b8ac
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75649818"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.InfoBox“

Ein Steuerelement, das ein Informationsfeld hinzugefügt. Das Feld enthält wichtigen Text oder Warnungen, damit die Benutzer ein besseres Verständnis der Werte erlangen, die sie angeben. Außerdem kann es einen Link zu einer URI bereitstellen, wenn weitere Informationen benötigt werden.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Schema

```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>Bemerkungen

* Verwenden Sie für `icon` entweder **None**, **Info**, **Warning** oder **Error**.
* Die `uri`-Eigenschaft ist optional.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
