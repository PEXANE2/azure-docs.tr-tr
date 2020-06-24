---
title: Azure Kubernetes hizmetlerindeki küme yapılandırması (AKS)
description: Azure Kubernetes hizmeti 'nde (AKS) bir kümeyi yapılandırmayı öğrenin
services: container-service
ms.topic: conceptual
ms.date: 06/20/2020
ms.openlocfilehash: 43aadd52f17367b488fcec086404caaba9158f33
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85205784"
---
# <a name="configure-an-aks-cluster"></a>AKS kümesini yapılandırma

AKS kümesi oluşturmanın bir parçası olarak, Küme yapılandırmanızı gereksinimlerinize uyacak şekilde özelleştirmeniz gerekebilir. Bu makalede AKS kümenizi özelleştirmek için birkaç seçenek sunulmaktadır.

## <a name="os-configuration-preview"></a>İşletim sistemi yapılandırması (Önizleme)

AKS artık, önizleme aşamasında düğüm işletim sistemi (OS) olarak Ubuntu 18,04 ' i desteklemektedir. Önizleme dönemi boyunca hem Ubuntu 16,04 hem de Ubuntu 18,04 kullanılabilir.

Aşağıdaki kaynakların yüklü olması gerekir:

- Azure CLı, sürüm 2.2.0 veya üzeri
- Aks-Preview 0.4.35 uzantısı

Aks-Preview 0.4.35 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

Özelliği kaydedin `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="new-clusters"></a>Yeni kümeler

Kümeyi, küme oluşturulduğunda Ubuntu 18,04 kullanacak şekilde yapılandırın. `--aks-custom-headers`Ubuntu 18,04 ' i varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Normal bir Ubuntu 16,04 kümesi oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

### <a name="existing-clusters"></a>Mevcut kümeler

Ubuntu 18,04 kullanmak için yeni bir düğüm havuzu yapılandırın. `--aks-custom-headers`Bu düğüm havuzu Için Ubuntu 18,04 ' ı varsayılan işletim sistemi olarak ayarlamak için bayrağını kullanın.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Normal bir Ubuntu 16,04 düğüm havuzu oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="generation-2-virtual-machines-preview"></a>2. nesil sanal makineler (Önizleme)
Azure [2. nesil (Gen2) sanal makineleri (VM)](../virtual-machines/windows/generation-2.md)destekler. 2. nesil VM 'Ler, 1. nesil VM 'lerde desteklenmeyen önemli özellikleri destekler (Gen1). Bu özellikler, artan bellek, Intel Software Guard uzantıları (Intel SGX) ve sanallaştırılmış kalıcı bellek (vPMEM) içerir.

2. nesil sanal makineler, 1. nesil VM 'Ler tarafından kullanılan BIOS tabanlı mimaride değil, yeni UEFı tabanlı önyükleme mimarisini kullanır.
Yalnızca belirli SKU 'Lar ve boyutlar Gen2 VM 'Leri destekler. SKU 'nuzun Gen2 destekleyip desteklemediğini veya gerektirip gerektirmediğini görmek için [Desteklenen boyutlar listesini](../virtual-machines/windows/generation-2.md#generation-2-vm-sizes)kontrol edin.

Ayrıca, AKS Gen2 VM 'lerinde Gen2 desteği olan tüm VM görüntüleri, yeni [aks Ubuntu 18,04 görüntüsünü](#os-configuration-preview)kullanır. Bu görüntü tüm Gen2 SKU 'Larını ve boyutlarını destekler.

Önizleme sırasında Gen2 VM 'Leri kullanmak için şunları yapmanız gerekir:
- `aks-preview`CLI uzantısı yüklendi.
- `Gen2VMPreview`Özellik bayrağı kaydedildi.

Özelliği kaydedin `Gen2VMPreview` :

```azurecli
az feature register --name Gen2VMPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/Gen2VMPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Aks-Preview CLı uzantısını yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
```

Aks-Preview CLı uzantısını güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension update --name aks-preview
```

### <a name="new-clusters"></a>Yeni kümeler
Kümeyi, küme oluşturulduğunda seçili SKU için Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Yeni bir kümede VM oluşturma olarak Gen2 ayarlamak için bayrağını kullanın.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

1. nesil (Gen1) VM 'Leri kullanarak düzenli bir küme oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` . Ayrıca, aşağıdaki gibi daha fazla Gen1 veya Gen2 VM eklemeyi de seçebilirsiniz.

### <a name="existing-clusters"></a>Mevcut kümeler
Yeni bir düğüm havuzunu Gen2 VM 'Leri kullanacak şekilde yapılandırın. `--aks-custom-headers`Gen2 'i bu düğüm havuzu IÇIN VM oluşturma olarak ayarlamak için bayrağını kullanın.

```azure-cli
az aks nodepool add --name gen2 --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

Normal Gen1 düğüm havuzları oluşturmak istiyorsanız, özel etiketi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="custom-resource-group-name"></a>Özel kaynak grubu adı

Azure 'da bir Azure Kubernetes hizmet kümesi dağıttığınızda, çalışan düğümleri için ikinci bir kaynak grubu oluşturulur. Varsayılan olarak AKS, düğüm kaynak grubunu adı verir `MC_resourcegroupname_clustername_location` , ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, aks-Preview Azure CLı uzantısı sürüm 0.3.2 veya üstünü yüklemelisiniz. Azure CLı 'yı kullanarak `--node-resource-group` `az aks create` kaynak grubu için özel bir ad belirtmek üzere komutun parametresini kullanın. AKS kümesi dağıtmak için bir Azure Resource Manager şablonu kullanıyorsanız, özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz `nodeResourceGroup` .

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur. Yalnızca küme oluşturulduğunda özel kaynak grubu adı belirtebilirsiniz. 

Düğüm kaynak grubuyla çalışırken şunları yapamazsınız:

- Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
- Düğüm kaynak grubu için farklı bir abonelik belirtin.
- Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
- Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
- Düğüm kaynak grubu içinde yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin.

## <a name="next-steps"></a>Sonraki adımlar

- `Kured`Kümenizdeki [Linux düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygulamak](node-updates-kured.md) için kullanmayı öğrenin.
- Kümenizi, Kubernetes 'in en son sürümüne nasıl yükselteceğinizi öğrenmek için bkz. [Azure Kubernetes hizmeti (AKS) kümesini yükseltme](upgrade-cluster.md) .
- Bazı yaygın AKS sorularına cevap bulmak için [aks hakkında sık sorulan soruların](faq.md) listesine bakın.
