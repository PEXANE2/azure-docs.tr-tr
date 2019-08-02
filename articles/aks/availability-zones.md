---
title: Azure Kubernetes Service (AKS) içinde Kullanılabilirlik Alanları kullanma
description: Azure Kubernetes Service (AKS) içindeki kullanılabilirlik bölgelerine düğümleri dağıtan bir küme oluşturmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/24/2019
ms.author: iainfou
ms.openlocfilehash: 0f99386aa9eeb75a990507e383c32412fb39eceb
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67840686"
---
# <a name="preview---create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Önizleme-Kullanılabilirlik Alanları kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma

Bir Azure Kubernetes hizmeti (AKS) kümesi, temel alınan Azure işlem altyapısının mantıksal bölümlerinin düğümleri ve depolanması gibi kaynakları dağıtır. Bu dağıtım modeli, düğümlerin tek bir Azure veri merkezinde ayrı güncelleştirme ve hata etki alanları arasında çalıştığından emin olmanızı sağlar. Bu varsayılan davranışla dağıtılan AKS kümeleri, bir donanım hatası veya planlı bakım olayına karşı korumak için yüksek düzeyde kullanılabilirlik sağlar.

Uygulamalarınıza yönelik daha yüksek düzeyde kullanılabilirlik sağlamak için, AKS kümeleri kullanılabilirlik alanları arasında dağıtılabilir. Bu bölgeler, belirli bir bölgedeki fiziksel olarak ayrı veri merkezlerdir. Küme bileşenleri birden çok bölgeye dağıtıldığında, AKS kümeniz bu bölgelerden birindeki bir hatayı kabul edebilir. Tüm veri merkezinde bir sorun olsa bile Uygulamalarınız ve yönetim işlemleri kullanılabilir olmaya devam eder.

Bu makalede bir AKS kümesi oluşturma ve düğüm bileşenlerini kullanılabilirlik alanları arasında dağıtma gösterilmektedir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme özellikleridir. Topluluğumuza geri bildirim ve hata toplamak için sağlanırlar. Önizlemede, bu özellikler üretim kullanımı için tasarlanmamıştır. Genel önizlemede bulunan Özellikler ' en iyi çaba ' desteği altına düşmektedir. AKS teknik destek ekiplerinden yardım yalnızca çalışma saatleri Pasifik saat dilimi (PST) sırasında kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.66 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü `az --version` bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Kullanılabilirlik alanları kullanan AKS kümeleri oluşturmak için, *aks-Preview* CLI uzantısının sürüm 0.4.1 veya üzeri bir sürümü gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flags-for-your-subscription"></a>Aboneliğiniz için özellik bayraklarını kaydetme

Bölge kullanılabilirliği olan bir AKS kümesi oluşturmak için, öncelikle aboneliğinizde bazı özellik bayraklarını etkinleştirin. Kümeler, Kubernetes düğümlerinin dağıtımını ve yapılandırmasını yönetmek için bir sanal makine ölçek kümesi kullanır. Azure Yük dengeleyicisinin *Standart* SKU 'su, trafiği kümenize yönlendiren ağ bileşenlerine dayanıklılık sağlamak için de gereklidir. Aşağıdaki örnekte gösterildiği gibi, [az Feature Register][az-feature-register] komutunu kullanarak, *kullanılabilirliği*olan *AKSAzureStandardLoadBalancer*ve *VMSSPreview* Özellik bayraklarını kaydedin:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AvailabilityZonePreview --namespace Microsoft.ContainerService
az feature register --name AKSAzureStandardLoadBalancer --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AvailabilityZonePreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations-and-region-availability"></a>Sınırlamalar ve bölge kullanılabilirliği

AKS kümeleri Şu anda şu bölgelerde kullanılabilirlik alanları kullanılarak oluşturulabilir:

* Doğu ABD 2
* Kuzey Avrupa
* Güneydoğu Asya
* Batı Avrupa
* Batı ABD 2

Kullanılabilirlik bölgelerini kullanarak bir AKS kümesi oluşturduğunuzda aşağıdaki sınırlamalar geçerlidir:

* Kullanılabilirlik bölgelerini yalnızca küme oluşturulduğunda etkinleştirebilirsiniz.
* Kullanılabilirlik alanı ayarları, küme oluşturulduktan sonra güncelleştirilemiyor. Kullanılabilirlik alanlarını kullanmak için mevcut, kullanılabilirlik dışı bir bölge kümesini de güncelleştiremezsiniz.
* Bir AKS kümesi oluşturulduktan sonra kullanılabilirlik bölgelerini devre dışı bırakamıyoruz.
* Seçilen düğüm boyutu (VM SKU 'SU) tüm kullanılabilirlik alanlarında kullanılabilir olmalıdır.
* Kullanılabilirlik alanları etkin olan kümeler, bölgeler arasında dağıtım için Azure Standart yük dengeleyiciler kullanılmasını gerektirir.
* Standart yük dengeleyiciler dağıtmak için Kubernetes sürüm 1.13.5 veya üstünü kullanmanız gerekir.

Kullanılabilirlik bölgelerini kullanan AKS kümelerinin Azure yük dengeleyici *Standart* SKU 'sunu kullanması gerekir. Azure Yük dengeleyicinin varsayılan *temel* SKU 'su, kullanılabilirlik alanları arasında dağıtımı desteklemez. Daha fazla bilgi ve standart yük dengeleyicinin sınırlamaları için bkz. [Azure yük dengeleyici standart SKU önizlemesi sınırlamaları][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Azure diskler sınırlamaları

Azure yönetilen diskleri kullanan birimler Şu anda bir kaynak değildir. Özgün bölgesinden farklı bir bölgede yeniden zamanlanan pods 'ler önceki diskleri yeniden ekleyemez. Bu sorunlara yönelik kalıcı depolama gerektirmeyen, durum bilgisiz iş yükleri çalıştırmanız önerilir.

Durum bilgisi olan iş yükleri çalıştırmanız gerekiyorsa, Kubernetes Scheduler ' ın disklerinizde aynı bölgede yer alan bir düğüm oluşturmasını söylemek için pod belirtimlerinizde tatları ve toleransı kullanın. Alternatif olarak, bölgeler arasında zamanlandıkları üzere Pod 'ye iliştirilabilen Azure dosyaları gibi ağ tabanlı depolama alanı kullanın.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>AKS kümelerine yönelik Kullanılabilirlik Alanları genel bakış

Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Bölgeler, bir Azure bölgesi içinde benzersiz fiziksel konumlardır. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Bir bölgedeki Kullanılabilirlik Alanları fiziksel ayrımı, uygulamaları ve verileri veri merkezi hatalarından korur. Bölgesel olarak yedekli hizmetler, uygulamalarınızı ve verilerinizi Kullanılabilirlik Alanları arasında çoğaltarak hata noktalarından koruyun.

Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik alanları nedir?][az-overview].

Kullanılabilirlik alanları kullanılarak dağıtılan AKS kümeleri, düğümleri tek bir bölge içinde birden çok bölgeye dağıtabilir. Örneğin, *Doğu ABD 2* bölgesindeki bir küme, *Doğu ABD 2*üç kullanılabilirlik alanında bulunan düğümleri oluşturabilir. AKS kümesi kaynaklarının bu dağıtımı, belirli bir bölgede hatalara dayanıklı oldukları için küme kullanılabilirliğini geliştirir.

![Kullanılabilirlik alanları arasında AKS düğüm dağılımı](media/availability-zones/aks-availability-zones.png)

Bir bölge kesintisi durumunda, düğümler el ile yeniden dengelenebilir veya küme otomatik olarak kullanılabilir. Tek bir bölge kullanılamaz duruma gelirse, uygulamalarınız çalışmaya devam eder.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik alanları arasında AKS kümesi oluşturma

[Az aks Create][az-aks-create] komutunu kullanarak bir küme oluşturduğunuzda, `--node-zones` parametresi hangi bölge Aracısı düğümlerinin dağıtılacağını tanımlar. Kümenizin aks denetim düzlemi bileşenleri, `--node-zones` parametresini belirten bir küme oluşturduğunuzda, en yüksek kullanılabilir yapılandırmadaki bölgelere de yayılır.

Bir AKS kümesi oluştururken varsayılan aracı havuzu için herhangi bir bölge tanımlamadıysanız, kümenizin AKS denetim düzlemi bileşenleri kullanılabilirlik bölgelerini kullanmaz. [Az aks nodepool Add][az-aks-nodepool-add] komutunu kullanarak diğer düğüm havuzlarını (Şu anda aks ' de önizleme aşamasındadır) ekleyebilir ve bu yeni `--node-zones` aracı düğümleri için belirtebilirsiniz, ancak denetim düzlemi bileşenleri kullanılabilirlik alanı tanıma olmadan kalır. Bir düğüm havuzu veya AKS denetim düzlemi bileşenleri dağıtıldıktan sonra bölge tanımayı değiştiremezsiniz.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı bir aks kümesi oluşturur. Toplam *3* düğüm oluşturulur-bölge *1*' de bir aracı, biri *2*' de ve diğeri *3*' te. AKS denetim düzlemi bileşenleri, küme oluşturma işleminin bir parçası olarak tanımlandıklarından, en yüksek kullanılabilir yapılandırma alanları arasında da dağıtılır.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version 1.13.5 \
    --generate-ssh-keys \
    --enable-vmss \
    --load-balancer-sku standard \
    --node-count 3 \
    --node-zones 1 2 3
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
