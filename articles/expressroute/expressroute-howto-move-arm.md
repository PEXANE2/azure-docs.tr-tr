---
title: "Azure ExpressRoute: Klasik devreleri Kaynak Yöneticisi'ne taşıyın"
description: Bu sayfa, PowerShell kullanarak klasik bir devrenin Kaynak Yöneticisi dağıtım modeline nasıl taşınır yapılacağını açıklar.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: charwen
ms.openlocfilehash: d3014aae44b8d63be67cd0d31f996470aeda40df
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616289"
---
# <a name="move-expressroute-circuits-from-classic-to-resource-manager-deployment-model-using-powershell"></a>PowerShell kullanarak ExpressRoute devrelerini klasikten Kaynak Yöneticisi dağıtım modeline taşıyın

Hem klasik hem de Kaynak Yöneticisi dağıtım modelleri için expressroute devresi kullanmak için, devreyi Kaynak Yöneticisi dağıtım modeline taşımanız gerekir. Aşağıdaki bölümler PowerShell'i kullanarak devrenizi hareket ettirmenize yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

* Hem klasik hem de Az Azure PowerShell modüllerini bilgisayarınıza yerel olarak yüklediğinizi doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirdiğinizden emin olun.
* [Bir ExpressRoute devresini klasikten Kaynak Yöneticisi'ne taşıma](expressroute-move.md)altında sağlanan bilgileri gözden geçirin. Sınırları ve sınırlamaları tam olarak anladığınızdan emin olun.
* Devrenin klasik dağıtım modelinde tam olarak çalışır durumda olduğunu doğrulayın.
* Kaynak Yöneticisi dağıtım modelinde oluşturulmuş bir kaynak grubunuz olduğundan emin olun.

## <a name="move-an-expressroute-circuit"></a>ExpressRoute devresi taşıma

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Adım 1: Klasik dağıtım modelinden devre ayrıntılarını toplama

Azure klasik ortamında oturum açın ve hizmet anahtarını toplayın.

1. Azure hesabınızda oturum açın.

   ```powershell
   Add-AzureAccount
   ```

2. Uygun Azure aboneliğini seçin.

   ```powershell
   Select-AzureSubscription "<Enter Subscription Name here>"
   ```

3. Azure ve ExpressRoute için PowerShell modüllerini içeri aktarın.

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
   ```

4. Tüm ExpressRoute devrelerinizin servis anahtarlarını almak için aşağıdaki cmdlet'i kullanın. Anahtarları aldıktan sonra, Kaynak Yöneticisi dağıtım modeline taşımak istediğiniz devrenin **servis anahtarını** kopyalayın.

   ```powershell
   Get-AzureDedicatedCircuit
   ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Adım 2: Oturum açın ve kaynak grubu oluşturun

Kaynak Yöneticisi ortamında oturum açın ve yeni bir kaynak grubu oluşturun.

1. Azure Kaynak Yöneticisi ortamınızda oturum açın.

   ```powershell
   Connect-AzAccount
   ```

2. Uygun Azure aboneliğini seçin.

   ```powershell
   Get-AzSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzSubscription
   ```

3. Zaten bir kaynak grubunuz yoksa yeni bir kaynak grubu oluşturmak için aşağıdaki snippet'i değiştirin.

   ```powershell
   New-AzResourceGroup -Name "DemoRG" -Location "West US"
   ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Adım 3: ExpressRoute devresini Kaynak Yöneticisi dağıtım modeline taşıyın

Artık ExpressRoute devrenizi klasik dağıtım modelinden Kaynak Yöneticisi dağıtım modeline taşımaya hazırsınız. İşleme geçmeden önce, [ExpressRoute devresini klasikten Kaynak Yöneticisi dağıtım modeline taşımada](expressroute-move.md)sağlanan bilgileri gözden geçirin.

Devrenizi taşımak için aşağıdaki parçacığı değiştirin ve çalıştırın:

```powershell
Move-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

Klasik modda, ExpressRoute devresi bir bölgeye bağlı olma kavramına sahip değildir. Ancak, Kaynak Yöneticisi'nde her kaynağın bir Azure bölgesine eşlenebilmesi gerekir. Move-AzExpressRouteCircuit cmdlet'te belirtilen bölge teknik olarak herhangi bir bölge olabilir. Kuruluş amaçları için, görünen konumunuzu yakından temsil eden bir bölge seçmek isteyebilirsiniz.

> [!NOTE]
> Taşıma tamamlandıktan sonra, kaynağı ele almak için önceki cmdlet'te listelenen yeni ad kullanılacaktır. Devre nin adı yeniden adlandırılacak.
> 

## <a name="modify-circuit-access"></a>Devre erişimini değiştirme

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>Her iki dağıtım modeli için ExpressRoute devre erişimi etkinleştirmek için

Klasik ExpressRoute devrenizi Kaynak Yöneticisi dağıtım modeline taşıyarak, her iki dağıtım modeline de erişimi etkinleştirebilirsiniz. Her iki dağıtım modeline erişimi sağlamak için aşağıdaki cmdletleri çalıştırın:

1. Devre detaylarını al.

   ```powershell
   $ckt = Get-AzExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
   ```

2. "Klasik İşlemlere İzin Ver"i TRUE olarak ayarlayın.

   ```powershell
   $ckt.AllowClassicOperations = $true
   ```

3. Devreyi güncelleyin. Bu işlem başarıyla tamamlandıktan sonra, devreyi klasik dağıtım modelinde görebilirsiniz.

   ```powershell
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

4. ExpressRoute devresinin ayrıntılarını almak için aşağıdaki cmdlet'i çalıştırın. Listelenen servis anahtarını görebilmelisin....

   ```powershell
   get-azurededicatedcircuit
   ```

5. Artık klasik VNet'ler için klasik dağıtım modeli komutlarını ve Kaynak Yöneticisi VNets için Kaynak Yöneticisi komutlarını kullanarak ExpressRoute devresine giden bağlantıları yönetebilirsiniz. Aşağıdaki makaleler ExpressRoute devresi bağlantılarını yönetmenize yardımcı olur:

    * [Kaynak Yöneticisi dağıtım modelinde sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-arm.md)
    * [Klasik dağıtım modelinde sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>ExpressRoute devresinin klasik dağıtım modeline erişimini devre dışı kınıklamak için

Klasik dağıtım modeline erişimi devre dışı kardMak için aşağıdaki cmdletleri çalıştırın.

1. ExpressRoute devresinin ayrıntılarını alın.

   ```powershell
   $ckt = Get-AzExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
   ```

2. "Klasik İşlemlere İzin Ver"i FALSE olarak ayarlayın.

   ```powershell
   $ckt.AllowClassicOperations = $false
   ```

3. Devreyi güncelleyin. Bu işlem başarıyla tamamlandıktan sonra, devreyi klasik dağıtım modelinde görüntüleyemeyeceksiniz.

   ```powershell
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute devreniz için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-arm.md)
