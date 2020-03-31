---
title: Önizleme - Azure Kubernetes Hizmeti (AKS) kümesine nokta düğümü havuzu ekleme
description: Azure Kubernetes Hizmeti (AKS) kümesine nokta düğümü havuzu eklemeyi öğrenin.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 02/25/2020
ms.author: zarhoads
ms.openlocfilehash: 466ad7c88547b6676ba0ae263b74d14059322f1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77622051"
---
# <a name="preview---add-a-spot-node-pool-to-an-azure-kubernetes-service-aks-cluster"></a>Önizleme - Azure Kubernetes Hizmeti (AKS) kümesine nokta düğümü havuzu ekleme

Nokta düğümü havuzu, [spot sanal makine ölçeği kümesi][vmss-spot]tarafından desteklenen bir düğüm havuzudur. AKS kümenizle düğümler için spot VM'leri kullanmak, önemli bir maliyet tasarrufuyla Azure'da kullanılmayan kapasiteden yararlanmanızı sağlar. Kullanılabilir kullanılmayan kapasite miktarı düğüm boyutu, bölge ve günün saati gibi birçok etkene bağlı olarak değişir.

Bir nokta düğümü havuzu dağıtırken, Azure kullanılabilir kapasite varsa nokta düğümlerini ayırır. Ama nokta düğümleri için SLA yok. Nokta düğümü havuzunu destekleyen nokta ölçeği tek bir hata etki alanında dağıtılır ve yüksek kullanılabilirlik garantisi sunmaz. Azure'un kapasiteye geri ihtiyacı olduğunda, Azure altyapısı nokta düğümlerini boşaltacaktır.

Nokta düğümleri, kesintileri, erken sonlandırmaları veya tahliyeleri işleyebilir iş yükleri için harikadır. Örneğin, toplu iş işleri, geliştirme ve test ortamları ve büyük bilgi işlem iş yükleri gibi iş yükleri, nokta düğümü havuzunda zamanlanacak iyi adaylar olabilir.

Bu makalede, varolan bir Azure Kubernetes Hizmeti (AKS) kümesine ikincil bir nokta düğümü havuzu eklersiniz.

Bu makalede, Kubernetes ve Azure Yük Dengeleyici kavramları temel bir anlayış varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS) için Kubernetes temel kavramlarına][kubernetes-concepts]bakın.

Bu özellik şu anda önizleme sürümündedir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Nokta düğümü havuzunu kullanmak üzere bir küme oluşturduğunuzda, bu küme düğüm havuzları ve *Standart* SKU yük dengeleyicisi için Sanal Makine Ölçeği Kümeleri de kullanmalıdır. Nokta düğümü havuzu kullanmak için kümenizi oluşturduktan sonra ek bir düğüm havuzu da eklemeniz gerekir. Ek bir düğüm havuzu ekleme daha sonraki bir adımda ele alınmıştır, ancak önce bir önizleme özelliğini etkinleştirmeniz gerekir.

> [!IMPORTANT]
> AKS önizleme özellikleri self servis, opt-in vardır. Onlar bizim toplumdan geribildirim ve hata toplamak için sağlanır. Önizlemede, bu özellikler üretim kullanımı için değildir. Genel önizlemedeki özellikler 'en iyi çaba' desteği altında kalır. AKS teknik destek ekiplerinden yardım yalnızca iş saatleri Pasifik saat diliminde (PST) kullanılabilir. Daha fazla bilgi için lütfen aşağıdaki destek makalelerini görün:
>
> * [AKS Destek Politikaları][aks-support-policies]
> * [Azure Destek SSS][aks-faq]

### <a name="register-spotpoolpreview-preview-feature"></a>Spotpoolpreview önizleme özelliğini kaydedin

Spot düğüm havuzu kullanan bir AKS kümesi oluşturmak için aboneliğinizde *spotpoolpreview* özellik bayrağını etkinleştirmeniz gerekir. Bu özellik, bir küme yi yapılandırırken en son hizmet geliştirmeleri kümesini sağlar.

> [!CAUTION]
> Bir özelliği bir abonelikte kaydettirdiğinizde, bu özelliği şu anda geri alamazsınız. Bazı önizleme özelliklerini etkinleştirdikten sonra, varsayılanlar abonelikte oluşturulan tüm AKS kümeleri için kullanılabilir. Üretim aboneliklerinde önizleme özelliklerini etkinleştirme. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

Aşağıdaki örnekte gösterildiği gibi [az özellik kayıt][az-feature-register] komutunu kullanarak *spotpoolpreview* özelliği bayrağını kaydedin:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "spotpoolpreview"
```

Durumun *Kayıtlı'yı*göstermesi birkaç dakika sürer. [Az özellik listesi][az-feature-list] komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/spotpoolpreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, [az sağlayıcı kayıt][az-provider-register] komutunu kullanarak *Microsoft.ContainerService* kaynak sağlayıcısının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI uzantısını yükleyin

Nokta düğümü havuzu kullanan bir AKS kümesi oluşturmak *için, aks önizleme* CLI uzantısı sürümü 0.4.32 veya daha yüksek gerekir. [az uzantı ekle][az-extension-add] komutunu kullanarak *aks önizleme* Azure CLI uzantısını yükleyin ve az [uzantı güncelleştirme][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview
 
# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="limitations"></a>Sınırlamalar

Nokta düğümü havuzu olan AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir:

* Nokta düğümü havuzu kümenin varsayılan düğüm havuzu olamaz. Nokta düğümü havuzu yalnızca ikincil bir havuz için kullanılabilir.
* Nokta düğümü havuzları kordon ve drenajı garanti edemediği için nokta düğümü havuzunu yükseltemezsiniz. Kubernetes sürümünü yükseltmek gibi işlemleri yapmak için varolan nokta düğümü havuzunuzu yenisiyle değiştirmeniz gerekir. Bir nokta düğümü havuzunu değiştirmek için, Kubernetes farklı bir sürümü ile yeni bir nokta düğüm havuzu oluşturmak, durumu *Hazır*olana kadar bekleyin, sonra eski düğüm havuzu kaldırın.
* Denetim düzlemi ve düğüm havuzları aynı anda yükseltilemez. Bunları ayrı olarak yükseltmeniz veya aynı anda kontrol düzlemi ve kalan düğüm havuzları yükseltmek için nokta düğüm havuzu kaldırmanız gerekir.
* Nokta düğümü havuzu Sanal Makine Ölçek Kümeleri kullanmalıdır.
* Oluşturulduktan sonra ScaleSetPriority veya SpotMaxPrice'ı değiştiremezsiniz.
* SpotMaxPrice'ı ayarlarken, değer -1 veya en fazla beş ondalık basamakiçeren pozitif bir değer olmalıdır.
* Bir nokta düğümü havuzu etiket *kubernetes.azure.com/scalesetpriority:spot*olacak , leke *kubernetes.azure.com/scalesetpriority=spot:NoSchedule*, ve sistem bölmeleri anti-yakınlık olacaktır.
* Nokta düğümü havuzunda iş yüklerini zamanlamak için [karşılık gelen][spot-toleration] bir hoşgörü eklemeniz gerekir.

## <a name="add-a-spot-node-pool-to-an-aks-cluster"></a>AKS kümesine nokta düğümü havuzu ekleme

Birden çok düğüm havuzu etkin olan varolan bir kümeye nokta düğümü havuzu eklemeniz gerekir. Birden çok düğüm havuzları ile bir AKS küme oluşturma hakkında daha fazla bilgi [burada][use-multiple-node-pools]bulabilirsiniz.

[Az aks nodepool ekle][az-aks-nodepool-add]kullanarak bir düğüm havuzu oluşturun.
```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name spotnodepool \
    --priority Spot \
    --eviction-policy Delete \
    --spot-max-price -1 \
    --enable-cluster-autoscaler \
    --min-count 1 \
    --max-count 3 \
    --no-wait
```

Varsayılan olarak, birden çok düğüm havuzu olan bir küme oluşturduğunuzda AKS kümenizde *Düzenli* *önceliği* olan bir düğüm havuzu oluşturursunuz. Yukarıdaki komut, *Spot* *önceliği* olan varolan bir AKS kümesine yardımcı düğüm havuzu ekler. *Spot'un* *önceliği* düğüm havuzunu nokta düğümü havuzu yapar. *Tahliye ilkesi* parametresi, varsayılan değer olan yukarıdaki örnekte *Silme* olarak ayarlanır. [Çıkarma ilkesini][eviction-policy] *Sil*olarak ayarladığınızda, düğüm havuzunun temel ölçek kümesindeki düğümler tahliye edildiklerinde silinir. Ayrıca *Deallocate*için tahliye ilkesi ayarlayabilirsiniz. Çıkarma ilkesini *Deallocate*olarak ayarladığınızda, temel ölçek kümesindeki düğümler, tahliye den sonra durdurulan duruma ayarlanır. İşlem kotanıza göre durdurulan durum sayısındaki düğümler ve küme ölçekleme veya yükseltme ile ilgili sorunlara neden olabilir. *Öncelik* ve *çıkarma ilkesi* değerleri yalnızca düğüm havuzu oluşturma sırasında ayarlanabilir. Bu değerler daha sonra güncelleştirilemez.

Komut ayrıca nokta düğümü havuzları ile kullanılması önerilen [küme otomatik ölçeklendirici][cluster-autoscaler]sağlar. Kümenizde çalışan iş yüklerini temel alan küme otomatik ölçeklendirici, düğüm havuzundaki düğüm sayısını ölçekler. Nokta düğümü havuzları için küme otomatik ölçeklendirici, ek düğümler hala gerekliyse, çıkarmadan sonra düğüm sayısını ölçeklendirecek. Bir düğüm havuzunun sahip olabileceği en büyük düğüm sayısını değiştirirseniz, `maxCount` küme otomatik ölçekleyicisiyle ilişkili değeri de ayarlamanız gerekir. Bir küme otomatik ölçekleyici kullanmazsanız, çıkarma dan sonra, nokta havuzu sonunda sıfıra düşer ve ek nokta düğümleri almak için el ile işlem gerektirir.

> [!Important]
> Yalnızca toplu işleme işleri ve test ortamları gibi kesintileri işleyebilen nokta düğümü havuzlarında iş yüklerini zamanlayın. Düğüm tahliyelerini yalnızca işleyebilir iş yüklerinin bir nokta düğümü havuzunda zamanlandığını sağlamak için nokta düğüm havuzunuzda [lekeler ve tolerasyonlar][taints-tolerations] ayarlamanız önerilir. Örneğin, yukarıdaki komut ny varsayılan *kubernetes.azure.com/scalesetpriority=spot:NoSchedule* bir leke ekler, bu nedenle yalnızca karşılık gelen bir tolerto ile bölmeleri bu düğüm üzerinde zamanlanır.

## <a name="verify-the-spot-node-pool"></a>Nokta düğümü havuzunu doğrulama

Düğüm havuzunuzu doğrulamak için nokta düğüm havuzu olarak eklendi:

```azurecli
az aks nodepool show --resource-group myResourceGroup --cluster-name myAKSCluster --name spotnodepool
```

Onay *scaleSetPriority* *Spot*olduğunu .

Nokta düğümünde çalışacak bir bölme zamanlamak için, nokta düğümünüze uygulanan lekeye karşılık gelen bir hoşgörü ekleyin. Aşağıdaki örnek, bir önceki adımda kullanılan *kubernetes.azure.com/scalesetpriority=spot:NoSchedule* bir lekeye karşılık gelen bir tolere ayırma yı tanımlayan bir yaml dosyasının bir bölümünü gösterir.

```yaml
spec:
  containers:
  - name: spot-example
  tolerations:
  - key: "kubernetes.azure.com/scalesetpriority"
    operator: "Equal"
    value: "spot"
    effect: "NoSchedule"
   ...
```

Bu tolere sahip bir bölme dağıtıldığında, Kubernetes uygulanan leke ile düğümler üzerinde pod başarıyla zamanlayabilirsiniz.

## <a name="max-price-for-a-spot-pool"></a>Spot havuz için maksimum fiyat
[Spot örnekler için fiyatlandırma,][pricing-spot]bölge ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux][pricing-linux] ve [Windows][pricing-windows]için fiyatlandırmaya bakın.

Değişken fiyatlandırma ile, en fazla 5 ondalık basamak kullanarak, ABD doları (USD) cinsinden maksimum fiyat belirleme seçeneğiniz bulunmaktadır. Örneğin, değer *0,98765* saat başına 0,98765 USD maksimum fiyat olacaktır. Maksimum fiyatı *-1*olarak ayarlarsanız, örnek fiyata göre tahliye olmaz. Örneğin fiyatı, kapasite ve kota mevcut olduğu sürece Spot'un geçerli fiyatı veya standart bir örneğin fiyatı (hangisi daha azsa) olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir AKS kümesine nokta düğümü havuzu eklemeyi öğrendiniz. Düğüm havuzları arasında bölmeleri nasıl denetleyiş hakkında daha fazla bilgi için [AKS'deki gelişmiş zamanlayıcı özellikleri için en iyi uygulamalara][operator-best-practices-advanced-scheduler]bakın.

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deploy-create]: /cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-template-deploy]: ../azure-resource-manager/templates/deploy-cli.md#deployment-scope
[cluster-autoscaler]: cluster-autoscaler.md
[eviction-policy]: ../virtual-machine-scale-sets/use-spot.md#eviction-policy
[kubernetes-concepts]: concepts-clusters-workloads.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[pricing-linux]: https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/
[pricing-spot]: ../virtual-machine-scale-sets/use-spot.md#pricing
[pricing-windows]: https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/
[spot-toleration]: #verify-the-spot-node-pool
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[use-multiple-node-pools]: use-multiple-node-pools.md
[vmss-spot]: ../virtual-machine-scale-sets/use-spot.md