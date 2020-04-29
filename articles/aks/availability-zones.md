---
title: Azure Kubernetes Service (AKS) içinde kullanılabilirlik bölgelerini kullanma
description: Azure Kubernetes Service (AKS) içindeki kullanılabilirlik bölgelerine düğümleri dağıtan bir küme oluşturmayı öğrenin
services: container-service
ms.custom: fasttrack-edit
ms.topic: article
ms.date: 06/24/2019
ms.openlocfilehash: 5693d9e90de9ba68e7b76e0f2bd5b75141dbda71
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77596819"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Kullanılabilirlik alanlarını kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma

Bir Azure Kubernetes hizmeti (AKS) kümesi, temel alınan Azure işlem altyapısının mantıksal bölümlerinin düğümleri ve depolanması gibi kaynakları dağıtır. Bu dağıtım modeli, düğümlerin tek bir Azure veri merkezinde ayrı güncelleştirme ve hata etki alanları arasında çalıştığından emin olmanızı sağlar. Bu varsayılan davranışla dağıtılan AKS kümeleri, bir donanım hatası veya planlı bakım olayına karşı korumak için yüksek düzeyde kullanılabilirlik sağlar.

Uygulamalarınıza yönelik daha yüksek düzeyde kullanılabilirlik sağlamak için, AKS kümeleri kullanılabilirlik alanları arasında dağıtılabilir. Bu bölgeler, belirli bir bölgedeki fiziksel olarak ayrı veri merkezlerdir. Küme bileşenleri birden çok bölgeye dağıtıldığında, AKS kümeniz bu bölgelerden birindeki bir hatayı kabul edebilir. Tüm veri merkezinde bir sorun olsa bile Uygulamalarınız ve yönetim işlemleri kullanılabilir olmaya devam eder.

Bu makalede bir AKS kümesi oluşturma ve düğüm bileşenlerini kullanılabilirlik alanları arasında dağıtma gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.76 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü `az --version` bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

## <a name="limitations-and-region-availability"></a>Sınırlamalar ve bölge kullanılabilirliği

AKS kümeleri Şu anda şu bölgelerde kullanılabilirlik alanları kullanılarak oluşturulabilir:

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
* Kullanılabilirlik alanı ayarları, küme oluşturulduktan sonra güncelleştirilemiyor. Kullanılabilirlik alanlarını kullanmak için mevcut, kullanılabilirlik dışı bir bölge kümesini de güncelleştiremezsiniz.
* Bir AKS kümesi oluşturulduktan sonra kullanılabilirlik bölgelerini devre dışı bırakamıyoruz.
* Seçilen düğüm boyutu (VM SKU 'SU) tüm kullanılabilirlik alanlarında kullanılabilir olmalıdır.
* Kullanılabilirlik alanları etkin olan kümeler, bölgeler arasında dağıtım için Azure Standart yük dengeleyiciler kullanılmasını gerektirir.
* Standart yük dengeleyiciler dağıtmak için Kubernetes sürüm 1.13.5 veya üstünü kullanmanız gerekir.

Kullanılabilirlik bölgelerini kullanan AKS kümelerinin, yük dengeleyici türü için varsayılan değer olan Azure yük dengeleyici *Standart* SKU 'su kullanması gerekir. Bu yük dengeleyici türü, yalnızca küme oluşturma zamanında tanımlanabilir. Daha fazla bilgi ve standart yük dengeleyicinin sınırlamaları için bkz. [Azure yük dengeleyici standart SKU sınırlamaları][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Azure diskler sınırlamaları

Azure yönetilen diskleri kullanan birimler Şu anda bir kaynak değildir. Özgün bölgesinden farklı bir bölgede yeniden zamanlanan pods 'ler önceki diskleri yeniden ekleyemez. Bu sorunlara yönelik kalıcı depolama gerektirmeyen, durum bilgisiz iş yükleri çalıştırmanız önerilir.

Durum bilgisi olan iş yükleri çalıştırmanız gerekiyorsa, Kubernetes Scheduler ' ın disklerinizde aynı bölgede yer alan bir düğüm oluşturmasını söylemek için pod belirtimlerinizde tatları ve toleransı kullanın. Alternatif olarak, bölgeler arasında zamanlandıkları üzere Pod 'ye iliştirilabilen Azure dosyaları gibi ağ tabanlı depolama alanı kullanın.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>AKS kümeleri için kullanılabilirlik bölgelerine genel bakış

Kullanılabilirlik alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Bölgeler, bir Azure bölgesi içinde benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılık sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Bölgenin içinde fiziksel olarak ayrılmış kullanılabilirlik alanları uygulamaları ve verileri veri merkezi hatalarına karşı korur. Bölgesel olarak yedekli hizmetler, uygulamalarınızı ve verilerinizi kullanılabilirlik alanları arasında çoğaltarak hata noktalarından koruyun.

Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik bölgeleri nelerdir?][az-overview].

Kullanılabilirlik alanları kullanılarak dağıtılan AKS kümeleri, düğümleri tek bir bölge içinde birden çok bölgeye dağıtabilir. Örneğin, *Doğu ABD 2* bölgesindeki bir küme, *Doğu ABD 2*üç kullanılabilirlik alanında bulunan düğümleri oluşturabilir. AKS kümesi kaynaklarının bu dağıtımı, belirli bir bölgede hatalara dayanıklı oldukları için küme kullanılabilirliğini geliştirir.

![Kullanılabilirlik alanları arasında AKS düğüm dağılımı](media/availability-zones/aks-availability-zones.png)

Bir bölge kesintisi durumunda, düğümler el ile yeniden dengelenebilir veya küme otomatik olarak kullanılabilir. Tek bir bölge kullanılamaz duruma gelirse, uygulamalarınız çalışmaya devam eder.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik alanları arasında AKS kümesi oluşturma

[Az aks Create][az-aks-create] komutunu kullanarak bir küme oluşturduğunuzda, `--zones` parametresi hangi bölge Aracısı düğümlerinin dağıtılacağını tanımlar. Kümeniz için AKS denetim düzlemi bileşenleri, küme oluşturma zamanında `--zones` parametresini tanımlarken kullanılabilir en yüksek yapılandırmaya sahip bölgelere yayılır.

Bir AKS kümesi oluştururken varsayılan aracı havuzu için herhangi bir bölge tanımlamadıysanız, kümenizin AKS denetim düzlemi bileşenleri kullanılabilirlik bölgelerini kullanmaz. [Az aks nodepool Add][az-aks-nodepool-add] komutunu kullanarak ek düğüm havuzları ekleyebilir ve bu yeni düğümleri belirtebilirsiniz `--zones` , ancak denetim düzlemi bileşenleri kullanılabilirlik alanı tanıma olmadan kalır. Bir düğüm havuzu veya AKS denetim düzlemi bileşenleri dağıtıldıktan sonra bölge tanımayı değiştiremezsiniz.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı bir aks kümesi oluşturur. Toplam *3* düğüm oluşturulur-bölge *1*' de bir aracı, biri *2*' de ve diğeri *3*' te. AKS denetim düzlemi bileşenleri, küme oluşturma işleminin bir parçası olarak tanımlandıklarından, en yüksek kullanılabilir yapılandırma alanları arasında da dağıtılır.

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

## <a name="verify-node-distribution-across-zones"></a>Bölgeler arasında düğüm dağıtımını doğrulama

Küme hazır olduğunda, hangi kullanılabilirlik bölgesini dağıttığımız görmek için ölçek kümesindeki aracı düğümlerini listeleyin.

İlk olarak, [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak aks kümesi kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Sonra, küme içindeki düğümleri listelemek için [kubectl açıkla][kubectl-describe] komutunu kullanın. Aşağıdaki örnekte gösterildiği gibi *Failure-Domain.Beta.Kubernetes.io/Zone* değerini filtreleyin:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

Aşağıdaki örnek çıktıda, ilk kullanılabilirlik bölgesi için *eastus2-1* ve ikinci kullanılabilirlik bölgesi için *eastus2-2* gibi belirtilen bölge ve kullanılabilirlik bölgeleri arasında dağıtılan üç düğüm gösterilmektedir:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

Bir aracı havuzuna ek düğümler eklediğinizde, Azure platformu, belirtilen kullanılabilirlik alanları genelinde temel alınan VM 'Leri otomatik olarak dağıtır.

Daha yeni Kubernetes sürümlerinde (1.17.0 ve üzeri), AKS 'in kullanım dışı `topology.kubernetes.io/zone` `failure-domain.beta.kubernetes.io/zone`öğesine ek olarak daha yeni etiketi kullandığını unutmayın.

## <a name="verify-pod-distribution-across-zones"></a>Bölgeler arasında Pod dağıtımını doğrulama

[Iyi bilinen Etiketler, ek açıklamalar ve tatları][kubectl-well_known_labels]de belgelendiği gibi, Kubernetes, `failure-domain.beta.kubernetes.io/zone` bir çoğaltma denetleyicisindeki veya hizmette yer alan farklı bölgelerde bulunan alanları otomatik olarak dağıtmak için etiketini kullanır. Bunu test etmek için, doğru Pod yayılmasını doğrulamak üzere kümenizi 3 ' ten 5 düğüme ölçeklendirebilirsiniz:

```azurecli-interactive
az aks scale \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 5
```

Ölçek işlemi birkaç dakika sonra tamamlandığında, komut `kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"` bu örneğe benzer bir çıktı vermelidir:

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

Gördüğünüz gibi, artık 1 ve 2. bölgelerde iki ek düğüm vardır. Üç çoğaltmalardan oluşan bir uygulamayı dağıtabilirsiniz. Örnek olarak NGıNX kullanacağız:

```console
kubectl run nginx --image=nginx --replicas=3
```

Yığınlarınızın çalıştığı düğümlerin olduğunu doğruladıktan sonra, üç farklı kullanılabilirlik bölgesine karşılık gelen düğüm üzerinde pod 'nin çalıştığını görürsünüz. Örneğin, komutuyla `kubectl describe pod | grep -e "^Name:" -e "^Node:"` şuna benzer bir çıktı alırsınız:

```console
Name:         nginx-6db489d4b7-ktdwg
Node:         aks-nodepool1-28993262-vmss000000/10.240.0.4
Name:         nginx-6db489d4b7-v7zvj
Node:         aks-nodepool1-28993262-vmss000002/10.240.0.6
Name:         nginx-6db489d4b7-xz6wj
Node:         aks-nodepool1-28993262-vmss000004/10.240.0.8
```

Önceki çıktıdan görebileceğiniz gibi, ilk Pod, kullanılabilirlik bölgesinde `eastus2-1`bulunan 0 düğümünde çalışır. İkinci Pod, ' a karşılık gelen `eastus2-3`düğüm 2 ' de ve içinde `eastus2-2`üçüncü tane düğüm 4 ' te çalışır. Ek bir yapılandırma olmadan, Kubernetes üç kullanılabilirlik alanı genelinde doğru şekilde yayılmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik bölgelerini kullanan bir AKS kümesi oluşturma hakkında ayrıntılı olarak açıklanmıştır. Yüksek oranda kullanılabilir kümeler hakkında daha fazla bilgi için bkz. [AKS 'de iş sürekliliği ve olağanüstü durum kurtarma Için en iyi uygulamalar][best-practices-bc-dr].

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
