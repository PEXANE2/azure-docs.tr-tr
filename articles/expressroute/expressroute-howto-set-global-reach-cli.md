---
title: "Azure ExpressRoute: ExpressRoute Global Reach'i yapılandır: CLI"
description: Bu makale, şirket içi ağlarınız arasında özel bir ağ yapmak ve Global Reach'i etkinleştirmek için ExpressRoute devrelerini birbirine bağlamanıza yardımcı olur.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: jaredro
ms.openlocfilehash: a39cf4e09a70ca2b1225d699c84abf0e7f1d2eab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476415"
---
# <a name="configure-expressroute-global-reach-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak ExpressRoute Global Erişimini Yapılandır

Bu makale, Azure CLI'yi kullanarak Azure ExpressRoute Global Reach'i yapılandırmanıza yardımcı olur. Daha fazla bilgi için bkz. [ExpressRoute Global Reach](expressroute-global-reach.md).
 
Yapılandırmayı başlatmadan önce aşağıdaki gereksinimleri tamamlayın:

* Azure CLI'nin en son sürümünü yükleyin. Bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).
* ExpressRoute devre sağlama [iş akışlarını](expressroute-workflows.md)anlayın.
* ExpressRoute devrelerinizin Sağlanan durumda olduğundan emin olun.
* Azure özel eşlemenin ExpressRoute devrelerinizde yapılandırıldığından emin olun.  

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Yapılandırmayı başlatmak için Azure hesabınızda oturum açın. Aşağıdaki komut varsayılan tarayıcınızı açar ve Azure hesabınız için oturum açma kimlik bilgilerini ister:  

```azurecli
az login
```

Birden fazla Azure aboneliğiniz varsa, hesabın aboneliklerini kontrol edin:

```azurecli
az account list
```

Kullanmak istediğiniz aboneliği belirtin:

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Yapılandırma için ExpressRoute devrelerinizi tanımlayın

ExpressRoute Global Reach'i, desteklenen ülkelerde/bölgelerde bulundukları ve farklı konumlarda oluşturuldukları sürece, herhangi iki ExpressRoute devresi arasında etkinleştirebilirsiniz. Aboneliğiniz her iki devreye de sahipse, yapılandırmayı çalıştırmak için bu makalede açıklandığı gibi her iki devreyi de seçebilirsiniz. İki devre farklı Azure aboneliklerindeyse, bir Azure aboneliğinden yetkilendirmeniz olmalıdır ve diğer Azure aboneliğinde yapılandırma komutunu çalıştırdığınızda yetkilendirme anahtarından geçmeniz gerekir.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantıyı etkinleştirme

Bağlantıyı etkinleştirmek için komutu çalıştırırken, parametre değerleri için aşağıdaki gereksinimleri not edin:

* *eş-devre* tam kaynak kimliği olmalıdır. Örnek:

  > /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}

* *adres öneki* bir "/29" IPv4 alt ağı olmalıdır (örneğin, "10.0.0.0/29"). Bu alt ağdaki IP adreslerini iki ExpressRoute devresi arasında bağlantı kurmak için kullanırız. Bu alt ağdaki adresleri Azure sanal ağlarınızda veya şirket içi ağlarınızda kullanmamalısınız.

İki ExpressRoute devresini bağlamak için aşağıdaki CLI komutunu çalıştırın:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

CLI çıktısı şu na benzer:

```output
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Bu işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla her iki taraftaki şirket içi ağlarınız arasında bağlantı kuracaksınız.

## <a name="enable-connectivity-between-expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute devreleri arasında bağlantı sağlama

İki devre aynı Azure aboneliğinde değilse, yetkilendirmeye ihtiyacınız vardır. Aşağıdaki yapılandırmada, devre 2 aboneliğinde yetkilendirme oluşturur ve yetkilendirme anahtarını devre 1'e geçirirsiniz.

1. Yetkilendirme anahtarı oluşturun:

   ```azurecli
   az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
   ```

   CLI çıktısı şu na benzer:

   ```output
   {
     "authorizationKey": "<authorizationKey>",
     "authorizationUseStatus": "Available",
     "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
     "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
     "name": "<AuthorizationName>",
     "provisioningState": "Succeeded",
     "resourceGroup": "<Circuit2ResourceGroupName>",
     "type": "Microsoft.Network/expressRouteCircuits/authorizations"
   }
   ```

1. Hem kaynak kimliğini hem de devre 2 için yetkilendirme anahtarını not alın.

1. Devre 2'nin kaynak kimliği ve yetkilendirme anahtarını geçerek devre 1'e karşı aşağıdaki komutu çalıştırın:

   ```azurecli
   az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
   ```

Bu işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla her iki taraftaki şirket içi ağlarınız arasında bağlantı kuracaksınız.

## <a name="get-and-verify-the-configuration"></a>Yapılandırmayı alın ve doğrulayın

Yapılandırmanın yapıldığı devredeki yapılandırmayı doğrulamak için aşağıdaki komutu kullanın (önceki örnekte devre 1):

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

CLI çıkışında *CircuitConnectionStatus'u*görürsünüz. İki devre arasındaki bağlantının kurulup kurulmadığını ("Bağlı") veya kurulmadığını ("Bağlantısı kesildi") söyler. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasındaki bağlantıyı devre dışı

Bağlantıyı devre dışı kardırmak için, konfigürasyonun yapılığının yapılabildiği devreye (daha önceki örnekte devre 1) karşı aşağıdaki komutu çalıştırın.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Durumu ```show``` doğrulamak için komutu kullanın.

Bu işlem tamamlandığında, ExpressRoute devreleriniz aracılığıyla şirket içi ağlarınız arasında bağlantı olmayacak.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
* [ExpressRoute bağlantısını doğrula](expressroute-troubleshooting-expressroute-overview.md)
* [ExpressRoute devresini sanal ağa bağlama](expressroute-howto-linkvnet-arm.md)
