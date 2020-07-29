---
title: Azure Kubernetes Service (AKS) içinde kullanılabilirlik bölgelerini kullanma
description: Azure Kubernetes Service (AKS) içindeki kullanılabilirlik bölgelerine düğümleri dağıtan bir küme oluşturmayı öğrenin
services: container-service
ms.custom: fasttrack-edit, references_regions
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: feea8c3cba170244be2ca3ec7a11c36a3c39f700
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281234"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Kullanılabilirlik alanlarını kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma

Bir Azure Kubernetes hizmeti (AKS) kümesi, temel alınan Azure altyapısının mantıksal bölümleri arasında düğümler ve depolama gibi kaynakları dağıtır. Bu dağıtım modeli kullanılabilirlik alanları kullanılırken, belirli bir kullanılabilirlik bölgesindeki düğümlerin, başka bir kullanılabilirlik alanında tanımlananlardan fiziksel olarak ayrılması güvence altına alınır. Bir küme genelinde yapılandırılmış birden çok kullanılabilirlik bölgesi ile dağıtılan AKS kümeleri, bir donanım hatasına veya planlı bakım olayına karşı koruma için daha yüksek düzeyde kullanılabilirlik sağlar.

Birden çok bölgeye yaymak üzere bir kümede düğüm havuzlarını tanımlayarak, belirli bir düğüm havuzundaki düğümler, tek bir bölge silinse bile çalışmaya devam edebilir. Tek bir veri merkezinde fiziksel bir hata olsa bile uygulamalarınız, düğümlerin bir alt kümesinin hatasını kabul etmek üzere ayarlandığında uygulamalarınız kullanılabilir olmaya devam edebilir.

Bu makalede bir AKS kümesi oluşturma ve düğüm bileşenlerini kullanılabilirlik alanları arasında dağıtma gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.76 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir.  `az --version`Sürümü bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

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
* West Europe
* Batı ABD 2

Kullanılabilirlik bölgelerini kullanarak bir AKS kümesi oluşturduğunuzda aşağıdaki sınırlamalar geçerlidir:

* Kullanılabilirlik bölgelerini yalnızca küme veya düğüm havuzu oluşturulduğunda tanımlayabilirsiniz.
* Kullanılabilirlik alanı ayarları, küme oluşturulduktan sonra güncelleştirilemiyor. Kullanılabilirlik alanlarını kullanmak için mevcut, kullanılabilirlik dışı bir bölge kümesini de güncelleştiremezsiniz.
* Seçilen düğüm boyutu (VM SKU 'SU) seçili olan tüm kullanılabilirlik bölgelerinde kullanılabilir olmalıdır.
* Kullanılabilirlik alanları etkin olan kümeler, bölgeler arasında dağıtım için Azure Standart yük dengeleyiciler kullanılmasını gerektirir. Bu yük dengeleyici türü, yalnızca küme oluşturma zamanında tanımlanabilir. Daha fazla bilgi ve standart yük dengeleyicinin sınırlamaları için bkz. [Azure yük dengeleyici standart SKU sınırlamaları][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Azure diskler sınırlamaları

Azure yönetilen diskleri kullanan birimler Şu anda bölgesel olarak yedekli kaynaklar değildir. Birimler bölgelere bağlanamaz ve hedef Pod 'u barındıran belirli bir düğümle aynı bölgede birlikte bulunması gerekir.

Durum bilgisi olan iş yükleri çalıştırmanız gerekiyorsa, Pod özelliklerini disklerinizdeki aynı bölgede gruplamak için pod özelliklerine göre düğüm havuzu talarını ve toleranlarını kullanın. Alternatif olarak, bölgeler arasında zamanlandıkları üzere Pod 'ye iliştirilabilen Azure dosyaları gibi ağ tabanlı depolama alanı kullanın.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>AKS kümeleri için kullanılabilirlik bölgelerine genel bakış

Kullanılabilirlik alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Bölgeler, bir Azure bölgesi içinde benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılık sağlamak için, tüm bölge etkin bölgelerde en az üç ayrı bölge vardır. Bölgenin içinde fiziksel olarak ayrılmış kullanılabilirlik alanları uygulamaları ve verileri veri merkezi hatalarına karşı korur.

Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik bölgeleri nelerdir?][az-overview].

Kullanılabilirlik alanları kullanılarak dağıtılan AKS kümeleri, düğümleri tek bir bölge içinde birden çok bölgeye dağıtabilir. Örneğin, Doğu ABD 2 bölgesindeki bir küme, *East US 2*   *Doğu ABD 2*üç kullanılabilirlik alanında bulunan düğümleri oluşturabilir. AKS kümesi kaynaklarının bu dağıtımı, belirli bir bölgede hatalara dayanıklı oldukları için küme kullanılabilirliğini geliştirir.

![Kullanılabilirlik alanları arasında AKS düğüm dağılımı](media/availability-zones/aks-availability-zones.png)

Tek bir bölge kullanılamaz duruma gelirse, küme birden çok bölgeye yayıldığında uygulamalarınız çalışmaya devam eder.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik alanları arasında AKS kümesi oluşturma

[Az aks Create][az-aks-create] komutunu kullanarak bir küme oluşturduğunuzda, `--zones` parametresi hangi bölge Aracısı düğümlerinin dağıtılacağını tanımlar. Parametreyi küme oluşturma zamanında tanımlarsanız, etcd gibi denetim düzlemi bileşenleri üç bölgeye yayılır `--zones` . Denetim düzlemi bileşenlerinin yayıldığı belirli bölgeler, ilk düğüm havuzu için seçilen açık bölgelerin bağımsızdır.

Bir AKS kümesi oluştururken varsayılan aracı havuzu için herhangi bir bölge tanımlamadıysanız, denetim düzlemi bileşenlerinin kullanılabilirlik alanları arasında yayılmasının garantisi yoktur. [Az aks nodepool Add][az-aks-nodepool-add] komutunu kullanarak ek düğüm havuzları ekleyebilir ve `--zones` yeni düğümler belirtebilirsiniz, ancak denetim düzleminin bölgeler arasında yayılmasını değiştirmez. Kullanılabilirlik alanı ayarları, yalnızca küme veya düğüm havuzu oluşturma sırasında tanımlanabilir.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı bir aks kümesi oluşturur. Toplam *3* düğüm oluşturulur-bölge *1*' de bir aracı, biri *2*' de ve diğeri *3*' te.

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

AKS kümesinin oluşturulması birkaç dakika sürer.

Yeni bir düğümün hangi bölgeye ait olduğuna karar verirken, belirli bir AKS düğüm havuzu [temel alınan Azure sanal makine ölçek kümeleri tarafından sunulan en iyi efor bölge dengelemesini][vmss-zone-balancing]kullanacaktır. Belirli bir AKS düğüm havuzu, ölçek kümesinin diğer bölgelerinde aynı sayıda VM veya + 1 VM varsa "dengeli" olarak değerlendirilir \- .

## <a name="verify-node-distribution-across-zones"></a>Bölgeler arasında düğüm dağıtımını doğrulama

Küme hazır olduğunda, hangi kullanılabilirlik bölgesini dağıttığımız görmek için ölçek kümesindeki aracı düğümlerini listeleyin.

İlk olarak, [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak aks kümesi kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Sonra, küme içindeki düğümleri listelemek ve *Failure-Domain.Beta.Kubernetes.io/Zone* değerini filtrelemek için [kubectl açıkla][kubectl-describe] komutunu kullanın. Aşağıdaki örnek bir bash kabuğu içindir.

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

Daha yeni Kubernetes sürümlerinde (1.17.0 ve üzeri), AKS 'in `topology.kubernetes.io/zone` kullanım dışı öğesine ek olarak daha yeni etiketi kullandığını unutmayın `failure-domain.beta.kubernetes.io/zone` .

## <a name="verify-pod-distribution-across-zones"></a>Bölgeler arasında Pod dağıtımını doğrulama

[Iyi bilinen Etiketler, ek açıklamalar ve tatları][kubectl-well_known_labels]de belgelendiği gibi, Kubernetes, `failure-domain.beta.kubernetes.io/zone` bir çoğaltma denetleyicisindeki veya hizmette yer alan farklı bölgelerde bulunan alanları otomatik olarak dağıtmak için etiketini kullanır. Bunu test etmek için, doğru Pod yayılmasını doğrulamak üzere kümenizi 3 ' ten 5 düğüme ölçeklendirebilirsiniz:

```azurecli-interactive
az aks scale \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 5
```

Ölçek işlemi birkaç dakika sonra tamamlandığında, `kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"` bir bash kabuğu 'ndaki komut bu örneğe benzer bir çıktı vermelidir:

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

Artık 1 ve 2. bölgelerde iki ek düğüm vardır. Üç çoğaltmalardan oluşan bir uygulamayı dağıtabilirsiniz. Örnek olarak NGıNX kullanacağız:

```console
kubectl run nginx --image=nginx --replicas=3
```

Yığınlarınızın çalıştığı düğümleri görüntüleyerek, üç farklı kullanılabilirlik bölgesine karşılık gelen düğümlerde Pod 'nin çalıştığını görürsünüz. Örneğin, `kubectl describe pod | grep -e "^Name:" -e "^Node:"` bir bash kabuğu 'ndaki komutuyla şuna benzer bir çıktı alırsınız:

```console
Name:         nginx-6db489d4b7-ktdwg
Node:         aks-nodepool1-28993262-vmss000000/10.240.0.4
Name:         nginx-6db489d4b7-v7zvj
Node:         aks-nodepool1-28993262-vmss000002/10.240.0.6
Name:         nginx-6db489d4b7-xz6wj
Node:         aks-nodepool1-28993262-vmss000004/10.240.0.8
```

Önceki çıktıdan görebileceğiniz gibi, ilk Pod, kullanılabilirlik bölgesinde bulunan 0 düğümünde çalışır `eastus2-1` . İkinci Pod, ' a karşılık gelen düğüm 2 ' de `eastus2-3` ve içinde üçüncü tane düğüm 4 ' te çalışır `eastus2-2` . Ek bir yapılandırma olmadan, Kubernetes üç kullanılabilirlik alanı genelinde doğru şekilde yayılmaktadır.

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
[vmss-zone-balancing]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md#zone-balancing

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-well_known_labels]: https://kubernetes.io/docs/reference/kubernetes-api/labels-annotations-taints/
