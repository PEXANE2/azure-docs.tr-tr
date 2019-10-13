---
title: Azure sanal ağ veri kaynakları için şirket içi veri ağ geçidini kullanma | Microsoft Docs
description: VNet üzerindeki veri kaynakları için bir ağ geçidini kullanmak üzere bir sunucuyu nasıl yapılandıracağınızı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d1993996c0f982624d6976b50871fa8c4592e2f6
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72301209"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Azure sanal ağı (VNet) üzerinde veri kaynakları için ağ geçidi kullanma

Bu makalede, veri kaynakları bir [Azure sanal ağı 'nda (VNet)](../virtual-network/virtual-networks-overview.md)olduğunda kullanılmak üzere **Alwaysusegateway** sunucusu özelliği açıklanır.

## <a name="server-access-to-vnet-data-sources"></a>VNet veri kaynaklarına sunucu erişimi

Veri kaynaklarınıza bir sanal ağ üzerinden erişiliyorsa, Azure Analysis Services sunucunuzun kendi ortamınızda, şirket içinde olduğu gibi bu veri kaynaklarına bağlanması gerekir. Bir [Şirket içi ağ geçidi](analysis-services-gateway.md)aracılığıyla tüm veri kaynaklarına erişmek için sunucuyu belirtmek üzere **Alwaysusegateway** sunucusu özelliğini yapılandırabilirsiniz. 

Azure SQL veritabanı yönetilen örnek veri kaynakları, Azure sanal ağı 'nda özel bir IP adresiyle çalışır. Örnekte ortak uç nokta etkinse, bir ağ geçidi gerekli değildir. Ortak uç nokta etkinleştirilmemişse, bir şirket içi veri ağ geçidi gerekir ve AlwaysUseGateway özelliği true olarak ayarlanmalıdır.

> [!NOTE]
> Bu özellik yalnızca şirket [Içi veri ağ geçidi](analysis-services-gateway.md) yüklenip yapılandırıldığında geçerlidir. Ağ Geçidi VNet üzerinde olabilir.

## <a name="configure-alwaysusegateway-property"></a>AlwaysUseGateway özelliğini yapılandırma

1. SSMS > Server > **özellikler** > **genel**, **Gelişmiş (tümü) özelliklerini göster**' i seçin.
2. **Aspaas\alwaysusegateway**içinde **true**seçeneğini belirleyin.

    ![Her zaman ağ geçidi özelliğini kullan](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Ayrıca bkz.
Şirket [içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Şirket içi veri ağ geçidi @no__t yükleyip yapılandırma](analysis-services-gateway-install.md)-1  
[Azure sanal ağı (VNET)](../virtual-network/virtual-networks-overview.md)   

