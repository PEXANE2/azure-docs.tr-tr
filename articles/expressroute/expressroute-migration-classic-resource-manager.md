---
title: "Azure ExpressRoute: klasik VNET 'leri Kaynak Yöneticisi geçirin"
description: Bu sayfada, devrenizi taşıdıktan sonra ExpressRoute ile ilişkili sanal ağların Kaynak Yöneticisi nasıl geçirileceği açıklanır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 02/06/2020
ms.author: cherylmc
ms.openlocfilehash: 85b0fdd56b51a89cfbf88fac58aec4cb2f110142
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738898"
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>ExpressRoute ile ilişkili sanal ağları klasik 'ten Kaynak Yöneticisi geçirme

Bu makalede, ExpressRoute bağlantı hattınızı taşıdıktan sonra ExpressRoute ile ilişkili sanal ağların klasik dağıtım modelinden Azure Resource Manager dağıtım modeline geçirilmesi açıklanmaktadır. 

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure PowerShell modüllerinin en son sürümlerine sahip olduğunuzu doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). PowerShell hizmeti yönetim modülünü (klasik dağıtım modeli için gereklidir) yüklemek için, bkz. [Azure PowerShell Service Management modülünü yükleme](/powershell/azure/servicemanagement/install-azure-ps).
* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirdiğinizden emin olun.
* [Bir ExpressRoute bağlantı hattını klasik ' ten Kaynak Yöneticisi taşıma](expressroute-move.md)altında sunulan bilgileri gözden geçirin. Sınırları ve sınırlamaları tam olarak anladığınızdan emin olun.
* Devreninin klasik dağıtım modelinde tam olarak çalıştığını doğrulayın.
* Kaynak Yöneticisi dağıtım modelinde oluşturulmuş bir kaynak grubuna sahip olduğunuzdan emin olun.
* Aşağıdaki kaynak geçiş belgelerini gözden geçirin:

    * [IaaS kaynaklarının klasik modelden Azure Resource Manager’a platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [SSS: IaaS kaynaklarının klasik 'ten Azure Resource Manager geçişi desteklenir](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [En yaygın geçiş hatalarını ve azaltmaları gözden geçirin](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Desteklenen ve desteklenmeyen senaryolar

* Bir ExpressRoute bağlantı hattı, herhangi bir kesinti olmadan klasik 'dan Kaynak Yöneticisi ortamına taşınabilir. Herhangi bir ExpressRoute devresini, kapalı kalma süresi olmadan Kaynak Yöneticisi ortamına taşıyabilirsiniz. ExpressRoute bağlantı hatlarını [Klasik 'dan Kaynak Yöneticisi dağıtım modeline PowerShell kullanarak taşıma](expressroute-howto-move-arm.md)yönergelerini izleyin. Bu, sanal ağa bağlı kaynakları taşımak için bir önkoşuldur.
* Aynı abonelikte bulunan bir ExpressRoute devresine bağlı sanal ağlar, ağ geçitleri ve ilişkili dağıtımlar, herhangi bir kesinti olmadan Kaynak Yöneticisi ortamına geçirilebilir. Sanal ağlar, ağ geçitleri ve sanal ağ içinde dağıtılan sanal makineler gibi kaynakları geçirmek için daha sonra açıklanan adımları izleyebilirsiniz. Sanal ağların geçirilmeden önce doğru şekilde yapılandırıldığından emin olmanız gerekir. 
* ExpressRoute bağlantı hattı ile aynı abonelikte olmayan sanal ağ içindeki sanal ağlar, ağ geçitleri ve ilişkili dağıtımlar, geçişin tamamlanabilmesi için bazı kapalı kalma süresi gerektirir. Belgenin son bölümünde, kaynakları geçirmek için izlenen adımlar açıklanmaktadır.
* Hem ExpressRoute ağ geçidi hem de VPN Gateway olan bir sanal ağ geçirilemez.
* ExpressRoute devresi çapraz abonelik geçişi desteklenmiyor. Daha fazla bilgi için bkz. [Microsoft. Network taşıma desteği](../azure-resource-manager/management/move-support-resources.md#microsoftnetwork).

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>ExpressRoute bağlantı hattını klasik ' ten Kaynak Yöneticisi taşıyın
ExpressRoute devresine eklenmiş kaynakları geçirmeye çalışmadan önce, bir ExpressRoute bağlantı hattını klasik bilgisayardan Kaynak Yöneticisi ortamına taşımanız gerekir. Bu görevi gerçekleştirmek için aşağıdaki makalelere bakın:

* [Bir ExpressRoute bağlantı hattını klasik ' ten Kaynak Yöneticisi taşıma](expressroute-move.md)altında sunulan bilgileri gözden geçirin.
* [Azure PowerShell kullanarak bir devreyi klasik gruptan Kaynak Yöneticisi taşıyın](expressroute-howto-move-arm.md).
* Azure hizmet yönetim portalı 'nı kullanın. [Yeni bir ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) bağlantı hattı oluşturmak ve içeri aktarma seçeneğini belirlemek için iş akışını takip edebilirsiniz. 

Bu işlem kapalı kalma süresini kapsamaz. Geçiş devam ederken, şirket içi ve Microsoft arasında veri aktarmaya devam edebilirsiniz.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Sanal ağları, ağ geçitlerini ve ilişkili dağıtımları geçirme

Geçirmek için izleyeceğiniz adımlar, kaynaklarınızın aynı abonelikte, farklı aboneliklerde veya her ikisinde de olup olmadığına bağlıdır.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Sanal ağları, ağ geçitlerini ve ilişkili dağıtımları ExpressRoute devresi ile aynı abonelikte geçirme
Bu bölümde, ExpressRoute devresi ile aynı abonelikte bir sanal ağ, ağ geçidi ve ilişkili dağıtımları geçirme adımları açıklanmaktadır. Bu geçişle ilişkili kapalı kalma süresi yok. Geçiş işlemi boyunca tüm kaynakları kullanmaya devam edebilirsiniz. Geçiş sürerken yönetim düzlemi kilitlenir. 

1. ExpressRoute bağlantı hattının klasik 'dan Kaynak Yöneticisi ortamına taşındığından emin olun.
2. Sanal ağın geçiş için uygun şekilde hazırlandığından emin olun.
3. Aboneliğinizi kaynak geçişi için kaydedin. Aboneliğinizi kaynak geçişi için kaydetmek için aşağıdaki PowerShell kod parçacığını kullanın:

   ```powershell 
   Select-AzSubscription -SubscriptionName <Your Subscription Name>
   Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
   Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
   ```
4. Doğrulayın, hazırlayın ve geçirin. Sanal ağı taşımak için aşağıdaki PowerShell kod parçacığını kullanın:

   ```powershell
   Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
   Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
   Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
   ```

   Ayrıca, aşağıdaki PowerShell cmdlet 'ini çalıştırarak geçişi de iptal edebilirsiniz:

   ```powershell
   Move-AzureVirtualNetwork -Abort $vnetName
   ```

## <a name="next-steps"></a>Sonraki adımlar
* [IaaS kaynaklarının klasik modelden Azure Resource Manager’a platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [SSS: IaaS kaynaklarının klasik 'ten Azure Resource Manager geçişi desteklenir](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [En yaygın geçiş hatalarını ve azaltmaları gözden geçirin](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
