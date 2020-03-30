---
title: Azure Kubernetes Hizmeti (AKS) denetleyici günlüklerini görüntüleyin
description: Azure Kubernetes Hizmeti'ndeki (AKS) Kubernetes ana düğümünün günlüklerini nasıl etkinleştirip görüntüleyebilirsiniz öğrenin
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: f759f15cf98546cb95ba0adb5890885f85ca6aa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259389"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Kubernetes ana düğüm günlüklerini etkinleştirme ve inceleme

Azure Kubernetes Service (AKS) ile *kube-apiserver* ve *kube-controller-manager* gibi ana bileşenler yönetilen bir hizmet olarak sağlanır. *Kubelet* ve kapsayıcı çalışma süresini çalıştıran düğümleri oluşturur ve yönetir ve uygulamalarınızı yönetilen Kubernetes API sunucusu üzerinden dağıtırsınız. Uygulamanızı ve hizmetlerinizi gidermeye yardımcı olmak için, bu ana bileşenler tarafından oluşturulan günlükleri görüntülemeniz gerekebilir. Bu makalede, Kubernetes ana bileşenlerinden günlükleri etkinleştirmek ve sorgulamak için Azure Monitor günlüklerini nasıl kullanacağınızı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure hesabınızda çalışan varolan bir AKS kümesi gerektirir. Zaten bir AKS kümeniz yoksa, Azure [CLI][cli-quickstart] veya [Azure portalını][portal-quickstart]kullanarak bir tane oluşturun. Azure Monitor günlükleri hem RBAC hem de RBAC özellikli olmayan AKS kümeleriyle çalışır.

## <a name="enable-diagnostics-logs"></a>Tanılama günlüklerini etkinleştirme

Azure Monitor günlükleri, birden çok kaynaktan veri toplamaya ve gözden geçirmeye yardımcı olmak için ortamınıza ilişkin öngörüler sağlayan bir sorgu dili ve analiz motoru sağlar. Bir çalışma alanı verileri harmanlamak ve çözümlemek için kullanılır ve Uygulama Öngörüleri ve Güvenlik Merkezi gibi diğer Azure hizmetleriyle tümleştirilebilir. Günlükleri çözümlemek için farklı bir platform kullanmak için, tanılama günlüklerini bir Azure depolama hesabına veya etkinlik merkezine göndermeyi seçebilirsiniz. Daha fazla bilgi için Azure [Monitor günlükleri nedir?][log-analytics-overview]

Azure Monitor günlükleri Azure portalında etkinleştirilir ve yönetilir. AKS kümenizdeki Kubernetes ana bileşenleri için günlük koleksiyonunu etkinleştirmek için, azure portalını bir web tarayıcısında açın ve aşağıdaki adımları tamamlayın:

1. *MYResourceGroup*gibi AKS kümenizin kaynak grubunu seçin. MC_myResourceGroup_myAKSCluster_eastus gibi tek tek AKS küme kaynaklarınızı içeren kaynak grubunu *seçmeyin.*
1. Sol tarafta **Tanılama ayarlarını**seçin.
1. *MYAKSCluster*gibi AKS kümenizi seçin ve **tanıayar eklemeyi**seçin.
1. *myAKSClusterLogs*gibi bir ad girin, ardından **Günlük Analitiğine Gönder**seçeneğini seçin.
1. Varolan bir çalışma alanı seçin veya yeni bir çalışma oluşturun. Bir çalışma alanı oluşturursanız, bir çalışma alanı adı, kaynak grubu ve bir konum sağlayın.
1. Kullanılabilir günlükler listesinde, etkinleştirmek istediğiniz günlükleri seçin. Ortak günlükleri *kube-apiserver*, *kube-controller-manager*, ve *kube-scheduler*içerir. *Kube-audit* ve *cluster-autoscaler*gibi ek günlükleri etkinleştirebilirsiniz. Log Analytics çalışma alanları etkinleştirildikten sonra toplanan günlükleri iade edebilir ve değiştirebilirsiniz.
1. Hazır olduğunuzda, seçili günlüklerin toplanmasını etkinleştirmek için **Kaydet'i** seçin.

Aşağıdaki örnek portal ekran görüntüsü *Tanılama ayarları* penceresini gösterir:

![AKS kümesinin Azure Monitör günlükleri için Günlük Analizi çalışma alanını etkinleştirme](media/view-master-logs/enable-oms-log-analytics.png)

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>AKS kümesinde bir test bölmesi zamanlama

Bazı günlükler oluşturmak için AKS kümenizde yeni bir bölme oluşturun. Aşağıdaki örnek YAML bildirimi temel bir NGINX örneği oluşturmak için kullanılabilir. Seçtiğiniz bir `nginx.yaml` düzenleyicide adı geçen bir dosya oluşturun ve aşağıdaki içeriği yapıştırın:

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

[Kubectl oluşturma][kubectl-create] komutu ile pod oluşturun ve aşağıdaki örnekte gösterildiği gibi YAML dosyanızı belirtin:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Toplanan günlükleri görüntüleme

Tanılama günlüklerinin etkinleştirilmesi ve Log Analytics çalışma alanında görünmesi birkaç dakika sürebilir. Azure portalında, *myResourceGroup*gibi Günlük Analizi çalışma alanınız için kaynak grubunu seçin ve ardından *myAKSLogs*gibi günlük analiz kaynağınızı seçin.

![AKS kümeniz için Günlük Analizi çalışma alanını seçin](media/view-master-logs/select-log-analytics-workspace.png)

Sol tarafta, **Günlükler'i**seçin. *Kube-apiserver'ı*görüntülemek için metin kutusuna aşağıdaki sorguyu girin:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

Birçok günlükleri büyük olasılıkla API sunucusu için döndürülür. Önceki adımda oluşturulan NGINX pod'u hakkındaki günlükleri görüntülemek için sorguyu kapsamak için, aşağıdaki örnek sorguda gösterildiği gibi *pod/nginx* aramak için ek bir *ifade* ekleyin:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

NGINX bölmeniz için belirli günlükler aşağıdaki örnek ekran görüntüsünde gösterildiği gibi görüntülenir:

![Örnek NGINX pod için günlük analiz sorgu sonuçları](media/view-master-logs/log-analytics-query-results.png)

Ek günlükleri görüntülemek için, kategori *adı* için sorguyu *kube-controller-manager* veya *kube-scheduler'a,* etkinleştirdiğiniz ek günlüklere bağlı olarak güncelleştirebilirsiniz. *İfadelerin* daha sonra aradığınız olayları hassaslaştırmak için kullanılabildiği ek.

Günlük verilerinizin nasıl sorgulandığı ve filtrelenebildiğiniz hakkında daha fazla bilgi için, [günlük analizi günlük aramasıyla toplanan verileri görüntüleme veya analiz etme][analyze-log-analytics]bölümüne bakın.

## <a name="log-event-schema"></a>Günlük olay şeması

Günlük verilerini çözümlemeye yardımcı olmak için, aşağıdaki tablo her olay için kullanılan şemayı ayrıntılarıyla anlatır:

| Alan adı               | Açıklama |
|--------------------------|-------------|
| *Resourceıd*             | Günlüğü oluşturan azure kaynağı |
| *time*                   | Günlüğün yüklendiği zaman ait zaman damgası |
| *category*               | Günlüğü oluşturan kapsayıcının/bileşenin adı |
| *operationName*          | Her zaman *Microsoft.ContainerService/managedClusters/diagnosticLogs/Read* |
| *properties.log*         | Bileşenden günlüğün tam metni |
| *properties.stream*      | *stderr* veya *stdout* |
| *özellikleri.pod*         | Günlüğün geldiği bölme adı |
| *properties.containerID* | Bu günlük gelen docker konteyner kimliği |

## <a name="log-roles"></a>Günlük Rolleri

| Rol                     | Açıklama |
|--------------------------|-------------|
| *aksService*             | Denetim düzlemi işlemi için denetim günlüğündeki görüntü adı (hcpService'den) |
| *masterclient*           | MasterClientCertificate için denetim günlüğündeki ekran adı, az aks get-credentials'dan aldığınız sertifika |
| *düğüm istemcisi*             | Aracı düğümleri tarafından kullanılan ClientCertificate'ın görüntü adı |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizdeki Kubernetes ana bileşenlerinin günlüklerini etkinleştirmeyi ve gözden geçirini öğrendiniz. Daha fazla izlemek ve sorun gidermek için [Kubelet günlüklerini görüntüleyebilir][kubelet-logs] ve [SSH düğümü erişimini etkinleştirebilirsiniz.][aks-ssh]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../azure-monitor/learn/tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
