---
title: Referenz zu App-Einstellungen für Azure Functions
description: Referenzdokumentation für die App-Einstellungen für Azure Functions oder Umgebungsvariablen.
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 3853ccbfd492bfaf4a82d62e6d31ab938285ee2e
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357575"
---
# <a name="app-settings-reference-for-azure-functions"></a>Referenz zu App-Einstellungen für Azure Functions

App-Einstellungen in einer Funktionen-App enthalten globale Konfigurationsoptionen, die sich auf alle Funktionen dieser Funktionen-App auswirken. Bei der lokalen Ausführung wird auf diese Einstellungen als lokale [Umgebungsvariablen](functions-run-local.md#local-settings-file) zugegriffen. In diesem Artikel werden die in Funktionen-Apps verfügbaren App-Einstellungen aufgelistet.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Es gibt andere globale Konfigurationsoptionen in der Datei [host.json](functions-host-json.md) und in der Datei [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Der Application Insights-Instrumentierungsschlüssel, wenn Sie Application Insights verwenden. Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

|Key|Beispielwert|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

Konfiguriert in Version 2.x oder höher der Functions-Runtime das App-Verhalten auf der Grundlage der Runtimeumgebung. Dieser Wert ist [während der Initialisierung lesen](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/Program.cs#L43). Sie können `AZURE_FUNCTIONS_ENVIRONMENT` auf beliebige Werte festlegen, aber [drei Werte](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) werden unterstützt: [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) und [Produktion](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Wenn `AZURE_FUNCTIONS_ENVIRONMENT` nicht festgelegt ist, wird als Standardwert in einer lokalen Umgebung `Development` und in Azure `Production` verwendet. Diese Einstellung sollte anstelle von `ASPNETCORE_ENVIRONMENT` verwendet werden, um die Laufzeitumgebung festzulegen. 

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Optionale Verbindungszeichenfolge für das Speicherkonto zum Speichern von Protokollen und zu deren Anzeige auf der Registerkarte **Überwachen** im Portal. Diese Einstellung ist nur für Apps gültig, die für Version 1.x der Azure Functions-Runtime gedacht sind. Das Speicherkonto muss ein allgemeines Konto sein, das BLOBs, Warteschlangen und Tabellen unterstützt. Weitere Informationen finden Sie unter [Speicherkontoanforderungen](storage-considerations.md#storage-account-requirements).

|Key|Beispielwert|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>|

> [!NOTE]
> Für bessere Leistung und Benutzerfreundlichkeit verwenden dir Runtimeversion 2.x und höhere Versionen APPINSIGHTS_INSTRUMENTATIONKEY und App Insights anstelle von `AzureWebJobsDashboard` für die Überwachung.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` bedeutet, dass die standardmäßige Landing Page deaktiviert wird, die für die Stamm-URL einer Funktionen-App angezeigt wird. Der Standardwert ist `false`.

|Key|Beispielwert|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Wenn diese App-Einstellung ausgelassen oder auf `false` gesetzt wird, erfolgt als Reaktion auf die URL `<functionappname>.azurewebsites.net` die Anzeige einer Seite, die dem folgenden Beispiel ähnelt.

![Landing Page der Funktionen-App](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` bedeutet, dass beim Kompilieren von .NET-Code der Releasemodus verwendet wird, während `false` bedeutet, dass der Debugmodus verwendet wird. Der Standardwert ist `true`.

|Key|Beispielwert|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Eine durch Trennzeichen getrennte Liste der zu aktivierenden Features der Betaversion. Durch diese Flags aktivierten Features der Betaversion sind nicht bereit für die Produktion, können aber zur experimentellen Verwendung aktiviert werden, bevor sie live geschaltet werden.

|Key|Beispielwert|
|---|------------|
|AzureWebJobsFeatureFlags|Feature1,Feature2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Gibt das Repository oder den Anbieter an, das bzw. der zum Speichern von Schlüsseln verwendet wird. Aktuell sind die unterstützten Repositorys der Blobspeicher („Blob“) und das lokale Dateisystem („Dateien“). In Version 2 ist der Blobspeicher die Standardeinstellung, in Version 1 das Dateisystem.

|Key|Beispielwert|
|---|------------|
|AzureWebJobsSecretStorageType|Dateien|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Die Azure Functions-Laufzeit verwendet diese Verbindungszeichenfolge des Speicherkontos für alle Funktionen, mit Ausnahme der per HTTP ausgelösten Funktionen. Das Speicherkonto muss ein allgemeines Konto sein, das BLOBs, Warteschlangen und Tabellen unterstützt. Weitere Informationen finden Sie unter [Speicherkonto](functions-infrastructure-as-code.md#storage-account) und [Anforderungen an das Speicherkonto](storage-considerations.md#storage-account-requirements).

|Key|Beispielwert|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[Name];AccountKey=[Schlüssel]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

Der Pfad zum Compiler, der für TypeScript verwendet wird. Bei Bedarf können Sie den Standardwert außer Kraft setzen.

|Key|Beispielwert|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="function_app_edit_mode"></a>FUNCTION\_APP\_EDIT\_MODE

Bestimmt, ob die Bearbeitung im Azure-Portal aktiviert ist. Gültige Werte sind „readwrite“ und „readonly“.

|Key|Beispielwert|
|---|------------|
|FUNCTION\_APP\_EDIT\_MODE|readonly|

## <a name="functions_extension_version"></a>FUNCTIONS\_EXTENSION\_VERSION

Die in dieser Funktionen-App zu verwendende Version der Functions-Runtime. Eine Tilde mit Hauptversion bedeutet, dass die neueste Version dieser Hauptversion verwendet wird (z.B. „~2“). Wenn neue Versionen für dieselbe Hauptversion verfügbar sind, werden sie automatisch in der Funktionen-App installiert. Verwenden Sie die vollständige Versionsnummer (z.B. „2.0.12345“), um die App an eine bestimmte Version anzuheften. Der Standardwert ist „~2“. Der Wert `~1` heftet Ihre App an Version 1.x der Runtime an.

|Key|Beispielwert|
|---|------------|
|FUNCTIONS\_EXTENSION\_VERSION|~2|

## <a name="functions_v2_compatibility_mode"></a>FUNCTIONS\_V2\_COMPATIBILITY\_MODE

Diese Einstellung ermöglicht die Ausführung Ihrer Funktions-App in einem mit Version 2.x kompatiblen Modus unter Runtimeversion 3.x. Verwenden Sie diese Einstellung nur, wenn beim [Aktualisieren Ihrer Funktions-App von Runtimeversion 2.x auf 3.x](functions-versions.md#migrating-from-2x-to-3x) Probleme auftreten. 

>[!IMPORTANT]
> Diese Einstellung ist nur als kurzfristige Problemumgehung gedacht, während Sie Ihre App für die ordnungsgemäße Ausführung unter Version 3.x aktualisieren. Diese Einstellung wird unterstützt, solange die [Runtime 2.x unterstützt wird](functions-versions.md). Wenn Probleme auftreten, die die Ausführung Ihrer App unter Version 3.x ohne diese Einstellung verhindern, [melden Sie Ihr Problem](https://github.com/Azure/azure-functions-host/issues/new?template=Bug_report.md).

[FUNCTIONS\_EXTENSION\_VERSION](functions-app-settings.md#functions_extension_version) muss dafür auf `~3` festgelegt werden.

|Key|Beispielwert|
|---|------------|
|FUNCTIONS\_V2\_COMPATIBILITY\_MODE|true|

## <a name="functions_worker_process_count"></a>FUNCTIONS\_WORKER\_PROCESS\_COUNT

Gibt die maximale Anzahl von Sprachworkerprozessen mit einem Standardwert von `1` an. Der zulässige Höchstwert ist `10`. Funktionsaufrufe werden gleichmäßig zwischen Sprachworkerprozessen verteilt. Sprachworkerprozesse werden so lange alle 10 Sekunden erzeugt, bis die von FUNCTIONS\_WORKER\_PROCESS\_COUNT festgelegte Anzahl erreicht ist. Die Verwendung mehrerer Sprachworkerprozesse ist nicht dasselbe wie [Skalierung](functions-scale.md). Erwägen Sie die Verwendung dieser Einstellung, wenn Ihr Workload aus einer Mischung von CPU-gebundenen und E/A-gebundenen Aufrufen besteht. Diese Einstellung gilt für alle .NET-Sprachen.

|Key|Beispielwert|
|---|------------|
|FUNCTIONS\_WORKER\_PROCESS\_COUNT|2|


## <a name="functions_worker_runtime"></a>FUNCTIONS\_WORKER\_RUNTIME

Die Sprachworkerruntime, die in der Funktionen-App geladen werden soll.  Dies entspricht der Sprache, die in Ihrer Anwendung verwendet wird (z.B. „dotnet“). Bei Funktionen in mehreren Sprachen müssen Sie diese in verschiedenen Apps mit dem jeweils passenden Workerruntimewert veröffentlichen.  Gültige Werte sind `dotnet` (C#/F#), `node` (JavaScript/TypeScript), `java` (Java) und `powershell` (PowerShell) und `python` (Python).

|Key|Beispielwert|
|---|------------|
|FUNCTIONS\_WORKER\_RUNTIME|dotnet|

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Nur für Verbrauchs- und Premium-Tarife. Die Verbindungszeichenfolge für das Speicherkonto, in dem der Code der Funktionen-App und die Konfiguration gespeichert werden. Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App](functions-infrastructure-as-code.md#create-a-function-app).

|Key|Beispielwert|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[Name];AccountKey=[Schlüssel]|

## <a name="website_contentshare"></a>WEBSITE\_CONTENTSHARE

Nur für Verbrauchs- und Premium-Tarife. Der Dateipfad zum Code der Funktionen-App und zur Konfiguration. Wird mit WEBSITE_CONTENTAZUREFILECONNECTIONSTRING verwendet. Standardmäßig wird eine eindeutige Zeichenfolge verwendet, die mit dem Namen der Funktionen-App beginnt. Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App](functions-infrastructure-as-code.md#create-a-function-app).

|Key|Beispielwert|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="website_max_dynamic_application_scale_out"></a>WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT

Die maximale Anzahl der Instanzen, auf denen die Funktionen-App horizontal hochskaliert werden kann. Dieser Wert ist standardmäßig unbegrenzt.

> [!NOTE]
> Diese Einstellung ist ein Vorschaufeature und funktioniert nur dann zuverlässig, wenn sie auf einen Wert <= 5 festgelegt wird.

|Key|Beispielwert|
|---|------------|
|WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT|5|

## <a name="website_node_default_version"></a>WEBSITE\_NODE\_DEFAULT_VERSION

_Nur Windows._  
Legt die Version von Node.js fest, die beim Ausführen Ihrer Funktions-App unter Windows verwendet werden soll. Verwenden Sie eine Tilde (~), damit die Laufzeit die neueste verfügbare Version der Zielhauptversion verwendet. Wenn Sie beispielsweise auf `~10` festlegen, wird die neueste Version von Node.js 10 verwendet. Wenn eine Hauptversion mittels Tilde vorgegeben wird, müssen Sie die Nebenversion nicht manuell aktualisieren. 

|Key|Beispielwert|
|---|------------|
|WEBSITE\_NODE\_DEFAULT_VERSION|~10|

## <a name="website_run_from_package"></a>WEBSITE\_RUN\_FROM\_PACKAGE

Ermöglicht es Ihrer Funktions-App, über eine bereitgestellte Paketdatei ausgeführt zu werden.

|Key|Beispielwert|
|---|------------|
|WEBSITE\_RUN\_FROM\_PACKAGE|1|

Gültige Werte sind entweder eine URL, die in den Speicherort einer Bereitstellungspaketdatei aufgelöst werden kann, oder `1`. Bei einer Festlegung auf `1` muss sich das Paket im Ordner `d:\home\data\SitePackages` befinden. Wenn Sie die Zip-Bereitstellung mit dieser Einstellung verwenden, wird das Paket automatisch an diesen Speicherort hochgeladen. In der Vorschau wurde diese Einstellung als `WEBSITE_RUN_FROM_ZIP` bezeichnet. Weitere Informationen finden Sie unter [Ausführen von Azure Functions über eine Paketdatei](run-functions-from-deployment-package.md).

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

Standardmäßig nutzen Functions-Proxys eine Verknüpfung, um API-Aufrufe von Proxys direkt an Funktionen in derselben Funktionen-App zu senden, anstatt eine neue HTTP-Anforderung zu erstellen. Mit dieser Einstellung können Sie dieses Verhalten deaktivieren.

|Key|value|BESCHREIBUNG|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|Aufrufe mit einer Back-End-URL, die auf eine Funktion in der lokalen Funktions-App verweist, werden nicht mehr direkt an die Funktion gesendet. Stattdessen werden sie an das HTTP-Front-End für die Funktions-App zurückgeleitet.|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|false|Dies ist der Standardwert. Aufrufe mit einer Back-End-URL, die auf eine Funktion in der lokalen Funktions-App verweist, werden direkt an diese Funktion geleitet.|


## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

Diese Einstellung steuert, ob %2F in Routenparametern als Schrägstrich decodiert wird, wenn dieser Code in der Back-End-URL eingefügt wird. 

|Key|value|Beschreibung|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|Bei Routenparameter mit codierten Schrägstrichen werden diese decodiert. `example.com/api%2ftest` wird zu `example.com/api/test`.|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|false|Dies ist das Standardverhalten. Alle Routenparameter werden unverändert übergeben.|

### <a name="example"></a>Beispiel

Hier sehen Sie ein Beispiel für „proxies.json“ in einer Funktionen-App unter der URL myfunction.com.

```JSON
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "root": {
            "matchCondition": {
                "route": "/{*all}"
            },
            "backendUri": "example.com/{all}"
        }
    }
}
```
|URL-Decodierung|Eingabe|Output|
|-|-|-|
|true|myfunction.com/test%2fapi|example.com/test/api
|false|myfunction.com/test%2fapi|example.com/test%2fapi|


## <a name="next-steps"></a>Nächste Schritte

[Informationen zum Aktualisieren von App-Einstellungen](functions-how-to-use-azure-function-app-settings.md#settings)

[Weitere Informationen finden Sie unter den globalen Einstellungen in der Datei host.json](functions-host-json.md)

[Weitere App-Einstellungen für App Service-Apps](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
