---
title: Azure Kubernetes Service (AKS) üzerinde ana bilgisayar tabanlı şifrelemeyi etkinleştirme
description: Azure Kubernetes Service (AKS) kümesinde konak tabanlı şifrelemeyi yapılandırma hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 07/10/2020
ms.openlocfilehash: 6f145750ac9158c0184ac9d4d6c303828ee5bdf4
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225360"
---
# <a name="host-based-encryption-on-azure-kubernetes-service-aks-preview"></a>Azure Kubernetes hizmeti (AKS) üzerinde ana bilgisayar tabanlı şifreleme (Önizleme)

Ana bilgisayar tabanlı şifreleme ile, AKS aracı düğümlerinizin VM konağında depolanan veriler, REST ve depolama hizmetine şifrelenmiş akışlarda şifrelenir. Diğer bir deyişle, geçici diskler platform tarafından yönetilen anahtarlarla birlikte şifrelenir. İşletim sistemi ve veri disklerinin önbelleği, bu disklerde ayarlanan şifreleme türüne bağlı olarak, platform tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla birlikte Rest 'de şifrelenir. Varsayılan olarak, AKS kullanırken, işletim sistemi ve veri diskleri, platformda yönetilen anahtarlarla birlikte şifrelenir, yani bu disklere yönelik önbellekler, platform tarafından yönetilen anahtarlarla aynı zamanda varsayılan olarak şifrelenir.  [Azure Kubernetes hizmetindeki Azure diskleriyle kendi anahtarlarınızı (BYOK)](azure-disk-customer-managed-keys.md)izleyerek kendi yönetilen anahtarlarınızı belirtebilirsiniz. Bu disklerin önbelleği daha sonra da bu adımda belirttiğiniz anahtar kullanılarak şifrelenir.


## <a name="before-you-begin"></a>Başlamadan önce

Bu özellik yalnızca küme oluşturma veya düğüm havuzu oluşturma zamanında ayarlanabilir.

> [!NOTE]
> Ana bilgisayar tabanlı şifreleme, Azure yönetilen disklerin sunucu tarafı şifrelemesini ve yalnızca belirli [desteklenen VM boyutlarını][supported-sizes]destekleyen [Azure bölgelerinde][supported-regions] kullanılabilir.

### <a name="prerequisites"></a>Önkoşullar

- `aks-preview`CLI uzantısının v 0.4.55 veya üzeri yüklü olduğundan emin olun
- `EncryptionAtHost`Özellik bayrağının etkin ' in altında olduğundan emin olun `Microsoft.Compute` .
- `EncryptionAtHost`Özellik bayrağının etkin ' in altında olduğundan emin olun `Microsoft.ContainerService` .

### <a name="register-encryptionathost--preview-features"></a>`EncryptionAtHost`Önizleme özelliklerini Kaydet

Konak tabanlı şifreleme kullanan bir AKS kümesi oluşturmak için `EncryptionAtHost` aboneliğinizdeki Özellik bayrağını etkinleştirmeniz gerekir.

`EncryptionAtHost`Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --namespace "Microsoft.Compute" --name "EncryptionAtHost"

az feature register --namespace "Microsoft.ContainerService"  --name "EncryptionAtHost"
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.Compute/EncryptionAtHost')].{Name:name,State:properties.state}"

az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EncryptionAtHost')].{Name:name,State:properties.state}"
```

Hazırlanıyor, `Microsoft.ContainerService` `Microsoft.Compute` [az Provider Register][az-provider-register] komutunu kullanarak ve kaynak sağlayıcılarının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.Compute

az provider register --namespace Microsoft.ContainerService
```

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> - [AKS destek Ilkeleri](support-policies.md)
> - [Azure desteği SSS](faq.md)

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Ana bilgisayar tabanlı şifrelemeyi bir AKS kümesi oluşturmak için en son *aks-Preview* CLI uzantısına ihtiyacınız vardır. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükler veya [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="limitations"></a>Sınırlamalar

- Yalnızca yeni düğüm havuzlarında veya yeni kümelerde etkinleştirilebilir.
- Yalnızca Azure yönetilen disklerinin sunucu tarafı şifrelemesini ve yalnızca belirli [desteklenen VM boyutlarını][supported-sizes]destekleyen [Azure bölgelerinde][supported-regions] etkinleştirilebilir.
- *VM kümesi türü*olarak sanal makine ölçek kümeleri (VMSS) tabanlı bir aks kümesi ve düğüm havuzu gerektirir.

## <a name="use-host-based-encryption-on-new-clusters-preview"></a>Yeni kümelerde ana bilgisayar tabanlı şifrelemeyi kullan (Önizleme)

Küme oluşturulduğunda küme Aracısı düğümlerini konak tabanlı şifrelemeyi kullanacak şekilde yapılandırın. `--aks-custom-headers`Üstbilgiyi ayarlamak için bayrağını kullanın `EnableEncryptionAtHost` .

```azurecli-interactive
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --aks-custom-headers EnableEncryptionAtHost=true
```

Konak tabanlı şifreleme olmadan kümeler oluşturmak istiyorsanız, özel parametreyi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="use-host-based-encryption-on-existing-clusters-preview"></a>Mevcut kümeler üzerinde ana bilgisayar tabanlı şifrelemeyi kullanma (Önizleme)

Kümenize yeni bir düğüm havuzu ekleyerek, var olan kümelerde konak tabanlı şifrelemeyi etkinleştirebilirsiniz. Bayrağını kullanarak ana bilgisayar tabanlı şifrelemeyi kullanmak için yeni bir düğüm havuzu yapılandırın `--aks-custom-headers` .

```azurecli
az aks nodepool add --name hostencrypt --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --aks-custom-headers EnableEncryptionAtHost=true
```

Konak tabanlı şifreleme özelliği olmadan yeni düğüm havuzları oluşturmak istiyorsanız, özel parametreyi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .

## <a name="next-steps"></a>Sonraki adımlar

[AKS kümesi güvenliğine yönelik en iyi uygulamaları][best-practices-security] gözden geçirin [ana bilgisayar tabanlı şifreleme](/azure/virtual-machines/linux/disk-encryption#host-based-encryption)hakkında daha fazla bilgi edinin.


<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[supported-regions]: /azure/virtual-machines/linux/disk-encryption#supported-regions
[supported-sizes]: /azure/virtual-machines/linux/disk-encryption#supported-vm-sizes
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
