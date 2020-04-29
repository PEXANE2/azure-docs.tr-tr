---
title: 'Azure ExpressRoute: Global Reach yapılandırma'
description: Bu makale, ExpressRoute bağlantı devresine, şirket içi ağlarınızla özel bir ağ oluşturmak ve Global Reach etkinleştirmek için bir araya yönlendirmenize yardımcı olur.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.openlocfilehash: 587a17659a412d6f894faf5a744a7d9c444935c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80656739"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach’i yapılandırma

Bu makale, PowerShell kullanarak ExpressRoute Global Reach yapılandırmanıza yardımcı olur. Daha fazla bilgi için bkz. [ExpressRouteRoute Global Reach](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce, aşağıdakileri onaylayın:

* ExpressRoute devre sağlama [iş akışlarını](expressroute-workflows.md)anlamış olursunuz.
* ExpressRoute devrelerinizi sağlanan bir durumda.
* Azure özel eşlemesi, ExpressRoute devrelerinizi yapılandırılmış.
* PowerShell 'i yerel olarak çalıştırmak istiyorsanız, Azure PowerShell en son sürümünün bilgisayarınızda yüklü olduğunu doğrulayın.

### <a name="working-with-azure-powershell"></a>Azure PowerShell çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="identify-circuits"></a>Devreleri tanımla

1. Yapılandırmayı başlatmak için Azure hesabınızda oturum açın ve kullanmak istediğiniz aboneliği seçin.

   [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]
2. Kullanmak istediğiniz ExpressRoute devreleri belirler. Desteklenen ülkelerde/bölgelerde bulundukları ve farklı eşleme konumlarında oluşturulan her iki ExpressRoute devresine ait özel eşleme arasında ExpressRoute Global Reach etkinleştirebilirsiniz. 

   * Aboneliğiniz her iki devrede sahipse, aşağıdaki bölümlerde yapılandırmayı çalıştırmak için devre seçeneğini belirleyebilirsiniz.
   * İki devre farklı Azure aboneliklerdeyse, bir Azure aboneliğinden yetkilendirme yapmanız gerekir. Ardından, diğer Azure aboneliğinde yapılandırma komutunu çalıştırdığınızda yetkilendirme anahtarını geçirirsiniz.

## <a name="enable-connectivity"></a>Bağlantıyı etkinleştir

Şirket içi ağlarınız arasında bağlantıyı etkinleştirin. Aynı Azure aboneliğinde ve farklı abonelikler içeren devrelere yönelik ayrı yönergeler kümesi vardır.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Aynı Azure aboneliğindeki ExpressRoute devreleri

1. Devre 1 ve devre 2 ' i almak için aşağıdaki komutları kullanın. İki devre aynı abonelikte bulunur.

   ```azurepowershell-interactive
   $ckt_1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   ```
2. Devre 1 ' de aşağıdaki komutu çalıştırın ve devre 2 ' nin özel eşleme KIMLIĞINI geçirin. Komutu çalıştırırken şunları göz önünde edin:

   * Özel eşleme KIMLIĞI aşağıdaki örneğe benzer şekilde görünür: 

     ```
     /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
     ```
   * *-Addresspredüzeltmesini* bir/29 IPv4 alt ağı olmalıdır, örneğin, "10.0.0.0/29". İki ExpressRoute devreleri arasında bağlantı kurmak için bu alt ağdaki IP adreslerini kullanıyoruz. Bu alt ağdaki adresleri Azure sanal ağlarınızda veya şirket içi ağınızda kullanmamalısınız.

     ```azurepowershell-interactive
     Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
     ```
3. Yapılandırmayı şu şekilde devre 1 ' de kaydedin:

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla şirket içi ağlarınızla her iki tarafa da bağlantınız olur.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute devreleri

İki devrede aynı Azure aboneliğinde yoksa, yetkilendirme yapmanız gerekir. Aşağıdaki yapılandırmada, yetkilendirme devre 2 aboneliğinde oluşturulur ve yetkilendirme anahtarı devre 1 ' e geçirilir.

1. Yetkilendirme anahtarı oluşturun.

   ```azurepowershell-interactive
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_2
   ```

   Devre 2 ' nin özel eşleme KIMLIĞINI ve yetkilendirme anahtarını bir yere unutmayın.
2. Devre 1 ' de aşağıdaki komutu çalıştırın. Devre 2 ' nin özel eşleme KIMLIĞINI ve yetkilendirme anahtarını geçirin.

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
   ```
3. Yapılandırmayı devre 1 ' de kaydedin.

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlem tamamlandığında, iki ExpressRoute devreniz aracılığıyla şirket içi ağlarınızla her iki tarafa da bağlantınız olur.

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmanın gerçekleştiği devre üzerindeki yapılandırmayı doğrulamak için aşağıdaki komutu kullanın (örneğin, önceki örnekte devre 1).
```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

PowerShell 'de *$CKT 1* ' i çalıştırırsanız, çıkışta devre bağlantı *durumu* ' nu görürsünüz. Bağlantının oluşturulup yüklenmediğini, "bağlı" veya "bağlantısı kesik" olduğunu söyler. 

## <a name="disable-connectivity"></a>Bağlantıyı devre dışı bırakma

Şirket içi ağlarınız arasında bağlantıyı devre dışı bırakmak için, yapılandırmanın yapıldığı devreye karşı komutları çalıştırın (örneğin, önceki örnekte devre 1).

```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için Get işlemini çalıştırabilirsiniz.

Önceki işlem tamamlandıktan sonra, ExpressRoute devrelerinizi kullanarak şirket içi ağınız arasında artık bağlantı kuramadınız.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
2. [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
3. [Azure sanal ağına bir ExpressRoute bağlantı hattı bağlama](expressroute-howto-linkvnet-arm.md)
