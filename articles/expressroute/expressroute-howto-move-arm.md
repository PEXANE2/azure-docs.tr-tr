---
title: 'Azure ExpressRoute: klasik devreleri Kaynak Yöneticisi taşıyın'
description: Bu sayfada, PowerShell kullanarak klasik bir devrenin Kaynak Yöneticisi dağıtım modeline nasıl taşınacağı açıklanır.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 02/25/2019
ms.author: charwen
ms.openlocfilehash: 5c4bbdde04176edf693fc58d172a7dcc250c495a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84725010"
---
# <a name="move-expressroute-circuits-from-classic-to-resource-manager-deployment-model-using-powershell"></a>ExpressRoute devrelerini klasik bilgisayardan Kaynak Yöneticisi dağıtım modeline PowerShell kullanarak taşıma

Hem klasik hem de Kaynak Yöneticisi dağıtım modelleri için bir ExpressRoute bağlantı hattı kullanmak için, devreyi Kaynak Yöneticisi dağıtım modeline taşımanız gerekir. Aşağıdaki bölümler, PowerShell 'i kullanarak devrenizi taşımanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

* Bilgisayarınızda hem klasik hem de az Azure PowerShell modüllerini yüklediğinizi doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirdiğinizden emin olun.
* [Bir ExpressRoute bağlantı hattını klasik ' ten Kaynak Yöneticisi taşıma](expressroute-move.md)altında sunulan bilgileri gözden geçirin. Sınırları ve sınırlamaları tam olarak anladığınızdan emin olun.
* Devreninin klasik dağıtım modelinde tam olarak çalıştığını doğrulayın.
* Kaynak Yöneticisi dağıtım modelinde oluşturulmuş bir kaynak grubuna sahip olduğunuzdan emin olun.

## <a name="move-an-expressroute-circuit"></a>ExpressRoute devresini taşıma

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>1. Adım: klasik dağıtım modelinden devre ayrıntılarını toplama

Klasik Azure ortamında oturum açın ve hizmet anahtarını toplayın.

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

4. ExpressRoute bağlantı hatlarınızın tamamına ait hizmet anahtarlarını almak için aşağıdaki cmdlet 'i kullanın. Anahtarları aldıktan sonra, Kaynak Yöneticisi dağıtım modeline taşımak istediğiniz devrenin **hizmet anahtarını** kopyalayın.

   ```powershell
   Get-AzureDedicatedCircuit
   ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>2. Adım: oturum açın ve bir kaynak grubu oluşturun

Kaynak Yöneticisi ortamında oturum açın ve yeni bir kaynak grubu oluşturun.

1. Azure Resource Manager ortamınızda oturum açın.

   ```powershell
   Connect-AzAccount
   ```

2. Uygun Azure aboneliğini seçin.

   ```powershell
   Get-AzSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzSubscription
   ```

3. Zaten bir kaynak grubunuz yoksa, yeni bir kaynak grubu oluşturmak için aşağıdaki kod parçacığını değiştirin.

   ```powershell
   New-AzResourceGroup -Name "DemoRG" -Location "West US"
   ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>3. Adım: ExpressRoute devresini Kaynak Yöneticisi dağıtım modeline taşıma

Artık ExpressRoute devrenizi klasik dağıtım modelinden Kaynak Yöneticisi dağıtım modeline taşımaya hazırsınız. Devam etmeden önce, [bir ExpressRoute bağlantı hattını klasik Kaynak Yöneticisi dağıtım modeline taşırken](expressroute-move.md)belirtilen bilgileri gözden geçirin.

Devrenizi taşımak için aşağıdaki kod parçacığını değiştirin ve çalıştırın:

```powershell
Move-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

Klasik modda, bir ExpressRoute bağlantı hattının bir bölgeye bağlı olması kavramı yoktur. Ancak, Kaynak Yöneticisi ' de, her kaynağın bir Azure bölgesine eşlenmesi gerekir. Move-Azexpressroutedevresi cmdlet 'inde belirtilen bölge Teknik olarak herhangi bir bölge olabilir. Kuruluş amacıyla, eşleme konumunuzu en iyi şekilde temsil eden bir bölge seçmek isteyebilirsiniz.

> [!NOTE]
> Taşıma tamamlandıktan sonra, önceki cmdlet 'te listelenen yeni ad, kaynağı ele almak için kullanılacaktır. Devre adı temel olarak yeniden adlandırılır.
> 

## <a name="modify-circuit-access"></a>Devre erişimini Değiştir

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>Her iki dağıtım modeli için ExpressRoute devresine erişimini etkinleştirmek için

Klasik ExpressRoute devrenizi Kaynak Yöneticisi dağıtım modeline taşıdıktan sonra, her iki dağıtım modeline de erişimi etkinleştirebilirsiniz. Dağıtım modellerine erişimi etkinleştirmek için aşağıdaki cmdlet 'leri çalıştırın:

1. Devre ayrıntılarını alın.

   ```powershell
   $ckt = Get-AzExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
   ```

2. "Klasik Işlemlere Izin ver" i TRUE olarak ayarlayın.

   ```powershell
   $ckt.AllowClassicOperations = $true
   ```

3. Devresini güncelleştirin. Bu işlem başarıyla tamamlandıktan sonra, bu devreyi klasik dağıtım modelinde görüntüleyebileceksiniz.

   ```powershell
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

4. ExpressRoute devresinin ayrıntılarını almak için aşağıdaki cmdlet 'i çalıştırın. Listelenen hizmet anahtarını görebilmeniz gerekir.

   ```powershell
   get-azurededicatedcircuit
   ```

5. Artık, klasik sanal ağlar için klasik dağıtım modeli komutlarını ve Kaynak Yöneticisi sanal ağlar için Kaynak Yöneticisi komutlarını kullanarak ExpressRoute bağlantı hattını yönetebilirsiniz. Aşağıdaki makaleler ExpressRoute bağlantı hattı bağlantılarını yönetmenize yardımcı olur:

    * [Sanal ağınızı Kaynak Yöneticisi dağıtım modelindeki ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-arm.md)
    * [Sanal ağınızı klasik dağıtım modelindeki ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>Klasik dağıtım modeline ExpressRoute devresine erişimini devre dışı bırakmak için

Klasik dağıtım modeline erişimi devre dışı bırakmak için aşağıdaki cmdlet 'leri çalıştırın.

1. ExpressRoute devresinin ayrıntılarını alın.

   ```powershell
   $ckt = Get-AzExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
   ```

2. "Klasik Işlemlere Izin ver" ayarını FALSE olarak belirleyin.

   ```powershell
   $ckt.AllowClassicOperations = $false
   ```

3. Devresini güncelleştirin. Bu işlem başarıyla tamamlandıktan sonra, bu devreyi klasik dağıtım modelinde görüntüleyemeyeceksiniz.

   ```powershell
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağınızı ExpressRoute devrenizi ile bağlama](expressroute-howto-linkvnet-arm.md)
