---
title: Azure Kubernetes Hizmeti'nde (AKS) birden çok düğüm havuzu kullanma
description: Azure Kubernetes Hizmeti'nde (AKS) bir küme için birden çok düğüm havuzu oluşturma ve yönetme yi öğrenin
services: container-service
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: 607419787bc0bab243d6cc2b8cbaa0ec22921e87
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422322"
---
# <a name="create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) bir küme için birden çok düğüm havuzu oluşturma ve yönetme

Azure Kubernetes Hizmeti'nde (AKS), aynı yapılandırmadaki düğümler düğüm *havuzlarında*gruplandırılır. Bu düğüm havuzları, uygulamalarınızı çalıştıran temel VM'leri içerir. Varsayılan düğüm *havuzu*oluşturan bir AKS kümesi oluşturduğunuzda ilk düğüm sayısı ve boyutları (SKU) tanımlanır. Farklı bilgi işlem veya depolama talepleri olan uygulamaları desteklemek için ek düğüm havuzları oluşturabilirsiniz. Örneğin, bilgi işlem yoğun uygulamalar veya yüksek performanslı SSD depolama erişimi için GPU'lar sağlamak için bu ek düğüm havuzlarını kullanın.

> [!NOTE]
> Bu özellik, birden çok düğüm havuzu oluşturma ve yönetme konusunda daha yüksek denetim sağlar. Sonuç olarak, oluşturma/güncelleme/silme için ayrı komutlar gereklidir. Daha önce yönetilen `az aks create` `az aks update` Cluster API üzerinden küme işlemleri veya kullanılan ve kontrol düzlemi ve tek bir düğüm havuzu değiştirmek için tek seçenek vardı. Bu özellik, agentPool API aracılığıyla aracı havuzları için ayrı bir `az aks nodepool` işlem kümesi ni ortaya çıkarır ve tek bir düğüm havuzunda işlemleri yürütmek için komut kümesinin kullanılmasını gerektirir.

Bu makalede, bir AKS kümesinde birden çok düğüm havuzu oluşturmak ve yönetmek nasıl gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürüm 2.2.0 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir:

* [Azure Kubernetes Hizmetinde (AKS) Kotalara, sanal makine boyutu kısıtlamalarına ve bölge kullanılabilirliğine][quotas-skus-regions]bakın.
* Varsayılan olarak ilk düğüm havuzunu sistem düğümü havuzunu silemezsiniz.
* AKS kümesi birden fazla düğüm havuzları kullanmak için Standart SKU yük dengeleyici kullanmanız gerekir, özellik Temel SKU yük dengeleyicileri ile desteklenmez.
* AKS kümesi düğümler için sanal makine ölçek kümeleri kullanmalıdır.
* Düğüm havuzunun adı yalnızca küçük alfasayısal karakterler içerebilir ve küçük harfle başlamalıdır. Linux düğümü havuzları için uzunluk 1 ile 12 karakter arasında olmalıdır, Windows düğüm havuzları için uzunluk 1 ile 6 karakter arasında olmalıdır.
* Tüm düğüm havuzları aynı sanal ağda olmalıdır.
* Küme oluşturma zamanında birden çok düğüm havuzu oluştururken, düğüm havuzları tarafından kullanılan tüm Kubernetes sürümleri kontrol düzlemi için ayarlanan sürüm le eşleşmelidir. Bu, küme her düğüm havuzu işlemleri kullanılarak sağlandıktan sonra güncelleştirilebilir.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Başlamak için, tek bir düğüm havuzu olan bir AKS kümesi oluşturun. Aşağıdaki örnek, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturmak için [az grubu oluşturma][az-group-create] komutunu kullanır. *MyAKSCluster* adlı bir AKS kümesi daha sonra [az aks oluşturma][az-aks-create] komutu kullanılarak oluşturulur. *1.15.7'nin* *kubernetes sürümü,* bir düğüm havuzunun sonraki adımda nasıl güncelleştirilebildiğini göstermek için kullanılır. [Desteklenen Kubernetes sürümünü][supported-versions]belirtebilirsiniz.

> [!NOTE]
> *Temel* yük dengeleyici SKU, birden çok düğüm havuzu kullanırken **desteklenmez.** Varsayılan olarak, AKS kümeleri Azure CLI ve Azure portalından *Standart* yük dengeleyici SKU ile oluşturulur.

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
    --kubernetes-version 1.15.7 \
    --load-balancer-sku standard
```

Kümenin oluşturulması birkaç dakika sürer.

> [!NOTE]
> Kümenizin güvenilir bir şekilde çalıştığından emin olmak için, temel sistem hizmetleri bu düğüm havuzunda çalıştığından varsayılan düğüm havuzunda en az 2 (iki) düğüm çalıştırmalısınız.

Küme hazır olduğunda, az [aks get-credentials][az-aks-get-credentials] komutunu kullanarak küme `kubectl`kimlik bilgilerini aşağıdakilerle birlikte alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Düğüm havuzu ekleme

Önceki adımda oluşturulan kümenin tek bir düğüm havuzu vardır. [Az aks nodepool add][az-aks-nodepool-add] komutunu kullanarak ikinci bir düğüm havuzu ekleyelim. Aşağıdaki örnek, *3* düğüm çalıştıran *mynodepool* adlı bir düğüm havuzu oluşturur:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --kubernetes-version 1.15.5
```

> [!NOTE]
> Düğüm havuzunun adı küçük harfle başlamalı ve yalnızca alfasayısal karakterler içerebilir. Linux düğümü havuzları için uzunluk 1 ile 12 karakter arasında olmalıdır, Windows düğüm havuzları için uzunluk 1 ile 6 karakter arasında olmalıdır.

Düğüm havuzlarının durumunu görmek için az [aks düğümü havuz listesi][az-aks-nodepool-list] komutunu kullanın ve kaynak grubunuzu ve küme adınızı belirtin:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster
```

Aşağıdaki örnek çıktı, düğüm havuzunda üç düğümle *minodehavuzunun* başarıyla oluşturulduğunu gösterir. Aks kümesi önceki adımda oluşturulduğunda, varsayılan *düğüm havuzu1* *2*düğüm sayısı ile oluşturuldu.

```output
[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.5",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

> [!TIP]
> Düğüm havuzu eklediğinizde *VmSize* belirtilmemişse, varsayılan boyut Windows düğüm havuzları için *Standard_DS2_v3* ve Linux düğüm havuzları için *Standard_DS2_v2.* *OrchestratorVersion* belirtilmemişse, varsayılan olarak denetim düzlemi ile aynı sürüme itilir.

### <a name="add-a-node-pool-with-a-unique-subnet-preview"></a>Benzersiz bir alt netiçeren bir düğüm havuzu ekleme (önizleme)

İş yükü, mantıksal yalıtım için kümedüğümlerinin ayrı havuzlara bölünmesini gerektirebilir. Bu yalıtım kümedeki her düğüm havuzuna adanmış ayrı alt ağlarla desteklenebilir. Bu, düğüm havuzları arasında bölünecek bitişik olmayan sanal ağ adresi alanına sahip olmak gibi gereksinimleri ele alabilir.

#### <a name="limitations"></a>Sınırlamalar

* Düğüm havuzlarına atanan tüm alt ağlar aynı sanal ağa ait olmalıdır.
* CoreDNS üzerinden DNS çözümü gibi kritik işlevsellik sağlamak için sistem bölmelerinin kümedeki tüm düğümlere erişimi olmalıdır.
* Önizleme sırasında düğüm havuzu başına benzersiz bir alt ağ ataması Azure CNI ile sınırlıdır.
* Önizleme sırasında düğüm başına benzersiz bir alt ağ havuzuna sahip ağ ilkelerinin kullanılması desteklenmez.

Özel bir alt netiçeren bir düğüm havuzu oluşturmak için, düğüm havuzu oluştururken alt kaynak kimliğini ek bir parametre olarak geçirin.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --kubernetes-version 1.15.5
    --vnet-subnet-id <YOUR_SUBNET_RESOURCE_ID>
```

## <a name="upgrade-a-node-pool"></a>Düğüm havuzuyükseltme

> [!NOTE]
> Bir hata denenmişse, küme veya düğüm havuzundaki yükseltme ve ölçeklendirme işlemleri aynı anda gerçekleşemez. Bunun yerine, her işlem türü, aynı kaynaktaki bir sonraki istekten önce hedef kaynakta tamamlanmalıdır. [Sorun giderme kılavuzumuzda](https://aka.ms/aks-pending-upgrade)bu konuda daha fazla bilgi edinin.

AKS kümeniz ilk adımda ilk oluşturulduğunda, `--kubernetes-version` *1.15.7'lik* bir küme belirtilmişti. Bu, hem denetim düzlemi hem de varsayılan düğüm havuzu için Kubernetes sürümünü ayarlar. Bu bölümdeki komutlar, belirli bir düğüm havuzunun nasıl yükseltilen nasıl açıklayacağını açıklar.

Kontrol düzleminin Kubernetes sürümünü yükseltme ve düğüm havuzu arasındaki ilişki [aşağıdaki bölümde](#upgrade-a-cluster-control-plane-with-multiple-node-pools)açıklanmıştır.

> [!NOTE]
> Düğüm havuzu OS görüntü sürümü kümenin Kubernetes sürümüne bağlıdır. Küme yükseltmesi sonrasında yalnızca işletim sistemi görüntü yükseltmeleri alırsınız.

Bu örnekte iki düğüm havuzu olduğundan, düğüm havuzuyükseltmek için [az aks nodepool yükseltme][az-aks-nodepool-upgrade] kullanmanız gerekir. *Mynodepool'u* Kubernetes *1.15.7'ye*yükseltelim. Aşağıdaki örnekte gösterildiği gibi düğüm havuzunu yükseltmek için [az aks nodepool yükseltme][az-aks-nodepool-upgrade] komutunu kullanın:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.15.7 \
    --no-wait
```

[Az aks düğümü havuz listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarının durumunu yeniden listele. Aşağıdaki örnek, *minodepool* *1.15.7* *yükseltme* durumunda olduğunu gösterir:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
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
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Düğümleri belirtilen sürüme yükseltmek birkaç dakika sürer.

En iyi uygulama olarak, bir AKS kümesindeki tüm düğüm havuzlarını aynı Kubernetes sürümüne yükseltmeniz gerekir. Varsayılan `az aks upgrade` davranış, bu hizalama elde etmek için tüm düğüm havuzları denetim düzlemi ile birlikte yükseltmektir. Tek tek düğüm havuzlarını yükseltme yeteneği, yukarıda belirtilen kısıtlamalar içinde uygulama çalışma süresini korumak için düğüm havuzları arasında bir yuvarlanma yükseltme ve zamanlama bölmeleri gerçekleştirmenize olanak sağlar.

## <a name="upgrade-a-cluster-control-plane-with-multiple-node-pools"></a>Birden çok düğüm havuzu olan bir küme denetim düzlemini yükseltme

> [!NOTE]
> Kubernetes standart [Anlamsal Sürüm](https://semver.org/) sürüm şemasını kullanır. Sürüm numarası *x.y.z*olarak ifade edilir , *x* ana sürümdür, *y* küçük sürümdür ve *z* yama sürümüdür. Örneğin, sürüm *1.12.6*, 1 ana sürümüdür, 12 küçük sürümüdür ve 6 yama sürümüdür. Denetim düzleminin Kubernetes sürümü ve ilk düğüm havuzu küme oluşturma sırasında ayarlanır. Tüm ek düğüm havuzları kümeye eklendiklerinde Kubernetes sürüm kümesine sahiptir. Kubernetes sürümleri düğüm havuzları arasında olduğu kadar düğüm havuzu ile kontrol düzlemi arasında farklılık gösterebilir.

Bir AKS kümesinde Kubernetes sürümleri ilişkili iki küme kaynak nesnesi vardır.

1. Bir küme kontrol düzlemi Kubernetes sürümü.
2. Kubernetes sürümü olan bir düğüm havuzu.

Bir kontrol düzlemi bir veya birden fazla düğüm havuzuyla eşler. Yükseltme işleminin davranışı, hangi Azure CLI komutunun kullanıldığına bağlıdır.

AKS kontrol düzlemini yükseltme `az aks upgrade`için .' yi kullanmak gerekiyor. Bu komut, kümedeki denetim düzlemi sürümünü ve tüm düğüm havuzlarını yükseltir.

Komutu `az aks upgrade` bayrakla `--control-plane-only` veren komut yalnızca küme denetim düzlemini yükseltir. Kümedeki ilişkili düğüm havuzlarının hiçbiri değiştirilmez.

Tek tek düğüm havuzlarını yükseltme `az aks nodepool upgrade`yi kullanmak gerekiyor. Bu komut, yalnızca belirtilen Kubernetes sürümüyle hedef düğüm havuzunu yükseltir

### <a name="validation-rules-for-upgrades"></a>Yükseltmeler için doğrulama kuralları

Bir kümenin denetim düzlemi ve düğüm havuzları için geçerli Kubernetes yükseltmeleri aşağıdaki kurallar kümeleri tarafından doğrulanır.

* Düğüm havuzlarını yükseltmek için geçerli sürümler için kurallar:
   * Düğüm havuzu sürümü, denetim düzlemi ile aynı *ana* sürüme sahip olmalıdır.
   * Düğüm havuzu *küçük* sürümü, denetim düzlemi sürümünün iki *küçük* sürümü içinde olmalıdır.
   * Düğüm havuzu sürümü denetim `major.minor.patch` sürümünden büyük olamaz.

* Yükseltme işlemi gönderme kuralları:
   * Kontrol düzlemini veya düğüm havuzu Kubernetes sürümünü düşüremezsiniz.
   * Düğüm havuzu Kubernetes sürümü belirtilmemişse, davranış kullanılan istemciye bağlıdır. Kaynak Yöneticisi şablonlarında bildirim, kullanıldığında düğüm havuzu için tanımlanan varolan sürüme geri döner, eğer hiçbiri ayarlanmışsa, denetim düzlemi sürümü geri düşmek için kullanılır.
   * Belirli bir zamanda bir denetim düzlemini veya düğüm havuzunı yükseltebilir veya ölçeklendirebilirsiniz, aynı anda tek bir denetim düzleminde veya düğüm havuzu kaynağında birden çok işlem gönderemezsiniz.

## <a name="scale-a-node-pool-manually"></a>Düğüm havuzunu el ile ölçeklendirin

Uygulama iş yükünün gerektirdiği nde, düğüm havuzundaki düğüm sayısını ölçeklendirmeniz gerekebilir. Düğüm sayısı yukarı veya aşağı ölçeklendirilebilir.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Düğüm havuzundaki düğüm sayısını ölçeklendirmek için [az aks düğümü havuzu ölçeği][az-aks-nodepool-scale] komutunu kullanın. Aşağıdaki örnek, *minodehavuzundaki* düğüm sayısını *5'e*ölçeklendir:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

[Az aks düğümü havuz listesi][az-aks-nodepool-list] komutunu kullanarak düğüm havuzlarının durumunu yeniden listele. Aşağıdaki örnek, *minodepool* *5* düğüm yeni bir sayı ile *Ölçekleme* durumunda olduğunu gösterir:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
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
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Ölçek işleminin tamamlanması birkaç dakika sürer.

## <a name="scale-a-specific-node-pool-automatically-by-enabling-the-cluster-autoscaler"></a>Küme otomatik ölçeklendiricisini etkinleştirerek belirli bir düğüm havuzunu otomatik olarak ölçeklendirin

AKS, [küme otomatik ölçeklendirici](cluster-autoscaler.md)adı verilen bir özellik ile düğüm havuzlarını otomatik olarak ölçeklendirmek için ayrı bir özellik sunar. Bu özellik, düğüm havuzu başına benzersiz minimum ve maksimum ölçek sayıları ile düğüm havuzu başına etkinleştirilebilir. [Küme otomatik ölçeklendirici düğüm havuzu nu](cluster-autoscaler.md#use-the-cluster-autoscaler-with-multiple-node-pools-enabled)nasıl kullanacağınızı öğrenin.

## <a name="delete-a-node-pool"></a>Düğüm havuzuni silme

Artık bir havuza ihtiyacınız yoksa, havuzsını silebilir ve temel VM düğümlerini kaldırabilirsiniz. Düğüm havuzunu silmek için [az aks düğümü havuzu silme][az-aks-nodepool-delete] komutunu kullanın ve düğüm havuzu adını belirtin. Aşağıdaki örnek, önceki adımlarda oluşturulan *miyonoodepool* siler:

> [!CAUTION]
> Düğüm havuzuni sildiğinizde oluşabilecek veri kaybı için kurtarma seçeneği yoktur. Diğer düğüm havuzlarında bölmeler zamanlanamıyorsa, bu uygulamalar kullanılamaz. Kullanımdaki uygulamaların veri yedekleri veya kümenizdeki diğer düğüm havuzlarında çalışma özelliği yoksa düğüm havuzunu silmediğinizden emin olun.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

[az aks düğümü havuz listesi][az-aks-nodepool-list] komutundan aşağıdaki örnek *çıktı, minodepool'un* *Silme* durumunda olduğunu gösterir:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
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
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Düğümleri ve düğüm havuzunu silmek birkaç dakika sürer.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Düğüm havuzu için VM boyutu belirtin

Düğüm havuzu oluşturmak için önceki örneklerde, kümede oluşturulan düğümler için varsayılan VM boyutu kullanılmıştır. Daha yaygın bir senaryo, farklı VM boyutları ve yeteneklerine sahip düğüm havuzları oluşturmanızdır. Örneğin, büyük miktarda CPU veya bellek içeren düğümler veya GPU desteği sağlayan bir düğüm havuzu oluşturabilirsiniz. Bir sonraki adımda, Kubernetes zamanlayıcısına bu düğümlerde çalıştırılabilen bölmelere erişimi nasıl sınırlandırabileceğinizi söylemek için [leke ve tolere kullanırsınız.](#schedule-pods-using-taints-and-tolerations)

Aşağıdaki örnekte, *Standard_NC6* VM boyutunu kullanan GPU tabanlı bir düğüm havuzu oluşturun. Bu VM'ler NVIDIA Tesla K80 kartı ile desteklenmektedir. Kullanılabilir VM boyutları hakkında daha fazla bilgi için [Azure'daki Linux sanal makineleri için Boyutlar'a][vm-sizes]bakın.

[Az aks düğüm havuzunu][az-aks-nodepool-add] kullanarak bir düğüm havuzu oluşturun yeniden komut ekleyin. Bu kez, *gpunodepool*adını belirtin `--node-vm-size` ve *Standard_NC6* boyutunu belirtmek için parametreyi kullanın:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

[Az aks düğümü havuz listesi][az-aks-nodepool-list] komutundan aşağıdaki örnek *çıktı, gpunodepool'un* belirtilen *VmSize*ile düğüm *oluşturduğunu* gösterir:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 1,
    ...
    "name": "gpunodepool",
    "orchestratorVersion": "1.15.7",
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
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

*Gpunodepool'un* başarıyla oluşturulması birkaç dakika sürer.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Leke ve tolere kullanarak bölmeleri zamanlama

Artık kümenizde iki düğüm havuzu var : başlangıçta oluşturulan varsayılan düğüm havuzu ve GPU tabanlı düğüm havuzu. Kümenizdeki düğümleri görüntülemek için [kubectl get düğümleri][kubectl-get] komutunu kullanın. Aşağıdaki örnek çıktı düğümleri gösterir:

```console
kubectl get nodes
```

```output
NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.15.7
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.15.7
```

Kubernetes zamanlayıcısı, düğümlerde hangi iş yüklerinin çalıştırılabildiği kısıtlamak için bozuk para ve tolere kullanabilir.

* Yalnızca belirli bölmelerin zamanlanabileceğini belirten bir düğüme **bir leke** uygulanır.
* Bir **tolerans** daha sonra onları bir düğüm leke *tolere* sağlayan bir bölme uygulanır.

Gelişmiş Kubernetes zamanlanmış özelliklerinin nasıl kullanılacağı hakkında daha fazla bilgi [için, AKS'deki gelişmiş zamanlayıcı özellikleri için en iyi uygulamalara][taints-tolerations] bakın

Bu örnekte, --düğüm-taints komutunu kullanarak GPU tabanlı düğümünüze bir leke uygulayın. Önceki `kubectl get nodes` komutun çıktısından GPU tabanlı düğümünüzün adını belirtin. Leke *bir anahtar: değer* ve daha sonra bir zamanlama seçeneği olarak uygulanır. Aşağıdaki *örneksku=gpu* çiftini kullanır ve bölmeleri tanımlar aksi takdirde *NoSchedule* özelliğine sahiptir:

```console
az aks nodepool add --node-taints aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Aşağıdaki temel örnek YAML bildirimi, Kubernetes zamanlayıcısının GPU tabanlı düğümüzerinde bir NGINX bölmesini çalıştırabilmesi için bir hoşgörü kullanır. MNIST veri kümesine karşı tensorflow işini çalıştırmak için daha uygun, ancak zaman yoğun bir örnek için, [AKS'de bilgi işlem yoğun iş yükleri için GPU'ları kullan'a][gpu-cluster]bakın.

Adlandırılmış `gpu-toleration.yaml` bir dosya oluşturun ve aşağıdaki örnekTE kopyalayın YAML:

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

Komutu kullanarak `kubectl apply -f gpu-toleration.yaml` bölmeyi zamanlama:

```console
kubectl apply -f gpu-toleration.yaml
```

Pod'u zamanlamak ve NGINX görüntüsünü çekmek birkaç saniye sürer. Pod durumunu görüntülemek için [kubectl describe pod][kubectl-describe] komutunu kullanın. Aşağıdaki yoğunlaştırılmış örnek *çıktısku=gpu:NoSchedule* tolerasyonunun uygulandığını gösterir. Olaylar bölümünde, zamanlayıcı *aks-gpunodepool-28993262-vmss00000G* GPU tabanlı düğüm pod atadı:

```console
kubectl describe pod mypod
```

```output
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

Yalnızca bu lekeyi uygulayan bölmeler *gpunodepool'daki*düğümlerde zamanlanabilir. Başka bir bölme *düğümhavuzunda* zamanlanır. Ek düğüm havuzları oluşturursanız, bu düğüm kaynaklarında zamanlanabilecek bölmeleri sınırlamak için ek leke ve tolere kullanabilirsiniz.

## <a name="specify-a-taint-label-or-tag-for-a-node-pool"></a>Düğüm havuzu için bir leke, etiket veya etiket belirtin

Düğüm havuzu oluştururken, düğüm havuzuna bozuk para, etiket veya etiket ekleyebilirsiniz. Bir bozuk leke, etiket veya etiket eklediğinizde, bu düğüm havuzundaki tüm düğümler de bu lekeyi, etiketi veya etiketi alır.

Bir düğüm ile bir düğüm havuzu oluşturmak için, [az aks nodepool ekleyin][az-aks-nodepool-add]kullanın. *Taintnp* adını belirtin `--node-taints` ve *sku=gpu:NoSchedule'ı* belirtmek için parametreyi kullanın.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name taintnp \
    --node-count 1 \
    --node-taints sku=gpu:NoSchedule \
    --no-wait
```

[az aks nodepool listesi][az-aks-nodepool-list] komutundan aşağıdaki örnek *çıktı, taintnp'in* belirtilen *düğümlerle*düğüm *oluşturduğunu* gösterir:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "taintnp",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "nodeTaints":  {
      "sku": "gpu:NoSchedule"
    },
    ...
  },
 ...
]
```

Düğümler için zamanlama kurallarını işlemek için Kubernetes'te kusurlu bilgiler görünür.

Düğüm havuzu oluşturma sırasında düğüm havuzuna etiketler de ekleyebilirsiniz. Düğüm havuzunda ayarlanan etiketler düğüm havuzundaki her düğüme eklenir. Bu etiketler düğümler için zamanlama kuralları işlemek için [Kubernetes görünür.][kubernetes-labels]

Etiketli bir düğüm havuzu oluşturmak için [az aks nodepool ekleyin.][az-aks-nodepool-add] *Labelnp* adını belirtin `--labels` ve etiketler için *dept=IT* ve *costcenter=9999* belirtmek için parametreyi kullanın.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name labelnp \
    --node-count 1 \
    --labels dept=IT costcenter=9999 \
    --no-wait
```

> [!NOTE]
> Etiket yalnızca düğüm havuzu oluşturma sırasında düğüm havuzları için ayarlanabilir. Etiketler de bir anahtar/değer çifti olmalı ve geçerli bir [sözdizimine][kubernetes-label-syntax]sahip olmalıdır.

[az aks nodepool listesi][az-aks-nodepool-list] komutundan aşağıdaki örnek *çıktı, labelnp'ın* belirtilen düğümlerle düğüm *oluşturduğunu* *gösterirEtiketler:*

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "labelnp",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "nodeLabels":  {
      "dept": "IT",
      "costcenter": "9999"
    },
    ...
  },
 ...
]
```

AKS kümenizdeki düğüm havuzlarına Bir Azure etiketi uygulayabilirsiniz. Düğüm havuzuna uygulanan etiketler düğüm havuzundaki her düğüme uygulanır ve yükseltmeler aracılığıyla kalıcıolarak uygulanır. Etiketler, ölçeklendirme işlemleri sırasında düğüm havuzuna eklenen yeni düğümlere de uygulanır. Etiket eklemek, ilke izleme veya maliyet tahmini gibi görevlerde yardımcı olabilir.

[Az aks nodepool ekle][az-aks-nodepool-add]kullanarak bir düğüm havuzu oluşturun. *Tagnodepool* adını belirtin `--tag` ve etiketler için *dept=IT* ve *costcenter=9999* belirtmek için parametreyi kullanın.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name tagnodepool \
    --node-count 1 \
    --tags dept=IT costcenter=9999 \
    --no-wait
```

> [!NOTE]
> [Az aks nodepool güncelleştirme][az-aks-nodepool-update] komutunu `--tags` kullanırken ve küme oluşturma sırasında parametreyi de kullanabilirsiniz. Küme oluşturma sırasında `--tags` parametre, kümeyle oluşturulan ilk düğüm havuzuna etiketi uygular. Azure kaynaklarınızı düzenlemek için tüm etiket adları Kullanım etiketlerinde sınırlamalara [uymalıdır.][tag-limitation] `--tags` Parametre ile bir düğüm havuzunu güncelleştirme, varolan etiket değerlerini güncelleştirir ve yeni etiketlerekler. Örneğin, düğüm havuzunuzda etiketler için *dept=IT* ve *costcenter=9999* olsaydı ve etiketler için *team=dev* ve *costcenter=111* ile güncellediyseniz, siz düğümde *dept=IT*, *costcenter=111*ve etiketler için *team=dev* olurdu.

[Az aks nodepool listesi][az-aks-nodepool-list] komutundan aşağıdaki örnek *çıktı, tagnodepool'un* belirtilen *etiketle*düğüm *oluşturduğunu* gösterir:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 1,
    ...
    "name": "tagnodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "tags": {
      "dept": "IT",
      "costcenter": "9999"
    },
    ...
  },
 ...
]
```

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak düğüm havuzlarını yönetme

Kaynakları oluşturmak ve yönetilen kaynaklar oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullandığınızda, genellikle şablonunuzdaki ayarları güncelleştirebilir ve kaynağı güncelleştirmek için yeniden dağıtabilirsiniz. AKS'deki düğüm havuzları yla, AKS kümesi oluşturulduktan sonra ilk düğüm havuzu profili güncelleştirilemez. Bu davranış, varolan bir Kaynak Yöneticisi şablonunu güncelleştiremediğiniz, düğüm havuzlarında değişiklik yapamadığınız ve yeniden dağıtamadığınız anlamına gelir. Bunun yerine, varolan bir AKS kümesi için yalnızca düğüm havuzlarını güncelleyen ayrı bir Kaynak Yöneticisi şablonu oluşturmanız gerekir.

Aşağıdaki örnek bildirimi `aks-agentpools.json` gibi bir şablon oluşturun ve yapıştırın. Bu örnek şablon aşağıdaki ayarları yapılandırır:

* *Myagentpool* adlı *Linux* düğümü havuzunu üç düğüm çalıştırmak için günceller.
* Düğüm havuzundaki düğümleri Kubernetes sürüm *1.15.7'yi*çalıştırmak için ayarlar.
* Düğüm boyutunu *Standard_DS2_v2*olarak tanımlar.

Gerektiğinde düğüm havuzlarını güncelleştirmek, eklemek veya silmek için gereken değerleri düzenleyin:

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
            "aks": "2020-01-01"
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
            "apiVersion": "2020-01-01",
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
                "orchestratorVersion": "1.15.7"
            }
        }
    ]
}
```

Aşağıdaki örnekte gösterildiği gibi [az grubu dağıtım oluşturma][az-group-deployment-create] komutunu kullanarak bu şablonu dağıtın. Varolan AKS küme adı ve konumu için istenir:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

> [!TIP]
> Aşağıdaki örnekte gösterildiği gibi, şablona *etiket* özelliğini ekleyerek düğüm havuzunuza bir etiket ekleyebilirsiniz.
> 
> ```json
> ...
> "resources": [
> {
>   ...
>   "properties": {
>     ...
>     "tags": {
>       "name1": "val1"
>     },
>     ...
>   }
> }
> ...
> ```

Kaynak Yöneticisi şablonunuzda tanımladığınız düğüm havuzu ayarlarına ve işlemlerine bağlı olarak AKS kümenizi güncelleştirmek birkaç dakika sürebilir.

## <a name="assign-a-public-ip-per-node-for-a-node-pool-preview"></a>Düğüm havuzu için düğüm başına ortak BIR IP atama (önizleme)

> [!WARNING]
> Düğüm başına genel BIR IP atama önizlemesi sırasında, VM sağlama ile çelişen olası yük dengeleyici kuralları nedeniyle *AKS'deki Standart Yük Dengeleyici SKU* ile kullanılamaz. Bu sınırlamanın bir sonucu olarak, Windows aracı havuzları bu önizleme özelliğiyle desteklenmez. Önizleme sırasında, düğüm başına genel bir IP atamanız gerekiyorsa *Temel Yük Dengeleyici SKU'yu* kullanmanız gerekir.

AKS düğümleri iletişim için kendi genel IP adreslerini gerektirmez. Ancak, senaryolar kendi özel genel IP adreslerini almak için düğüm havuzunda düğümler gerektirebilir. Sık karşılaşılan bir senaryo, bir konsolun atlamaları en aza indirmek için bulut sanal makinesine doğrudan bağlantı kurması gereken oyun iş yükleri içindir. Bu senaryo, aks'te bir önizleme özelliği olan Node Public IP (önizleme) için kaydolarak elde edilebilir.

Aşağıdaki Azure CLI komutunu vererek Düğüm Genel IP özelliğine kaydolun.

```azurecli-interactive
az feature register --name NodePublicIPPreview --namespace Microsoft.ContainerService
```

Başarılı bir kayıttan sonra, [yukarıdaki](#manage-node-pools-using-a-resource-manager-template) yle aynı yönergeleri izleyerek bir `enableNodePublicIP` Azure Kaynak Yöneticisi şablonu dağıtın ve boolean özelliğini agentPoolProfiles'a ekleyin. Değeri varsayılan `true` olarak ayarla, belirtilmemiş gibi `false` ayarlanır. 

Bu özellik, yalnızca oluşturma zamanı özelliğidir ve 2019-06-01'in en az API sürümünü gerektirir. Bu, hem Linux hem de Windows düğüm havuzlarına uygulanabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, GPU tabanlı düğümleri içeren bir AKS kümesi oluşturdunuz. Gereksiz maliyeti azaltmak *için, gpunodepool*veya tüm AKS kümesini silmek isteyebilirsiniz.

GPU tabanlı düğüm havuzunu silmek için aşağıdaki örnekte gösterildiği gibi [az aks nodepool silme][az-aks-nodepool-delete] komutunu kullanın:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Kümenin kendisini silmek için AKS kaynak grubunu silmek için [az grubu silme][az-group-delete] komutunu kullanın:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir AKS kümesinde birden çok düğüm havuzu oluşturmayı ve nasıl yönetileceğinizi öğrendiniz. Düğüm havuzları arasında bölmeleri nasıl denetleyiş hakkında daha fazla bilgi için [AKS'deki gelişmiş zamanlayıcı özellikleri için en iyi uygulamalara][operator-best-practices-advanced-scheduler]bakın.

Windows Server kapsayıcı düğüm havuzları oluşturmak ve kullanmak için [bkz.][aks-windows]

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/resource-group-using-tags.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/linux/sizes.md