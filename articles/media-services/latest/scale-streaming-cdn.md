---
title: Skalieren des Streamings mit CDN
titleSuffix: Azure Media Services
description: Erfahren Sie mehr über einen Streamingdienst, der Inhalte direkt an eine Clientplayer-App oder an ein Content Delivery Network (CDN) übermittelt.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: 90fa3b06e2696e9b45c333c75c8a8e117d5c0c96
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562761"
---
# <a name="scaling-streaming-with-cdn"></a>Skalieren des Streamings mit CDN

Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung zur schnellen Übermittlung von Inhalten mit hoher Bandbreite an Benutzer. Hierzu werden Inhalte auf physischen Knoten zwischengespeichert, die strategisch auf der ganzen Welt verteilt sind.  

CDN speichert Inhalte zwischen, die von einem Media Services-[Streamingendpunkt (Ursprung)](streaming-endpoint-concept.md) per Codec, per Streamingprotokoll, per Bitrate, per Containerformat und per Verschlüsselung/DRM gestreamt werden. Für jede Kombination von Codec-Streamingprotokoll-Containerformat-Bitrate-Verschlüsselung wird es einen separaten CDN-Cache geben. 

Der beliebte Inhalt wird direkt über den CDN-Cache bereitgestellt, sofern das Videofragment zwischengespeichert ist. Liveinhalte werden wahrscheinlich zwischengespeichert, da normalerweise viele Benutzer sich genau dasselbe ansehen. On-Demand-Inhalte können sich etwas schwieriger gestalten, da beliebte Inhalte und weniger beliebte Inhalte enthalten sein können. Wenn Sie über Millionen von Video-Medienobjekten verfügen, von denen keines beliebt ist (nur zwei oder drei Aufrufe pro Woche), jedoch tausende Personen die unterschiedlichen Videos ansehen, wird die Effektivität des CDN viel stärker beeinträchtigt. 

Sie müssen zudem die Funktionsweise des adaptiven Streamings berücksichtigen. Jedes einzelne Videofragment wird als eigene Entität zwischengespeichert. Stellen Sie sich beispielsweise die erste Wiedergabe eines bestimmten Videos vor. Wenn der Betrachter sich nur einige wenige Sekunden hier und dort ansieht, werden nur die von ihm angesehenen Videofragmente im CDN zwischengespeichert. Mit dem adaptiven Streaming gibt es normalerweise 5 bis 7 unterschiedliche Videobitraten. Wenn ein Benutzer eine Bitrate ansieht und ein anderer Benutzer eine andere Bitrate, werden die Bitraten jeweils separat im CDN zwischengespeichert. Auch wenn zwei Benutzer die gleiche Bitrate ansehen, kann es sein, dass das Streaming über unterschiedliche Protokolle erfolgt. Jedes Protokoll (HLS, MPEG-DASH, Smooth Streaming) wird separat zwischengespeichert. Somit werden jede Bitrate und jedes Protokoll separat zwischengespeichert, und nur die Videofragmente, die angefordert wurden, werden zwischengespeichert.

Bei der Entscheidung, ob das CDN auf dem Media Services-[Streamingendpunkt](streaming-endpoint-concept.md) aktiviert werden soll, ist die Anzahl der erwarteten Viewer zu berücksichtigen. CDN ist nur hilfreich, wenn Sie viele Viewer für Ihre Inhalte erwarten. Wenn die Höchstzahl der gleichzeitigen Betrachter unter 500 liegt, wird empfohlen, CDN zu deaktivieren, da sich das CDN am besten mit der Parallelität skalieren lässt. 

In diesem Thema wird die Aktivierung der [CDN-Integration](#enable-azure-cdn-integration) erörtert. Es erläutert auch die Vorabrufe (aktives Zwischenspeichern) und das Konzept [Origin-Assist CDN-Prefetch](#origin-assist-cdn-prefetch) (Ursprungsunterstützung – CDN-Vorabruf).

## <a name="considerations"></a>Überlegungen

* Der [Streamingendpunkt](streaming-endpoint-concept.md) `hostname` und die Streaming-URL ändern sich nicht, und zwar unabhängig davon, ob Sie CDN aktivieren oder nicht.
* Wenn Sie Ihre Inhalte mit oder ohne CDN testen können möchten, erstellen Sie einen anderen Streamingendpunkt, für den kein CDN aktiviert ist.

## <a name="enable-azure-cdn-integration"></a>Aktivieren der Azure CDN-Integration

> [!IMPORTANT]
> CDN kann nicht für Konten von Azure-Testversionen oder Studentenversionen aktiviert werden.
>
> Die CDN-Integration ist in allen Azure-Rechenzentren mit Ausnahme der Regionen US-Regierung und China aktiviert.

Nachdem ein Streamingendpunkt mit aktiviertem CDN bereitgestellt wurde, gibt es eine definierte Wartezeit auf Media Services, bevor das DNS-Update durchgeführt wird, um den Streamingendpunkt dem CDN-Endpunkt zuzuordnen.

Wenn Sie das CDN später deaktivieren bzw. aktivieren möchten, muss Ihr Streamingendpunkt den Zustand **Beendet** haben. Es kann bis zu zwei Stunden dauern, bis die Azure CDN-Integration aktiviert ist und die Änderungen auf allen CDN-POPs aktiv sind. Sie können jedoch Ihren Streamingendpunkt starten und von ihm aus unterbrechungsfrei streamen. Sobald die Integration abgeschlossen ist, wird der Stream vom CDN übermittelt. Während der Bereitstellungsphase hat Ihr Streamingendpunkt den Zustand **Wird gestartet**, und seine Leistung ist eventuell eingeschränkt.

Wenn der standardmäßige Streamingendpunkt erstellt wird, ist er standardmäßig mit Verizon Standard konfiguriert. Sie können Verizon Premium- oder Akamai Standard-Anbieter über REST-APIs konfigurieren.

Die Azure Media Services-Integration in Azure CDN ist in **Azure CDN von Verizon**für Standard-Streamingendpunkte implementiert. Premium-Streamingendpunkte können mithilfe aller **Azure CDN-Tarife und -Anbieter** konfiguriert werden. 

> [!NOTE]
> Ausführliche Informationen zu Azure CDN finden Sie in der [CDN-Übersicht](../../cdn/cdn-overview.md).

## <a name="determine-if-a-dns-change-was-made"></a>Bestimmen, ob eine DNS-Änderung vorgenommen wurde

Sie können mit https://www.digwebinterface.com feststellen, ob eine DNS-Änderung an einem Streamingendpunkt vorgenommen wurde (der Datenverkehr wird an das Azure CDN weitergeleitet). Wenn die Ergebnisse azureedge.net-Domänennamen in den Ergebnissen enthalten, wird der Datenverkehr nun auf das CDN geleitet.

## <a name="origin-assist-cdn-prefetch"></a>Origin-Assist CDN-Prefetch (Ursprungsunterstützung – CDN-Vorabruf)

Die CDN-Zwischenspeicherung ist ein reaktiver Prozess. Wenn CDN vorhersagen kann, was als nächstes angefordert wird, kann CDN proaktiv das nächste Objekt anfordern und zwischenspeichern. Mit diesem Verfahren können Sie einen Cachetreffer für alle (oder die meisten) Objekte erzielen und damit die Leistung verbessern.

Das Konzept des Vorabrufs zielt darauf ab, Objekte in der Erwartung am „Rand des Internets“ zu positionieren, dass diese vom Player unmittelbar angefordert werden, wodurch die Zeit bis zur Bereitstellung des Objekts für den Player verkürzt wird.

Um dieses Ziel zu erreichen, müssen ein Streamingendpunkt (Ursprung) und das CDN Hand in Hand arbeiten: 

- Der Media Services Ursprung muss über die „Intelligenz“ verfügen (Origin-Assist), um CDN über das nächste Objekt zu informieren, das vorab abgerufen werden soll, und 
- CDN übernimmt den Vorabruf und die Zwischenspeicherung (CDN-Prefetch-Teil). CDN muss auch über die „Intelligenz“ verfügen, um den Ursprung zu informieren, ob es sich um einen Vorabruf oder einen regulären Fetch handelt, Antworten vom Typ 404 zu verarbeiten und eine Möglichkeit zu nutzen, eine endlose Vorabrufschleife zu vermeiden.

### <a name="benefits"></a>Vorteile

Zu den Vorteilen des Features *Origin-Assist CDN-Prefetch* gehören:

- Der Vorabruf verbessert die Qualität der Videowiedergabe durch Folgendes: die Vorabpositionierung der erwarteten Videosegmente im Edge-Bereich während der Wiedergabe, die Verringerung der Latenzzeit für den Viewer und die Verbesserung der Downloadzeiten der Videosegmente. Dies führt zu einer schnelleren Startzeit des Videos und zu weniger erneuten Pufferereignissen.
- Dieses Konzept gilt für das allgemeine Szenario von „CDN-Ursprung“ und ist nicht auf Medien beschränkt.
- Akamai hat dieses Feature zu [Akamai Cloud Embed (ACE)](https://learn.akamai.com/en-us/products/media_delivery/cloud_embed.html) hinzugefügt.

> [!NOTE]
> Dieses Feature ist noch nicht auf das Akamai CDN anwendbar, das in den Streamingendpunkt von Media Services integriert ist. Es ist jedoch für Media Services-Kunden verfügbar, die über einen bereits bestehenden Akamai-Vertrag verfügen und eine benutzerdefinierte Integration zwischen dem Akamai CDN und dem Media Services-Ursprung benötigen.

### <a name="how-it-works"></a>Funktionsweise

CDN-Unterstützung für *Origin-Assist CDN-Prefetch*-Header (für Livestreaming und On-Demand-Videostreaming) ist für Kunden mit direktem Vertrag mit Akamai CDN verfügbar. Das Feature umfasst den folgenden HTTP-Headeraustausch zwischen Akamai CDN und dem Media Services-Ursprung:

|HTTP-Header|Werte|Sender|Receiver|Zweck|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-Assist-Prefetch-Enabled | „1“ (Standardwert) oder „0“ |CDN|Origin|Angabe, dass CDN für den Vorabruf aktiviert ist|
|CDN-Origin-Assist-Prefetch-Path| Beispiel: <br/>Fragments(video=1400000000,format=mpd-time-cmaf)|Origin|CDN|Angabe des Vorabrufpfads zu CDN|
|CDN-Origin-Assist-Prefetch-Request|„1“ (Vorabrufanforderung) oder „0“ (reguläre Anforderung)|CDN|Origin|Angabe, dass die Anforderung von CDN ein Vorabruf ist|

Um einen Teil des Headeraustausches in Aktion zu sehen, können Sie folgende Schritte ausführen:

1. Verwenden Sie Postman oder cURL, um eine Anforderung an den Media Services-Ursprung für ein Audio- oder Videosegment oder Audio- oder Videofragment auszugeben. Fügen Sie den Header „CDN-Origin-Assist-Prefetch-Enabled: 1“ in der Anforderung hinzu.
2. In der Antwort sollte der Header „CDN-Origin-Assist-Prefetch-Path“ mit einem relativen Pfad als Wert angezeigt werden.

### <a name="supported-streaming-protocols"></a>Unterstützte Streamingprotokolle 

Das Feature *Origin-Assist CDN-Prefetch* unterstützt die folgenden Streamingprotokolle für Live- und On-Demand-Streaming:

* HLS v3
* HLS v4
* HLS CMAF
* DASH (CSF)
* DASH (CMAF)
* Smooth Streaming

### <a name="faqs"></a>Häufig gestellte Fragen

* Was geschieht, wenn eine URL des Vorabrufpfads ungültig ist, sodass der CDN-Vorabruf eine Antwort vom Typ 404 erhält? 

    CDN speichert eine 404-Antwort nur für 10 Sekunden (oder einen anderen konfigurierten Wert) zwischen.
* Angenommen, Sie verfügen über ein bedarfsgesteuertes Video. Wenn der CDN-Vorabruf aktiviert ist, bedeutet dieses Feature, dass der Vorabruf einer Schleife startet, sobald ein Client das erste Videosegment anfordert, um alle nachfolgenden Videosegmente mit derselben Bitrate vorab abzurufen? 

    Nein, der CDN-Vorabruf wird nur nach einer vom Client initiierten Anforderung/Antwort durchgeführt. Der CDN-Vorabruf wird niemals durch einen Vorabruf ausgelöst, um eine Vorabrufschleife zu vermeiden. 
* Ist das Feature „Origin-Assist CDN-Prefetch“ immer aktiviert? Wie kann es aktiviert oder deaktiviert werden? 

    Dieses Feature ist standardmäßig deaktiviert. Kunden müssen es über die Akamai-API aktivieren.
* Was würde beim Livestreaming mit „Origin-Assist“ geschehen, wenn das nächste Segment oder Fragment noch nicht verfügbar ist? 

    In diesem Fall liefert der Media Services-Ursprung keinen CDN-Origin-Assist-Prefetch-Pfadheader und der CDN-Vorabruf tritt nicht auf.
* Wie funktioniert *Origin-Assist CDN-Prefetch* mit dynamischen Manifestfiltern? 

    Dieses Feature funktioniert unabhängig vom Manifestfilter. Wenn sich das nächste Fragment außerhalb eines Filterfensters befindet, wird seine URL immer noch durch die Suche im unformatierten Clientmanifest gefunden und dann als Antwortheader für den CDN-Vorabruf zurückgegeben. Das bedeutet, dass das CDN die URL eines Fragments erhält, das aus dem DASH/HLS/Smooth-Manifest herausgefiltert wird. Der Player wird jedoch niemals eine GET-Anforderung an CDN stellen, um dieses Fragment abzurufen, da dieses Fragment nicht im DASH/HLS/Smooth-Manifest des Players enthalten ist (der Player weiß nicht, dass dieses Fragment existiert).
* Kann ein DASH-MPD/HLS-Playlist/Smooth-Manifest vorab abgerufen werden?

    Nein, DASH-MPD, HLS-Masterplaylist, HLS-Variantenplaylist oder die Smooth-Manifest-URL wird dem Vorabrufheader nicht hinzugefügt.
* Sind Vorabruf-URLs relativ oder absolut? 

    Obwohl das Akamai CDN beides zulässt, bietet der Media Services-Ursprung nur relative URLs für den Vorabrufpfad an, da die Verwendung absoluter URLs keinen offensichtlichen Vorteil bietet.
* Funktioniert dieses Feature mit DRM-geschützten Inhalten?

    Ja, da dieses Feature auf HTTP-Ebene arbeitet, werden Segmente/Fragmente weder decodiert noch analysiert. Dabei ist es egal, ob der Inhalt verschlüsselt ist oder nicht.
* Funktioniert dieses Feature mit der serverseitigen Werbeeinblendung (SSAI)?
    
    Für den Original-/Hauptinhalt (der ursprüngliche Videoinhalt vor der Werbeeinblendung) funktioniert es, da SSAI den Zeitstempel des Quellinhalts vom Media Services-Ursprung nicht ändert. Ob dieses Feature mit Werbeinhalten funktioniert, hängt davon ab, ob der Ursprung der Anzeige „Origin-Assist“ unterstützt. Wenn z. B. Werbeinhalte auch in Azure Media Services gehostet werden (gleicher oder separater Ursprung), werden die Werbeinhalte ebenfalls vorab abgerufen.
* Funktioniert dieses Feature mit UHD/HEVC-Inhalten?

    Ja.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

Mit dem Beispiel [in diesem Repository](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) wird gezeigt, wie der Standard-Streamingendpunkt mit .NET gestartet wird.
