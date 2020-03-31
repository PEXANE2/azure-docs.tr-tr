---
title: "Azure ExpressRoute: Klasik VNet'leri Kaynak Yöneticisine geçirin"
description: Bu sayfa, expressroute ile ilişkili sanal ağların devrenizi hareket ettirdikten sonra Kaynak Yöneticisi'ne nasıl geçirilen leri açıklar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: cherylmc
ms.openlocfilehash: 8033c80b72c19a9473ce7ecfaa8fe5a1da9f12ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061329"
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>ExpressRoute ile ilişkili sanal ağları klasikten Kaynak Yöneticisi'ne geçirin

Bu makalede, ExpressRoute devrenizi hareket ettirdikten sonra ExpressRoute ile ilişkili sanal ağların klasik dağıtım modelinden Azure Kaynak Yöneticisi dağıtım modeline nasıl geçirildirilen açıklanmaktadır. 

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure PowerShell modüllerinin en son sürümlerine sahip olduğunuzu doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). PowerShell Hizmet Yönetimi modüllerini yüklemek için (klasik dağıtım modeli için gereklidir), [bkz.](/powershell/azure/servicemanagement/install-azure-ps)
* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirdiğinizden emin olun.
* [Bir ExpressRoute devresini klasikten Kaynak Yöneticisi'ne taşıma](expressroute-move.md)altında sağlanan bilgileri gözden geçirin. Sınırları ve sınırlamaları tam olarak anladığınızdan emin olun.
* Devrenin klasik dağıtım modelinde tam olarak çalışır durumda olduğunu doğrulayın.
* Kaynak Yöneticisi dağıtım modelinde oluşturulmuş bir kaynak grubunuz olduğundan emin olun.
* Aşağıdaki kaynak geçiş belgelerini gözden geçirin:

    * [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [SSS'ler: IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [En yaygın geçiş hatalarını ve azaltıcı etkenleri gözden geçirme](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Desteklenen ve desteklenmeyen senaryolar

* ExpressRoute devresi, herhangi bir kesinti olmaksızın klasik kaynak yöneticisi ortamına taşınabilir. Herhangi bir ExpressRoute devresini klasikten Kaynak Yöneticisi ortamına hiçbir kesinti olmadan taşıyabilirsiniz. [PowerShell'i kullanarak ExpressRoute devrelerini klasikten Kaynak Yöneticisi dağıtım modeline taşıma](expressroute-howto-move-arm.md)yönergelerini izleyin. Bu, sanal ağa bağlı kaynakları taşımak için bir ön koşuldur.
* Aynı abonelikteki bir ExpressRoute devresine bağlı sanal ağ, ağ geçidi ve ilişkili dağıtımlar, herhangi bir kesinti olmaksızın Kaynak Yöneticisi ortamına geçirilebilir. Sanal ağ içinde dağıtılan sanal ağlar, ağ geçitleri ve sanal makineler gibi kaynakları geçirmek için daha sonra açıklanan adımları izleyebilirsiniz. Sanal ağların geçirilmeden önce doğru şekilde yapılandırıldığından emin olmalısınız. 
* ExpressRoute devresi ile aynı abonelikte olmayan sanal ağ, ağ geçidi ve ilişkili dağıtımlar, geçişi tamamlamak için biraz kapalı kalma süresi gerektirir. Belgenin son bölümünde, kaynakları geçirmek için izlenecek adımlar açıklanmaktadır.
* Hem ExpressRoute Ağ Geçidi hem de VPN Ağ Geçidi'ne sahip bir sanal ağ geçirilemiyor.
* ExpressRoute devresi çapraz abonelik geçişi desteklenmez. Daha fazla bilgi için [Microsoft.Network taşıma desteğine](../azure-resource-manager/management/move-support-resources.md#microsoftnetwork)bakın.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>ExpressRoute devresini klasikten Kaynak Yöneticisi'ne taşıma
ExpressRoute devresine bağlı kaynakları taşımaya çalışmadan önce bir ExpressRoute devresini klasikten Kaynak Yöneticisi ortamına taşımanız gerekir. Bu görevi gerçekleştirmek için aşağıdaki makalelere bakın:

* [Bir ExpressRoute devresini klasikten Kaynak Yöneticisi'ne taşıma](expressroute-move.md)altında sağlanan bilgileri gözden geçirin.
* [Azure PowerShell'i kullanarak bir devreyi klasikkaynak yöneticisine taşıyın.](expressroute-howto-move-arm.md)
* Azure Hizmet Yönetimi portalını kullanın. [Yeni bir ExpressRoute devresi oluşturmak](expressroute-howto-circuit-portal-resource-manager.md) ve alma seçeneğini seçmek için iş akışını takip edebilirsiniz. 

Bu işlem kapalı kalma süresi içermez. Geçiş devam ederken binanız ve Microsoft arasında veri aktarmaya devam edebilirsiniz.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Sanal ağları, ağ geçitleri ve ilişkili dağıtımları geçirme

Geçiş yapmak için izleyeceğiniz adımlar, kaynaklarınızın aynı abonelikte mi, farklı aboneliklerde mi yoksa her ikisinde mi olduğuna bağlıdır.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Virtual Ağları, ağ geçitlerini ve ilişkili dağıtımları ExpressRoute devresi ile aynı abonelikte geçirme
Bu bölümde, Bir sanal ağ, ağ geçidi ve ilişkili dağıtımları ExpressRoute devresi ile aynı abonelikte geçirmek için izlenecek adımları açıklanır. Bu geçişle hiçbir kapalı kalma süresi ilişkilendirilmez. Geçiş işlemi boyunca tüm kaynakları kullanmaya devam edebilirsiniz. Geçiş devam ederken yönetim düzlemi kilitlenir. 

1. ExpressRoute devresinin klasikten Kaynak Yöneticisi ortamına taşındığından emin olun.
2. Sanal ağın geçiş için uygun şekilde hazırlandığından emin olun.
3. Kaynak geçişi için aboneliğinizi kaydedin. Kaynak geçişi için aboneliğinizi kaydetmek için aşağıdaki PowerShell parçacıklarını kullanın:

   ```powershell 
   Select-AzSubscription -SubscriptionName <Your Subscription Name>
   Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
   Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
   ```
4. Doğrulayın, hazırlayın ve geçirin. Sanal ağı taşımak için aşağıdaki PowerShell parçacıklarını kullanın:

   ```powershell
   Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
   Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
   Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
   ```

   Ayrıca aşağıdaki PowerShell cmdlet çalıştırarak geçiş iptal edebilirsiniz:

   ```powershell
   Move-AzureVirtualNetwork -Abort $vnetName
   ```

## <a name="next-steps"></a>Sonraki adımlar
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [SSS'ler: IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [En yaygın geçiş hatalarını ve azaltıcı etkenleri gözden geçirme](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
