---
title: Azure Kubernetes Hizmetinde (AKS) Kured ile Linux düğümlerini güncelleştirin ve yeniden başlatın
description: Azure Kubernetes Hizmetinde (AKS) Kured ile Linux düğümlerini nasıl güncelleştirip otomatik olarak yeniden başlattınız öğrenin
services: container-service
ms.topic: article
ms.date: 02/28/2019
ms.openlocfilehash: 74b12c1bc6e2a88582cc357c8091b5590e6bf3cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78191291"
---
# <a name="apply-security-and-kernel-updates-to-linux-nodes-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) Linux düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygulayın

Kümelerinizi korumak için güvenlik güncelleştirmeleri AKS'deki Linux düğümlerine otomatik olarak uygulanır. Bu güncelleştirmeler işletim sistemi güvenlik düzeltmeleri veya çekirdek güncelleştirmeleri içerir. Bu güncelleştirmelerden bazıları işlemi tamamlamak için bir düğüm yeniden başlatma gerektirir. AKS, güncelleştirme işlemini tamamlamak için bu Linux düğümlerini otomatik olarak yeniden başlatmaz.

Windows Server düğümlerini güncel tutma işlemi (şu anda AKS önizlemede) biraz farklıdır. Windows Server düğümleri günlük güncelleştirmeleri almaz. Bunun yerine, en son temel Pencere Sunucusu görüntüsü ve yamaları ile yeni düğümler dağıtan bir AKS yükseltmesi gerçekleştirirsiniz. Windows Server düğümlerini kullanan AKS kümeleri için [AKS'de düğüm havuzuyükseltme][nodepool-upgrade]bölümüne bakın.

Bu makalede, yeniden başlatma gerektiren Linux düğümlerini izlemek için açık kaynak [kured'i (KUbernetes REboot Daemon)][kured] nasıl kullanacağınızı, ardından çalışan bölmelerin ve düğüm yeniden başlatma işleminin yeniden zamanlanmasının otomatik olarak nasıl kullanılacağını gösterir.

> [!NOTE]
> `Kured`Weaveworks tarafından bir açık kaynak projesidir. AKS'de bu projeye en iyi şekilde destek verilmektedir. Ek destek #weave topluluk Slack kanal bulunabilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="understand-the-aks-node-update-experience"></a>AKS düğüm güncelleştirme deneyimini anlama

Bir AKS kümesinde, Kubernetes düğümleriniz Azure sanal makineleri (VM) olarak çalışır. Bu Linux tabanlı VM'ler, işletim sistemi her gece güncellemeleri otomatik olarak denetlemek üzere yapılandırılan bir Ubuntu görüntüsü kullanır. Güvenlik veya çekirdek güncelleştirmeleri varsa, bunlar otomatik olarak indirilir ve yüklenir.

![AKS düğümü güncelleme ve kured ile yeniden başlatma işlemi](media/node-updates-kured/node-reboot-process.png)

Çekirdek güncelleştirmeleri gibi bazı güvenlik güncelleştirmeleri, işlemi sonuçlandırmak için bir düğüm yeniden başlatma gerektirir. Yeniden başlatma gerektiren bir Linux düğümü */var/run/reboot-required*adlı bir dosya oluşturur. Bu yeniden başlatma işlemi otomatik olarak gerçekleşmez.

Düğüm yeniden başlatmaişlemlerini işlemek için kendi iş akışlarınızı `kured` ve işlemlerinizi kullanabilir veya işlemi düzenlemek için kullanabilirsiniz. Ile, `kured`kümedeki her Linux düğümünde bir pod çalıştıran bir [DaemonSet][DaemonSet] dağıtılır. DaemonSet'teki bu bölmeler */var/run/reboot gerekli* dosyanın varlığını izler ve ardından düğümleri yeniden başlatmak için bir işlem başlatır.

### <a name="node-upgrades"></a>Düğüm yükseltmeleri

AKS'de *kümeyükseltmenize* olanak tanıyan ek bir işlem vardır. Yükseltme genellikle Kubernetes'in daha yeni bir sürümüne geçmek içindir, yalnızca düğüm güvenlik güncelleştirmelerini uygulamakla olmaz. BIR AKS yükseltmesi aşağıdaki eylemleri gerçekleştirir:

* En son güvenlik güncelleştirmeleri ve uygulanan Kubernetes sürümüyle yeni bir düğüm dağıtılır.
* Eski bir düğüm kordon altına alındı ve boşaltılır.
* Podlar yeni düğümde zamanlanır.
* Eski düğüm silinir.

Yükseltme etkinliği sırasında aynı Kubernetes sürümünde kalamazsınız. Kubernetes'in daha yeni bir sürümünü belirtmeniz gerekir. Kubernetes'in en son sürümüne yükseltmek için [AKS kümenizi yükseltebilirsiniz.][aks-upgrade]

## <a name="deploy-kured-in-an-aks-cluster"></a>Bir AKS kümesinde kured dağıtma

`kured` DaemonSet dağıtmak için aşağıdaki resmi Kured Helm grafik yükleyin. Bu bir rol ve küme rolü, bağlamalar ve bir hizmet hesabı oluşturur, `kured`sonra kullanarak DaemonSet dağıtır.

```console
# Add the stable Helm repository
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

# Update your local Helm chart repository cache
helm repo update

# Create a dedicated namespace where you would like to deploy kured into
kubectl create namespace kured

# Install kured in that namespace with Helm 3 (only on Linux nodes, kured is not working on Windows nodes)
helm install kured stable/kured --namespace kured --set nodeSelector."beta\.kubernetes\.io/os"=linux
```

Prometheus veya Slack ile tümleştirme gibi ek parametreleri `kured`de yapılandırabilirsiniz. Ek yapılandırma parametreleri hakkında daha fazla bilgi için [kured Helm grafiğine][kured-install]bakın.

## <a name="update-cluster-nodes"></a>Küme düğümlerini güncelleştirme

Varsayılan olarak, AKS'deki Linux düğümleri her akşam güncellemeleri denetler. Beklemek istemiyorsanız, doğru `kured` çalışan denetlemek için el ile bir güncelleştirme gerçekleştirebilirsiniz. İlk olarak, [AKS düğümlerinizden birine SSH][aks-ssh]adımlarını izleyin. Linux düğümüne Bir SSH bağlantınız olduktan sonra güncelleştirmeleri denetleyin ve aşağıdaki gibi uygulayın:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Düğüm yeniden başlatma gerektiren güncelleştirmeler uygulandıysa, bir dosya */var/run/reboot-required*'ye yazılır. `Kured`varsayılan olarak her 60 dakikada bir yeniden başlatma gerektiren düğümleri denetler.

## <a name="monitor-and-review-reboot-process"></a>Yeniden başlatma işlemini izleme ve gözden geçirme

DaemonSet'teki yinelemelerden biri düğüm yeniden başlatmanın gerekli olduğunu algıladığında, Kubernetes API'si aracılığıyla düğüme bir kilit yerleştirilir. Bu kilit düğümüzerinde zamanlanan ek bölmeleri önler. Kilit ayrıca, aynı anda yalnızca bir düğümün yeniden başlatılması gerektiğini de gösterir. Düğüm kordon altına alındıktan sonra, çalışan bölmeler düğümden boşaltılır ve düğüm yeniden başlatılır.

[Kubectl get düğümleri][kubectl-get-nodes] komutunu kullanarak düğümlerin durumunu izleyebilirsiniz. Aşağıdaki örnek çıktı, düğüm yeniden başlatma işlemi için hazırlanırken *Zamanlama Devre Dengelleme* durumuna sahip bir düğüm gösterir:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Güncelleştirme işlemi tamamlandıktan sonra, `--output wide` [kubectl get düğümleri][kubectl-get-nodes] komutunu kullanarak düğümlerin durumunu parametre ile görüntüleyebilirsiniz. Bu ek çıktı, aşağıdaki örnek çıktıda gösterildiği gibi, temel düğümlerin *KERNEL-VERSION'unda* bir fark görmenizi sağlar. *Aks-nodepool1-28993262-0* bir önceki adımda güncellendi ve çekirdek sürüm *4.15.0-1039-azure*gösterir. Güncellenmemiş düğüm *aks-nodepool1-28993262-1* çekirdek sürüm *4.15.0-1037-azure'u*gösterir.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik `kured` güncelleştirme sürecinin bir parçası olarak otomatik olarak Linux düğümleri yeniden başlatmak için nasıl kullanılacağı ayrıntılı. Kubernetes'in en son sürümüne yükseltmek için [AKS kümenizi yükseltebilirsiniz.][aks-upgrade]

Windows Server düğümlerini kullanan AKS kümeleri için [AKS'de düğüm havuzuyükseltme][nodepool-upgrade]bölümüne bakın.

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://hub.helm.sh/charts/stable/kured
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
