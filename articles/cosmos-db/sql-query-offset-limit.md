---
title: OFFSET LIMIT-Klausel in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie mit der OFFSET LIMIT-Klausel beim Abfragen in Azure Cosmos DB einige bestimmte Werte überspringen und übernehmen.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 3d23676885323e370cee1e9cc9e98c7128faf2e0
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76771571"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>OFFSET LIMIT-Klausel in Azure Cosmos DB

Die OFFSET LIMIT-Klausel ist eine optionale Klausel zum Überspringen einer bestimmten Anzahl von Werten aus der Abfrage. In der OFFSET LIMIT-Klausel müssen sowohl für OFFSET als auch für LIMIT Werte angegeben werden.

Wenn OFFSET LIMIT in Verbindung mit einer ORDER BY-Klausel verwendet wird, wird das Resultset erstellt, indem zunächst Werte übersprungen und dann die sortierten Werte angenommen werden. Wenn keine ORDER BY-Klausel verwendet wird, gilt eine deterministische Reihenfolge der Werte.

## <a name="syntax"></a>Syntax
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumente

- `<offset_amount>`

   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse überspringen sollen.

- `<limit_amount>`
  
   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse enthalten sollen.

## <a name="remarks"></a>Bemerkungen
  
  Die `OFFSET`-Anzahl und die `LIMIT`-Anzahl sind in der `OFFSET LIMIT`-Klausel erforderlich. Wenn eine optionale `ORDER BY`-Klausel verwendet wird, wird das Resultset durch Überspringen der sortierten Werte erzeugt. Andernfalls gibt die Abfrage eine feste Reihenfolge der Werte zurück.

  Die RU-Gebühr für eine Abfrage mit `OFFSET LIMIT` steigt mit zunehmender Anzahl der versetzten Begriffe. Für Abfragen mit mehreren Ergebnisseiten empfiehlt es sich in der Regel, Fortsetzungstoken zu verwenden. Fortsetzungstoken sind „Lesezeichen“ für die Stelle, an der die Abfrage später fortgesetzt werden kann. Wenn Sie `OFFSET LIMIT` verwenden, gibt es kein „Lesezeichen“. Wenn Sie die nächste Seite der Abfrage zurückgeben möchten, müssen Sie vom Anfang beginnen.
  
  Sie sollten `OFFSET LIMIT` in Fällen verwenden, in denen Sie Dokumente vollständig überspringen und Clientressourcen speichern möchten. Sie sollten `OFFSET LIMIT` z. B. verwenden, wenn Sie alles bis zum 1.000. Ergebnis der Abfrage überspringen und die Ergebnisse 1 bis 999 nicht anzeigen möchten. Auf dem Back-End lädt `OFFSET LIMIT` immer noch jedes Dokument, einschließlich der übersprungenen Dokumente. Der Leistungsvorteil ergibt sich aus der Einsparung von Clientressourcen, indem die Verarbeitung von nicht benötigten Dokumenten vermieden wird.

## <a name="examples"></a>Beispiele

Dies ist z. B. eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (in der Reihenfolge der Wohnortnamen) zurückgibt:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Dies ist eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (ohne Sortierung) zurückgibt:

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [SELECT-Klausel](sql-query-select.md)
- [ORDER BY-Klausel](sql-query-order-by.md)
