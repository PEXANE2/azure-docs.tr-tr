---
title: Azure Uygulama Yapılandırması için özel uç noktaları kullanma
description: Özel uç noktaları kullanarak Uygulama Yapılandırma mağazanızı güvenli hale
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 3/12/2020
ms.author: lcozzens
ms.openlocfilehash: f18672b9e3a368a833fc8cba279d748dfe3c2a9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366777"
---
# <a name="using-private-endpoints-for-azure-app-configuration"></a>Azure Uygulama Yapılandırması için özel uç noktaları kullanma

Sanal ağdaki (VNet) [istemcilerin](../private-link/private-endpoint-overview.md) özel bir bağlantı üzerinden verilere güvenli bir şekilde erişmesine olanak sağlamak için Azure Uygulama Yapılandırması için [özel](../private-link/private-link-overview.md)uç noktaları kullanabilirsiniz. Özel bitiş noktası, Uygulama Yapılandırma mağazanız için VNet adres alanından bir IP adresi kullanır. VNet ve App Configuration mağazasındaki istemciler arasındaki ağ trafiği, Microsoft omurga ağındaki özel bir bağlantıyı kullanarak VNet üzerinden geçerek genel internete maruz kalmamayı ortadan kaldırır.

App Configuration mağazanız için özel uç noktaları kullanmak şunları yapmanızı sağlar:
- Güvenlik duvarını, genel bitiş noktasındaKi App Configuration'a olan tüm bağlantıları engelleyecek şekilde yapılandırarak uygulama yapılandırma ayrıntılarınızı güvence altına alın.
- Verilerin VNet'ten kaçmasını sağlayan sanal ağ (VNet) için güvenliği artırın.
- Özel bakışlı [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoutes](../expressroute/expressroute-locations.md) kullanarak VNet'e bağlanan şirket içi ağlardan Uygulama Yapılandırma deposuna güvenli bir şekilde bağlanın.

> [!NOTE]
> Azure Uygulama Yapılandırması, genel önizleme olarak özel uç noktaların kullanımını sunar. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel bitiş noktası, [Sanal Ağınızda](../virtual-network/virtual-networks-overview.md) (VNet) bir Azure hizmeti için özel bir ağ arabirimidir. App Config mağazanız için özel bir bitiş noktası oluşturduğunuzda, VNet'inizdeki istemciler ile yapılandırma mağazanız arasında güvenli bağlantı sağlar. Özel bitiş noktasına VNet'inizin IP adresi aralığından bir IP adresi atanır. Özel bitiş noktası ile yapılandırma deposu arasındaki bağlantı güvenli bir özel bağlantı kullanır.

VNet'teki uygulamalar, **aksi takdirde kullanacakları aynı bağlantı dizelerini ve yetkilendirme mekanizmalarını kullanarak**özel bitiş noktası üzerinden yapılandırma deposuna bağlanabilir. Özel uç noktalar, App Configuration mağazası tarafından desteklenen tüm protokollerle kullanılabilir.

Uygulama Yapılandırması hizmet bitiş noktalarını desteklemese de, [Hizmet Bitiş Noktaları'nı](../virtual-network/virtual-network-service-endpoints-overview.md)kullanan alt ağlarda özel uç noktalar oluşturulabilir. Alt ağdaki istemciler, başkalarına erişmek için hizmet bitiş noktalarını kullanırken özel bitiş noktasını kullanarak bir Uygulama Yapılandırma mağazasına güvenli bir şekilde bağlanabilir.  

VNet'inizde bir hizmet için özel bir bitiş noktası oluşturduğunuzda, hizmet hesabı sahibine onay için bir onay isteği gönderilir. Özel bitiş noktasının oluşturulmasını isteyen kullanıcı da hesabın sahibiyse, bu onay isteği otomatik olarak onaylanır.

Hizmet hesabı sahipleri, Azure `Private Endpoints` [portalındaki](https://portal.azure.com)config mağazasının sekmesinden onay isteklerini ve özel bitiş noktalarını yönetebilir.

### <a name="private-endpoints-for-app-configuration"></a>Uygulama Yapılandırması için özel uç noktalar 

Özel bir bitiş noktası oluştururken, bağlandığı Uygulama Yapılandırma deposunu belirtmeniz gerekir. Bir hesapta birden çok Uygulama Yapılandırma örneği varsa, her mağaza için ayrı bir özel bitiş noktası gerekir.

### <a name="connecting-to-private-endpoints"></a>Özel uç noktalara bağlanma

Azure, bağlantıları VNet'ten yapılandırma deposuna özel bir bağlantı üzerinden yönlendirmek için DNS çözünürlüğüne dayanır. Uygulama Yapılandırma mağazanızı seçip **Ayarlar** > **Erişim Anahtarları'nı**seçerek Azure portalındaki bağlantı dizelerini hızla bulabilirsiniz.  

> [!IMPORTANT]
> Ortak bir bitiş noktası için kullanacağınız özel uç noktaları kullanarak Uygulama Yapılandırma mağazanıza bağlanmak için aynı bağlantı dizesini kullanın. Alt etki alanı URL'sini `privatelink` kullanarak depolama hesabına bağlanma.

## <a name="dns-changes-for-private-endpoints"></a>Özel uç noktalar için DNS değişiklikleri

Özel bir bitiş noktası oluşturduğunuzda, yapılandırma deposuiçin DNS CNAME kaynak kaydı önek `privatelink`ile bir alt etki alanında bir diğer ad için güncelleştirilir. Azure ayrıca `privatelink` alt etki alanına karşılık gelen özel bir [DNS bölgesi](../dns/private-dns-overview.md) oluşturur ve özel uç noktalar için DNS A kaynak kayıtları kaydeder.

Bitiş noktası URL'sini VNet'in dışından çözdüğünüzde, bu URL mağazanın genel bitiş noktasına kadar çözülür. Özel bitiş noktasını barındıran VNet içinden çözüldüğünde, bitiş noktası URL'si özel bitiş noktasına kadar çözülür.

Azure Güvenlik Duvarı hizmetini kullanarak, VNet dışındaki istemcilerin genel bitiş noktası üzerinden erişimi denetleyebilirsiniz.

Bu yaklaşım, Özel uç noktaları barındıran VNet'teki istemciler ve VNet dışındaki istemciler için **aynı bağlantı dizesini kullanarak** mağazaya erişim sağlar.

Ağınızda özel bir DNS sunucusu kullanıyorsanız, istemcilerin hizmet bitiş noktası için özel bitiş noktası IP adresine tam nitelikli alan adı (FQDN) çözebilmesi gerekir. Özel bağlantı alt etki alanınızı VNet için özel DNS bölgesine devretmek için DNS `AppConfigInstanceA.privatelink.azconfig.io` sunucunuzu yapılandırın veya A kayıtlarını özel bitiş noktası IP adresiyle yapılandırın.

> [!TIP]
> Özel veya şirket içi bir DNS sunucusu kullanırken, `privatelink` alt etki alanında bulunan mağaza adını özel bitiş noktası IP adresine göre çözümlemek için DNS sunucunuzu yapılandırmanız gerekir. Bunu, `privatelink` alt etki alanını VNet'in özel DNS bölgesine atayarak veya DNS sunucunuzdaki DNS bölgesini yapılandırarak ve DNS A kayıtlarını ekleyerek yapabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

Özel uç noktaları etkinleştirmek için [Standart katman](https://azure.microsoft.com/pricing/details/app-configuration/) Uygulama Yapılandırma deposu gerektirir.  Özel bağlantı fiyatlandırma ayrıntıları hakkında bilgi edinmek için [Azure Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Yapılandırma mağazanız için özel bir bitiş noktası oluşturma hakkında daha fazla bilgi edinin, aşağıdaki makalelere bakın:

- [Azure portalındaki Özel Bağlantı Merkezi'ni kullanarak özel bir bitiş noktası oluşturma](../private-link/create-private-endpoint-portal.md)
- [Azure CLI'yi kullanarak özel bir bitiş noktası oluşturma](../private-link/create-private-endpoint-cli.md)
- [Azure PowerShell'i kullanarak özel bir bitiş noktası oluşturma](../private-link/create-private-endpoint-powershell.md)

DNS sunucunuzu özel uç noktalarla yapılandırmayı öğrenin:

- [Azure sanal ağlarındaki kaynaklar için ad çözümlemesi](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [Özel Uç Noktalar için DNS yapılandırması](/azure/private-link/private-endpoint-overview#dns-configuration)
