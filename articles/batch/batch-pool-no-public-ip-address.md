---
title: Genel IP adresleri olmayan Azure Batch havuzu oluşturma
description: Genel IP adresleri olmadan havuz oluşturmayı öğrenin
author: pkshultz
ms.topic: how-to
ms.date: 06/26/2020
ms.author: peshultz
ms.openlocfilehash: 30792314f5bffaf4d40fc4bf60a2706acdaad34b
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85962450"
---
# <a name="create-an-azure-batch-pool-without-public-ip-addresses"></a>Genel IP adresleri olmayan Azure Batch havuzu oluşturma

Bir Azure Batch havuzu oluşturduğunuzda, bir genel IP adresi olmadan sanal makine yapılandırma havuzunu sağlayabilirsiniz. Bu makalede, genel IP adresleri olmadan bir Batch havuzunun nasıl ayarlanacağı açıklanır.

## <a name="why-use-a-pool-without-public-ip-addresses"></a>Ortak IP adresleri olmadan neden havuz kullanılmalıdır?

Varsayılan olarak, bir Azure Batch sanal makine yapılandırma havuzundaki tüm işlem düğümlerine genel bir IP adresi atanır. Bu adres, Batch hizmeti tarafından, internet 'e giden erişim de dahil olmak üzere, görevleri zamanlamak ve işlem düğümleri ile iletişim kurmak için kullanılır. 

Bu düğümlere erişimi kısıtlamak ve bu düğümlerin Internet 'ten bulunabilirliğini azaltmak için, havuzu genel IP adresleri olmadan sağlayabilirsiniz.

> [!IMPORTANT]
> Azure Batch genel IP adresleri olmayan havuzlar için destek şu anda Orta Batı ABD, Doğu ABD, Orta Güney ABD, Batı ABD 2, US Gov Virginia ve US Gov Arizona bölgeleri için genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

- **Kimlik doğrulaması**. Bir [sanal ağ](./batch-virtual-network.md)IÇINDE genel IP adresleri olmayan bir havuz kullanmak için Batch istemci apı 'sinin Azure ACTIVE DIRECTORY (ad) kimlik doğrulamasını kullanması gerekir. Azure AD için Azure Batch desteği, [Batch hizmeti çözümlerinin kimliğini Active Directory ile doğrulama](batch-aad-auth.md) makalesinde belirtilmiştir. Havuzunuzu bir sanal ağ içinde oluşturmadıysanız, Azure AD kimlik doğrulaması veya anahtar tabanlı kimlik doğrulaması kullanılabilir.

- **Bir Azure sanal ağı**. Havuzunuzu bir [Sanal ağda](batch-virtual-network.md)oluşturuyorsanız, bu gereksinimleri ve konfigürasyonları izleyin. Bir sanal ağı bir veya daha fazla alt ağ ile önceden hazırlamak için Azure portal, Azure PowerShell, Azure komut satırı arabirimi (CLı) veya diğer yöntemleri kullanabilirsiniz.
  - Sanal ağın havuzunuzu oluşturmak için kullandığınız Batch hesabıyla aynı abonelikte ve bölgede olması gerekir.
  - Havuz için belirtilen alt ağda havuz için hedeflenen VM sayısına yetecek kadar atanmamış IP adresi bulunması gerekir. Başka bir deyişle bu değerin havuzun `targetDedicatedNodes` ve `targetLowPriorityNodes` özelliklerinin toplamı olması gerekir. Alt ağda yeterli sayıda atanmamış IP adresi yoksa havuz işlem düğümlerini kısmen ayırır ve bir yeniden boyutlandırma hatası oluşur.
  - Özel bağlantı hizmeti ve uç nokta ağ ilkelerini devre dışı bırakmanız gerekir. Bu işlem, Azure CLı kullanılarak yapılabilir:```az network vnet subnet update --vnet-name <vnetname> -n <subnetname> --disable-private-endpoint-network-policies --disable-private-link-service-network-policies```
  
> [!IMPORTANT]
> Toplu Işlem, her 100 adanmış veya düşük öncelikli düğüm için bir özel bağlantı hizmeti ve bir yük dengeleyici ayırır. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Büyük havuzlar için, bu kaynaklardan bir veya daha fazlası için [bir kota artışı istemeniz](batch-quota-limit.md#increase-a-quota) gerekebilir. Ayrıca, bir havuz silme veya sıfıra boyutlandırma gibi kullanıcı tarafından başlatılan eylemlerin bir sonucu olarak kaynakların temizlenmesini önlediği için toplu Işlem tarafından oluşturulan herhangi bir kaynağa kaynak kilidi uygulanmamalıdır.

## <a name="current-limitations"></a>Geçerli sınırlamalar

1. Genel IP adresleri olmayan havuzlar, Cloud Services yapılandırma değil, sanal makine yapılandırmasını kullanmalıdır.
1. Toplu işlem düğümlerine [Özel uç nokta yapılandırması](pool-endpoint-configuration.md) , genel IP adresleri olmayan havuzlarla birlikte çalışmaz.
1. Genel IP adresi olmadığından, [belirtilen genel IP adreslerinizi](create-pool-public-ip.md) bu havuz türüyle kullanamazsınız.

## <a name="create-a-pool-without-public-ip-addresses-in-the-azure-portal"></a>Azure portal genel IP adresleri olmadan havuz oluşturma

1. Azure portalında Batch hesabınıza gidin. 
1. Soldaki **Ayarlar** penceresinde **havuzlar**' ı seçin.
1. **Havuzlar** penceresinde **Ekle**' yi seçin.
1. **Havuz Ekle** penceresinde, **görüntü türü** açılan menüsünden kullanmayı düşündüğünüz seçeneği belirleyin.
1. Görüntünüzün doğru **yayımcısını/teklifini/SKU** 'sunu seçin.
1. **Düğüm boyutu**, **hedef adanmış düğümler**ve **düşük öncelikli düğümlerin**yanı sıra istediğiniz isteğe bağlı ayarları da kapsayan, kalan gerekli ayarları belirtin.
1. İsteğe bağlı olarak kullanmak istediğiniz bir sanal ağ ve alt ağ seçin. Bu sanal ağ, oluşturmakta olduğunuz havuz ile aynı kaynak grubunda olmalıdır.
1. **IP adresi sağlama türü**' nde **Nopublicıpaddresses**' ı seçin.

![Nopublicıpaddresses seçili havuz ekranı ekle](./media/batch-pool-no-public-ip-address/create-pool-without-public-ip-address.png)

## <a name="use-the-batch-rest-api-to-create-a-pool-without-public-ip-addresses"></a>Genel IP adresleri olmayan bir havuz oluşturmak için Batch REST API kullanma

Aşağıdaki örnekte, genel IP adresleri kullanan bir havuz oluşturmak için [Azure Batch REST API](/rest/api/batchservice/pool/add) nasıl kullanılacağı gösterilmektedir.

### <a name="rest-api-uri"></a>REST API URI'si

```http
POST {batchURL}/pools?api-version=2020-03-01.11.0
client-request-id: 00000000-0000-0000-0000-000000000000
```

### <a name="request-body"></a>İstek gövdesi

```json
"pool": {
     "id": "pool2",
     "vmSize": "standard_a1",
     "virtualMachineConfiguration": {
          "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.040-LTS"
          },
     "nodeAgentSKUId": "batch.node.ubuntu 16.04"
     }
     "networkConfiguration": {
          "subnetId": "/subscriptions/<your_subscription_id>/resourceGroups/<your_resource_group>/providers/Microsoft.Network/virtualNetworks/<your_vnet_name>/subnets/<your_subnet_name>",
          "publicIPAddressConfiguration": {
               "provision": "NoPublicIPAddresses"
          }
     },
     "resizeTimeout": "PT15M",
     "targetDedicatedNodes": 5,
     "targetLowPriorityNodes": 0,
     "maxTasksPerNode": 3,
     "taskSchedulingPolicy": {
          "nodeFillType": "spread"
     },
     "enableAutoScale": false,
     "enableInterNodeCommunication": true,
     "metadata": [
    {
      "name": "myproperty",
      "value": "myvalue"
    }
       ]
}
```

## <a name="outbound-access-to-the-internet"></a>İnternet 'e giden erişim

Genel IP adreslerine sahip olmayan bir havuzda, ağ kurulumunuzu, [sanal ağ NAT](../virtual-network/nat-overview.md)kullanarak gibi uygun şekilde yapılandırmadığınız sürece, sanal makineleriniz herkese açık internet 'e erişemez. NAT 'nin yalnızca sanal ağdaki sanal makinelerden gelen internet 'e giden erişime izin verdiğini unutmayın. Toplu oluşturulan işlem düğümlerine ortak IP adresleri ilişkilendirilmediğinden, genel olarak erişilebilir olmayacaktır.

Giden bağlantı sağlamanın bir diğer yolu da Kullanıcı tanımlı yol (UDR) kullanmaktır. Bu, trafiği genel internet erişimi olan bir ara makineye yönlendirmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir sanal ağda havuz oluşturma](batch-virtual-network.md)hakkında daha fazla bilgi edinin.
- [Batch hesaplarıyla özel uç noktaları nasıl kullanacağınızı](private-connectivity.md)öğrenin.
