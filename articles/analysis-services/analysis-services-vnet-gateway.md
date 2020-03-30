---
title: VNet veri kaynakları için Azure Analiz Hizmetlerini yapılandırma | Microsoft Dokümanlar
description: Azure Sanal Ağı'ndaki (VNet) veri kaynakları için ağ geçidi kullanacak şekilde bir Azure Analiz Hizmetleri sunucusunun nasıl yapılandırılabildiğini öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 384f4c2ee3021678c530aa5c98eaf4dbfd661e95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572273"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Azure Sanal Ağı'ndaki (VNet) veri kaynakları için ağ geçidini kullanma

Bu makalede, veri kaynakları bir [Azure Sanal Ağ (VNet)](../virtual-network/virtual-networks-overview.md)üzerinde olduğunda kullanılmak üzere Azure Çözümleme Hizmetleri **AlwaysUseGateway** sunucu özelliği açıklanır.

## <a name="server-access-to-vnet-data-sources"></a>VNet veri kaynaklarına sunucu erişimi

Veri kaynaklarına bir VNet üzerinden erişiliyorsa, Azure Analiz Hizmetleri sunucunuz bu veri kaynaklarına kendi ortamınızda şirket içindeymiş gibi bağlanmalıdır. **AlwaysUseGateway** sunucu özelliğini, sunucunun tüm veri kaynaklarına [şirket içi ağ geçidi](analysis-services-gateway.md)üzerinden erişecek şekilde belirtecek şekilde yapılandırabilirsiniz. 

Azure SQL Veritabanı Yönetilen Örnek veri kaynakları Azure VNet içinde özel bir IP adresiyle çalışır. Örnekte ortak bitiş noktası etkinse, bir ağ geçidi gerekmez. Ortak bitiş noktası etkinleştirilmezse, şirket içi Veri Ağ Geçidi gereklidir ve AlwaysUseGateway özelliğinin doğru olarak ayarlanabilmesi gerekir.

> [!NOTE]
> Bu özellik, yalnızca [şirket içi Bir Veri Ağ Geçidi](analysis-services-gateway.md) yüklendiğinde ve yapılandırıldığında etkilidir. Ağ geçidi VNet'te olabilir.

## <a name="configure-alwaysusegateway-property"></a>AlwaysUseGateway özelliğini yapılandır

1. SSMS > sunucu > **Properties** > **General'de** **Gelişmiş (Tümü) Özelliklerini Göster'i**seçin.
2. **ASPaaS\AlwaysUseGateway'de** **, doğru**yu seçin.

    ![Her zaman ağ geçidi özelliğini kullanın](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Ayrıca bkz.
[Şirket içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Şirket içi veri ağ geçidini yükleme ve yapılandırma](analysis-services-gateway-install.md)   
[Azure Sanal Ağ (VNET)](../virtual-network/virtual-networks-overview.md)   

