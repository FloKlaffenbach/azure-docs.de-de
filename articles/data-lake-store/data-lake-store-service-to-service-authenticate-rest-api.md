---
title: Dienst-zu-Dienst-Authentifizierung – Data Lake Storage Gen1 – REST-API
description: Hier erfahren Sie, wie Sie mithilfe der REST-API Dienst-zu-Dienst-Authentifizierung bei Azure Data Lake Storage Gen1 und Azure Active Directory durchführen.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 59d0bf20b16beda47d76e6a9940ac9fa4436da3f
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904526"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>Dienst-zu-Dienst-Authentifizierung bei Azure Data Lake Storage Gen1 mithilfe der REST-API
> [!div class="op_single_selector"]
> * [Verwenden von Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Verwenden des .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Verwenden von Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Verwenden der REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie mithilfe der REST-API die Dienst-zu-Dienst-Authentifizierung bei Azure Data Lake Storage Gen1 durchführen. Informationen zur Authentifizierung von Endbenutzern bei Data Lake Storage Gen1 mithilfe der REST-API finden Sie unter [Authentifizieren von Endbenutzern bei Azure Data Lake Storage Gen1 mit der REST-API](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer Azure Active Directory-Webanwendung.** Sie müssen die Schritte unter [Dienst-zu-Dienst-Authentifizierung bei Azure Data Lake Storage Gen1 mithilfe von Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md) ausgeführt haben.

## <a name="service-to-service-authentication"></a>Dienst-zu-Dienst-Authentifizierung

In diesem Szenario stellt die Anwendung eigene Anmeldeinformationen zum Durchführen der Vorgänge bereit. Hierzu müssen Sie eine POST-Anforderung wie im folgenden Codeausschnitt gezeigt ausgeben:

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung enthält ein Autorisierungstoken (gekennzeichnet durch `access-token` in der folgenden Ausgabe), das Sie anschließend mit Ihren REST-API-Aufrufen übergeben. Speichern Sie das Authentifizierungstoken in einer Textdatei. Sie benötigen es, wenn Sie REST-Aufrufe an Data Lake Storage Gen1 richten.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel wird der **nicht interaktive** Ansatz verwendet. Weitere Informationen zur nicht interaktiven Authentifizierung (Dienst-zu-Dienst-Aufrufe) finden Sie unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die Authentifizierung zwischen Diensten verwenden, um sich mit der REST-API bei Data Lake Storage Gen1 zu authentifizieren. In den folgenden Artikeln wird erörtert, wie Sie die REST-API mit Data Lake Storage Gen1 verwenden.

* [Kontoverwaltungsvorgänge für Azure Data Lake Storage Gen1 mit der REST-API](data-lake-store-get-started-rest-api.md)
* [Dateisystemvorgänge in Data Lake Storage Gen1 mit der REST-API](data-lake-store-data-operations-rest-api.md)
