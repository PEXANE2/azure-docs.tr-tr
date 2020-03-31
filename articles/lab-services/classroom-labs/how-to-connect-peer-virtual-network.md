---
title: Azure Laboratuvar Hizmetleri'nde bir eş ağına bağlanma | Microsoft Dokümanlar
description: Laboratuvar ağınızı eş olarak başka bir ağa nasıl bağlayabilirsiniz öğrenin. Örneğin, şirket içi okul/üniversite ağınızı Azure'daki Lab'ın sanal ağına bağlayın.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: d2115b1dc7e9f3150e44eb5ee9417e88ebeaa279
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370842"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde laboratuvar ağınızı bir eş sanal ağıyla bağlayın 
Bu makalede, başka bir ağ ile laboratuvarağı izleme hakkında bilgi sağlar. 

## <a name="overview"></a>Genel Bakış
Sanal ağ eşleme, Azure sanal ağlarına sorunsuz bir şekilde bağlanmanızı sağlar. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, aynı sanal ağdaki sanal makineler arasında yalnızca özel IP adresleri üzerinden yönlendirilmiş gibi Microsoft omurga altyapısı üzerinden yönlendirilir. Daha fazla bilgi için [Sanal ağ eşleme'ye](../../virtual-network/virtual-network-peering-overview.md)bakın.

Aşağıdakiler de dahil olmak üzere bazı senaryolarda laboratuvarınızın ağını bir eş sanal ağa bağlamanız gerekebilir:

- Laboratuvardaki sanal makineler, lisans almak için şirket içi lisans sunucularına bağlanan yazılımlara sahiptir
- Laboratuvardaki sanal makinelerin üniversitenin ağ paylaşımlarında veri kümelerine (veya diğer dosyalara) erişmeleri gerekir. 

Bazı şirket içi ağlar [ExpressRoute](../../expressroute/expressroute-introduction.md) veya [Virtual Network Ağ Ağ Geçidi](../../vpn-gateway/vpn-gateway-about-vpngateways.md)aracılığıyla Azure Sanal Ağı'na bağlanır. Bu hizmetlerin Azure Laboratuvar Hizmetleri dışında ayarlanması gerekir. ExpressRoute kullanarak şirket içi ağı Azure'a bağlama hakkında daha fazla bilgi edinmek için [ExpressRoute'a genel bakış](../../expressroute/expressroute-introduction.md)bilgisini görün. Sanal Ağ Ağ Ağ Geçidi, ağ geçidi, belirtilen sanal ağ ve laboratuvar hesabı kullanılarak şirket içi bağlantı için tüm bunlar aynı bölgede olmalıdır.

> [!NOTE]
> Bir laboratuvar hesabıyla bakılması gereken bir Azure Sanal Ağı oluştururken, sanal ağın bölgesinin sınıf laboratuvarlarının oluşturulduğu yeri nasıl etkilediğini anlamak önemlidir.  Daha fazla bilgi için, [bölgeler\konumları](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regions-or-locations)ile ilgili yönetici kılavuzubölümüne bakın.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında yapılandırma
Yeni laboratuvar hesabı oluşturma sırasında, **Gelişmiş** sekmesinde Eş **sanal ağ** açılır listesinde gösteren varolan bir sanal ağ seçebilirsiniz. Seçili sanal ağ, laboratuvar hesabı altında oluşturulan laboratuvarlara bağlanır (eşli) olur. Bu değişikliği yaptıktan sonra oluşturulan laboratuarlarda oluşturulan tüm sanal makineler, bakan sanal ağdaki kaynaklara erişebilir. 

![Eşler için VNet'i seçin](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Laboratuvar hesabı oluşturmak için ayrıntılı adım adım yönergeler [için](tutorial-setup-lab-account.md) bkz.


## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandır
Laboratuvar hesabı oluşturma sırasında bir eş ağı kurmadıysanız, aynı özellik **Laboratuvar Hesabı** sayfasının **Labs yapılandırma** sekmesinden etkinleştirilebilir. Bu ayarda yapılan değişiklik yalnızca değişiklikten sonra oluşturulan laboratuarlar için geçerlidir. Resimde de görebileceğiniz gibi, laboratuvar hesabındaki laboratuvarlar için **Eş sanal ağını** etkinleştirebilir veya devre dışı kullanabilirsiniz. 

![Laboratuvar oluşturulduktan sonra VNet eşlemesini etkinleştirme veya devre dışı etme](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

**Eşler sanal ağ** alanı için bir sanal ağ seçtiğinizde, **laboratuvar oluşturucusu laboratuvar konumu** seçeneğini seçmek için izin verir devre dışı bırakılır. Bunun nedeni, laboratuvar hesabındaki laboratuvarların eş sanal ağdaki kaynaklarla bağlantı kurabilmeleri için laboratuvar hesabıyla aynı bölgede olması gerektiğidir. 

> [!IMPORTANT]
> Bu ayar değişikliği yalnızca değişiklik yapıldıktan sonra oluşturulan laboratuarlar için geçerlidir, varolan laboratuarlar için değil. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Paylaşılan bir resim galerisini laboratuvara ekleme](how-to-attach-detach-shared-image-gallery.md)
- [Kullanıcıyı laboratuvar sahibi olarak ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)