---
title: 'Designer: Beispiel zur Vorhersage von Autopreisen (Erweitert)'
titleSuffix: Azure Machine Learning
description: Erstellen und vergleichen Sie mehrere ML-Regressionsmodelle, um den Preis eines Autos basierend auf technischen Merkmalen mit dem Azure Machine Learning-Designer vorherzusagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: a80a1567c84ff3c2eda8ad22391aa862bb7d9d82
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77915825"
---
# <a name="train--compare-multiple-regression-models-to-predict-car-prices-with-azure-machine-learning-designer"></a>Trainieren und vergleichen Sie mehrere Regressionsmodelle, um Autopreise mit dem Azure Machine Learning-Designer vorherzusagen.

**Designer (Vorschauversion) – Beispiel 2**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Erfahren Sie, wie Sie im Designer (Vorschauversion) eine komplexe Machine Learning-Pipeline erstellen, ohne eine einzige Codezeile zu schreiben. In diesem Beispiel werden mehrere Regressionsmodelle trainiert und verglichen, um den Preis eines Fahrzeugs anhand seiner technischen Merkmale vorherzusagen. Wir stellen die Gründe für die Entscheidungen bereit, die in dieser Pipeline getroffen wurden, damit Sie Ihre eigenen Probleme durch maschinelles Lernen lösen können.

Wenn Sie noch keine Erfahrung mit maschinellem Lernen haben, sehen Sie sich zunächst die [Basisversion](how-to-designer-sample-regression-automobile-price-basic.md) dieser Pipeline an.

Der fertige Graph für diese Pipeline sieht wie folgt aus:

[![Graph der Pipeline](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/graph.png)](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/graph.png#lightbox)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 2, um es zu öffnen. 

## <a name="pipeline-summary"></a>Pipelineübersicht

Führen Sie die folgenden Schritte aus, um die Machine Learning-Pipeline zu erstellen:

1. Abrufen der Daten.
1. Vorverarbeiten der Daten.
1. Trainieren des Modells.
1. Testen, Auswerten und Vergleichen der Modelle.

## <a name="get-the-data"></a>Abrufen von Daten

In diesem Beispiel wird das Dataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) aus dem UCI Machine Learning-Repository verwendet. Dieses Dataset enthält 26 Spalten mit Informationen zu Automobilen, darunter Marke, Modell, Preis, Fahrzeugmerkmale (etwa die Anzahl der Zylinder), Treibstoffverbrauch und eine Versicherungsrisikobewertung.

## <a name="pre-process-the-data"></a>Vorverarbeiten der Daten

Zu den Hauptaufgaben der Datenaufbereitung gehören die Datenbereinigung, Integration, Transformation, Reduktion und Diskretisierung oder Quantisierung. Im Designer finden Sie Module für diese Vorgänge und andere Aufgaben der Datenvorverarbeitung in der Gruppe **Datentransformation** im linken Bereich.

Verwenden Sie das Modul **Select Columns in Dataset** (Spalten in Dataset auswählen), um normalisierte Verluste auszuschließen, die viele fehlende Werte aufweisen. Anschließend verwenden wir **Clean Missing Data** (Fehlende Daten bereinigen), um die Zeilen zu entfernen, die fehlende Werte aufweisen. Dies hilft, einen bereinigten Satz von Trainingsdaten zu erstellen.

![Datenvorverarbeitung](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Trainieren des Modells

Machine Learning-Probleme sind vielfältig. Machine Learning-Aufgaben sind z.B. Klassifizierung, Clustering, Regression und Empfehlungssysteme, die möglicherweise jeweils einen anderen Algorithmus erfordern. Die Auswahl des Algorithmus hängt häufig von den Anforderungen des Anwendungsfalls ab. Nachdem Sie einen Algorithmus ausgewählt haben, müssen Sie seine Parameter optimieren, um ein genaueres Modell zu trainieren. Sie müssen alle Modelle basierend auf Metriken wie Genauigkeit, Verständlichkeit und Effizienz auswerten.

Da das Ziel dieser Pipeline darin besteht, Automobilpreise vorherzusagen, und die Bezeichnungsspalte (Preis) reelle Zahlen enthält, ist ein Regressionsmodell eine gute Wahl.

Um die Leistung verschiedener Algorithmen zu vergleichen, verwenden wir zwei nichtlineare Algorithmen (**Boosted Decision Tree Regression** und **Decision Forest Regression**), um Modelle zu erstellen. Beide Algorithmen haben Parameter, die Sie ändern können, aber in diesem Beispiel werden die Standardwerte für diese Pipeline verwendet.

Verwenden Sie das Modul **Split Data** (Daten teilen), um nach dem Zufallsprinzip die eingegebenen Daten so aufzuteilen, dass das Trainingsdataset 70 % der ursprünglichen Daten enthält und das Testdataset 30 % der ursprünglichen Daten.

## <a name="test-evaluate-and-compare-the-models"></a>Testen, Auswerten und Vergleichen der Modelle

Sie verwenden zwei verschiedene Sätze von zufällig ausgewählten Daten zum Trainieren und anschließenden Testen des Modells, wie im vorherigen Abschnitt beschrieben. Teilen Sie das Dataset auf, und verwenden Sie unterschiedliche Datasets zum Trainieren und Testen des Modells, um die Auswertung des Modells objektiver zu gestalten.

Nachdem das Modell trainiert wurde, verwenden Sie die Module **Score Model** (Modell bewerten) und **Evaluate Model** (Modell auswerten), um die vorhergesagten Ergebnisse zu generieren und die Modelle auszuwerten. **Score Model** generiert Vorhersagen für das Testdataset mithilfe des trainierten Modells. Übergeben Sie die Bewertungen dann an **Evaluate Model**, um Auswertungsmetriken zu generieren.



Dies sind die Ergebnisse:

![Vergleichen der Ergebnisse](./media/how-to-designer-sample-regression-automobile-price-compare-algorithms/result.png)

Diese Ergebnisse zeigen, dass das Modell, das mit **Boosted Decision Tree Regression** erstellt wurde, einen geringeren mittleren quadratischen Fehler als das Modell aufweist, das mit **Decision Forest Regression** erstellt wurde.



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)
