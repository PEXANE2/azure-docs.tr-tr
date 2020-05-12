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
ms.date: 03/31/2020
ms.author: spelluru
ms.openlocfilehash: 8d8f2c747a4bc0ab2119c92e61188e3c57f2b212
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83118371"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Laboratuvarınızın ağını Azure Lab Services içindeki bir eş sanal ağla bağlayın

Bu makalede, Labs ağınızı başka bir ağla eşleme hakkında bilgi sağlanır.

## <a name="overview"></a>Genel Bakış

Sanal ağ eşlemesi, Azure sanal ağlarına sorunsuz bir şekilde bağlanmanıza olanak sağlar. Eşleme yapıldıktan sonra, bağlantı açısından sanal ağlar tek bir sanal ağ gibi görünür. Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla yönlendirilir ve aynı sanal ağdaki sanal makineler arasında çok benzer trafik, yalnızca özel IP adresleri üzerinden yönlendirilir. Daha fazla bilgi için bkz. [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Laboratuvar ağınızı, bazı senaryolarda aşağıdakiler de dahil olmak üzere bir eş sanal ağla bağlamanız gerekebilir:

- Laboratuvardaki sanal makinelerde lisans almak için şirket içi lisans sunucularına bağlanan yazılımlar vardır.
- Laboratuvardaki sanal makinelerin, University 'in ağ paylaşımlarında veri kümelerine (veya diğer dosyalara) erişmesi gerekir.

Belirli şirket içi ağlar, [ExpressRoute](../../expressroute/expressroute-introduction.md) veya [sanal ağ geçidi](../../vpn-gateway/vpn-gateway-about-vpngateways.md)aracılığıyla Azure sanal ağı 'na bağlanır. Bu hizmetlerin Azure Lab Services dışında ayarlanması gerekir. ExpressRoute kullanarak şirket içi bir ağı Azure 'a bağlama hakkında daha fazla bilgi edinmek için bkz. [ExpressRoute 'a genel bakış](../../expressroute/expressroute-introduction.md). Bir sanal ağ geçidini kullanarak şirket içi bağlantı için, ağ geçidinin, belirtilen sanal ağın ve laboratuar hesabının hepsi aynı bölgede olmalıdır.

> [!NOTE]
> Laboratuvar hesabıyla eşlenmeyecek bir Azure sanal ağı oluştururken, sanal ağın bölgesinin, derslik laboratuvarlarının nerede oluşturulduğunu anlamak önemlidir.  Daha fazla bilgi için, [regions\locations](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)içindeki Yönetici Kılavuzu bölümüne bakın.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında yapılandırma

Yeni [Laboratuvar hesabı oluşturma](tutorial-setup-lab-account.md)sırasında, **Gelişmiş** sekmesinde **eş sanal ağ** açılan listesinde gösterilen mevcut bir sanal ağı seçebilirsiniz.  Bu liste yalnızca laboratuvar hesabıyla aynı bölgedeki sanal ağları gösterir. Seçilen sanal ağ, laboratuvar hesabı altında oluşturulan laboratuvarlara bağlandı (eşlenmiş).  Bu değişikliğin yapılması sonrasında oluşturulan laboratuvarlarda bulunan tüm sanal makinelerin eşlenen sanal ağdaki kaynaklara erişimi olur.

![VNet 'ten eşe seçin](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

### <a name="address-range"></a>Adres aralığı

Ayrıca, laboratuvarlara yönelik sanal makineler için **adres aralığı** sağlama seçeneği de vardır.  **Adres aralığı** özelliği yalnızca laboratuvar için **eş sanal ağ** etkinse geçerlidir.  Adres aralığı sağlanmışsa, laboratuvar hesabı kapsamındaki laboratuvarlarda bulunan tüm sanal makineler bu adres aralığında oluşturulur. Adres aralığı CıDR gösteriminde (ör. 10.20.0.0/20) olmalıdır ve mevcut adres aralıklarıyla çakışmaz.  Bir adres aralığı sağlarken, oluşturulacak *laboratuvarların* sayısını göz önünde bulundurmamak ve buna uyum sağlamak için bir adres aralığı sağlamanız gerekir. Laboratuvar Hizmetleri, laboratuvar başına en fazla 512 sanal makine olduğunu varsayar.  Örneğin, '/23 ' olan bir IP aralığı yalnızca bir laboratuvar oluşturabilir.  '/21 ' olan bir Aralık dört laboratuvarın oluşturulmasına olanak sağlar.

**Adres aralığı** belirtilmemişse, Laboratuvar Hizmetleri sanal ağınızla ilişkilendirilen sanal ağı oluştururken Azure tarafından verilen varsayılan adres aralığını kullanır.  Aralık genellikle 10. x. 0.0/16 gibi bir şeydir.  Bu, IP aralığı çakışmasına neden olabilir, bu nedenle laboratuvar ayarlarında bir adres aralığı belirttiğinizden emin olun veya sanal ağınızın eşlenmekte olduğu adres aralığını kontrol edin.

## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandırma

Laboratuvar hesabı oluşturma sırasında bir eş ağ ayarlamadıysanız, **Laboratuvar hesabı** sayfasının **Labs yapılandırma** sekmesinden aynı özellik etkinleştirilebilir. Bu ayarda yapılan değişiklik yalnızca değişiklikten sonra oluşturulan laboratuvarlara uygulanır. Görüntüde görebileceğiniz gibi, laboratuvar hesabındaki laboratuvarlar için **eş sanal ağı** etkinleştirebilir veya devre dışı bırakabilirsiniz.

![Laboratuvar oluşturulduktan sonra VNet eşlemesini etkinleştirme veya devre dışı bırakma](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png)

**Eş sanal ağ** alanı için bir sanal ağ seçtiğinizde, laboratuvar **oluşturucusunun laboratuvar konumunu seçmesine izin ver** seçeneği devre dışı bırakılır. Bunun nedeni, laboratuvar hesabındaki laboratuvarların, eş sanal ağdaki kaynaklarla bağlantı kurmak için laboratuvar hesabıyla aynı bölgede olması gerekir.

> [!IMPORTANT]
> Eşlenen sanal ağ ayarı yalnızca değişiklik yapıldıktan sonra oluşturulan Labs için geçerlidir, mevcut laboratuvarlara uygulanmaz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Laboratuvara paylaşılan görüntü Galerisi iliştirme](how-to-attach-detach-shared-image-gallery.md)
- [Laboratuvar sahibi olarak Kullanıcı ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)
