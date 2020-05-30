---
title: Azure Uygulama yapılandırması için özel uç noktaları kullanma
description: Özel uç noktaları kullanarak uygulama yapılandırma deponuzda güvenli hale getirin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 3/12/2020
ms.author: lcozzens
ms.openlocfilehash: 8f39c9cf159f8ce5068cf10460ba6f195baa7806
ms.sourcegitcommit: 0fa52a34a6274dc872832560cd690be58ae3d0ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84205067"
---
# <a name="using-private-endpoints-for-azure-app-configuration"></a>Azure Uygulama yapılandırması için özel uç noktaları kullanma

Bir sanal ağdaki (VNet) istemcilerin [özel bir bağlantı](../private-link/private-link-overview.md)üzerinden güvenli bir şekilde verilere erişmesini sağlamak Için Azure Uygulama yapılandırması için [Özel uç noktaları](../private-link/private-endpoint-overview.md) kullanabilirsiniz. Özel uç nokta, uygulama yapılandırma deponuza ait VNet adres alanından bir IP adresi kullanır. VNet ve uygulama yapılandırma deposundaki istemciler arasındaki ağ trafiği, Microsoft omurga ağındaki özel bir bağlantı kullanılarak VNet 'in üzerinde geçiş yaparken, genel İnternet üzerinden etkilenme olasılığını ortadan kaldırır.

Uygulama yapılandırma deponuzda özel uç noktalar kullanmak şunları yapmanızı sağlar:
- Güvenlik duvarını genel uç noktasındaki uygulama yapılandırmasına yönelik tüm bağlantıları engelleyecek şekilde yapılandırarak uygulama yapılandırma ayrıntılarınızı güvenli hale getirin.
- Sanal ağ (VNet) için güvenliği artırın ve verilerin VNet 'ten çıkış olmamasını sağlar.
- [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-locations.md) Ile özel eşleme ile sanal ağa bağlanan şirket içi ağlardan uygulama yapılandırma deposuna güvenli bir şekilde bağlanın.

> [!NOTE]
> Azure Uygulama yapılandırması, Özel uç noktaların genel önizleme olarak kullanılmasını sağlar. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel uç nokta, [sanal ağınızdaki](../virtual-network/virtual-networks-overview.md) (VNet) bir Azure hizmeti için özel bir ağ arabirimidir. Uygulama yapılandırma depniz için özel bir uç nokta oluşturduğunuzda, VNet 'iniz ve yapılandırma deponuzda istemciler arasında güvenli bağlantı sağlar. Özel uç noktaya sanal Ağınızın IP adresi aralığından bir IP adresi atanır. Özel uç nokta ve yapılandırma deposu arasındaki bağlantı güvenli bir özel bağlantı kullanır.

VNet 'teki uygulamalar, **aynı bağlantı dizelerini ve diğer kimlik doğrulama mekanizmalarını kullanarak**özel uç nokta üzerinden yapılandırma deposuna bağlanabilir. Özel uç noktalar, uygulama yapılandırma deposu tarafından desteklenen tüm protokollerle kullanılabilir.

Uygulama yapılandırması hizmet uç noktalarını desteklemediğinden, [hizmet uç](../virtual-network/virtual-network-service-endpoints-overview.md)noktaları kullanan alt ağlarda özel uç noktalar oluşturulabilir. Bir alt ağdaki istemciler, diğer kullanıcılara erişmek için hizmet uç noktalarını kullanırken özel uç nokta kullanarak bir uygulama yapılandırma deposuna güvenli bir şekilde bağlanabilir.  

VNet 'iniz içindeki bir hizmet için özel bir uç nokta oluşturduğunuzda, hizmet hesabı sahibine onay için bir izin isteği gönderilir. Özel uç noktanın oluşturulmasını isteyen kullanıcı aynı zamanda hesabın sahibiyseniz, bu onay isteği otomatik olarak onaylanır.

Hizmet hesabı sahipleri, `Private Endpoints` [Azure Portal](https://portal.azure.com)yapılandırma deposunun sekmesinden izin isteklerini ve özel uç noktaları yönetebilir.

### <a name="private-endpoints-for-app-configuration"></a>Uygulama yapılandırması için özel uç noktalar 

Özel bir uç nokta oluştururken, bağlandığı uygulama yapılandırma deposunu belirtmeniz gerekir. Bir hesapta birden çok uygulama yapılandırma örneğine sahipseniz, her mağaza için ayrı bir özel uç nokta gerekir.

### <a name="connecting-to-private-endpoints"></a>Özel uç noktalara bağlanma

Azure, VNet 'ten gelen bağlantıları özel bir bağlantı üzerinden yapılandırma deposuna yönlendirmek için DNS çözümlemesine bağımlıdır. Azure Portal bağlantı dizelerini, uygulama yapılandırma deponuzu seçerek ve ardından **Ayarlar**  >  **erişim anahtarları**' nı seçerek hızlı bir şekilde bulabilirsiniz.  

> [!IMPORTANT]
> Ortak bir uç nokta için kullandığınız gibi özel uç noktaları kullanarak uygulama yapılandırma deponuza bağlanmak için aynı bağlantı dizesini kullanın. Alt `privatelink` etki alanı URL 'sini kullanarak mağazaya bağlanmayın.

## <a name="dns-changes-for-private-endpoints"></a>Özel uç noktalar için DNS değişiklikleri

Özel bir uç nokta oluşturduğunuzda, yapılandırma deposu için DNS CNAME kaynak kaydı, ön ek içeren bir alt etki alanındaki diğer ada güncelleştirilir `privatelink` . Azure, [private DNS zone](../dns/private-dns-overview.md) `privatelink` Özel uç noktalar için DNS a kaynak kayıtlarıyla birlikte alt etki alanına karşılık gelen özel bir DNS bölgesi de oluşturur.

Uç nokta URL 'sini özel uç noktasını barındıran VNet içinden çözdüğünde, deponun özel uç noktasına çözümlenir. VNet dışından çözümlendiğinde, uç nokta URL 'SI genel uç noktaya dönüşür. Özel bir uç nokta oluşturduğunuzda, genel uç nokta devre dışıdır.

Ağınızda özel bir DNS sunucusu kullanıyorsanız, istemciler hizmet uç noktasının tam etki alanı adını (FQDN) özel uç nokta IP adresine çözümleyebilmelidir. DNS sunucunuzu özel bağlantı alt etki alanınızı, sanal ağın özel DNS bölgesine devretmek veya `AppConfigInstanceA.privatelink.azconfig.io` Özel uç nokta IP adresi ile Için bir kayıt yapılandırmak üzere yapılandırın.

> [!TIP]
> Özel veya şirket içi bir DNS sunucusu kullanırken, DNS sunucunuzu, alt etki alanındaki depo adını `privatelink` Özel uç nokta IP adresine çözümlemek üzere yapılandırmanız gerekir. Bunu, alt `privatelink` etki alanını VNET 'in özel DNS bölgesine devredererek veya DNS SUNUCUNUZDA DNS bölgesini yapılandırarak ve DNS A kayıtlarını ekleyerek yapabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

Özel uç noktaların etkinleştirilmesi için [Standart katman](https://azure.microsoft.com/pricing/details/app-configuration/) uygulama yapılandırma deposu gerekir.  Özel bağlantı fiyatlandırma ayrıntıları hakkında bilgi edinmek için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama yapılandırma depolu özel bir uç nokta oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portal özel bağlantı merkezini kullanarak özel bir uç nokta oluşturma](../private-link/create-private-endpoint-portal.md)
- [Azure CLı kullanarak özel uç nokta oluşturma](../private-link/create-private-endpoint-cli.md)
- [Azure PowerShell kullanarak özel uç nokta oluşturma](../private-link/create-private-endpoint-powershell.md)

DNS sunucunuzu özel uç noktalarla yapılandırmayı öğrenin:

- [Azure sanal ağlarındaki kaynaklar için ad çözümlemesi](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [Özel uç noktalar için DNS yapılandırması](/azure/private-link/private-endpoint-overview#dns-configuration)
