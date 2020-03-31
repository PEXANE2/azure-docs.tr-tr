---
title: Azure Kubernetes Hizmetinde (AKS) kubelet günlüklerini görüntüleyin
description: Azure Kubernetes Hizmeti (AKS) düğümlerinden kubelet günlüklerinde sorun giderme bilgilerini nasıl görüntüleyeceğinizi öğrenin
services: container-service
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: b7a74803af916f9e9de72dd528273007ce37832f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595391"
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Azure Kubernetes Service (AKS) kümesi düğümlerinden kubelet günlüklerini alma

AKS kümesini çalıştırmanın bir parçası olarak, bir sorunu gidermek için günlükleri gözden geçirmeniz gerekebilir. Azure portalında yerleşik [olarak, AKS ana bileşenlerinin][aks-master-logs] veya kapsayıcılarının günlüklerini [bir AKS kümesinde][azure-container-logs]görüntüleme olanağı vardır. Bazen, sorun giderme amacıyla bir AKS düğümünden *kubelet* günlükleri almanız gerekebilir.

Bu makalede, bir AKS düğümündeki `journalctl` *kubelet* günlüklerini görüntülemek için nasıl kullanabileceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

## <a name="create-an-ssh-connection"></a>SSH bağlantısı oluşturma

İlk olarak, *kubelet* günlüklerini görüntülemeniz gereken düğümle bir SSH bağlantısı oluşturun. Bu işlem [SSH'de Azure Kubernetes Hizmeti (AKS) küme düğümleri belgesinde][aks-ssh] ayrıntılı olarak açıklanır.

## <a name="get-kubelet-logs"></a>Kubelet günlüklerini alma

Düğüme bağlandıktan *sonra, kubelet* günlüklerini çekmek için aşağıdaki komutu çalıştırın:

```console
sudo journalctl -u kubelet -o cat
```

Aşağıdaki örnek çıktı *kubelet* günlük verilerini gösterir:

```
I0508 12:26:17.905042    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:27.943494    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:28.920125    8672 server.go:796] GET /stats/summary: (10.370874ms) 200 [[Ruby] 10.244.0.2:52292]
I0508 12:26:37.964650    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:47.996449    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:58.019746    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:05.107680    8672 server.go:796] GET /stats/summary/: (24.853838ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:27:08.041736    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:18.068505    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:28.094889    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:38.121346    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:44.015205    8672 server.go:796] GET /stats/summary: (30.236824ms) 200 [[Ruby] 10.244.0.2:52588]
I0508 12:27:48.145640    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:58.178534    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:05.040375    8672 server.go:796] GET /stats/summary/: (27.78503ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:28:08.214158    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:18.242160    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:28.274408    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:38.296074    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:48.321952    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:58.344656    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes yöneticisinden ek sorun giderme bilgilerine ihtiyacınız varsa, [AKS'deki Kubernetes ana düğüm günlüklerini görüntüleyin.][aks-master-logs]

<!-- LINKS - internal -->
[aks-ssh]: ssh.md
[aks-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-master-logs]: view-master-logs.md
[azure-container-logs]: ../azure-monitor/insights/container-insights-overview.md
