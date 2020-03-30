---
title: Güvenlik duvarı kuralları - MariaDB için Azure Veritabanı
description: MariaDB sunucusu için Azure Veritabanınıza bağlantı sağlamak için güvenlik duvarı kurallarını kullanma hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 743e3f50d747993250399493d97fc2becab19319
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532051"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>MariaDB sunucu güvenlik duvarı kuralları için Azure Veritabanı
Güvenlik duvarları, siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her isteğin menşeli IP adresine bağlı olarak sunucuya erişim sağlar.

Güvenlik duvarını yapılandırmak için, kabul edilebilir IP adresleriaralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, istemcilerin MariaDB sunucusu için tüm Azure Veritabanınıza, yani aynı mantıksal sunucudaki tüm veritabanlarına erişmesini sağlar. Sunucu düzeyinde güvenlik duvarı kuralları, Azure portalı veya Azure CLI komutları kullanılarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katılımcısı olmalısınız.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
MariaDB sunucusu için Azure Veritabanınıza tüm veritabanı erişimi varsayılan olarak güvenlik duvarı tarafından engellenir. Sunucunuzu başka bir bilgisayardan kullanmaya başlamak için, sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralı belirtmeniz gerekir. İzin vermek için Internet'ten hangi IP adresinin aralıklarını belirtecek güvenlik duvarı kurallarını kullanın. Azure portalı web sitesine erişim güvenlik duvarı kurallarından etkilenmez.

Aşağıdaki diyagramda gösterildiği gibi, Internet ve Azure'dan gelen bağlantı girişimlerinin, MariaDB veritabanınız için Azure Veritabanınıza erişebilmeleri için önce güvenlik duvarından geçmesi gerekir:

![Güvenlik duvarının nasıl çalıştığına örnek akış](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Sunucu düzeyinde güvenlik duvarı kuralları, MariaDB sunucusu için Azure Veritabanı'ndaki tüm veritabanları için geçerlidir.

İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan birindeyse, bağlantı verilir.

İsteğin IP adresi veritabanı düzeyinde veya sunucu düzeyinde güvenlik duvarı kurallarının herhangi birinde belirtilen aralıkların dışındaysa, bağlantı isteği başarısız olur.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Herhangi bir uygulamanın veya hizmetin giden IP adresini bulmanız ve bu bireysel IP adreslerine veya aralıklarına erişime açıkça izin vermeniz önerilir. Örneğin, bir Azure Uygulama Hizmetinin giden IP adresini bulabilir veya sanal bir makineye veya başka bir kaynağa bağlı genel bir IP kullanabilirsiniz (hizmet bitiş noktaları üzerinden sanal makinenin özel IP'sine bağlanma hakkında bilgi için aşağıya bakın). 

Azure hizmetiniz için sabit giden bir IP adresi kullanılamıyorsa, tüm Azure veri merkezi IP adreslerinden bağlantılar etkinleştirmeyi düşünebilirsiniz. Bu ayar, **Azure hizmetlerine Erişime İzin Verme** seçeneğini Bağlantı **güvenlik** bölmesinden **A.C.'ye** ayarlayarak ve **Kaydet'e**vurarak Azure portalından etkinleştirilebilir. Azure CLI'den, 0.0.0.0'a eşit başlangıç ve bitiş adresine sahip bir güvenlik duvarı kuralı ayarı eşdeğeri yapar. Bağlantı denemesine izin verilmezse, istek MariaDB sunucusu için Azure Veritabanı'na ulaşmaz.

> [!IMPORTANT]
> **Azure hizmetlerine Erişime İzin Verme** seçeneği, güvenlik duvarını diğer müşterilerin aboneliklerinden gelen bağlantılar da dahil olmak üzere Azure'daki tüm bağlantılara izin verecek şekilde yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Yapılandırma Portalında Azure hizmetlerine erişime izin ver](./media/concepts-firewall-rules/allow-azure-services.png)

### <a name="connecting-from-a-vnet"></a>VNet'ten bağlanma
Bir VNet'ten MariaDB sunucusu için Azure Veritabanınıza güvenli bir şekilde bağlanmak için [VNet hizmet uç noktalarını](./concepts-data-access-security-vnet.md)kullanmayı düşünün. 

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak, güvenlik duvarı kuralları Azure CLI kullanılarak programlı olarak yönetilebilir. 

Ayrıca Azure [CLI'yi kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme 'ye](./howto-manage-firewall-cli.md)bakın.

## <a name="troubleshooting-firewall-issues"></a>Sorun giderme güvenlik duvarı sorunları
MariaDB sunucu hizmeti için Microsoft Azure Veritabanı'na erişim beklendiği gibi görünmüyorsa aşağıdaki noktaları göz önünde bulundurun:

* **İzin listesindeyapılan değişiklikler henüz yürürlüğe ekolmadı:** MariaDB Server güvenlik duvarı yapılandırması için Azure Veritabanı'nda yapılan değişikliklerin etkili olması için beş dakika kadar gecikme olabilir.

* **Giriş yetkisi ne de yanlış bir parola kullanılmıştır:** Bir girişte MariaDB sunucusu için Azure Veritabanı'nda izin yoksa veya kullanılan parola yanlışsa, MariaDB sunucusu için Azure Veritabanı'na bağlantı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır.

* **Dinamik IP adresi:** Dinamik IP adresine sahip bir Internet bağlantınız varsa ve güvenlik duvarını geçmekte sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:

   * Internet Servis Sağlayıcınızdan (ISS) MariaDB sunucusu için Azure Veritabanı'na erişen istemci bilgisayarlarınıza atanan IP adresi aralığını isteyin ve ardından IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.

   * İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

* **Sunucunun IP'si herkese açık gibi görünür:** MariaDB sunucusu için Azure Veritabanı'na bağlantılar, herkese açık bir Azure ağ geçidi üzerinden yönlendirilir. Bununla birlikte gerçek sunucu IP'si güvenlik duvarı tarafından korunur. Daha fazla bilgi için [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalını kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-portal.md)
- [Azure CLI'yi kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-cli.md)
- [MariaDB için Azure Veritabanı'ndaki VNet hizmet bitiş noktaları](./concepts-data-access-security-vnet.md)
