---
title: Azure Kubernetes Service (AKS) içindeki küme otomatik Scaler 'ı kullanma
description: Bir Azure Kubernetes Service (AKS) kümesindeki uygulama taleplerini karşılamak üzere kümenizi otomatik olarak ölçeklendirmek için küme otomatik olarak nasıl kullanacağınızı öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/18/2019
ms.author: mlearned
ms.openlocfilehash: dc5e862109a766f708338ebddb91a75ffc550306
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69031917"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Önizleme-Azure Kubernetes Service (AKS) üzerinde uygulama taleplerini karşılamak üzere bir kümeyi otomatik olarak ölçeklendirme

Azure Kubernetes Service 'te (AKS) uygulama taleplerine devam etmek için, iş yüklerinizi çalıştıran düğümlerin sayısını ayarlamanız gerekebilir. Küme otomatik Scaler bileşeni, kümenizde kaynak kısıtlamaları nedeniyle zamanlanabilecek Pod 'leri izleyebilir. Sorunlar algılandığında, bir düğüm havuzundaki düğümlerin sayısı uygulama talebini karşılayacak şekilde artmıştır. Düğümler, düğüm sayısıyla daha sonra gerektiği şekilde azaldıkça, bir yük eksikliği olmaması için düzenli olarak kontrol edilir. AKS kümenizdeki düğüm sayısını otomatik olarak ölçeklendirme veya azaltma yeteneği, verimli ve ekonomik bir küme çalıştırmanızı sağlar.

Bu makalede, bir AKS kümesinde Küme otomatik olarak nasıl etkinleştirileceği ve yönetileceği gösterilmektedir. Küme otomatik olarak yalnızca AKS kümelerinde önizlemede test edilmelidir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Küme otomatik olarak kullanılması için, *aks-Preview* CLI uzantısının sürüm 0.4.4 veya üzeri olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-scale-set-feature-provider"></a>Ölçek kümesi Özellik sağlayıcısını Kaydet

Ölçek kümeleri kullanan bir AKS oluşturmak için, aboneliğinizde bir özellik bayrağını da etkinleştirmeniz gerekir. *VMSSPreview* Özellik bayrağını kaydetmek için, aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanın:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

Küme otomatik Scaler 'ı kullanan AKS kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* HTTP uygulama yönlendirme eklentisi kullanılamıyor.

## <a name="about-the-cluster-autoscaler"></a>Küme otomatik yüklemesi hakkında

Workday ve akşam ya da bir hafta sonu arasındaki uygulama taleplerini değiştirmek üzere ayarlamak için kümeler genellikle otomatik olarak ölçeklendirilmesi için bir yönteme ihtiyaç duyar. AKS kümeleri, iki şekilde ölçeklendirebilir:

* **Küme otomatik yüklemesi** , kaynak kısıtlamaları nedeniyle düğümlerde zamanlanabilecek düğüm sayısını izler. Küme otomatik olarak düğüm sayısını artırır.
* **Yatay Pod otomatik Scaler** , Kubernetes kümesinde ölçüm sunucusunu kullanarak pods 'nin kaynak talebini izler. Bir hizmette daha fazla kaynak gerekiyorsa, bu sayı, talebi karşılamak için otomatik olarak artar.

![Küme otomatik olarak ve yatay Pod otomatik Scaler, gerekli uygulama taleplerini desteklemek için genellikle birlikte çalışır](media/autoscaler/cluster-autoscaler.png)

Hem yatay Pod otomatik Scaler hem de Cluster otomatik Scaler, Ayrıca, gereken sayıda düğüm ve düğüm sayısını azaltabilir. Küme otomatik yüklemesi, bir süre için kullanılmayan kapasiteden fazla düğüm sayısını düşürür. Küme otomatik olarak kaldırılacak bir düğümdeki düğüm, kümede başka bir yerde güvenle zamanlanır. Küme otomatik olarak, aşağıdaki durumlarda olduğu gibi, Pod taşınmadığı takdirde ölçeği ölçeklendiremeyebilir:

* Doğrudan oluşturulup oluşturulan bir pod, bir dağıtım veya çoğaltma kümesi gibi bir denetleyici nesnesi tarafından yedeklenmez.
* Pod kesinti bütçesi (pdb) çok kısıtlayıcıdır ve pod sayısının belirli bir eşiğin altına düşmeye izin vermez.
* Pod, farklı bir düğümde zamanlanırsa, düğüm seçicileri veya benzeşim önleme kullanır.

Küme otomatik olarak ölçeklendirilmesine nasıl ölçeklenebileceğinize ilişkin daha fazla bilgi için bkz [. küme otomatik Scaler 'ın bir düğümü kaldırmasını engelleyebilir][autoscaler-scaledown]

Küme otomatik yüklemesi, ölçek olayları ve kaynak eşikleri arasındaki zaman aralıkları gibi şeyler için başlangıç parametrelerini kullanır. Bu parametreler Azure platformu tarafından tanımlanır ve şu anda ayarlamanız için sunulmamaktadır. Kümenin otomatik olarak kullandığı parametreler hakkında daha fazla bilgi için bkz. [küme otomatik Scaler parametreleri nedir?][autoscaler-parameters].

İki otomatik scalers birlikte çalışabilir ve genellikle bir kümede dağıtılır. Birleştirildiğinde, yatay Pod otomatik Scaler, uygulama talebini karşılamak için gereken sayıda Pod çalıştırmaya odaklanılmıştır. Küme otomatik yüklemesi, zamanlanmış pods 'yi desteklemek için gereken düğüm sayısını çalıştırmaya odaklanır.

> [!NOTE]
> Küme otomatik ölçeklendirme kullandığınızda el ile ölçekleme devre dışıdır. Küme otomatik olarak gerekli düğüm sayısını belirlemesine izin verin. Kümenizi el ile ölçeklendirmek isterseniz, [küme otomatik Scaler ' ı devre dışı bırakın](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>AKS kümesi oluşturma ve küme otomatik Scaler 'ı etkinleştirme

AKS kümesi oluşturmanız gerekiyorsa, [az aks Create][az-aks-create] komutunu kullanın. Küme için düğüm havuzunda küme otomatik listesini etkinleştirmek ve yapılandırmak için *--Enable-Cluster-otomatik Scaler* parametresini kullanın ve bir düğüm belirtin *--Min-Count* ve *--Max-Count*.

> [!IMPORTANT]
> Küme otomatik yüklemesi, bir Kubernetes bileşenidir. AKS kümesi düğümler için bir sanal makine ölçek kümesi kullansa da, Azure portal ölçek kümesi otomatik ölçek ayarlarını el ile etkinleştirmeyin veya Azure CLı kullanarak ayarları düzenleyin. Kubernetes kümesi otomatik olarak gerekli ölçek ayarlarını yönetmesine izin verin. Daha fazla bilgi için bkz. [düğüm kaynak grubundaki AKS kaynaklarını değiştirebilir miyim?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

Aşağıdaki örnek, bir sanal makine ölçek kümesi tarafından desteklenen tek düğümlü havuz içeren bir AKS kümesi oluşturur. Ayrıca küme için düğüm havuzunda küme otomatik Scaler 'ı ve en az *1* ve en fazla *3* düğüm ayarlar:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

> [!NOTE]
> Çalıştırılırken`az aks create`bir *--Kubernetes-Version* belirtirseniz, bu sürüm [başlamadan önce, başlamadan önce](#before-you-begin) belirtilen en düşük sürüm numarasını karşılamalıdır veya aşmalıdır.

Kümeyi oluşturmak ve küme otomatik Scaler ayarlarını yapılandırmak birkaç dakika sürer.

### <a name="update-the-cluster-autoscaler-on-an-existing-node-pool-in-a-cluster-with-a-single-node-pool"></a>Tek düğümlü havuz içeren bir kümede bulunan mevcut düğüm havuzunda küme otomatik Scaler 'ı güncelleştirme

Önceki küme otomatik Scaler ayarlarını, [başlamadan önce](#before-you-begin) önceki bölümünde özetlenen gereksinimleri karşılayan bir kümede güncelleştirebilirsiniz. Kümenizdeki küme otomatik listesini *tek* bir düğüm havuzuyla etkinleştirmek için [az aks Update][az-aks-update] komutunu kullanın.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Daha sonra küme otomatik, `az aks update --enable-cluster-autoscaler` veya `az aks update --disable-cluster-autoscaler` komutlarıyla etkinleştirilebilir veya devre dışı bırakılabilir.

### <a name="enable-the-cluster-autoscaler-on-an-existing-node-pool-in-a-cluster-with-multiple-node-pools"></a>Birden çok düğüm havuzu içeren bir kümede var olan düğüm havuzunda küme otomatik Scaler özelliğini etkinleştirme

Küme otomatik [izleme özelliği, birden çok düğüm havuzları önizleme özelliği](use-multiple-node-pools.md) etkinleştirilmiş olarak da kullanılabilir. Birden çok düğüm havuzu tutan bir AKS kümesi içindeki tek düğüm havuzlarında küme otomatik Scaler 'ı etkinleştirebilir ve [başlamadan önce, başlamadan önce](#before-you-begin) bölümünde açıklandığı gibi gereksinimleri karşılarlar. Tek bir düğüm havuzunda küme otomatik Scaler özelliğini etkinleştirmek için [az aks nodepool Update][az-aks-nodepool-update] komutunu kullanın.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Daha sonra küme otomatik, `az aks nodepool update --enable-cluster-autoscaler` veya `az aks nodepool update --disable-cluster-autoscaler` komutlarıyla etkinleştirilebilir veya devre dışı bırakılabilir.

## <a name="change-the-cluster-autoscaler-settings"></a>Küme otomatik Scaler ayarlarını değiştirme

Önceki adımda, bir AKS kümesi oluşturmak veya var olan bir düğüm havuzunu güncelleştirmek için, küme otomatik algılama en düşük düğüm sayısı *1*olarak ayarlanmıştır ve en fazla düğüm sayısı *3*olarak ayarlanmıştır. Uygulamanız değiştikçe değişiklik yaparken, küme otomatik Scaler düğüm sayısını ayarlamanız gerekebilir.

Düğüm sayısını değiştirmek için [az aks nodepool Update][az-aks-nodepool-update] komutunu kullanın.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Yukarıdaki örnek, *Myakscluster* içindeki *mynodepool* düğüm havuzunda küme otomatik Scaler 'sini en az *1* ve en fazla *5* düğüm olarak güncelleştirir.

> [!NOTE]
> Önizleme süresince, düğüm havuzu için şu anda ayarlanmış olandan daha yüksek bir düğüm sayısı ayarlayamazsınız. Örneğin, şu anda en az *1*olarak ayarlandıysa, en az sayıyı *3*olarak güncelleştiremezsiniz.

Uygulamalarınızın ve hizmetlerinizin performansını izleyin ve küme otomatik Scaler düğüm sayılarını gerekli performansla eşleşecek şekilde ayarlayın.

## <a name="disable-the-cluster-autoscaler"></a>Küme otomatik Scaler 'ı devre dışı bırakma

Artık küme otomatik özelliğini kullanmak istemiyorsanız, *--Disable-Cluster-otomatik Scaler* parametresini belirterek [az aks nodepool Update][az-aks-nodepool-update] komutunu kullanarak devre dışı bırakabilirsiniz. Küme otomatik yüklemesi devre dışı bırakıldığında düğümler kaldırılmaz.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name mynodepool \
  --disable-cluster-autoscaler
```

[Az aks Scale][az-aks-scale] komutunu kullanarak kümenizi el ile ölçekleyebilirsiniz. Yatay Pod otomatik Scaler ' ı kullanırsanız, bu özellik küme otomatik olarak devre dışı bırakılmış olarak çalışmaya devam eder, ancak düğüm kaynakları kullanımda ise, Pod çalışmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS düğümlerinin sayısını otomatik olarak ölçeklendirirsiniz. Ayrıca, uygulamanızı çalıştıran Pod sayısını otomatik olarak ayarlamak için yatay Pod otomatik Scaler ' yı da kullanabilirsiniz. Yatay Pod otomatik Scaler 'ı kullanma adımları için bkz. [aks 'de Uygulamaları ölçeklendirme][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
