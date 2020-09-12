---
title: Azure Kubernetes hizmeti (AKS) denetleyici günlüklerini görüntüleme
description: Azure Kubernetes Service (AKS) ' de Kubernetes ana düğümü için günlükleri etkinleştirme ve görüntüleme hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: a0207ebbb1596e41ad65e21a769d7041a239f767
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90004876"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Kubernetes ana düğüm günlüklerini etkinleştirme ve inceleme

Azure Kubernetes hizmeti (AKS) ile, *kuas-apiserver* ve *kuin-Controller-Manager* gibi ana bileşenler yönetilen bir hizmet olarak sağlanır. *Kubelet* ve kapsayıcı çalışma zamanı 'nı çalıştıran düğümleri oluşturup yönetirsiniz ve uygulamalarınızı yönetilen Kubernetes API sunucusu aracılığıyla dağıtırsınız. Uygulamanızın ve hizmetlerinizin sorunlarını gidermenize yardımcı olmak için bu ana bileşenler tarafından oluşturulan günlükleri görüntülemeniz gerekebilir. Bu makalede, Kubernetes ana bileşenlerinden günlükleri etkinleştirmek ve sorgulamak için Azure Izleyici günlüklerinin nasıl kullanılacağı gösterilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure hesabınızda çalışan mevcut bir AKS kümesi gerekir. Zaten bir AKS kümeniz yoksa, [Azure CLI][cli-quickstart] veya [Azure Portal][portal-quickstart]kullanarak bir tane oluşturun. Azure Izleyici günlükleri hem RBAC hem de RBAC olmayan AKS kümeleriyle birlikte kullanılabilir.

## <a name="enable-resource-logs"></a>Kaynak günlüklerini etkinleştirme

Azure Izleyici günlükleri, birden fazla kaynaktaki verileri toplayıp gözden geçirmesine yardımcı olmak için ortamınıza ilişkin Öngörüler sağlayan bir sorgu dili ve analiz altyapısı sağlar. Çalışma alanı, verileri harmanlamak ve analiz etmek için kullanılır ve Application Insights ve Güvenlik Merkezi gibi diğer Azure hizmetleriyle tümleştirilebilir. Günlükleri çözümlemek üzere farklı bir platform kullanmak için, kaynak günlüklerini bir Azure depolama hesabına veya Olay Hub 'ına göndermek yerine seçebilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici günlükleri nedir?][log-analytics-overview].

Azure Izleyici günlükleri Azure portal etkinleştirilir ve yönetilir. AKS kümenizdeki Kubernetes ana bileşenleri için günlük toplamayı etkinleştirmek üzere, Azure portal bir Web tarayıcısında açın ve aşağıdaki adımları uygulayın:

1. AıKS kümeniz için kaynak grubunu ( *Myresourcegroup*gibi) seçin. *MC_myResourceGroup_myAKSCluster_eastus*gibi bireysel aks kümesi kaynaklarınızı içeren kaynak grubunu seçmeyin.
1. Sol taraftaki **Tanılama ayarları**' nı seçin.
1. *Myakscluster*gibi aks kümenizi seçin ve ardından **Tanılama ayarı eklemeyi**seçin.
1. *Myaksclusterlogs*gibi bir ad girin ve **Log Analytics gönder**seçeneğini belirleyin.
1. Mevcut bir çalışma alanını seçin veya yenisini oluşturun. Bir çalışma alanı oluşturursanız, bir çalışma alanı adı, bir kaynak grubu ve bir konum sağlayın.
1. Kullanılabilir Günlükler listesinde, etkinleştirmek istediğiniz günlükleri seçin. Bu örnekte, *KUIN-denetim* günlüklerini etkinleştirin. Ortak Günlükler *kuin-apiserver*, *kuin-Controller-Manager*ve *kuin-Scheduler*' ı kapsar. Toplama Log Analytics çalışma alanları etkinleştirildikten sonra, toplanan günlükleri döndürebilir ve değiştirebilirsiniz.
1. Hazırsanız, seçili günlüklerin toplanmasını etkinleştirmek için **Kaydet** ' i seçin.

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>AKS kümesinde bir test Pod 'u zamanlama

Bazı günlükler oluşturmak için AKS kümenizde yeni bir pod oluşturun. Aşağıdaki örnek YAML bildirimi, temel bir NGıNX örneği oluşturmak için kullanılabilir. Seçtiğiniz düzenleyicide adlı bir dosya oluşturun `nginx.yaml` ve aşağıdaki içeriği yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

[Kubectl Create][kubectl-create] komutuyla Pod oluşturun ve aşağıdaki örnekte gösterildiği gibi YAML Dosyanızı belirtin:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Toplanan günlükleri görüntüle

Tanılama günlüklerinin etkinleştirilmesi ve gösterilmesi birkaç dakika sürebilir. Azure portal, AKS kümenize gidin ve sol taraftaki **Günlükler** ' i seçin. Görüntülenirse, *örnek sorgular* penceresini kapatın.

Sol taraftaki **Günlükler**' i seçin. *Kuin-denetim* günlüklerini görüntülemek için metin kutusuna aşağıdaki sorguyu girin:

```
KubePodInventory
| where TimeGenerated > ago(1d)
```

Büyük olasılıkla çok sayıda günlük döndürülür. Önceki adımda oluşturulan NGıNX Pod hakkındaki günlükleri görüntülemek için sorguyu aşağı bağlamak üzere, aşağıdaki örnek sorguda gösterildiği gibi *NGINX* için arama yapmak üzere ek bir *WHERE* ifadesini ekleyin:

```
KubePodInventory
| where TimeGenerated > ago(1d)
| where Name contains "nginx"
```

Günlük verilerinizi sorgulama ve filtreleme hakkında daha fazla bilgi için bkz. [Log Analytics günlük araması ile toplanan verileri görüntüleme veya çözümleme][analyze-log-analytics].

## <a name="log-event-schema"></a>Olay şemasını günlüğe kaydet

AKS aşağıdaki olayları günlüğe kaydeder:

* [AzureActivity][log-schema-azureactivity]
* [AzureMetrics][log-schema-azuremetrics]
* [Containerımageınventory][log-schema-containerimageinventory]
* [Containerınventory][log-schema-containerinventory]
* [ContainerLog][log-schema-containerlog]
* [Containernodeınventory][log-schema-containernodeinventory]
* [ContainerServiceLog][log-schema-containerservicelog]
* [Sinyal][log-schema-heartbeat]
* [Insightsölçümlerini][log-schema-insightsmetrics]
* [KubeEvents][log-schema-kubeevents]
* [KubeHealth][log-schema-kubehealth]
* [KubeMonAgentEvents][log-schema-kubemonagentevents]
* [Kubenodeınventory][log-schema-kubenodeinventory]
* [KubePodInventory][log-schema-kubepodinventory]
* [KubeServices][log-schema-kubeservices]
* [Erişen][log-schema-perf]

## <a name="log-roles"></a>Günlük rolleri

| Rol                     | Açıklama |
|--------------------------|-------------|
| *aksService*             | Denetim düzlemi işlemi için denetim günlüğünde görünen ad (hcpService 'ten) |
| *MasterClient*           | MasterClientCertificate için denetim günlüğünde görünen ad, az aks Get-Credentials öğesinden aldığınız sertifika |
| *nodeclient*             | Aracı düğümleri tarafından kullanılan ClientCertificate için görünen ad |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizdeki Kubernetes ana bileşenlerine yönelik günlüklerin nasıl etkinleştirileceğini ve gözden geçirildiğini öğrendiniz. Daha fazla izlemek ve sorun gidermek için [Kubelet günlüklerini görüntüleyebilir][kubelet-logs] ve [SSH düğüm erişimini etkinleştirebilirsiniz][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../azure-monitor/log-query/log-query-overview.md
[analyze-log-analytics]: ../azure-monitor/log-query/get-started-portal.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[log-schema-azureactivity]: /azure/azure-monitor/reference/tables/azureactivity
[log-schema-azuremetrics]: /azure/azure-monitor/reference/tables/azuremetrics
[log-schema-containerimageinventory]: /azure/azure-monitor/reference/tables/containerimageinventory
[log-schema-containerinventory]: /azure/azure-monitor/reference/tables/containerinventory
[log-schema-containerlog]: /azure/azure-monitor/reference/tables/containerlog
[log-schema-containernodeinventory]: /azure/azure-monitor/reference/tables/containernodeinventory
[log-schema-containerservicelog]: /azure/azure-monitor/reference/tables/containerservicelog
[log-schema-heartbeat]: /azure/azure-monitor/reference/tables/heartbeat
[log-schema-insightsmetrics]: /azure/azure-monitor/reference/tables/insightsmetrics
[log-schema-kubeevents]: /azure/azure-monitor/reference/tables/kubeevents
[log-schema-kubehealth]: /azure/azure-monitor/reference/tables/kubehealth
[log-schema-kubemonagentevents]: /azure/azure-monitor/reference/tables/kubemonagentevents
[log-schema-kubenodeinventory]: /azure/azure-monitor/reference/tables/kubenodeinventory
[log-schema-kubepodinventory]: /azure/azure-monitor/reference/tables/kubepodinventory
[log-schema-kubeservices]: /azure/azure-monitor/reference/tables/kubeservices
[log-schema-perf]: /azure/azure-monitor/reference/tables/perf
