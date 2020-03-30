---
title: Azure Kubernetes Hizmetinde (AKS) kullanılabilirlik bölgelerini kullanma
description: Azure Kubernetes Hizmeti'nde (AKS) düğümleri kullanılabilirlik bölgeleri arasında dağıtan bir küme oluşturmayı öğrenin
services: container-service
ms.custom: fasttrack-edit
ms.topic: article
ms.date: 06/24/2019
ms.openlocfilehash: 5693d9e90de9ba68e7b76e0f2bd5b75141dbda71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596819"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Kullanılabilirlik bölgelerini kullanan bir Azure Kubernetes Hizmeti (AKS) kümesi oluşturma

Azure Kubernetes Hizmeti (AKS) kümesi düğümler ve depolama gibi kaynakları temel Azure işlem altyapısının mantıksal bölümlerine dağıtır. Bu dağıtım modeli, düğümlerin tek bir Azure veri merkezinde ayrı güncelleştirme ve hata etki alanlarında çalışmasını sağlar. Bu varsayılan davranışla dağıtılan AKS kümeleri, donanım hatasına veya planlanmış bakım olayına karşı korumak için yüksek düzeyde kullanılabilirlik sağlar.

Uygulamalarınız için daha yüksek bir kullanılabilirlik düzeyi sağlamak için AKS kümeleri kullanılabilirlik bölgeleri arasında dağıtılabilir. Bu bölgeler, belirli bir bölge içindeki fiziksel olarak ayrı veri merkezleridir. Küme bileşenleri birden çok bölgeye dağıtıldığında, AKS kümeniz bu bölgelerden birinde bir hataya tolerans gösterebilir. Tüm veri merkezinin bir sorunu olsa bile uygulamalarınız ve yönetim işlemleriniz kullanılabilir olmaya devam eder.

Bu makalede, bir AKS kümesi oluşturmak ve kullanılabilirlik bölgeleri arasında düğüm bileşenleri dağıtmak nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.76 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="limitations-and-region-availability"></a>Sınırlamalar ve bölge kullanılabilirliği

AKS kümeleri şu anda aşağıdaki bölgelerdeki kullanılabilirlik bölgeleri kullanılarak oluşturulabilir:

* Orta ABD
* Doğu ABD 2
* Doğu ABD
* Orta Fransa
* Doğu Japonya
* Kuzey Avrupa
* Güneydoğu Asya
* Güney Birleşik Krallık
* Batı Avrupa
* Batı ABD 2

Kullanılabilirlik bölgelerini kullanarak bir AKS kümesi oluşturduğunuzda aşağıdaki sınırlamalar geçerlidir:

* Kullanılabilirlik bölgelerini yalnızca küme oluşturulduğunda etkinleştirebilirsiniz.
* Kullanılabilirlik bölgesi ayarları küme oluşturulduktan sonra güncelleştirilemez. Ayrıca, kullanılabilirlik bölgelerini kullanmak için varolan, kullanılabilir olmayan bir bölge kümesini de güncelleştiremezsiniz.
* Oluşturulduktan sonra bir AKS kümesi için kullanılabilirlik bölgelerini devre dışı kalamazsınız.
* Seçilen düğüm boyutu (VM SKU) tüm kullanılabilirlik bölgelerinde kullanılabilir olmalıdır.
* Kullanılabilirlik bölgeleri etkinleştirilmiş kümeler, bölgeler arasında dağıtım için Azure Standart Yük Dengeleyicileri'nin kullanılmasını gerektirir.
* Standart Yük Dengeleyicileri dağıtmak için Kubernetes sürüm 1.13.5 veya üzeri kullanmalısınız.

Kullanılabilirlik bölgelerini kullanan AKS kümeleri, yük bakiyesi türü için varsayılan değer olan Azure yük dengeleyici *standardı* SKU'yu kullanmalıdır. Bu yük dengeleyici türü yalnızca küme oluşturma zamanında tanımlanabilir. Daha fazla bilgi ve standart yük dengeleyicisinin sınırlamaları için [Azure yük dengeleyicisi standart SKU sınırlamalarına][standard-lb-limitations]bakın.

### <a name="azure-disks-limitations"></a>Azure disksınırlamaları

Azure yönetilen diskleri kullanan birimler şu anda zonal kaynaklar değildir. Özgün bölgelerinden farklı bir bölgede yeniden zamanlanan bölmeler önceki disk(leri yeniden ekemez). Zonal sorunlarla karşılaşabilecek kalıcı depolama gerektirmeyen devletsiz iş yüklerini çalıştırmak önerilir.

Durum dolu iş yüklerini çalıştırmanız gerekiyorsa, Kubernetes zamanlayıcısına disklerinizle aynı bölgede bölmeler oluşturmasını söylemek için pod özelliklerinizdeki bozuk para ve tolere kullanın. Alternatif olarak, bölgeler arasında zamanlanırken bölmelere ekleyebilen Azure Dosyaları gibi ağ tabanlı depolama alanını kullanın.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>AKS kümeleri için kullanılabilirlik bölgelerine genel bakış

Kullanılabilirlik bölgeleri, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirlik teklifidir. Bölgeler, Azure bölgesindeki benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Bölgenin içinde fiziksel olarak ayrılmış kullanılabilirlik alanları uygulamaları ve verileri veri merkezi hatalarına karşı korur. Bölge yedekli hizmetler, tek hata noktalarından korunmak için uygulamalarınızı ve verilerinizi kullanılabilirlik bölgeleri arasında çoğaltır.

Daha fazla bilgi için [Azure'daki kullanılabilirlik bölgeleri nelerdir?][az-overview]

Kullanılabilirlik bölgeleri kullanılarak dağıtılan AKS kümeleri, düğümleri tek bir bölge içinde birden çok bölgeye dağıtabilir. Örneğin, Doğu ABD 2 bölgesindeki bir küme, *Doğu ABD 2'deki* *East US 2* üç kullanılabilirlik bölgesinde de düğüm oluşturabilir. AKS küme kaynaklarının bu dağılımı, belirli bir bölgenin başarısızlığına karşı dirençli oldukları için küme kullanılabilirliğini artırır.

![Kullanılabilirlik bölgeleri arasında AKS düğümü dağıtımı](media/availability-zones/aks-availability-zones.png)

Bölge kesintisinde düğümler el ile veya küme otomatik ölçekleyicikullanılarak yeniden dengelenebilir. Tek bir bölge kullanılamıyorsa, uygulamalarınız çalışmaya devam eder.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik bölgeleri arasında bir AKS kümesi oluşturma

[Az aks oluşturma][az-aks-create] komutunu kullanarak bir `--zones` küme oluşturduğunuzda, parametre aracı düğümlerinin hangi bölgelere dağıtılan bölgeleri tanımlar. Küme oluşturma zamanında `--zones` parametreyi tanımladığınızda, kümeniz için AKS denetim düzlemi bileşenleri de kullanılabilir en yüksek yapılandırmadaki bölgelere yayılır.

Aks kümesi oluşturduğunuzda varsayılan aracı havuzu için herhangi bir bölge tanımlamazsanız, kümeniz için AKS denetim düzlemi bileşenleri kullanılabilirlik bölgeleri kullanmaz. [Az aks nodepool eklemek][az-aks-nodepool-add] komutu kullanarak ek düğüm `--zones` havuzları ekleyebilirsiniz ve bu yeni düğümler için belirtin, ancak kontrol düzlemi bileşenleri kullanılabilirlik bölge farkındaolmadan kalır. Bir düğüm havuzu veya AKS denetim düzlemi bileşenleri dağıtıldıktan sonra bölge bilinirliğini değiştiremezsiniz.

Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myAKSCluster* adında bir AKS kümesi oluşturur. Toplam *3* düğüm oluşturulur - bölge *1'de*bir ajan, *2'de*bir ve *3'te*bir. AKS denetim düzlemi bileşenleri, küme oluşturma işleminin bir parçası olarak tanımlandıkları için kullanılabilir en yüksek yapılandırmadaki bölgeler eteklerine de dağıtılır.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --generate-ssh-keys \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --node-count 3 \
    --zones 1 2 3
```

AKS kümesini oluşturmak birkaç dakika sürer.

## <a name="verify-node-distribution-across-zones"></a>Düğüm dağılımını bölgeler arasında doğrulama

Küme hazır olduğunda, hangi kullanılabilirlik bölgesinde dağıtıldıklarını görmek için ajan düğümlerini ölçek kümesinde listelendirin.

İlk olarak, [az aks get-credentials][az-aks-get-credentials] komutunu kullanarak AKS küme kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Ardından, kümedeki düğümleri listelemek için [kubectl açıklama][kubectl-describe] komutunu kullanın. Aşağıdaki örnekte gösterildiği gibi *failure-domain.beta.kubernetes.io/zone* değeri filtreleyin:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

Aşağıdaki örnek çıktı, belirtilen bölge ve kullanılabilirlik bölgeleri arasında dağıtılan ilk kullanılabilirlik bölgesi için *eastus2-1* ve ikinci kullanılabilirlik bölgesi için *eastus2-2* gibi üç düğümü gösterir:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

Aracı havuzuna ek düğümler eklediğinizde, Azure platformu temel VM'leri belirtilen kullanılabilirlik bölgeleri arasında otomatik olarak dağıtır.

Yeni Kubernetes sürümlerinde (1.17.0 ve sonrası) AKS'nin, `topology.kubernetes.io/zone` amortismana yeni `failure-domain.beta.kubernetes.io/zone`etiketek kullandığını unutmayın.

## <a name="verify-pod-distribution-across-zones"></a>Bölgeler arasında pod dağılımını doğrulama

[Bilinen Etiketler, Ek Açıklamalar ve Taints'te][kubectl-well_known_labels]belgelendirildikçe, `failure-domain.beta.kubernetes.io/zone` Kubernetes kullanılabilir farklı bölgeler arasında bölmeleri otomatik olarak çoğaltma denetleyicisinde veya hizmette dağıtmak için etiketi kullanır. Bunu test etmek için, doğru bölme yayılımı doğrulamak için kümenizi 3'ten 5'e ölçeklendirebilirsiniz:

```azurecli-interactive
az aks scale \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 5
```

Ölçek işlemi birkaç dakika sonra tamamlandığında, `kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"` komut bu örneğe benzer bir çıktı vermelidir:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
Name:       aks-nodepool1-28993262-vmss000003
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000004
            failure-domain.beta.kubernetes.io/zone=eastus2-2
```

Gördüğünüz gibi, şimdi bölge 1 ve 2'de iki ek düğümüz daha var. Üç yinelemeden oluşan bir uygulama dağıtabilirsiniz. NGINX'i örnek olarak kullanacağız:

```console
kubectl run nginx --image=nginx --replicas=3
```

Bölmelerinizin çalıştığı düğümün çalıştığını doğrularsanız, bölmelerin üç farklı kullanılabilirlik bölgesine karşılık gelen bölmelerde çalıştığını görürsünüz. Örneğin komutile `kubectl describe pod | grep -e "^Name:" -e "^Node:"` buna benzer bir çıktı alırsınız:

```console
Name:         nginx-6db489d4b7-ktdwg
Node:         aks-nodepool1-28993262-vmss000000/10.240.0.4
Name:         nginx-6db489d4b7-v7zvj
Node:         aks-nodepool1-28993262-vmss000002/10.240.0.6
Name:         nginx-6db489d4b7-xz6wj
Node:         aks-nodepool1-28993262-vmss000004/10.240.0.8
```

Önceki çıktıdan da görebileceğiniz gibi, ilk bölme kullanılabilirlik bölgesinde `eastus2-1`bulunan düğüm 0 üzerinde çalışıyor. İkinci bölme düğüm 2 üzerinde çalışıyor, hangi `eastus2-3`karşılık gelir , ve düğüm `eastus2-2`4 üçüncü bir, içinde . Herhangi bir ek yapılandırma olmadan, Kubernetes pods doğru üç kullanılabilirlik bölgeleri arasında yayılıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik bölgeleri kullanan bir AKS kümesinin nasıl oluşturulacak ayrıntılı olarak anlatılabilir. Yüksek kullanılabilir kümeler hakkında daha fazla bilgi için, [AKS'de iş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalara][best-practices-bc-dr]bakın.

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-well_known_labels]: https://kubernetes.io/docs/reference/kubernetes-api/labels-annotations-taints/
