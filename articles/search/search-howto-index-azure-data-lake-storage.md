---
title: Suche über Azure Data Lake Storage Gen2 (Vorschauversion)
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie Inhalte und Metadaten in Azure Data Lake Storage Gen2 indizieren. Dieses Feature befindet sich zurzeit in der öffentlichen Vorschau.
manager: nitinme
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4b725c8a1bf0649a640c02a9a1828ec9014d36d6
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905660"
---
# <a name="indexing-documents-in-azure-data-lake-storage-gen2"></a>Indizieren von Dokumenten in Azure Data Lake Storage Gen2

> [!IMPORTANT] 
> Azure Data Lake Storage Gen2 befindet sich zurzeit in der öffentlichen Vorschau. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) aus, wenn Sie Zugriff auf die Vorschauversionen anfordern möchten. Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.


Beim Einrichten eines Azure-Speicherkontos haben Sie die Möglichkeit, einen [hierarchischen Namespace](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) zu aktivieren. Auf diese Weise kann die Sammlung von Inhalten in einem Konto in einer Hierarchie von Verzeichnissen und geschachtelten Unterverzeichnissen organisiert werden. Durch das Aktivieren eines hierarchischen Namespace aktivieren Sie [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction).

In diesem Artikel wird beschrieben, wie Sie mit dem Indizieren von Dokumenten beginnen, die in Azure Data Lake Storage Gen2 vorliegen.

## <a name="set-up-azure-data-lake-storage-gen2-indexer"></a>Einrichten des Azure Data Lake Storage Gen2-Indexers

Sie müssen einige wenige Schritte ausführen, um Inhalte aus Data Lake Storage Gen2 zu indizieren.

### <a name="step-1-sign-up-for-the-preview"></a>Schritt 1: Anmelden für die Vorschau

Melden Sie sich für die Data Lake Storage Gen2-Indexervorschau an, indem Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) ausfüllen. Sie erhalten eine Bestätigungs-E-Mail, sobald Sie für die Vorschau registriert wurden.

### <a name="step-2-follow-the-azure-blob-storage-indexing-setup-steps"></a>Schritt 2: Ausführen der Schritte zum Einrichten der Azure Blob Storage-Indizierung

Sobald Sie die Bestätigung erhalten haben, dass Ihre Registrierung für die Vorschau erfolgreich war, können Sie die Indizierungspipeline erstellen.

Sie können Inhalte und Metadaten aus Data Lake Storage Gen2 unter Verwendung der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) indizieren. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Das Indizieren von Inhalten in Data Lake Storage Gen2 ist mit dem Indizieren von Inhalten in Azure Blob Storage identisch. Deshalb finden Sie die Informationen zum Einrichten von Datenquelle, Index und Indexer von Data Lake Storage Gen2 unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Cognitive Search](search-howto-indexing-azure-blob-storage.md). In diesem Artikel werden auch Informationen dazu bereitgestellt, welche Dokumentformate unterstützt werden, welche Blobmetadaten-Eigenschaften extrahiert wird, wie die inkrementelle Indizierung funktioniert usw. Diese Informationen gelten auch für Data Lake Storage Gen2.

## <a name="access-control"></a>Zugriffssteuerung

Azure Data Lake Storage Gen2 implementiert ein [Zugriffssteuerungsmodell](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control), das sowohl die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) in Azure als auch POSIX-ähnliche Zugriffssteuerungslisten (Access Control Lists, ACLs) unterstützt. Beim Indizieren von Inhalten aus Data Lake Storage Gen2 extrahiert Azure Cognitive Search keine RBAC- und ACL-Informationen aus den Inhalten. Als Ergebnis sind diese Informationen nicht in Ihrem Azure Cognitive Search-Index enthalten.

Wenn die Verwaltung der Zugriffssteuerung für jedes Dokument im Index wichtig ist, kann der Anwendungsentwickler [Sicherheitsfilter](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) implementieren.

## <a name="change-detection"></a>Änderungserkennung

Der Data Lake Storage Gen2-Indexer unterstützt die Änderungserkennung. Dies bedeutet, dass beim Ausführen des Indexers nur die geänderten Blobs neu indiziert werden, die anhand des `LastModified`-Zeitstempels des Blobs ermittelt werden.

> [!NOTE] 
> Mit Data Lake Storage Gen2 können Verzeichnisse umbenannt werden. Wenn ein Verzeichnis umbenannt wird, werden die Zeitstempel für die Blobs in diesem Verzeichnis nicht aktualisiert. Demzufolge werden diese Blobs vom Indexer nicht neu indiziert. Wenn die Blobs in einem Verzeichnis nach einer Umbenennung des Verzeichnisses neu indiziert werden müssen, da sie nun über neue URLs verfügen, müssen Sie den `LastModified`-Zeitstempel für alle Blobs im Verzeichnis aktualisieren, damit der Indexer erkennt, dass diese bei einer zukünftigen Ausführung neu indiziert werden müssen.
