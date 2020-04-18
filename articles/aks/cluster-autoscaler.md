---
title: Azure Kubernetes Hizmetinde (AKS) küme otomatik ölçekleyicisini kullanma
description: Bir Azure Kubernetes Hizmeti (AKS) kümesindeki uygulama taleplerini karşılamak için kümenizi otomatik olarak ölçeklendirmek için küme otomatik ölçeklendiricisini nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: article
ms.date: 07/18/2019
ms.openlocfilehash: 2baa64779713d0bac063e1d2c06107ba2ab291fb
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617536"
---
# <a name="automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) üzerindeki uygulama taleplerini karşılamak için kümeyi otomatik olarak ölçeklendirin

Azure Kubernetes Hizmeti'ndeki (AKS) uygulama taleplerini karşılamak için iş yüklerinizi çalıştıran düğüm sayısını ayarlamanız gerekebilir. Küme otomatik ölçeklendirici bileşeni, kaynak kısıtlamaları nedeniyle zamanlanamayan kümenizdeki bölmeleri izleyebilir. Sorunlar algılandığında, uygulama talebini karşılamak için düğüm havuzundaki düğüm sayısı artırılır. Düğümler de düzenli olarak çalışan bölmelerin eksikliği için kontrol edilir, düğüm lerin sayısı daha sonra gerektiği gibi azalmıştır. AKS kümenizdeki düğüm sayısını otomatik olarak ölçeklendirme veya küçültme yeteneği, verimli ve uygun maliyetli bir küme çalıştırmanıza olanak tanır.

Bu makalede, bir AKS kümesinde küme otomatik ölçeklendirici etkinleştirmek ve yönetmek nasıl gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLI sürümünü 2.0.76 veya sonraki sürümde çalıştırmanız gerekmektedir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="limitations"></a>Sınırlamalar

Küme otomatik ölçekleyicisini kullanan AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir:

* HTTP uygulama yönlendirme eklentisi kullanılamaz.

## <a name="about-the-cluster-autoscaler"></a>Küme otomatik ölçeklendirici hakkında

İş günü ile akşam veya hafta sonu gibi değişen uygulama taleplerine uyum sağlamak için kümelerin genellikle otomatik olarak ölçeklendirmenin bir yolunu niçin gerekir? AKS kümeleri iki şekilde ölçeklendirilebilir:

* **Küme otomatik ölçeklendirici,** kaynak kısıtlamaları nedeniyle düğümlerde zamanlanamayan bölmeleri izler. Küme daha sonra otomatik olarak düğüm sayısını artırır.
* **Yatay pod otomatik ölçeklendirici,** bölmelerin kaynak talebini izlemek için Kubernetes kümesindeki Metrics Server'ı kullanır. Bir uygulama daha fazla kaynağa ihtiyaç duyarsa, talebi karşılamak için bölme sayısı otomatik olarak artırılır.

![Küme oto ölçekleyici ve yatay pod otoölçekleyici genellikle gerekli uygulama taleplerini desteklemek için birlikte çalışır](media/autoscaler/cluster-autoscaler.png)

Hem yatay pod otomatik ölçekleyici si hem de küme otoölçekleyici gerektiğinde bölme ve düğüm sayısını azaltabilir. Küme otomatik ölçeklendirici, belirli bir süre için kullanılmayan kapasite olduğunda düğüm sayısını azaltır. Küme otomatik ölçekleyicisi tarafından kaldırılacak bir düğüm üzerindeki bölmeler, kümenin başka bir yerinde güvenle zamanlanır. Küme otomatik ölçeklendirici, aşağıdaki gibi bölmeler hareket edemiyorsa küçültülemez:

* Bir bölme doğrudan oluşturulur ve dağıtım veya çoğaltma kümesi gibi bir denetleyici nesnesi tarafından geri değildir.
* Bir bölme kesintisi bütçesi (PDB) çok kısıtlayıcıdır ve bölme sayısının belirli bir eşiğin altına düşmesine izin vermez.
* Bir bölme, farklı bir düğüm üzerinde zamanlanırsa onurlandırılamayacağı düğüm seçicileri veya karşı-yakınlık kullanır.

Küme otomatik ölçeklendiricinin nasıl küçültülemeyeceği hakkında daha fazla bilgi için, küme [otomatik ölçeklendiricinin düğüm kaldırmasını hangi bölme türlerinin engelleyebilir?][autoscaler-scaledown]

Küme otomatik ölçeklendirici, ölçek olayları ve kaynak eşikleri arasındaki zaman aralıkları gibi şeyler için başlangıç parametrelerini kullanır. Bu parametreler Azure platformu tarafından tanımlanır ve şu anda ayarlamanız için açıkta değildir. Küme otomatik ölçekleyicinin hangi parametreleri kullandığı hakkında daha fazla bilgi [için][autoscaler-parameters]bkz.

Küme ve yatay bölme otomatik ölçeklendirciler birlikte çalışabilir ve genellikle her ikisi de bir kümede dağıtılır. Birleştirildiğinde, yatay pod otomatik ölçeklendirici, uygulama talebini karşılamak için gereken bölme sayısını çalıştırmaya odaklanır. Küme otomatik ölçeklendirici, zamanlanan bölmeleri desteklemek için gereken düğüm sayısını çalıştırmaya odaklanmıştır.

> [!NOTE]
> Küme otomatik ölçeklendirici kullandığınızda manuel ölçeklendirme devre dışı bırakılır. Küme otomatik ölçekleyicinin gerekli düğüm sayısını belirlemesine izin verin. Kümenizi el ile ölçeklendirmek istiyorsanız, [küme otomatik ölçeklendiricisini devre dışı kındırın.](#disable-the-cluster-autoscaler)

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Bir AKS kümesi oluşturun ve küme otomatik ölçeklendiricietkinleştirin

Bir AKS kümesi oluşturmanız gerekiyorsa, [az aks oluşturma][az-aks-create] komutunu kullanın. Küme nin düğüm havuzundaki küme otomatik ölçekleyicisini etkinleştirmek ve yapılandırmak için *,--enable-cluster-autoscaler* parametresini kullanın ve bir düğüm *belirtin -min sayısı* ve *--max-count.*

> [!IMPORTANT]
> Küme otomatik ölçeklendirici bir Kubernetes bileşenidir. AKS kümesi düğümler için sanal bir makine ölçeği kümesi kullansa da, Azure portalında veya Azure CLI'yi kullanarak ölçek kümesi otomatik ölçeklendirmesi için ayarları el ile etkinleştirmeyin veya düzenlemayın. Kubernetes küme otomatik ölçekleyici gerekli ölçek ayarlarını yönetmesine izin verin. Daha fazla bilgi için bkz. [Düğüm kaynak grubundaki AKS kaynaklarını değiştirebilir miyim?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

Aşağıdaki örnek, sanal makine ölçeği kümesi tarafından desteklenen tek bir düğüm havuzuna sahip bir AKS kümesi oluşturur. Ayrıca küme için düğüm havuzunda küme otomatik ölçeklendirici sağlar ve en az *1* ve en fazla *3* düğüm ayarlar:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Kümeoluşturmak ve küme otomatik ölçeklendirici ayarlarını yapılandırmak birkaç dakika sürer.

## <a name="change-the-cluster-autoscaler-settings"></a>Küme otomatik ölçeklendirme ayarlarını değiştirme

> [!IMPORTANT]
> AKS kümenizde birden çok düğüm havuzu varsa, [birden çok aracı havuzu bölümüyle otomatik ölçeklendirmeye](#use-the-cluster-autoscaler-with-multiple-node-pools-enabled)atlayın. Birden çok aracı havuzu olan kümeler, düğüm havuzu nun yerine belirli özelliklerini değiştirmek için komut kümesinin `az aks nodepool` kullanılmasını `az aks`gerektirir.

Aks kümesi oluşturmak veya varolan bir düğüm havuzunu güncelleştirmek için önceki adımda, küme otomatik ölçeklendirici minimum düğüm sayısı *1*olarak ayarlandı ve en yüksek düğüm sayısı *3*olarak ayarlandı. Uygulamataleplerini değiştirmek gibi, küme otomatik ölçeklendirici düğüm sayısını ayarlamanız gerekebilir.

Düğüm sayısını değiştirmek için [az aks güncelleştirme][az-aks-update] komutunu kullanın.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Yukarıdaki örnek, *myAKSCluster'daki* tek düğüm havuzundaki küme otomatik ölçekleyicisini en az *1* ve en fazla *5* düğüme güncelleştirir.

> [!NOTE]
> Şu anda düğüm havuzu için ayarlanandan daha yüksek bir minimum düğüm sayısı ayarlayamadığınız. Örneğin, şu anda min sayısı *1*olarak ayarlanmışsa, min sayısını *3'e*güncelleştiremezsiniz.

Uygulamalarınızın ve hizmetlerinizin performansını izleyin ve küme otomatik ölçeklendirici düğüm sayılarını gerekli performansla eşleşecek şekilde ayarlayın.

## <a name="using-the-autoscaler-profile"></a>Otomatik ölçeklendirme profilini kullanma

Ayrıca küme genelindeki otomatik ölçeklendirici profilindeki varsayılan değerleri değiştirerek küme otomatik ölçekleyicisinin daha ayrıntılı ayrıntılarını yapılandırabilirsiniz. Örneğin, düğümler 10 dakika sonra az kullanıldıktan sonra bir küçülme olayı gerçekleşir. Her 15 dakikada bir çalıştırılan iş yüklerin varsa, 15 veya 20 dakika sonra kullanılan düğümlerin altında küçültülecek otomatik ölçeklendirici profilini değiştirmek isteyebilirsiniz. Küme otomatik ölçeklendiricisini etkinleştirdiğinizde, farklı ayarlar belirtmediğiniz sürece varsayılan bir profil kullanılır. Küme otomatik ölçeklendirici profilinde güncelleştirebileceğiniz aşağıdaki ayarlar vardır:

| Ayar                          | Açıklama                                                                              | Varsayılan değer |
|----------------------------------|------------------------------------------------------------------------------------------|---------------|
| tama aralığı                    | Kümenin yukarı veya aşağı ölçeklendirme için ne sıklıkta yeniden değerlendirildiği                                    | 10 saniye    |
| ölçek-aşağı-gecikme-sonra-ekle       | Ne kadar sonra ölçeklendirme bu ölçek aşağı değerlendirme devam eder                               | 10 dakika    |
| ölçek-aşağı-gecikme-sonra-silme    | Değerlendirmeyi küçülten düğüm silme işleminden ne kadar sonra değerlendirme devam eder                          | tama aralığı |
| ölçek-aşağı-gecikme-sonra-başarısızlık   | Küçültme değerlendirmesini küçülten başarısızlığı küçültmeden ne kadar süre sonra yeniden değerlendirme devam eder                     | 3 dakika     |
| ölçek-down-gereksiz zaman         | Bir düğümün küçültünebilmesi için uygun hale gelmeden önce ne kadar süre gereksiz olması gerekir?                  | 10 dakika    |
| ölçek-down-hazır olmayan-zaman          | Küçültme için uygun hale gelmeden önce hazır olmayan bir düğümün ne kadar süre gereksiz olması gerekir?         | 20 dakika    |
| ölçek-aşağı kullanım-eşik | Kapasiteye bölünen istenen kaynakların toplamı olarak tanımlanan düğüm kullanım düzeyi, aşağıda bir düğümün küçültülme için düşünülebileceği | 0,5 |
| maksimum zarif-sonlandırma-sn     | Küme otomatik ölçeklendirici, düğüm küçültmeye çalışırken bölme sonlandırmasını bekler saniye sayısı. | 600 saniye   |
| denge-benzer düğüm grupları | Benzer düğüm havuzlarını algılayın ve aralarındaki düğüm sayısını dengeleyin | yanlış |

> [!IMPORTANT]
> Küme otomatik ölçeklendirici profili küme otomatik ölçekleyicikullanan tüm düğüm havuzlarını etkiler. Düğüm havuzu başına otomatik ölçeklendirici profili ayarlayamadığınızda.

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI uzantısını yükleyin

Küme otomatik ölçeklendirici ayarları profilini ayarlamak için *aks önizleme* CLI uzantısı sürümü 0.4.30 veya daha yüksek olması gerekir. [az uzantı ekle][az-extension-add] komutunu kullanarak *aks önizleme* Azure CLI uzantısını yükleyin ve az [uzantı güncelleştirme][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="set-the-cluster-autoscaler-profile-on-an-existing-aks-cluster"></a>Küme otomatik ölçeklendirici profilini varolan bir AKS kümesinde ayarlama

Kümenizdeki küme otomatik ölçeklendirici profilini ayarlamak için *küme-otomatik ölçeklendirici-profil* parametresi ile [az aks güncelleştirme][az-aks-update] komutunu kullanın. Aşağıdaki örnek, profildeki tazyik aralığı ayarını 30'lar olarak yapılandırır.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --cluster-autoscaler-profile scan-interval=30s
```

Kümedeki düğüm havuzlarında küme otomatik ölçeklendiricisini etkinleştirdiğinizde, bu kümeler küme otomatik ölçeklendirici profilini de kullanır. Örneğin:

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

> [!IMPORTANT]
> Küme otomatik ölçeklendirici profilini ayarladığınızda, küme otomatik ölçekleyici etkin olan varolan tüm düğüm havuzları profili hemen kullanmaya başlar.

### <a name="set-the-cluster-autoscaler-profile-when-creating-an-aks-cluster"></a>AKS kümesi oluştururken küme otomatik ölçeklendirici profilini ayarlama

Kümenizi oluştururken *küme-otomatik ölçeklendirici-profil* parametresini de kullanabilirsiniz. Örneğin:

```azurecli-interactive
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3 \
  --cluster-autoscaler-profile scan-interval=30s
```

Yukarıdaki komut bir AKS kümesi oluşturur ve küme çapında otomatik ölçeklendirici profili için 30 saniye olarak tarar aralığını tanımlar. Komut ayrıca küme otomatik ölçeklendirici ilk düğüm havuzunda sağlar, 1 ve en büyük düğüm sayısı 3 olarak minimum düğüm sayısını ayarlar.

### <a name="reset-cluster-autoscaler-profile-to-default-values"></a>Küme otomatik ölçeklendirici profilini varsayılan değerlere sıfırlama

Kümenizdeki küme otomatik ölçeklendirici profilini sıfırlamak için [az aks güncelleştirme][az-aks-update] komutunu kullanın.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --cluster-autoscaler-profile ""
```

## <a name="disable-the-cluster-autoscaler"></a>Küme otomatik ölçeklendiricisini devre dışı

Küme otomatik ölçekleyicisini artık kullanmak istemiyorsanız, [az aks güncelleştirme][az-aks-update] komutunu kullanarak *-devre dışı bırak-küme-otomatik ölçeklendirme* parametresini belirterek devre dışı bırakabilirsiniz. Küme otomatik ölçeklendirici devre dışı bırakıldığında düğümler kaldırılmaz.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

[Az aks ölçeği][az-aks-scale] komutunu kullanarak küme otomatik ölçeklendirmesini devre dışı bırakarak kümenizi el ile ölçeklendirebilirsiniz. Yatay bölme otomatik ölçekleyicikullanıyorsanız, bu özellik küme otomatik ölçeklendirici devre dışı bırakılmış olarak çalışır, ancak tüm düğüm kaynakları kullanılıyorsa bölmeler zamanlanamamaktadır.

## <a name="re-enable-a-disabled-cluster-autoscaler"></a>Devre dışı bırakılmış küme otomatik ölçekleyicisini yeniden etkinleştirme

Varolan bir kümedeki küme otomatik ölçekleyicisini yeniden etkinleştirmek istiyorsanız, [az aks güncelleştirme][az-aks-update] komutunu kullanarak *,--enable-cluster-autoscaler*, *--min-count*, ve *-max-count* parametrelerini belirterek yeniden etkinleştirebilirsiniz.

## <a name="retrieve-cluster-autoscaler-logs-and-status"></a>Küme otomatik ölçeklendirici günlüklerini ve durumunu alma

Otomatik ölçeklendirici olaylarını tanılamak ve hata ayıklamak için, günlükler ve durum otomatik ölçeklendirici eklentisinden alınabilir.

AKS küme otomatik ölçeklendiriyi sizin adınıza yönetir ve yönetilen kontrol düzleminde çalıştırın. Ana düğüm günlükleri bir sonucu olarak görüntülenecek şekilde yapılandırılmalıdır.

Küme otomatik ölçekleyicisinden Log Analytics'e itilecek günlükleri yapılandırmak için aşağıdaki adımları izleyin.

1. Küme-otomatik ölçeklendirici günlüklerini Log Analytics'e itmek için tanılama günlükleri için bir kural ayarlayın. [Talimatlar burada ayrıntılı olarak açıklanır,](https://docs.microsoft.com/azure/aks/view-master-logs#enable-diagnostics-logs)"Günlükler" için `cluster-autoscaler` seçenekler seçerken kutuyu işaret ettiğinizden emin olun.
1. Azure portalı üzerinden kümenizdeki "Günlükler" bölümüne tıklayın.
1. Aşağıdaki örnek sorguyu Log Analytics'e gir:

```
AzureDiagnostics
| where Category == "cluster-autoscaler"
```

Alınacak günlükler olduğu sürece aşağıdaki örneğe benzer günlükleri görmeniz gerekir.

![Günlük Analytics günlükleri](media/autoscaler/autoscaler-logs.png)

Küme otomatik ölçeklendirici de adlı `cluster-autoscaler-status`bir configmap sağlık durumu yazacaktır. Bu günlükleri almak için `kubectl` aşağıdaki komutu çalıştırın. Küme otomatik ölçeklendirici ile yapılandırılan her düğüm havuzu için bir sistem durumu bildirilir.

```
kubectl get configmap -n kube-system cluster-autoscaler-status -o yaml
```

Otomatik ölçekleyiciden nelerin günlüğe kaydedildiği hakkında daha fazla bilgi edinmek için [Kubernetes/autoscaler GitHub projesinde](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#ca-doesnt-work-but-it-used-to-work-yesterday-why)SSS'yi okuyun.

## <a name="use-the-cluster-autoscaler-with-multiple-node-pools-enabled"></a>Birden çok düğüm havuzu etkin olan küme otomatik ölçekleyicisini kullanma

Küme otomatik ölçeklendirici, [birden çok düğüm havuzu](use-multiple-node-pools.md) etkinken birlikte kullanılabilir. Birden çok düğüm havuzuetkinleştirmeyi ve varolan bir kümeye ek düğüm havuzları eklemeyi öğrenmek için bu belgeyi izleyin. Her iki özelliği birlikte kullanırken, kümedeki her bir düğüm havuzunda küme otomatik ölçeklendiricisini etkinleştirin ve her birine benzersiz otomatik ölçeklendirme kuralları geçirebilirsiniz.

Aşağıdaki komut, bu belgede daha önce [ilk yönergeleri](#create-an-aks-cluster-and-enable-the-cluster-autoscaler) takip varsayar ve 3 ile *5* *3* varolan bir düğüm havuzunun maksimum sayısını güncelleştirmek istiyorum. Varolan bir düğüm havuzunun ayarlarını güncelleştirmek için [az aks nodepool güncelleştirme][az-aks-nodepool-update] komutunu kullanın.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Küme otoölçekleyici az [aks nodepool güncelleştirme][az-aks-nodepool-update] ve `--disable-cluster-autoscaler` parametre geçerek devre dışı bırakılabilir.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --disable-cluster-autoscaler
```

Varolan bir kümedeki küme otomatik ölçekleyicisini yeniden etkinleştirmek istiyorsanız, [az aks nodepool güncelleştirme][az-aks-nodepool-update] komutunu kullanarak *,--enable-cluster-autoscaler*, *--min-count*, ve *-max-count* parametrelerini belirterek yeniden etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS düğümlerinin sayısını otomatik olarak nasıl ölçeklendirdiğinizi gösterin. Uygulamanızı çalıştıran bölme sayısını otomatik olarak ayarlamak için yatay pod otomatik ölçekleyicisini de kullanabilirsiniz. Yatay pod otomatik ölçekleyicisini kullanma adımları için [AKS'deki Ölçek uygulamalarına][aks-scale-apps]bakın.

<!-- LINKS - internal -->
[aks-faq]: faq.md
[aks-scale-apps]: tutorial-kubernetes-scale.md
[aks-support-policies]: support-policies.md
[aks-upgrade]: upgrade-cluster.md
[autoscaler-profile-properties]: #using-the-autoscaler-profile
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
