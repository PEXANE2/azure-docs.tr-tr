---
title: Sanal Ağ hizmeti bitiş noktasını kullanarak Azure Cosmos DB hesaplarına erişimi güvenli hale
description: Bu belgede, bir Azure Cosmos hesabı için sanal ağ ve alt ağ erişim denetimi hakkında açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: c1c5bdd1d210a1933699cad52dbf123b50048e01
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421327"
---
# <a name="access-azure-cosmos-db-from-virtual-networks-vnet"></a>Sanal ağlardan (VNet) Azure Cosmos DB'ye erişme

Azure Cosmos hesabını yalnızca belirli bir sanal ağ alt ağından (VNet) erişime izin verecek şekilde yapılandırabilirsiniz. [Hizmet bitiş noktasının](../virtual-network/virtual-network-service-endpoints-overview.md) sanal ağ daki alt ağdaki Azure Cosmos DB'ye erişmesini sağlayarak, bu alt ağdan gelen trafik alt ağ ve Sanal Ağ kimliğiyle Azure Cosmos DB'ye gönderilir. Azure Cosmos DB hizmet bitiş noktası etkinleştirildikten sonra, azure cosmos hesabınıza ekleyerek alt ağa erişimi sınırlandırabilirsiniz.

Varsayılan olarak, isteğe geçerli bir yetkilendirme belirteci eşlik ederse, azure Cosmos hesabına herhangi bir kaynaktan erişilebilir. VNets'e bir veya daha fazla alt ağ eklediğinizde, yalnızca bu alt ağlardan kaynaklanan istekler geçerli bir yanıt alır. Başka bir kaynaktan gelen istekler 403 (Yasak) yanıt alır. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sanal ağlardan erişimi yapılandırma klasmanında sık sorulan bazı sorular şunlardır:

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>Azure Cosmos hesabında hem sanal ağ hizmeti bitiş noktası hem de IP erişim denetim ilkesini belirtebilir miyim? 

Azure Cosmos hesabınızda hem sanal ağ hizmeti bitiş noktasını hem de IP erişim denetim ilkesini (diğer adıyla güvenlik duvarı) etkinleştirebilirsiniz. Bu iki özellik tamamlayıcıdır ve Azure Cosmos hesabınızın tümcesi ve güvenliğini topluca sağlar. IP güvenlik duvarının kullanılması, statik IP'lerin hesabınıza erişmesini sağlar. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>Sanal ağdaki alt ağa erişimi nasıl sınırlandırım? 

Azure Cosmos hesabına erişimi bir alt ağdan sınırlamak için iki adım gerekir. İlk olarak, alt ağdan gelen trafiğin alt ağ ve sanal ağ kimliğini Azure Cosmos DB'ye taşımasına izin verirsiniz. Bu işlem, Azure Cosmos DB için hizmet bitiş noktasını alt ağda etkinleştirerek yapılır. Sonraki, Azure Cosmos hesabında bu alt ağı hangi hesaba erişilebilen bir kaynak olarak belirten bir kural eklemektir.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>Sanal ağ ALA'ları ve IP Güvenlik Duvarı istekleri veya bağlantıları reddeder mi? 

IP güvenlik duvarı veya sanal ağ erişim kuralları eklendiğinde, yalnızca izin verilen kaynaklardan gelen istekler geçerli yanıtlar alır. Diğer istekler 403 (Yasak) ile reddedilir. Azure Cosmos hesabının güvenlik duvarLarını bağlantı düzeyindeki güvenlik duvarından ayırt etmek önemlidir. Kaynak yine de hizmete bağlanabilir ve bağlantıların kendileri reddedilmez.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>Alt ağdaki Azure Cosmos DB'nin hizmet bitiş noktasını etkinleştirdiğimde isteklerim engellenmeye başladı. Ne oldu?

Azure Cosmos DB'nin hizmet bitiş noktası bir alt ağda etkinleştirildiğinde, hesaba ulaşan trafiğin kaynağı ortak IP'den sanal ağa ve alt ağa geçer. Azure Cosmos hesabınızda yalnızca IP tabanlı güvenlik duvarı varsa, hizmet etkin alt ağdan gelen trafik artık IP güvenlik duvarı kurallarıyla eşleşmez ve bu nedenle reddedilir. IP tabanlı güvenlik duvarından sanal ağ tabanlı erişim denetimine sorunsuz bir şekilde geçiş yapmak için adımların üzerinden geçin.

### <a name="are-additional-rbac-permissions-needed-for-azure-cosmos-accounts-with-vnet-service-endpoints"></a>VNET hizmet bitiş noktalarına sahip Azure Cosmos hesapları için ek RBAC izinleri gerekli midir?

Hesap ayarlarında herhangi bir değişiklik yapmak için Bir Azure Cosmos hesabına VNet hizmet `Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action` bitiş noktalarını ekledikten sonra, Azure Cosmos hesabınızda yapılandırılan tüm VNET'ler için eyleme erişmeniz gerekir. Yetkilendirme işlemi, herhangi bir özelliği değerlendirmeden önce kaynaklara (veritabanı ve sanal ağ kaynakları gibi) erişimi doğruladığı için bu izin gereklidir.
 
Yetkilendirme, kullanıcı Azure CLI kullanarak VNET ALA'larını belirtmese bile VNet kaynak eylemi için izni doğrular. Şu anda Azure Cosmos hesabının denetim düzlemi Azure Cosmos hesabının tam durumunu ayarlamayı destekler. Kontrol uçağı aramalarının parametrelerinden `virtualNetworkRules`biri . Bu parametre belirtilmemişse, Azure CLI, güncelleştirme çağrısında `virtualNetworkRules` bu değeri almak için bir veritabanı çağrısı yapar ve bu değeri kullanır.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>Bakan sanal ağların Azure Cosmos hesabına erişimi de var mı? 
Yalnızca sanal ağ ve Azure Cosmos hesabına eklenen alt ağları erişebilir. Eşlenen sanal ağlardaki alt ağlar hesaba eklenene kadar, eşlenen VNet'leri hesaba erişemez.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>Tek bir Cosmos hesabına erişmesine izin verilen maksimum alt ağ sayısı nedir? 
Şu anda, bir Azure Cosmos hesabı için en fazla 256 alt ağ izni niz olabilir.

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>VPN ve Express Route'dan erişimi etkinleştirebilir miyim? 
Azure Cosmos hesabına şirket içi Express rotası üzerinden erişmek için Microsoft'un karşıdan erişimini etkinleştirmeniz gerekir. IP güvenlik duvarı veya sanal ağ erişim kurallarını koyduktan sonra, şirket içi hizmetlerin Azure Cosmos hesabına erişmesine izin vermek için Azure Cosmos hesabıip güvenlik duvarınızda Microsoft'a bakan Microsoft için kullanılan genel IP adreslerini ekleyebilirsiniz. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>Ağ Güvenlik Grupları (NSG) kurallarını güncelleştirmem gerekiyor mu? 
NSG kuralları, sanal ağa sahip bir alt ağa bağlantı sağlamak için kullanılır. Alt ağa Azure Cosmos DB için hizmet bitiş noktası eklediğinizde, Azure Cosmos hesabınız için NSG'de giden bağlantı açmaya gerek yoktur. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>Tüm VNet'ler için hizmet bitiş noktaları kullanılabilir mi?
Hayır, yalnızca Azure Kaynak Yöneticisi sanal ağları hizmet bitiş noktası etkin olabilir. Klasik sanal ağlar hizmet bitiş noktalarını desteklemez.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Azure Cosmos DB için hizmet bitiş noktası erişimi etkinleştirildiğinde "Genel Azure veri merkezlerinden bağlantıları kabul edebilir miyim?"  
Bu, yalnızca Azure Cosmos DB hesabınıza Azure Veri fabrikası, Azure Bilişsel Arama veya belirli Azure bölgesinde dağıtılan herhangi bir hizmet gibi diğer Azure birinci taraf hizmetleri tarafından erişibilmesini istediğinizde gereklidir.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos hesap erişimini sanal ağlardaki alt ağlara nasıl sınırlandırabilirsiniz?](how-to-configure-vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP güvenlik duvarı nasıl yapılandırılabilen](how-to-configure-firewall.md)

