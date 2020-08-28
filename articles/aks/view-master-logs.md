---
title: Azure Kubernetes hizmeti (AKS) denetleyici günlüklerini görüntüleme
description: Azure Kubernetes Service (AKS) ' de Kubernetes ana düğümü için günlükleri etkinleştirme ve görüntüleme hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: 721ef4f60d263602b01b5957bfb9bc3b5682a2df
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89048287"
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
AzureDiagnostics
| where Category == "kube-audit"
| project log_s
```

Büyük olasılıkla çok sayıda günlük döndürülür. Önceki adımda oluşturulan NGıNX Pod hakkındaki günlükleri görüntülemek için sorguyu aşağı bağlamak üzere, aşağıdaki örnek sorguda gösterildiği gibi *NGINX* için arama yapmak üzere ek bir *WHERE* ifadesini ekleyin:

```
AzureDiagnostics
| where Category == "kube-audit"
| where log_s contains "nginx"
| project log_s
```

Ek günlükleri görüntülemek için, etkinleştirdiğiniz ek günlüklere bağlı olarak *Kategori* adı sorgusunu *Kuto-Controller-Manager* veya *Kuto-Scheduler*olarak güncelleştirebilirsiniz. Daha sonra, aradığınız olayları iyileştirmek için ek *WHERE* deyimleri kullanılabilir.

Günlük verilerinizi sorgulama ve filtreleme hakkında daha fazla bilgi için bkz. [Log Analytics günlük araması ile toplanan verileri görüntüleme veya çözümleme][analyze-log-analytics].

## <a name="log-event-schema"></a>Olay şemasını günlüğe kaydet

Günlük verilerini çözümlemeye yardımcı olması için aşağıdaki tabloda her bir olay için kullanılan şemanın ayrıntıları verilmiştir:

| Alan adı               | Açıklama |
|--------------------------|-------------|
| *RESOURCEID*             | Günlüğü üreten Azure kaynağı |
| *ışınızda*                   | Günlüğün karşıya yüklendiği zaman damgası |
| *alan*               | Günlüğü oluşturan kapsayıcı/bileşen adı |
| *operationName*          | Her zaman *Microsoft. ContainerService/Managedkümeler/diagnosticLogs/Read* |
| *Properties. log*         | Bileşenden alınan günlüğün tam metni |
| *Properties. Stream*      | *stderr* veya *stdout* |
| *Properties. Pod*         | Günlüğün geldiği Pod adı |
| *Properties. Containerıd* | Bu günlüğün geldiği Docker kapsayıcısının KIMLIĞI |

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
