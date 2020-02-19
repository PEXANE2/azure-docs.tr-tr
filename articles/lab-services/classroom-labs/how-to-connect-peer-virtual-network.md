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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 5e013011f81542aa279ba9276a6a1aac01eb9e41
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443217"
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
Yeni laboratuvar hesabı oluşturma sırasında, **Gelişmiş** sekmesinde **eş sanal ağ** açılan listesinde gösterilen mevcut bir sanal ağı seçebilirsiniz. Seçilen sanal ağ, laboratuvar hesabı altında oluşturulan laboratuvarlara bağlandı (eşlenmiş). Bu değişikliğin yapılması sonrasında oluşturulan laboratuvarlarda bulunan tüm sanal makinelerin eşlenen sanal ağdaki kaynaklara erişimi olur. 

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

- [Laboratuvar oluşturucusunun laboratuvar konumunu seçmesine izin ver](allow-lab-creator-pick-lab-location.md)
- [Laboratuvara paylaşılan görüntü Galerisi iliştirme](how-to-attach-detach-shared-image-gallery.md)
- [Laboratuvar sahibi olarak Kullanıcı ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)