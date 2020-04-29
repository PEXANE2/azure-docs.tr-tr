---
title: Azure Kubernetes hizmetlerindeki küme yapılandırması (AKS)
description: Azure Kubernetes hizmeti 'nde (AKS) bir kümeyi yapılandırmayı öğrenin
services: container-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 94f84beee2d7a76e48ac1470a0ce0b387929cc08
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79479169"
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

`UseCustomizedUbuntuPreview` Özelliği kaydedin:

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

Kümeyi, küme oluşturulduğunda Ubuntu 18,04 kullanacak şekilde yapılandırın. Ubuntu 18,04 ' i varsayılan işletim sistemi olarak ayarlamak için `--aks-custom-headers` bayrağını kullanın.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Normal bir Ubuntu 16,04 kümesi oluşturmak istiyorsanız, özel `--aks-custom-headers` etiketi atlayarak bunu yapabilirsiniz.

## <a name="custom-resource-group-name"></a>Özel kaynak grubu adı

Azure 'da bir Azure Kubernetes hizmet kümesi dağıttığınızda, çalışan düğümleri için ikinci bir kaynak grubu oluşturulur. Varsayılan olarak AKS, düğüm kaynak grubunu `MC_resourcegroupname_clustername_location`adı verir, ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, aks-Preview Azure CLı uzantısı sürüm 0.3.2 veya üstünü yüklemelisiniz. Azure CLı 'yı kullanarak kaynak grubu için `--node-resource-group` özel bir ad `az aks create` belirtmek üzere komutun parametresini kullanın. AKS kümesi dağıtmak için bir Azure Resource Manager şablonu kullanıyorsanız, `nodeResourceGroup` özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur. Küme oluşturulduğunda yalnızca özel kaynak grubu adını belirleyebilirsiniz. 

Düğüm kaynak grubuyla çalışırken şunları yapmanız gerektiğini aklınızda bulundurun:

- Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
- Düğüm kaynak grubu için farklı bir abonelik belirtin.
- Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
- Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
- Düğüm kaynak grubu içinde yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin.

## <a name="next-steps"></a>Sonraki adımlar

- Kümenizdeki `Kured` [Linux düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygulamak](node-updates-kured.md) için kullanmayı öğrenin.
- Kümenizi, Kubernetes 'in en son sürümüne nasıl yükselteceğinizi öğrenmek için bkz. [Azure Kubernetes hizmeti (AKS) kümesini yükseltme](upgrade-cluster.md) .
- Bazı yaygın AKS sorularına cevap bulmak için [aks hakkında sık sorulan soruların](faq.md) listesine bakın.