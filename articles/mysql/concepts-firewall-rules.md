---
title: MySQL için Azure veritabanı sunucu güvenlik duvarı kuralları
description: MySQL için Azure veritabanı sunucunuza yönelik güvenlik duvarı kurallarını açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 0802185b7fb0d1a6d7d41cd1fa5a30f5ce10424b
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443920"
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>MySQL için Azure veritabanı sunucu güvenlik duvarı kuralları
Güvenlik duvarları, hangi bilgisayarların izne sahip olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik Duvarı, her isteğin kaynak IP adresini temel alarak sunucuya erişim izni verir.

Bir güvenlik duvarını yapılandırmak için, kabul edilebilir IP adresleri aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, istemcilerin tüm MySQL Server için Azure veritabanı sunucusuna, diğer bir deyişle, aynı mantıksal sunucu içindeki tüm veritabanlarına erişmesini sağlar. Sunucu düzeyinde güvenlik duvarı kuralları Azure portal veya Azure CLı komutları kullanılarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için, abonelik sahibi veya abonelik katılımcısı olmanız gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
MySQL için Azure veritabanı sunucunuza yönelik tüm veritabanı erişimi, varsayılan olarak güvenlik duvarı tarafından engelleniyor. Sunucunuzu başka bir bilgisayardan kullanmaya başlamak için, sunucunuza erişimi etkinleştirmek üzere bir veya daha fazla sunucu düzeyi güvenlik duvarı kuralı belirtmeniz gerekir. Internet 'ten izin verilecek IP adresi aralıklarını belirtmek için güvenlik duvarı kurallarını kullanın. Azure portal web sitesinin kendisi için erişim, güvenlik duvarı kurallarından etkilenmez.

Internet 'ten ve Azure 'dan gelen bağlantı girişimleri, aşağıdaki diyagramda gösterildiği gibi, MySQL için Azure veritabanı veritabanınıza ulaşabilmesi için önce güvenlik duvarından geçmelidir:

![Güvenlik duvarının nasıl çalıştığına ilişkin örnek akış](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Sunucu düzeyinde güvenlik duvarı kuralları, MySQL için Azure veritabanı sunucusundaki tüm veritabanlarına uygulanır.

İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı verilir.

İsteğin IP adresi, veritabanı düzeyinde veya sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıkların dışındaysa bağlantı isteği başarısız olur.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure 'daki uygulamaların MySQL sunucusu için Azure veritabanı 'na bağlanmasına izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulamasını veya bir Azure VM 'de çalışan bir uygulamayı barındırmak ya da bir Azure Data Factory veri yönetimi ağ geçidinden bağlanmak için. Bu bağlantıları etkinleştirmek için kaynakların, güvenlik duvarı kuralı için aynı sanal ağ (VNet) veya kaynak grubunda olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu tür bağlantıları etkinleştirmek için birkaç yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, **bağlantı güvenliği** bölmesinden portalda **Azure hizmetlerine erişime izin ver** seçeneğini ayarlayabilir ve **Kaydet** **' e basın** . Bağlantı girişimine izin verilmiyorsa, istek MySQL için Azure veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Portalda Azure hizmetlerine erişime Izin ver yapılandırma](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portal ek olarak, güvenlik duvarı kuralları Azure CLı kullanılarak programlı bir şekilde yönetilebilir. Ayrıca bkz. [Azure CLI kullanarak MySQL Için Azure Güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme
MySQL Server hizmetine yönelik Microsoft Azure veritabanına erişim beklendiği gibi davranmıyorsa aşağıdaki noktaları göz önünde bulundurun:

* **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:** MySQL Server Güvenlik Duvarı yapılandırmasının etkili olması için Azure veritabanı 'nda yapılan değişiklikler için beş dakikalık bir gecikme olabilir.

* **Oturum açma yetkisi yok veya yanlış parola kullanıldı:** Bir oturum açma, MySQL için Azure veritabanı sunucusunda izinlere sahip değilse veya kullanılan parola yanlışsa, MySQL için Azure veritabanı sunucusu bağlantısı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır.

* **Dinamik IP adresi:** Dinamik IP adresleme ile Internet bağlantınız varsa ve güvenlik duvarıyla ilgili sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:

* MySQL için Azure veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanan IP adresi aralığı için Internet servis sağlayıcınıza (ISS) sorun ve IP adresi aralığını bir güvenlik duvarı kuralı olarak ekleyin.

* İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-portal.md)
* [Azure CLı kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-cli.md)
