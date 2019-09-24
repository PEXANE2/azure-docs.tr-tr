---
title: MariaDB sunucusu için Azure veritabanı güvenlik duvarı kuralları
description: MariaDB sunucusu için Azure veritabanınız için güvenlik duvarı kurallarını açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/22/2019
ms.openlocfilehash: 94efc87cdea8e1d9b63361d4652b63909c6ce3e7
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203604"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>MariaDB sunucusu için Azure veritabanı güvenlik duvarı kuralları
Güvenlik duvarları, hangi bilgisayarların izne sahip olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik Duvarı, her isteğin kaynak IP adresini temel alarak sunucuya erişim izni verir.

Bir güvenlik duvarını yapılandırmak için, kabul edilebilir IP adresleri aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar istemcilerin tüm MariaDB sunucusu, diğer bir deyişle aynı mantıksal sunucu içindeki tüm veritabanları için Azure veritabanı 'na erişmesini sağlar. Sunucu düzeyinde güvenlik duvarı kuralları Azure portal veya Azure CLı komutları kullanılarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için, abonelik sahibi veya abonelik katılımcısı olmanız gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
MariaDB sunucusu için Azure veritabanınıza yönelik tüm veritabanı erişimi, varsayılan olarak güvenlik duvarı tarafından engelleniyor. Sunucunuzu başka bir bilgisayardan kullanmaya başlamak için, sunucunuza erişimi etkinleştirmek üzere bir veya daha fazla sunucu düzeyi güvenlik duvarı kuralı belirtmeniz gerekir. Internet 'ten izin verilecek IP adresi aralıklarını belirtmek için güvenlik duvarı kurallarını kullanın. Azure portal web sitesinin kendisi için erişim, güvenlik duvarı kurallarından etkilenmez.

Internet 'ten ve Azure 'dan gelen bağlantı girişimleri, aşağıdaki diyagramda gösterildiği gibi, MariaDB veritabanı için Azure veritabanınıza ulaşabilmesi için önce güvenlik duvarından geçmelidir:

![Güvenlik duvarının nasıl çalıştığına ilişkin örnek akış](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Sunucu düzeyinde güvenlik duvarı kuralları, MariaDB sunucusu için Azure veritabanı 'ndaki tüm veritabanları için geçerlidir.

İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı verilir.

İsteğin IP adresi, veritabanı düzeyinde veya sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıkların dışındaysa bağlantı isteği başarısız olur.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure 'daki uygulamaların, MariaDB sunucusu için Azure veritabanınıza bağlanmasına izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulamasını veya bir Azure VM 'de çalışan bir uygulamayı barındırmak ya da bir Azure Data Factory veri yönetimi ağ geçidinden bağlanmak için. Bu bağlantıları etkinleştirmek için kaynakların, güvenlik duvarı kuralı için aynı sanal ağ (VNet) veya kaynak grubunda olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu tür bağlantıları etkinleştirmek için birkaç yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, **bağlantı güvenliği** bölmesinden portalda **Azure hizmetlerine erişime izin ver** seçeneğini ayarlayabilir ve **Kaydet** **' e basın** . Bağlantı girişimine izin verilmiyorsa, istek, MariaDB sunucusu için Azure veritabanına ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Portalda Azure hizmetlerine erişime Izin ver yapılandırma](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portal ek olarak, güvenlik duvarı kuralları Azure CLı kullanılarak programlı bir şekilde yönetilebilir. 

<!--See also [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)-->

## <a name="troubleshooting-firewall-issues"></a>Güvenlik Duvarı sorunlarını giderme
MariaDB Server hizmeti için Microsoft Azure veritabanına erişim beklendiği gibi davranmıyorsa aşağıdaki noktaları göz önünde bulundurun:

* **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:** MariaDB için Azure Veritabanı Sunucusu güvenlik duvarı yapılandırması üzerindeki değişikliklerin geçerli olması beş dakika kadar gecikebilir.

* **Oturum açma yetkisi yok veya yanlış parola kullanıldı:** Bir oturum açma, MariaDB sunucusu için Azure veritabanı 'na izin içermiyorsa veya kullanılan parola yanlışsa, MariaDB sunucusu için Azure veritabanı bağlantısı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır.

* **Dinamik IP adresi:** Dinamik IP adresleme ile Internet bağlantınız varsa ve güvenlik duvarıyla ilgili sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:

   * Internet servis sağlayıcınıza (ISS), MariaDB sunucusu için Azure veritabanına erişen istemci bilgisayarlarınıza atanan IP adresi aralığı için sorun ve IP adresi aralığını bir güvenlik duvarı kuralı olarak ekleyin.

   * İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

* **Sunucunun IP 'si genel gibi görünür:** MariaDB sunucusu için Azure veritabanı 'na bağlantılar, genel olarak erişilebilen bir Azure ağ geçidiyle yönlendirilir. Ancak gerçek sunucu IP 'si güvenlik duvarı tarafından korunur. Daha fazla bilgi için, [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portal kullanarak MariaDB için Azure veritabanı güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-portal.md)

<!--
- [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md) -->
