---
title: 'Azure ExpressRoute: Küresel Erişimi Yapılandır'
description: Bu makale, şirket içi ağlarınız arasında özel bir ağ yapmak ve Global Reach'i etkinleştirmek için ExpressRoute devrelerini birbirine bağlamanıza yardımcı olur.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.openlocfilehash: 587a17659a412d6f894faf5a744a7d9c444935c8
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656739"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach’i yapılandırma

Bu makale, PowerShell kullanarak ExpressRoute Global Reach'i yapılandırmanıza yardımcı olur. Daha fazla bilgi için [ExpressRouteRoute Global Reach'e](expressroute-global-reach.md)bakın.

 ## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmayı başlatmadan önce aşağıdakileri onaylayın:

* ExpressRoute devresi sağlama [iş akışlarını](expressroute-workflows.md)anlıyorsunuz.
* ExpressRoute devreleriniz hazır durumdadır.
* Azure özel eşleme, ExpressRoute devrelerinizde yapılandırılır.
* PowerShell'i yerel olarak çalıştırmak istiyorsanız, Azure PowerShell'in en son sürümünün bilgisayarınızda yüklü olduğunu doğrulayın.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="identify-circuits"></a>Devreleri tanımlama

1. Yapılandırmayı başlatmak için Azure hesabınızda oturum açın ve kullanmak istediğiniz aboneliği seçin.

   [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]
2. Kullanmak istediğiniz ExpressRoute devrelerini tanımlayın. ExpressRoute Global Reach'i, desteklenen ülkelerde/bölgelerde bulundukları ve farklı konumlarda oluşturuldukları sürece, herhangi iki ExpressRoute devresinin özel bakışları arasında etkinleştirebilirsiniz. 

   * Aboneliğiniz her iki devreye de sahipse, aşağıdaki bölümlerde yapılandırmayı çalıştırmak için her iki devreyi de seçebilirsiniz.
   * İki devre farklı Azure aboneliklerindeyse, bir Azure aboneliğinden yetkilendirme yapmanız gerekir. Ardından, diğer Azure aboneliğinde yapılandırma komutunu çalıştırdığınızda yetkilendirme anahtarını geçersiniz.

## <a name="enable-connectivity"></a>Bağlantıyı etkinleştirme

Şirket içi ağlarınız arasında bağlantıyı etkinleştirin. Aynı Azure aboneliğinde olan devreler ve farklı abonelikler olan devreler için ayrı yönerge kümeleri vardır.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Aynı Azure aboneliğinde ExpressRoute devreleri

1. Devre 1 ve devre 2 almak için aşağıdaki komutları kullanın. İki devre aynı abonelikte.

   ```azurepowershell-interactive
   $ckt_1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   ```
2. Devre 1'e karşı aşağıdaki komutu çalıştırın ve devre 2'nin özel eşleme kimliğine geçin. Komutu çalıştırırken aşağıdakileri unutmayın:

   * Özel eşleme kimliği aşağıdaki örneğe benzer: 

     ```
     /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
     ```
   * *-AddressPrefix* bir /29 IPv4 alt ağı olmalıdır, örneğin, "10.0.0.0/29". Bu alt ağdaki IP adreslerini iki ExpressRoute devresi arasında bağlantı kurmak için kullanırız. Bu alt ağdaki adresleri Azure sanal ağlarınızda veya şirket içi ağınızda kullanmamalısınız.

     ```azurepowershell-interactive
     Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
     ```
3. Konfigürasyonu devre 1'e aşağıdaki şekilde kaydedin:

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla her iki taraftaki şirket içi ağlarınız arasında bağlantı olacaktır.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute devreleri

İki devre aynı Azure aboneliğinde değilse, yetkilendirmeye ihtiyacınız vardır. Aşağıdaki yapılandırmada, devre 2 aboneliğinde yetkilendirme oluşturulur ve yetkilendirme anahtarı devre 1'e geçirilir.

1. Bir yetkilendirme anahtarı oluşturun.

   ```azurepowershell-interactive
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_2
   ```

   Devre 2'nin özel eşleme kimliğini ve yetkilendirme anahtarını not alın.
2. Devre 1'e karşı aşağıdaki komutu çalıştırın. Devre 2'nin özel eşleme kimliğini ve yetki anahtarını geçirin.

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
   ```
3. Konfigürasyonu devre 1'e kaydedin.

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla her iki taraftaki şirket içi ağlarınız arasında bağlantı olacaktır.

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmanın yapıldığı devredeki yapılandırmayı doğrulamak için aşağıdaki komutu kullanın (örneğin, önceki örnekte devre 1).
```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

PowerShell'de *yalnızca $ckt1* çalıştırın, çıkışta *CircuitConnectionStatus'i* görürsünüz. Bağlantının kurulup kurulmadığını söyler, "Bağlı", "Bağlantı" veya "Bağlantısı kesildi". 

## <a name="disable-connectivity"></a>Bağlantıyı devre dışı bırakma

Şirket içi ağlarınız arasındaki bağlantıyı devre dışı kılabilir, komutları yapılandırmanın yapıldığı devreye karşı çalıştırın (örneğin, önceki örnekte devre 1).

```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için Get işlemini çalıştırabilirsiniz.

Önceki işlem tamamlandıktan sonra, ExpressRoute devreleriniz aracılığıyla şirket içi ağınız arasında artık bağlantı yoktur.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
2. [ExpressRoute bağlantısını doğrula](expressroute-troubleshooting-expressroute-overview.md)
3. [ExpressRoute devresini Azure sanal ağına bağlama](expressroute-howto-linkvnet-arm.md)
