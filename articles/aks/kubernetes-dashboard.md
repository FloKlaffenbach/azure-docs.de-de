---
title: Verwalten eines Azure Kubernetes Service-Clusters mit dem Webdashboard
description: Erfahren Sie mehr über die Verwendung des integrierten Kubernetes-Dashboards mit Webbenutzeroberfläche zur Verwaltung eines Azure Kubernetes Service-Clusters (AKS).
services: container-service
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 15fcf765be0a754575713eebcdaa7d68e1c299b9
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77595347"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Zugreifen auf das Kubernetes-Webdashboard in Azure Kubernetes Service (AKS)

Kubernetes enthält ein Webdashboard, das für einfache Verwaltungsvorgänge verwendet werden kann. In diesem Dashboard können Sie den Integritätsstatus und grundlegende Metriken für Ihre Anwendungen anzeigen, Dienste erstellen und bereitstellen und vorhandene Anwendungen bearbeiten. In diesem Artikel wird erläutert, wie Sie mithilfe von Azure CLI auf das Kubernetes-Dashboard zugreifen können. Anschließend werden einige grundlegende Vorgänge im Dashboard vorgestellt.

Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Web UI (Dashboard)][kubernetes-dashboard] (Webbenutzeroberfläche (Dashboard)).

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie einen AKS-Cluster erstellen müssen, finden Sie weitere Informationen in der [AKS-Schnellstartanleitung][aks-quickstart].

Außerdem muss die Version 2.0.46 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie  `az --version`  aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Starten des Kubernetes-Dashboards

Verwenden Sie den Befehl [az aks browse][az-aks-browse], um das Kubernetes-Dashboard zu starten. Im folgenden Beispiel wird das Dashboard für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* geöffnet:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Der Befehl erstellt einen Proxy zwischen Ihrem Entwicklungssystem und der Kubernetes-API und öffnet einen Webbrowser mit dem Kubernetes-Dashboard. Wenn ein Webbrowser das Kubernetes-Dashboard nicht öffnet, kopieren Sie die in der Azure-Befehlszeilenschnittstelle angegebene URL-Adresse (in der Regel `http://127.0.0.1:8001`).

<!--
![The login page of the Kubernetes web dashboard](./media/kubernetes-dashboard/dashboard-login.png)

You have the following options to sign in to your cluster's dashboard:

* A [kubeconfig file][kubeconfig-file]. You can generate a kubeconfig file using [az aks get-credentials][az-aks-get-credentials].
* A token, such as a [service account token][aks-service-accounts] or user token. On [AAD-enabled clusters][aad-cluster], this token would be an AAD token. You can use `kubectl config view` to list the tokens in your kubeconfig file. For more details on creating an AAD token for use with an AKS cluster see [Integrate Azure Active Directory with Azure Kubernetes Service using the Azure CLI][aad-cluster].
* The default dashboard service account, which is used if you click *Skip*.

> [!WARNING]
> Never expose the Kubernetes dashboard publicly, regardless of the authentication method used.
> 
> When setting up authentication for the Kubernetes dashboard, it is recommended that you use a token over the default dashboard service account. A token allows each user to use their own permissions. Using the default dashboard service account may allow a user to bypass their own permissions and use the service account instead.
> 
> If you do choose to use the default dashboard service account and your AKS cluster uses RBAC, a *ClusterRoleBinding* must be created before you can correctly access the dashboard. By default, the Kubernetes dashboard is deployed with minimal read access and displays RBAC access errors. A cluster administrator can choose to grant additional access to the *kubernetes-dashboard* service account, however this can be a vector for privilege escalation. You can also integrate Azure Active Directory authentication to provide a more granular level of access.
>
> To create a binding, use the [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] command as shown in the following example. **This sample binding does not apply any additional authentication components and may lead to insecure use.**
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> You can now access the Kubernetes dashboard in your RBAC-enabled cluster. To start the Kubernetes dashboard, use the [az aks browse][az-aks-browse] command as detailed in the previous step.
>
> If your cluster does not use RBAC, it is not recommended to create a *ClusterRoleBinding*.
> 
> For more information on using the different authentication methods, see the Kubernetes dashboard wiki on [access controls][dashboard-authentication].

After you choose a method to sign in, the Kubernetes dashboard is displayed. If you chose to use *token* or *skip*, the Kubernetes dashboard will use the permissions of the currently logged in user to access the cluster.
-->

> [!IMPORTANT]
> Wenn Ihr AKS-Cluster RBAC verwendet, muss ein *ClusterRoleBinding*-Element erstellt werden, bevor Sie ordnungsgemäß auf das Dashboard zugreifen können. Das Kubernetes-Dashboard wird standardmäßig mit minimalem Lesezugriff bereitgestellt und zeigt RBAC-Zugriffsfehler an. Das Kubernetes-Dashboard unterstützt derzeit keine vom Benutzer bereitgestellten Anmeldeinformationen für die Ermittlung der Zugriffsebene, sondern verwendet die dem Dienstkonto zugewiesenen Rollen. Ein Clusteradministrator kann zusätzlichen Zugriff auf das Dienstkonto *kubernetes-dashboard* gewähren, dies kann jedoch ein Vektor für Berechtigungsausweitung sein. Sie können auch Azure Active Directory-Authentifizierung integrieren, um differenziertere Zugriffsmöglichkeiten zu bieten.
> 
> Verwenden Sie den Befehl [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding], um eine Bindung zu erstellen. Im folgenden Beispiel wird das Erstellen einer Bindung gezeigt. Diese Beispielbindung wendet jedoch keine zusätzlichen Authentifizierungskomponenten an und kann daher zu unsicherer Verwendung führen. Das Kubernetes-Dashboard steht allen Personen zur Verfügung, die Zugriff auf die URL haben. Machen Sie das Kubernetes-Dashboard nicht öffentlich verfügbar.
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> Weitere Informationen zur Verwendung der unterschiedlichen Authentifizierungsmethoden finden Sie im Wiki des Kubernetes-Dashboards unter [Zugriffssteuerung][dashboard-authentication].

![Die Übersichtsseite des Kubernetes-Webdashboards](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Erstellen einer Anwendung

Um zu demonstrieren, wie das Kubernetes-Dashboard die Komplexität von Verwaltungsaufgaben verringern kann, erstellen wir nun eine Anwendung. Sie können eine Anwendung über das Kubernetes-Dashboard erstellen, indem Sie Texteingaben oder eine YAML-Datei bereitstellen oder einen grafischen Assistenten verwenden.

Um eine Anwendung zu erstellen, führen Sie die folgenden Schritte aus:

1. Wählen Sie die Schaltfläche **Erstellen** in der rechten oberen Ecke aus.
1. Um den grafischen Assistenten zu verwenden, wählen Sie **App erstellen** aus.
1. Geben Sie einen Namen für die Bereitstellung an, z.B. *nginx*.
1. Geben Sie den Namen des zu verwendenden Containerimages ein, z.B. *nginx:1.15.5*.
1. Um Port 80 für Webdatenverkehr verfügbar zu machen, erstellen Sie einen Kubernetes-Dienst. Wählen Sie unter **Dienst** die Option **Extern** aus, und geben Sie für den Port und den Zielport jeweils **80** ein.
1. Wenn Sie fertig sind, wählen Sie **Bereitstellen** aus, um die App zu erstellen.

![Bereitstellen einer App im Kubernetes-Webdashboard](./media/kubernetes-dashboard/create-app.png)

Es dauert einen Moment, bis eine externe öffentliche IP-Adresse dem Kubernetes-Dienst zugewiesen wird. Wählen Sie auf der linken Seite unter **Discovery and Load Balancing** (Ermittlung und den Lastenausgleich) die Option **Dienste** aus. Der Dienst Ihrer Anwendung wird aufgeführt, einschließlich der *externen Endpunkte*, wie im folgenden Beispiel gezeigt:

![Anzeigen der Liste der Dienste und Endpunkte](./media/kubernetes-dashboard/view-services.png)

Wählen Sie die Endpunktadresse aus, um ein Webbrowserfenster auf der NGINX-Standardseite zu öffnen:

![Anzeigen der NGINX-Standardseite der bereitgestellten Anwendung](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Anzeigen von Podinformationen

Das Kubernetes-Dashboard kann grundlegende Überwachungsmetriken und Informationen zur Problembehandlung wie z.B. Protokolle bereitstellen.

Um weitere Informationen zu Ihren Anwendungspods anzuzeigen, wählen Sie im linken Menü **Pods** aus. Die Liste der verfügbaren Pods wird angezeigt. Wählen Sie Ihren *nginx*-Pod aus, um die Informationen anzuzeigen, etwa den Ressourcenverbrauch:

![Anzeigen von Podinformationen](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Bearbeiten der Anwendung

Über das Kubernetes-Dashboard können Sie nicht nur Anwendungen erstellen und anzeigen, sondern auch Anwendungsbereitstellungen bearbeiten und aktualisieren. Um zusätzliche Redundanz für die Anwendung bereitzustellen, erhöhen Sie die Anzahl der NGINX-Replikate.

So bearbeiten Sie eine Bereitstellung

1. Wählen Sie im linken Menü **Bereitstellungen** und dann Ihre *nginx*-Bereitstellung aus.
1. Wählen Sie auf der Navigationsleiste rechts oben **Bearbeiten** aus.
1. Suchen Sie den Wert `spec.replica` in der Nähe von Zeile 20. Um die Anzahl der Replikate für die Anwendung zu erhöhen, ändern Sie diesen Wert von *1* in *3*.
1. Klicken Sie abschließend auf **Aktualisieren**.

![Bearbeiten der Bereitstellung zum Aktualisieren der Anzahl von Replikaten](./media/kubernetes-dashboard/edit-deployment.png)

Es dauert einige Zeit, bis die neuen Pods in einer Replikatgruppe erstellt werden. Wählen Sie im linken Menü **Replikatsätze** und dann Ihre *nginx*-Replikatgruppe aus. Die Liste der Pods entspricht nun der aktualisierten Replikatanzahl, wie in der folgenden Beispielausgabe gezeigt:

![Anzeigen von Informationen zur Replikatgruppe](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Kubernetes-Dashboard mit Webbenutzeroberfläche][kubernetes-dashboard].

<!-- LINKS - external -->
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubeconfig-file]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aad-cluster]: ./azure-ad-integration-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[aks-service-accounts]: ./concepts-identity.md#kubernetes-service-accounts
[az-account-get-access-token]: /cli/azure/account?view=azure-cli-latest#az-account-get-access-token
[az-aks-browse]: /cli/azure/aks#az-aks-browse
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
