---
title: 'Schnellstart: Textanalyse-Clientbibliothek für C# | Microsoft-Dokumentation'
description: Steigen Sie in die Textanalyse-Clientbibliothek für C# ein.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/24/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 81d69f2274abf075be2f97b0edc67af2eea62327
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614474"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

[v3-Referenzdokumentation](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [Quellcode der v3-Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3-Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3-Beispiele](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[v2-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [Quellcode der v2-Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2-Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2-Beispiele](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Visual Studio-IDE](https://visualstudio.microsoft.com/vs/)

[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-net-core-application"></a>Erstellen einer neuen .NET Core-Anwendung

Erstellen Sie über die Visual Studio-IDE eine neue .NET Core-Konsolenanwendung. Dadurch wird ein Projekt vom Typ „Hallo Welt“ mit einer einzelnen C#-Quelldatei (*program.cs*) erstellt.

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**, und suchen Sie nach `Azure.AI.TextAnalytics`. Wählen Sie die Version `1.0.0-preview.2` und anschließend **Installieren** aus. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.

Öffnen Sie die Datei *program.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:

```csharp
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Erstellen Sie in der `Program`-Klasse der Anwendung Variablen für den Schlüssel und Endpunkt Ihrer Ressource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly TextAnalyticsApiKeyCredential credentials = new TextAnalyticsApiKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Ersetzen Sie die `Main`-Methode der Anwendung. Die hier aufgerufenen Methoden definieren Sie später.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityPIIExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, und suchen Sie nach `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`. Klicken Sie darauf, und klicken Sie dann auf **Installieren**. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.

Öffnen Sie die Datei *program.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Erstellen Sie in der `Program`-Klasse der Anwendung Variablen für den Schlüssel und Endpunkt Ihrer Ressource. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Ersetzen Sie die `Main`-Methode der Anwendung. Die hier aufgerufenen Methoden definieren Sie später.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein Objekt vom Typ `TextAnalyticsClient`, das sich mit Ihrem Schlüssel bei Azure authentifiziert und Funktionen bereitstellt, um Text als einzelne Zeichenfolgen oder als Batch zu akzeptieren. Sie können Text synchron oder asynchron an die API senden. Das Antwortobjekt enthält die Analyseinformationen für die einzelnen gesendeten Dokumente. 

Wenn Sie Version `3.0-preview` verwenden, können Sie eine optionale `TextAnalyticsClientOptions`-Instanz nutzen, um den Client mit verschiedenen Standardeinstellungen wie Standardsprache oder Länderhinweis zu initialisieren. Sie können für die Authentifizieren auch ein Azure Active Directory-Token verwenden. 

## <a name="code-examples"></a>Codebeispiele

* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Erkennung benannter Entitäten](#named-entity-recognition-ner)
* [Erkennung personenbezogener Informationen](#detect-personal-information)
* [Entitätsverknüpfung](#entity-linking)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Stellen Sie sicher, dass mit Ihrer zuvor erstellten Main-Methode ein neues Clientobjekt mit Ihrem Endpunkt und Ihren Anmeldeinformationen erstellt wird.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Klasse vom Typ `ApiKeyServiceClientCredentials`, um die Anmeldeinformationen zu speichern und den Anforderungen des Clients hinzuzufügen. Erstellen Sie darin eine Außerkraftsetzung für `ProcessHttpRequestAsync()`, um Ihren Schlüssel dem Header `Ocp-Apim-Subscription-Key` hinzuzufügen.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Erstellen Sie eine Methode, um das [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet)-Objekt mit Ihrem Endpunkt und einem `ApiKeyServiceClientCredentials`-Objekt zu instanziieren, das Ihren Schlüssel enthält.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Stimmungsanalyse

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `AnalyzeSentiment()` auf. Das zurückgegebene Objekt vom Typ `Response<DocumentSentiment>` enthält die Stimmungsbezeichnung und den Stimmungswert des gesamten Eingabedokuments sowie eine Standpunktanalyse für jeden Satz (sofern erfolgreich). Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    var si = new StringInfo(inputText);
    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tSentence [offset {sentence.Offset}, length {sentence.Length}]");
        Console.WriteLine($"\tText: \"{si.SubstringByTextElements(sentence.Offset, sentence.Length)}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.SentimentScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.SentimentScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.SentimentScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Document sentiment: Positive

        Sentence [offset 0, length 30]
        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [offset 31, length 30]
        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) auf. Das zurückgegebene Objekt vom Typ [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Stimmung `Score` und andernfalls `errorMessage`. 

Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Spracherkennung

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)


Erstellen Sie eine neue Funktion namens `LanguageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `DetectLanguage()` auf. Das zurückgegebene Objekt vom Typ `Response<DetectedLanguage>` enthält die erkannte Sprache sowie den zugehörigen Namen und ISO-6391-Code. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen. Wenn Sie einen anderen Standardwert festlegen möchten, legen Sie die Eigenschaft `TextAnalyticsClientOptions.DefaultCountryHint` fest, und übergeben Sie sie während der Initialisierung des Clients.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Ausgabe

```console
Language:
        French, ISO-6391: fr
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `languageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das zurückgegebene Objekt vom Typ [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Sprachen in `DetectedLanguages` und andernfalls `errorMessage`. Geben Sie die erste zurückgegebene Sprache aus.

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Ausgabe

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)


> [!NOTE]
> Neu in Version `3.0-preview`:
> * Die Entitätserkennung ermöglicht nun die Erkennung personenbezogener Informationen im Text.
> * Die Entitätsverknüpfung ist nun von der Entitätserkennung getrennt.


Erstellen Sie eine neue Funktion namens `EntityRecognitionExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<IReadOnlyCollection<CategorizedEntity>>` enthält die Liste erkannter Entitäten. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach(var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Named Entities:
        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Offset: 26,     Length: 7,      Score: 0.920

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Offset: 34,     Length: 9,      Score: 0.800
```

## <a name="detect-personal-information"></a>Erkennung personenbezogener Informationen

Erstellen Sie eine neue Funktion namens `EntityPIIExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizePiiEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<IReadOnlyCollection<CategorizedEntity>>` enthält ähnlich wie bei der vorherigen Funktion die Liste erkannter Entitäten. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    string inputText = "Insurance policy for SSN on file 123-12-1234 is here by approved.";
    var response = client.RecognizePiiEntities(inputText);
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach(var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Category: U.S. Social Security Number (SSN),    Sub-Category: None
                Offset: 33,     Length: 11,     Score: 0.850
```


## <a name="entity-linking"></a>Entitätsverknüpfung

Erstellen Sie eine neue Funktion namens `EntityLinkingExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeLinkedEntities()` auf, und durchlaufen Sie die Ergebnisse. Die Rückgabe (`Response<IReadOnlyCollection<LinkedEntity>>`) stellt die Liste der erkannten Entitäten dar. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst. Da verknüpfte Entitäten eindeutig identifiziert werden, werden Vorkommen der gleichen Entität unter einem Objekt vom Typ `LinkedEntity` als Liste von Objekten des Typs `LinkedEntityMatch` gruppiert.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.Id},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.Score:F3}\n");
        }
    }
}
```

### <a name="output"></a>Ausgabe

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Offset: 116,    Length: 11,     Score: 0.777

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25,     Length: 10,     Score: 0.555

                Text: Gates
                Offset: 161,    Length: 5,      Score: 0.555

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40,     Length: 10,     Score: 0.533

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0,      Length: 9,      Score: 0.469

                Text: Microsoft
                Offset: 150,    Length: 9,      Score: 0.469

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54,     Length: 7,      Score: 0.248

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89,     Length: 5,      Score: 0.281
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

> [!NOTE]
> In Version 2.1 ist die Entitätsverknüpfung in der NER-Antwort enthalten.

Erstellen Sie eine neue Funktion namens `RecognizeEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Entitäten in `Entities` und andernfalls `errorMessage`. Geben Sie für jede erkannte Entität ihren Typ, Untertyp, Wikipedia-Namen (sofern vorhanden) sowie die Fundorte im Originaltext aus.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `ExtractKeyPhrases()` auf. Das zurückgegebene Objekt vom Typ `<Response<IReadOnlyCollection<string>>` enthält die Liste erkannter Schlüsselbegriffe. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Key phrases:
    cat
    veterinarian
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Schlüsselbegriffe in `KeyPhrases` und andernfalls `errorMessage`. Geben Sie alle erkannten Schlüsselbegriffe aus.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Ausgabe

```console
Key phrases:
    cat
    veterinarian
```

---
