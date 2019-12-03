---
title: Azure Lab Services bir eş ağa bağlan | Microsoft Docs
description: Laboratuvar ağınızı bir eş olarak başka bir ağla bağlamayı öğrenin. Örneğin, şirket içi okulunuzu/üniversite ağınızı Azure 'daki sanal ağ ile birlikte bağlayın.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: d3f6acef7491a07f94eec0b2c3b2f3bcd9c01a33
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74701679"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Laboratuvarınızın ağını Azure Lab Services içindeki bir eş sanal ağla bağlayın 
Bu makalede, Labs ağınızı başka bir ağla eşleme hakkında bilgi sağlanır. 

## <a name="overview"></a>Genel Bakış
Sanal ağ eşlemesi, Azure sanal ağlarına sorunsuz bir şekilde bağlanmanıza olanak sağlar. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla yönlendirilir ve aynı sanal ağdaki sanal makineler arasında çok benzer trafik, yalnızca özel IP adresleri üzerinden yönlendirilir. Daha fazla bilgi için bkz. [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Laboratuvar ağınızı, bazı senaryolarda aşağıdakiler de dahil olmak üzere bir eş sanal ağla bağlamanız gerekebilir:

- Laboratuvardaki sanal makinelerde lisans almak için şirket içi lisans sunucularına bağlanan yazılımlar vardır
- Laboratuvardaki sanal makinelerin, University 'in ağ paylaşımlarında veri kümelerine (veya diğer dosyalara) erişmesi gerekir. 

Belirli şirket içi ağlar, [ExpressRoute](../../expressroute/expressroute-introduction.md) veya [sanal ağ geçidi](../../vpn-gateway/vpn-gateway-about-vpngateways.md)aracılığıyla Azure sanal ağı 'na bağlanır. Bu hizmetlerin Azure Lab Services dışında ayarlanması gerekir. ExpressRoute kullanarak şirket içi bir ağı Azure 'a bağlama hakkında daha fazla bilgi edinmek için bkz. [ExpressRoute 'a genel bakış](../../expressroute/expressroute-introduction.md). Bir sanal ağ geçidini kullanarak şirket içi bağlantı için, ağ geçidinin, belirtilen sanal ağın ve laboratuar hesabının hepsi aynı bölgede olmalıdır.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında yapılandırma
Yeni laboratuvar hesabı oluşturma sırasında, **eş sanal ağ** açılan listesinde gösterilen mevcut bir sanal ağı seçebilirsiniz. Seçilen sanal ağ, laboratuvar hesabı altında oluşturulan laboratuvarlara bağlandı (eşlenmiş). Bu değişikliğin yapılması sonrasında oluşturulan laboratuvarlarda bulunan tüm sanal makinelerin eşlenen sanal ağdaki kaynaklara erişimi olur. 

![VNet 'ten eşe seçin](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Laboratuvar hesabı oluşturmaya yönelik ayrıntılı adım adım yönergeler için bkz. [Laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)


## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandırma
Laboratuvar hesabı oluşturma sırasında bir eş ağ ayarlamadıysanız, **Laboratuvar hesabı** sayfasının **Labs yapılandırma** sekmesinden aynı özellik etkinleştirilebilir. Bu ayarda yapılan değişiklik yalnızca değişiklikten sonra oluşturulan laboratuvarlara uygulanır. Görüntüde görebileceğiniz gibi, laboratuvar hesabındaki laboratuvarlar için **eş sanal ağı** etkinleştirebilir veya devre dışı bırakabilirsiniz. 

![Laboratuvar oluşturulduktan sonra VNet eşlemesini etkinleştirme veya devre dışı bırakma](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

**Eş sanal ağ** alanı için bir sanal ağ seçtiğinizde, laboratuvar **oluşturucusunun laboratuvar konumunu seçmesine izin ver** seçeneği devre dışı bırakılır. Bunun nedeni, laboratuvar hesabındaki laboratuvarların, eş sanal ağdaki kaynaklarla bağlantı kurmak için laboratuvar hesabıyla aynı bölgede olması gerekir. 

> [!IMPORTANT]
> Bu ayar değişikliği yalnızca değişiklik yapıldıktan sonra oluşturulan Labs için geçerlidir, mevcut laboratuvarlara uygulanmaz. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)

