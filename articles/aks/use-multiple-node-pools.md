---
title: Azure Kubernetes hizmetinde (AKS) birden çok düğüm havuzu kullanma
description: Azure Kubernetes Service (AKS) ' de bir küme için birden çok düğüm havuzu oluşturma ve yönetme hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 08/9/2019
ms.author: mlearned
ms.openlocfilehash: 92accf4317ef8d0e3837ce3789615b5aaf6f6919
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996891"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Önizleme-Azure Kubernetes Service (AKS) ' de bir küme için birden çok düğüm havuzu oluşturma ve yönetme

Azure Kubernetes hizmeti 'nde (AKS), aynı yapılandırmanın düğümleri *düğüm havuzlarında*birlikte gruplandırılır. Bu düğüm havuzları, uygulamalarınızı çalıştıran temel VM 'Leri içerir. Bir *varsayılan düğüm havuzu*oluşturan bir aks kümesi oluşturduğunuzda, ilk düğüm sayısı ve boyutu (SKU) tanımlanmıştır. Farklı işlem veya depolama taleplerine sahip uygulamaları desteklemek için ek düğüm havuzları oluşturabilirsiniz. Örneğin, işlem yoğunluklu uygulamalar için GPU 'Lar sağlamak veya yüksek performanslı SSD depolamaya erişmek için bu ek düğüm havuzlarını kullanın.

> [!NOTE]
> Bu özellik, birden çok düğüm havuzunun oluşturulması ve yönetilmesi üzerinde daha yüksek denetim sağlar. Sonuç olarak, oluşturma/güncelleştirme/silme için ayrı komutlar gerekir. Daha önce, managedcluster `az aks update` API 'yi aracılığıyla veya kullanarak `az aks create` işlem yapın ve denetim düzlemini ve tek bir düğüm havuzunu değiştirmek için tek seçenektir. Bu özellik, agentpool API 'si aracılığıyla aracı havuzları için ayarlanan ayrı bir işlem sunar ve tek bir düğüm `az aks nodepool` havuzunda işlemleri yürütmek için komut kümesinin kullanılmasını gerektirir.

Bu makalede bir AKS kümesinde birden çok düğüm havuzu oluşturma ve yönetme konusu gösterilmektedir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.61 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Birden çok düğüm havuzu kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.12 veya daha yüksek olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-multiple-node-pool-feature-provider"></a>Birden çok düğüm havuzu Özellik sağlayıcısını Kaydet

Birden çok düğüm havuzu kullanan bir AKS kümesi oluşturmak için, öncelikle aboneliğinizde bir özellik bayrağını etkinleştirin. Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak *Multiagentpoolpreview* Özellik bayrağını kaydedin:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> *Multiagentpoolpreview* 'ı başarıyla kaydettikten sonra oluşturduğunuz aks kümesi, bu önizleme kümesi deneyimini kullanır. Düzenli, tam olarak desteklenen kümeler oluşturmaya devam etmek için üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* Birden çok düğüm havuzu yalnızca, aboneliğiniz için *Multiagentpoolpreview* özelliğini başarıyla kaydettikten sonra oluşturulan kümeler için kullanılabilir. Bu özellik başarıyla kaydedilmeden önce oluşturulan mevcut bir AKS kümesiyle düğüm havuzları ekleyemez veya yönetemezsiniz.
* Varsayılan (ilk) düğüm havuzunu silemezsiniz.
* HTTP uygulama yönlendirme eklentisi kullanılamıyor.
* Birçok işlem ile olduğu gibi mevcut bir Kaynak Yöneticisi şablonu kullanarak düğüm havuzları ekleyemez veya silemezsiniz. Bunun yerine, bir AKS kümesindeki düğüm havuzlarında değişiklik yapmak için [ayrı bir kaynak yöneticisi şablonu kullanın](#manage-node-pools-using-a-resource-manager-template) .

Bu özellik önizlemedeyken aşağıdaki ek sınırlamalar geçerlidir:

* AKS kümesinde en fazla sekiz düğüm havuzu olabilir.
* AKS kümesi, bu sekiz düğüm havuzunda en fazla 400 düğüme sahip olabilir.
* Tüm düğüm havuzları aynı alt ağda bulunmalıdır.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Başlamak için, tek düğümlü havuz ile bir AKS kümesi oluşturun. Aşağıdaki örnek, *eastus* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu kullanır. *Myakscluster* adlı bir aks kümesi daha sonra [az aks Create][az-aks-create] komutu kullanılarak oluşturulur. Bir *--Kubernetes-* *1.13.10* , aşağıdaki adımlarda bir düğüm havuzunun nasıl güncelleştiğine göstermek için kullanılır. [Desteklenen Kubernetes sürümünü][supported-versions]belirtebilirsiniz.

Birden çok düğüm havuzu kullanılırken standart SKU yük dengeleyicinin kullanılması kesinlikle önerilir. AKS ile standart yük dengeleyiciler kullanma hakkında daha fazla bilgi edinmek için [Bu belgeyi](load-balancer-standard.md) okuyun.

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 2 \
    --generate-ssh-keys \
    --kubernetes-version 1.13.10 \
    --load-balancer-sku standard
```

Kümenin oluşturulması birkaç dakika sürer.

> [!NOTE]
> Kümenizin güvenilir bir şekilde çalışmasını sağlamak için, bu düğüm havuzunda önemli sistem hizmetleri çalıştığı için varsayılan düğüm havuzunda en az 2 (iki) düğüm çalıştırmalısınız.

Küme kullanıma hazırsa, şu ile `kubectl`kullanılacak küme kimlik bilgilerini almak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Düğüm havuzu ekleme

Önceki adımda oluşturulan kümenin tek bir düğüm havuzu vardır. [Az aks nodepool Add][az-aks-nodepool-add] komutunu kullanarak ikinci bir düğüm havuzu ekleyelim. Aşağıdaki örnek *3* düğümleri çalıştıran *mynodepool* adlı bir düğüm havuzu oluşturur:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --kubernetes-version 1.12.7
```

Düğüm havuzlarınızın durumunu görmek için [az aks node Pool List][az-aks-nodepool-list] komutunu kullanın ve kaynak grubunuzu ve küme adınızı belirtin:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster
```

Aşağıdaki örnek çıktı, düğüm havuzundaki üç düğüm ile *mynodepool* başarıyla oluşturulduğunu gösterir. Önceki adımda AKS kümesi oluşturulduğunda, düğüm sayısı *2*olan varsayılan bir *nodepool1* oluşturulmuştur.

```console
$ az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.12.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

> [!TIP]
> Bir düğüm havuzu eklediğinizde bir *Orchestratorversion* veya *VMSize* belirtilmemişse düğümler, aks kümesinin varsayılan değerleri temel alınarak oluşturulur. Bu örnekte, Kubernetes sürüm *1.13.10* ve *Standard_DS2_v2*düğüm boyutudur.

## <a name="upgrade-a-node-pool"></a>Düğüm havuzunu yükseltme
 
> [!NOTE]
> Bir hata denendiğinde, bir küme veya düğüm havuzundaki yükseltme ve ölçeklendirme işlemleri aynı anda gerçekleşemez. Bunun yerine, her işlem türünün aynı kaynaktaki bir sonraki istekten önce hedef kaynakta tamamlaması gerekir. [Sorun giderme kılavuzumuzdan](https://aka.ms/aks-pending-upgrade)bu konuda daha fazla bilgi edinin.

Aks kümeniz ilk adımda ilk kez oluşturulduğunda, bir `--kubernetes-version` *1.13.10* belirtildiğinde. Bu, Kubernetes sürümünü hem denetim düzlemi hem de varsayılan düğüm havuzu için ayarlar. Bu bölümdeki komutlar, tek bir belirli düğüm havuzunun nasıl yükseltileceğini açıklamaktadır. Denetim düzlemi ve düğüm havuzunun Kubernetes sürümünü yükseltme arasındaki ilişki [aşağıdaki bölümde](#upgrade-a-cluster-control-plane-with-multiple-node-pools)açıklanmıştır.

> [!NOTE]
> Düğüm havuzu işletim sistemi görüntüsü sürümü, kümenin Kubernetes sürümüne bağlıdır. Yalnızca bir küme yükseltmesini izleyerek işletim sistemi görüntüsü yükseltmelerini alacaksınız.

Bu örnekte iki düğüm havuzu olduğundan, bir düğüm havuzunu yükseltmek için [az aks nodepool Upgrade][az-aks-nodepool-upgrade] kullanmanız gerekir. *Mynodepool* , Kubernetes *1.13.10*'e yükseltelim. Aşağıdaki örnekte gösterildiği gibi, düğüm havuzunu yükseltmek için [az aks nodepool Upgrade][az-aks-nodepool-upgrade] komutunu kullanın:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.13.10 \
    --no-wait
```

> [!Tip]
> Denetim düzlemi 'ni *1.14.6*' e yükseltmek için öğesini `az aks upgrade -k 1.14.6`çalıştırın. [Birden çok düğümlü havuzlarla denetim düzlemi yükseltmeleri](#upgrade-a-cluster-control-plane-with-multiple-node-pools)hakkında daha fazla bilgi edinin.

[Az aks düğüm havuzu listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarınızın durumunu yeniden listeleyin. Aşağıdaki örnek, *mynodepool* 'in *yükseltme* durumunda olduğunu *gösterir:*

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Upgrading",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Düğümlerin belirtilen sürüme yükseltilmesi birkaç dakika sürer.

En iyi uygulama olarak, bir AKS kümesindeki tüm düğüm havuzlarını aynı Kubernetes sürümüne yükseltmeniz gerekir. Tek tek düğüm havuzlarını yükseltebilme özelliği, yukarıdaki kısıtlamalar dahilinde uygulama çalışma süresini korumak için sıralı yükseltme gerçekleştirmenize ve düğüm havuzları arasında dizin zamanlamauygulamanıza olanak tanır.

## <a name="upgrade-a-cluster-control-plane-with-multiple-node-pools"></a>Birden çok düğümlü havuzlarla küme denetim düzlemi 'ni yükseltme

> [!NOTE]
> Kubernetes, standart [anlamsal sürüm](https://semver.org/) oluşturma düzenini kullanır. Sürüm numarası *x. y. z*olarak ifade edilir; burada *x* ana sürümdür, *y* ise ikincil sürümdür ve *z* , yama sürümüdür. Örneğin, sürüm *1.12.6*' de, 1 ana sürümdür, 12 ise ikincil sürümdür ve 6 Düzeltme Eki sürümüdür. Denetim düzleminin Kubernetes sürümü ve ilk düğüm havuzu küme oluşturma sırasında ayarlanır. Tüm ek düğüm havuzlarının, kümeye eklendiğinde Kubernetes sürümü ayarlanmış olmalıdır. Kubernetes sürümleri, düğüm havuzlarının yanı sıra düğüm havuzu ile denetim düzlemi arasında farklılık gösterebilir, ancak aşağıdaki kısıtlamalar geçerlidir:
> 
> * Düğüm havuzu sürümü, denetim düzlemi ile aynı ana sürüme sahip olmalıdır.
> * Düğüm havuzu sürümü, denetim düzlemi sürümünden düşük bir alt sürüm olabilir.
> * Düğüm havuzu sürümü, diğer iki kısıtlama izlendiği sürece herhangi bir yama sürümü olabilir.

AKS kümesi iki küme kaynak nesnesine sahiptir. Birincisi, bir denetim düzlemi Kubernetes sürümüdür. İkincisi, Kubernetes sürümü olan bir aracı havuzudur. Bir denetim düzlemi bir veya daha fazla düğüm havuzlarıyla eşlenir ve her birinin kendi Kubernetes sürümü vardır. Bir yükseltme işleminin davranışı hangi kaynağın hedeflendiğinden ve temel alınan API 'nin hangi sürümünün çağrıldığı üzerinde farklılık gösterir.

1. Denetim düzlemi 'nin yükseltilmesi için kullanılması gerekir`az aks upgrade`
   * Bu, kümedeki tüm düğüm havuzlarını de yükseltir
1. İle yükseltme`az aks nodepool upgrade`
   * Bu, yalnızca hedef düğüm havuzunu belirtilen Kubernetes sürümüyle yükseltecek

Düğüm havuzlarının tuttuğu Kubernetes sürümleri arasındaki ilişki Ayrıca bir kural kümesine uymalıdır.

1. Denetim düzlemi veya düğüm havuzu Kubernetes sürümünü indirgeyemezsiniz.
1. Düğüm havuzu Kubernetes sürümü belirtilmemişse, kullanılan varsayılan değer denetim düzlemi sürümüne geri dönecektir.
1. Belirli bir zamanda bir denetim düzlemi veya düğüm havuzu yükseltebilir ya da ölçeklendirebilirsiniz, her iki işlemi de aynı anda gönderemezsiniz.
1. Düğüm havuzu Kubernetes sürümü, denetim düzlemi ile aynı ana sürüm olmalıdır.
1. Düğüm havuzu Kubernetes sürümü en fazla iki (2) alt sürüm olan denetim düzleden daha düşük, hiçbir zaman daha az olabilir.
1. Düğüm havuzu, hiçbir Kubernetes düzeltme eki veya denetim düzlemine eşit veya daha küçük olabilir.

## <a name="scale-a-node-pool-manually"></a>Düğüm havuzunu el ile ölçeklendirme

Uygulama iş yükünüz değiştikçe, bir düğüm havuzundaki düğüm sayısını ölçeklendirmeniz gerekebilir. Düğüm sayısı yukarı veya aşağı ölçeklendirilebilirler.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Düğüm havuzundaki düğüm sayısını ölçeklendirmek için [az aks node Pool Scale][az-aks-nodepool-scale] komutunu kullanın. Aşağıdaki örnek, *mynodepool* içindeki düğüm sayısını *5*olarak ölçeklendirir:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

[Az aks düğüm havuzu listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarınızın durumunu yeniden listeleyin. Aşağıdaki örnek, *mynodepool* 'in yeni sayısı *5* düğüm olan *ölçekleme* durumunda olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Scaling",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Ölçek işleminin tamamlanabilmesi birkaç dakika sürer.

## <a name="scale-a-specific-node-pool-automatically-by-enabling-the-cluster-autoscaler"></a>Küme otomatik Scaler 'ı etkinleştirerek belirli bir düğüm havuzunu otomatik olarak ölçeklendirin

AKS, düğüm havuzlarını [küme](cluster-autoscaler.md)otomatik olarak adlandırılan bir özellik ile otomatik olarak ölçeklendirmek için önizleme aşamasında ayrı bir özellik sunar. Bu özellik, düğüm havuzu başına benzersiz minimum ve maksimum ölçek sayısı olan düğüm havuzu başına etkinleştirilebilen bir AKS eklentisi. [Düğüm havuzu başına küme otomatik Scaler 'ı kullanmayı](cluster-autoscaler.md#use-the-cluster-autoscaler-with-multiple-node-pools-enabled)öğrenin.

## <a name="delete-a-node-pool"></a>Düğüm havuzunu silme

Artık havuz gerekmiyorsa, onu silebilir ve temel alınan VM düğümlerini kaldırabilirsiniz. Bir düğüm havuzunu silmek için [az aks node Pool Delete][az-aks-nodepool-delete] komutunu kullanın ve düğüm havuzu adını belirtin. Aşağıdaki örnekte, önceki adımlarda oluşturulan *mynoodepool* silinir:

> [!CAUTION]
> Bir düğüm havuzunu sildiğinizde oluşabilecek veri kaybı için kurtarma seçeneği yoktur. Diğer düğüm havuzlarında Eğer Pod zamanlanamaz, bu uygulamalar kullanılamaz. Kullanımda olan uygulamalarda veri yedeklemeleri olmadığında veya kümenizdeki diğer düğüm havuzlarında çalıştırma yeteneğine sahip olmayan bir düğüm havuzunu silmemenizi unutmayın.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

[Az aks düğüm havuzu List][az-aks-nodepool-list] komutundan alınan aşağıdaki örnek çıktı, *mynodepool* 'in *silme* durumunda olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Deleting",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Düğümlerin ve düğüm havuzunun silinmesi birkaç dakika sürer.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Düğüm havuzu için VM boyutu belirtme

Önceki örneklerde, bir düğüm havuzu oluşturmak için, kümede oluşturulan düğümler için varsayılan bir VM boyutu kullanıldı. Daha yaygın bir senaryo, farklı VM boyutları ve özellikleri olan düğüm havuzları oluşturmanıza yöneliktir. Örneğin, büyük miktarlarda CPU veya belleğe sahip düğümleri veya GPU desteği sağlayan bir düğüm havuzunu içeren düğüm havuzu oluşturabilirsiniz. Sonraki adımda, Kubernetes Scheduler 'a bu düğümlerde çalışabilecek düğüm erişimini nasıl sınırlayacağınızı bildirmek için [tatları ve toleransı kullanırsınız](#schedule-pods-using-taints-and-tolerations) .

Aşağıdaki örnekte, *Standard_NC6* VM boyutunu kullanan bir GPU tabanlı düğüm havuzu oluşturun. Bu VM 'Ler NVıDıA Tesla K80 kartı tarafından desteklenir. Kullanılabilir VM boyutları hakkında daha fazla bilgi için bkz. [Azure 'Da Linux sanal makineleri Için boyutlar][vm-sizes].

[Az aks düğüm havuzu Add][az-aks-nodepool-add] komutunu yeniden kullanarak bir düğüm havuzu oluşturun. Bu kez, *gpunodepool*adını belirtin ve *Standard_NC6* boyutunu belirtmek için `--node-vm-size` parametresini kullanın:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

[Az aks düğüm havuzu List][az-aks-nodepool-list] komutundan aşağıdaki örnek çıktı, *Gpunodepool* belirtilen *VMSize*sahip düğümleri *oluşturuyor* olduğunu gösterir:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "gpunodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Creating",
    ...
    "vmSize": "Standard_NC6",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

*Gpunodepool* 'nin başarıyla oluşturulması birkaç dakika sürer.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Talara ve toleranları kullanarak Pod zamanlama

Artık kümenizde iki düğüm havuzu vardır; başlangıçta oluşturulan varsayılan düğüm havuzu ve GPU tabanlı düğüm Havuzu. Kümenizdeki düğümleri görüntülemek için [kubectl Get Nodes][kubectl-get] komutunu kullanın. Aşağıdaki örnek çıktı, düğümleri göstermektedir:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.13.10
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.13.10
```

Kubernetes Zamanlayıcı, düğümlerde hangi iş yüklerinin çalıştırılacağını kısıtlamak için tatları ve toleranları kullanabilir.

* Yalnızca belirli yığınların zamanlanabileceğini gösteren bir düğüme bir **taınt** uygulanır.
* Daha sonra bir **tolerans** , düğümün Taint 'e kabul etmesine izin veren bir pod öğesine uygulanır.

Gelişmiş Kubernetes zamanlanmış özelliklerini kullanma hakkında daha fazla bilgi için bkz. [AKS 'de gelişmiş Zamanlayıcı özellikleri Için en iyi yöntemler][taints-tolerations]

Bu örnekte, [kubectl taınt node][kubectl-taint] komutunu kullanarak GPU tabanlı düğümünüz için bir Taint uygulayın. Önceki `kubectl get nodes` komutun çıktısından GPU tabanlı düğümünüz adını belirtin. Taınt bir anahtar olarak uygulanır *: değer* ve ardından zamanlama seçeneği. Aşağıdaki örnek *SKU = GPU* çiftini kullanır ve pod 'yi tanımlar, aksi takdirde *NoSchedule* özelliğine sahiptir:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Aşağıdaki temel örnek YAML bildirimi, Kubernetes Scheduler 'ın GPU tabanlı düğümde bir NGıNX Pod çalıştırmasına izin vermek için bir tolerans kullanır. Daha uygun olan, ancak veri kümesine karşı bir TensorFlow işi çalıştırmak için yoğun zaman tüketen bir örnek için bkz. [AKS üzerinde işlem yoğunluğu yoğun iş yükleri Için GPU 'Ları kullanma][gpu-cluster].

Adlı `gpu-toleration.yaml` bir dosya oluşturun ve aşağıdaki örnekteki YAML 'yi kopyalayın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Şu `kubectl apply -f gpu-toleration.yaml` komutu kullanarak Pod 'u zamanlayın:

```console
kubectl apply -f gpu-toleration.yaml
```

Pod 'u zamanlamak ve NGıNX görüntüsünü çekmek birkaç saniye sürer. Pod durumunu görüntülemek için [kubectl betimleyen Pod][kubectl-describe] komutunu kullanın. Aşağıdaki sıkıştırılmış örnek çıktı *SKU = GPU: NoSchedule* toleranation ' ı gösterir. Olaylar bölümünde Zamanlayıcı, Pod 'yi *aks-gpunodepool-28993262-vmss000000* GPU tabanlı düğüme atamıştır:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Yalnızca bu taınt uygulanmış olan bir düğüm, *gpunodepool*içindeki düğümlerde zamanlanabilir. Diğer Pod 'lar *nodepool1* düğüm havuzunda zamanlanır. Ek düğüm havuzları oluşturursanız, bu düğüm kaynakları üzerinde hangi yığınların zamanlanabileceği ile ilgili ek litre ve toleransyonlar kullanabilirsiniz.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak düğüm havuzlarını yönetme

Kaynakları oluşturmak ve yönetmek için bir Azure Resource Manager şablonu kullandığınızda, genellikle şablonunuzda ayarları güncelleştirebilir ve kaynağı güncelleştirmek için yeniden dağıtabilirsiniz. AKS içindeki düğüm havuzlarıyla, AKS kümesi oluşturulduktan sonra ilk düğüm havuzu profili güncelleştirilemiyor. Bu davranış, mevcut bir Kaynak Yöneticisi şablonunu güncelleştiremeyeceğiniz, düğüm havuzlarında değişiklik yapamayacağı ve yeniden dağımeyeceğiniz anlamına gelir. Bunun yerine, yalnızca mevcut bir AKS kümesi için aracı havuzlarını güncelleştiren ayrı bir Kaynak Yöneticisi şablonu oluşturmanız gerekir.

Gibi bir şablon `aks-agentpools.json` oluşturun ve aşağıdaki örnek bildirimi yapıştırın. Bu örnek şablon aşağıdaki ayarları yapılandırır:

* *Myagentpool* adlı *Linux* aracı havuzunu üç düğüm çalıştıracak şekilde güncelleştirir.
* Düğüm havuzundaki düğümleri Kubernetes sürüm *1.13.10*çalıştıracak şekilde ayarlar.
* Düğüm boyutunu *Standard_DS2_v2*olarak tanımlar.

Gerektiğinde düğüm havuzlarını güncelleştirme, ekleme veya silme gereksinimi olarak bu değerleri düzenleyin:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of your existing AKS cluster."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "The location of your existing AKS cluster."
            }
        },
        "agentPoolName": {
            "type": "string",
            "defaultValue": "myagentpool",
            "metadata": {
                "description": "The name of the agent pool to create or update."
            }
        },
        "vnetSubnetId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The Vnet subnet resource ID for your existing AKS cluster."
            }
        }
    },
    "variables": {
        "apiVersion": {
            "aks": "2019-04-01"
        },
        "agentPoolProfiles": {
            "maxPods": 30,
            "osDiskSizeGB": 0,
            "agentCount": 3,
            "agentVmSize": "Standard_DS2_v2",
            "osType": "Linux",
            "vnetSubnetId": "[parameters('vnetSubnetId')]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.ContainerService/managedClusters/agentPools",
            "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
            "location": "[parameters('location')]",
            "properties": {
                "maxPods": "[variables('agentPoolProfiles').maxPods]",
                "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
                "count": "[variables('agentPoolProfiles').agentCount]",
                "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
                "osType": "[variables('agentPoolProfiles').osType]",
                "storageProfile": "ManagedDisks",
                "type": "VirtualMachineScaleSets",
                "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
                "orchestratorVersion": "1.13.10"
            }
        }
    ]
}
```

Bu şablonu, aşağıdaki örnekte gösterildiği gibi [az Group Deployment Create][az-group-deployment-create] komutunu kullanarak dağıtın. Mevcut AKS kümesi adı ve konumu sorulur:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Kaynak Yöneticisi şablonunuzda tanımladığınız düğüm havuzu ayarlarına ve işlemlerine bağlı olarak AKS kümenizin güncelleştirilmesi birkaç dakika sürebilir.

## <a name="assign-a-public-ip-per-node-in-a-node-pool"></a>Düğüm havuzunda düğüm başına genel IP atama

> [!NOTE]
> Düğüm başına genel IP atama önizlemesi sırasında, sanal makine sağlama ile çakışan olası yük dengeleyici kuralları nedeniyle *AKS 'de standart Load Balancer SKU 'su* ile kullanılamaz. Önizlemede, düğüm başına genel IP atamanız gerekiyorsa *temel Load Balancer SKU 'su* kullanın.

AKS düğümleri iletişim için kendi genel IP adreslerini gerektirmez. Ancak bazı senaryolar, düğüm havuzundaki düğümlerin kendi genel IP adreslerine sahip olmasını gerektirebilir. Örneğin, bir konsolun, atlamaları en aza indirmek için bir bulut sanal makinesine doğrudan bağlantı kurmak için gereken oyun. Bu, ayrı bir önizleme özelliği olan düğüm genel IP (Önizleme) için kaydolarak elde edilebilir.

```azurecli-interactive
az feature register --name NodePublicIPPreview --namespace Microsoft.ContainerService
```

Kayıt başarılı olduktan sonra, [Yukarıdaki](#manage-node-pools-using-a-resource-manager-template) şekilde aynı yönergeleri izleyerek bir Azure Resource Manager şablonu dağıtın ve agentPoolProfiles üzerinde "Enablenodepublicıp" Boole değeri özelliğini ekleyin. Bunu `true` varsayılan`false` olarak olarak ayarla ayarı belirtilmezse, belirtilen şekilde ayarlanır. Bu yalnızca bir oluşturma zamanı özelliğidir ve en düşük API sürümü olan 2019-06-01 gerektirir. Bu, hem Linux hem de Windows düğüm havuzlarına uygulanabilir.

```
"agentPoolProfiles":[  
    {  
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]",
      "enableNodePublicIP":true
    }
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, GPU tabanlı düğümleri içeren bir AKS kümesi oluşturdunuz. Gereksiz maliyeti azaltmak için, *gpunodepool*veya tüm aks kümesini silmek isteyebilirsiniz.

GPU tabanlı düğüm havuzunu silmek için, aşağıdaki örnekte gösterildiği gibi [az aks nodepool Delete][az-aks-nodepool-delete] komutunu kullanın:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Kümeyi silmek için, AKS kaynak grubunu silmek için [az Group Delete][az-group-delete] komutunu kullanın:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir AKS kümesinde birden çok düğüm havuzu oluşturmayı ve yönetmeyi öğrendiniz. Düğüm havuzlarının tamamında nasıl denetim yapılacağı hakkında daha fazla bilgi için bkz. [AKS 'de gelişmiş Zamanlayıcı özellikleri Için en iyi yöntemler][operator-best-practices-advanced-scheduler].

Windows Server kapsayıcısı düğüm havuzlarını oluşturmak ve kullanmak için bkz. [AKS 'de Windows Server kapsayıcısı oluşturma][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
