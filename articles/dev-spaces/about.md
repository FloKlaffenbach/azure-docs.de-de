---
title: Einführung in Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/07/2019
ms.topic: overview
description: Erfahren Sie, wie Azure Dev Spaces eine schnelle, iterative Kubernetes-Entwicklungsumgebung für Teams in Azure Kubernetes Service-Clustern bietet.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 586b19070ec36517add21f7aac86ddf15121be2d
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771188"
---
# <a name="introduction-to-azure-dev-spaces"></a>Einführung in Azure Dev Spaces

Azure Dev Spaces bietet eine schnelle, iterative Kubernetes-Entwicklungsumgebung für Teams in AKS-Clustern (Azure Kubernetes Service). Azure Dev Spaces ermöglicht es Ihnen auch, alle Komponenten Ihrer Anwendung mit minimalem Einrichtungsaufwand für Entwicklungscomputer in AKS zu debuggen und zu testen, ohne Abhängigkeiten zu replizieren oder zu simulieren.

![](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>So vereinfacht Azure Dev Spaces die Kubernetes-Entwicklung

Dank Azure Dev Spaces können sich Teams auf die Entwicklung und schnelle Iteration ihrer Microserviceanwendung konzentrieren, da dieser Dienst es den Teams ermöglicht, direkt mit der vollständigen Microservicearchitektur oder -anwendung zu arbeiten, die in AKS ausgeführt wird. Azure Dev Spaces bietet auch eine Möglichkeit, Teile der Microservicearchitektur isoliert und unabhängig zu aktualisieren, ohne den Rest des AKS-Clusters oder andere Entwickler zu beeinträchtigen. Azure Dev Spaces eignet sich für Aufgaben in Entwicklungs- und Testumgebungen einer niedrigeren Ebene und ist nicht für die Ausführung in AKS-Produktionsclustern konzipiert.

Da Teams mit der gesamten Anwendung arbeiten und direkt in AKS zusammenarbeiten können, bietet Azure Dev Spaces folgende Vorteile:

* Minimaler Einrichtungsaufwand für lokale Computer
* Schnellere Einrichtung für neue Entwickler im Team
* Schnelleres Arbeiten im Team dank schnellerer Iterationen
* Weniger redundante Entwicklungs- und Integrationsumgebungen, da Teammitglieder einen Cluster gemeinsam nutzen können
* Kein Replizieren oder Simulieren von Abhängigkeiten
* Verbesserte Zusammenarbeit innerhalb der Entwicklungsteams sowie mit anderen Teams, z.B. für DevOps

Azure Dev Spaces stellt Tools zum Generieren von Docker- und Kubernetes-Ressourcen für Ihre Projekte bereit. Mit diesen Tools können Sie ganz einfach neue und vorhandene Anwendungen zu einem Dev Spaces-Cluster- oder anderen AKS-Clustern hinzufügen.

Weitere Informationen zur Funktionsweise von Azure Dev Spaces finden Sie unter [Funktionsweise und Konfiguration von Azure Dev Spaces][how-dev-spaces-works].

## <a name="supported-regions-and-configurations"></a>Unterstützte Regionen und Konfigurationen

Azure Dev Spaces wird nur von AKS-Clustern in [einigen Regionen][supported-regions] unterstützt. Azure Dev Spaces unterstützt die Verwendung der [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder von [Visual Studio Code](https://code.visualstudio.com/download) mit der [Azure Dev Spaces-Erweiterung](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds), die unter Linux, MacOS oder Windows 8 oder höher installiert ist, um Ihre Anwendungen in AKS zu erstellen und auszuführen. [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) unter Windows 8 oder höher wird ebenfalls unterstützt. Um Visual Studio 2019 zu nutzen, benötigen Sie die Workload „Azure-Entwicklung“. Für Visual Studio 2017 benötigen Sie die Workload „Webentwicklung“ sowie die [Visual Studio-Tools für Kubernetes](https://aka.ms/get-vsk8stools).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in der Schnellstartanleitung für die Entwicklung im Team mehr über die schnelle, iterative Entwicklung mit Azure Dev Spaces.

> [!div class="nextstepaction"]
> [Schnellstartanleitung für die Entwicklung im Team](quickstart-team-development.md)


[how-dev-spaces-works]: how-dev-spaces-works.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-quickstart]: quickstart-team-development.md
