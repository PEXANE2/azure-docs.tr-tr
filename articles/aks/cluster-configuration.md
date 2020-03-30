---
title: Azure Kubernetes Hizmetlerinde (AKS) küme yapılandırması
description: Azure Kubernetes Hizmeti'nde (AKS) kümeyi nasıl yapılandırılatırın öğrenin
services: container-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 94f84beee2d7a76e48ac1470a0ce0b387929cc08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479169"
---
# <a name="configure-an-aks-cluster"></a>AKS kümesini yapılandırma

AKS kümesi oluşturmanın bir parçası olarak, küme yapılandırmanızı gereksinimlerinize göre özelleştirmeniz gerekebilir. Bu makalede, AKS kümenizi özelleştirmek için birkaç seçenek tanıyın.

## <a name="os-configuration-preview"></a>İşletim sistemi yapılandırması (Önizleme)

AKS şimdi önizleme düğüm işletim sistemi (OS) olarak Ubuntu 18.04 destekler. Önizleme döneminde hem Ubuntu 16.04 hem de Ubuntu 18.04 mevcuttur.

Aşağıdaki kaynakların yüklü olması gerekir:

- Azure CLI, sürüm 2.2.0 veya sonrası
- Aks-önizleme 0.4.35 uzantısı

aks-preview 0.4.35 uzantısını veya daha sonra yüklemek için aşağıdaki Azure CLI komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

Özelliği `UseCustomizedUbuntuPreview` kaydedin:

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Durumun **Kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az özellik listesi](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak gösterildiğinde, az `Microsoft.ContainerService` sağlayıcı kayıt komutunu kullanarak kaynak [sağlayıcısının](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) kaydını yenileyin:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Küme oluşturulduğunda Ubuntu 18.04 kullanacak şekilde kümeyi yapılandırın. Varsayılan `--aks-custom-headers` işletim sistemi olarak Ubuntu 18.04 ayarlamak için bayrağı kullanın.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Normal bir Ubuntu 16.04 kümesi oluşturmak istiyorsanız, bunu özel `--aks-custom-headers` etiketi atlayarak yapabilirsiniz.

## <a name="custom-resource-group-name"></a>Özel kaynak grubu adı

Azure'da bir Azure Kubernetes Hizmet kümesi dağıttığınızda, alt düğümler için ikinci bir kaynak grubu oluşturulur. Varsayılan olarak, AKS düğüm kaynak `MC_resourcegroupname_clustername_location`grubunu adlandırmaz, ancak kendi adınızı da sağlayabilirsiniz.

Kendi kaynak grup adınızı belirtmek için aks önizleme Azure CLI uzantısı sürümünü 0.3.2 veya sonraki sürüm yükleyin. Azure CLI'yi kullanarak, kaynak `az aks create` grubu için özel bir ad belirtmek için komutun `--node-resource-group` parametresini kullanın. Aks kümesini dağıtmak için bir Azure Kaynak Yöneticisi şablonu kullanıyorsanız, `nodeResourceGroup` özelliği kullanarak kaynak grubu adını tanımlayabilirsiniz.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

İkincil kaynak grubu, kendi aboneliğinizde Azure kaynak sağlayıcısı tarafından otomatik olarak oluşturulur. Yalnızca küme oluşturulduğunda özel kaynak grubu adını belirtebileceğinizi unutmayın. 

Düğüm kaynak grubuyla çalışırken şunları yapamayacağınızı unutmayın:

- Düğüm kaynak grubu için varolan bir kaynak grubu belirtin.
- Düğüm kaynak grubu için farklı bir abonelik belirtin.
- Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
- Düğüm kaynak grubundaki yönetilen kaynakların adlarını belirtin.
- Düğüm kaynak grubundaki yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin.

## <a name="next-steps"></a>Sonraki adımlar

- Kümenizdeki Linux `Kured` [düğümlerine güvenlik ve çekirdek güncelleştirmelerini uygulamak için](node-updates-kured.md) nasıl kullanacağınızı öğrenin.
- Kümenizi Kubernetes'in en son sürümüne nasıl yükselteceklerini öğrenmek için [Azure Kubernetes Hizmetini (AKS) yükseltme](upgrade-cluster.md) kümesine bakın.
- Bazı yaygın [AKS sorularının](faq.md) yanıtlarını bulmak için AKS hakkında sık sorulan soruların listesine bakın.