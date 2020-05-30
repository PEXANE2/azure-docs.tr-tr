---
title: VNet veri kaynakları için Azure Analysis Services yapılandırma | Microsoft Docs
description: Azure sanal ağ (VNet) üzerinde veri kaynakları için bir ağ geçidi kullanmak üzere bir Azure Analysis Services sunucusunu nasıl yapılandıracağınızı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 912dab16cd854ae940b7bbdfe88a8da7adf1c5e0
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84197187"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Azure sanal ağı (VNet) üzerinde veri kaynakları için ağ geçidi kullanma

Bu makalede, veri kaynakları bir [Azure sanal ağı 'nda (VNet)](../virtual-network/virtual-networks-overview.md)olduğunda kullanılmak üzere Azure Analysis Services **Alwaysusegateway** Server özelliği açıklanmaktadır.

## <a name="server-access-to-vnet-data-sources"></a>VNet veri kaynaklarına sunucu erişimi

Veri kaynaklarınıza bir sanal ağ üzerinden erişiliyorsa, Azure Analysis Services sunucunuzun kendi ortamınızda, şirket içinde olduğu gibi bu veri kaynaklarına bağlanması gerekir. Bir [Şirket içi ağ geçidi](analysis-services-gateway.md)aracılığıyla tüm veri kaynaklarına erişmek için sunucuyu belirtmek üzere **Alwaysusegateway** sunucusu özelliğini yapılandırabilirsiniz. 

Azure SQL yönetilen örnek veri kaynakları, Azure sanal ağı 'nda özel bir IP adresiyle çalışır. Örnekte ortak uç nokta etkinse, bir ağ geçidi gerekli değildir. Ortak uç nokta etkinleştirilmemişse, bir şirket içi veri ağ geçidi gerekir ve AlwaysUseGateway özelliği true olarak ayarlanmalıdır.

> [!NOTE]
> Bu özellik yalnızca şirket [Içi veri ağ geçidi](analysis-services-gateway.md) yüklenip yapılandırıldığında geçerlidir. Ağ Geçidi VNet üzerinde olabilir.

## <a name="configure-alwaysusegateway-property"></a>AlwaysUseGateway özelliğini yapılandırma

1. SSMS > Server > **özellikleri**  >  **genel**' te, **Gelişmiş (tümü) özelliklerini göster**' i seçin.
2. **Aspaas\alwaysusegateway**içinde **true**seçeneğini belirleyin.

    ![Her zaman ağ geçidi özelliğini kullan](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Ayrıca bkz.
[Şirket içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Şirket içi veri ağ geçidi yükleyip yapılandırma](analysis-services-gateway-install.md)   
[Azure sanal ağı (VNET)](../virtual-network/virtual-networks-overview.md)   

