---
title: Güvenlik duvarı kuralları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'na bağlanmak için güvenlik duvarı kurallarının nasıl kullanılacağı açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 5d462be1caa3787cb7ff9a455be595ec5784eefe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157279"
---
# <a name="firewall-rules-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda Güvenlik Duvarı kuralları - Tek Sunucu
PostgreSQL sunucu güvenlik duvarı için Azure Veritabanı, hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her isteğin menşeli IP adresine bağlı olarak sunucuya erişim sağlar.
Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, istemcilerin PostgreSQL Server için tüm Azure Veritabanınıza, yani aynı mantıksal sunucudaki tüm veritabanlarına erişmesini sağlar. Sunucu düzeyinde güvenlik duvarı kuralları, Azure portalı kullanılarak veya Azure CLI komutları kullanılarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katılımcısı olmalısınız.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
PostgreSQL sunucusu için Azure Veritabanınıza tüm veritabanı erişimi varsayılan olarak güvenlik duvarı tarafından engellenir. Sunucunuzu başka bir bilgisayardan kullanmaya başlamak için, sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralı belirtmeniz gerekir. İzin vermek için Internet'ten hangi IP adresinin aralıklarını belirtecek güvenlik duvarı kurallarını kullanın. Azure portalı web sitesine erişim güvenlik duvarı kurallarından etkilenmez.
Aşağıdaki diyagramda gösterildiği gibi, Internet ve Azure'dan gelen bağlantı girişimlerinin, PostgreSQL Veritabanınıza erişebilmeleri için önce güvenlik duvarından geçmesi gerekir:

![Güvenlik duvarının nasıl çalıştığına örnek akış](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Sunucu düzeyinde güvenlik duvarı kuralları, PostgreSQL sunucusu için aynı Azure Veritabanı'ndaki tüm veritabanları için geçerlidir. İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir.
İsteğin IP adresi sunucu düzeyindegüvenlik duvarı kurallarının herhangi birinde belirtilen aralıklarda değilse, bağlantı isteği başarısız olur.
Örneğin, uygulamanız PostgreSQL için JDBC sürücüsüne bağlanıyorsa, güvenlik duvarı bağlantıyı engellediğinde bağlanmaya çalışan bu hatayla karşılaşabilirsiniz.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: host "123.45.67.890", kullanıcı "adminuser", veritabanı "postgresql", SSL için hiçbir pg\_hba.conf giriş

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Herhangi bir uygulamanın veya hizmetin giden IP adresini bulmanız ve bu bireysel IP adreslerine veya aralıklarına erişime açıkça izin vermeniz önerilir. Örneğin, bir Azure Uygulama Hizmetinin giden IP adresini bulabilir veya sanal bir makineye veya başka bir kaynağa bağlı genel bir IP kullanabilirsiniz (hizmet bitiş noktaları üzerinden sanal makinenin özel IP'sine bağlanma hakkında bilgi için aşağıya bakın). 

Azure hizmetiniz için sabit giden bir IP adresi kullanılamıyorsa, tüm Azure veri merkezi IP adreslerinden bağlantılar etkinleştirmeyi düşünebilirsiniz. Bu ayar, **Azure hizmetlerine Erişime İzin Verme** seçeneğini Bağlantı **güvenlik** bölmesinden **A.C.'ye** ayarlayarak ve **Kaydet'e**vurarak Azure portalından etkinleştirilebilir. Azure CLI'den, 0.0.0.0'a eşit başlangıç ve bitiş adresine sahip bir güvenlik duvarı kuralı ayarı eşdeğeri yapar. Bağlantı denemesine izin verilmezse, istek PostgreSQL sunucusu için Azure Veritabanı'na ulaşmaz.

> [!IMPORTANT]
> **Azure hizmetlerine Erişime İzin Verme** seçeneği, güvenlik duvarını diğer müşterilerin aboneliklerinden gelen bağlantılar da dahil olmak üzere Azure'daki tüm bağlantılara izin verecek şekilde yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Yapılandırma Portalında Azure hizmetlerine erişime izin ver](media/concepts-firewall-rules/allow-azure-services.png)

### <a name="connecting-from-a-vnet"></a>VNet'ten bağlanma
Bir VNet'ten PostgreSQL sunucusu için Azure Veritabanınıza güvenli bir şekilde bağlanmak için [VNet hizmet uç noktalarını](./concepts-data-access-and-security-vnet.md)kullanmayı düşünün. 

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak, güvenlik duvarı kuralları Azure CLI kullanılarak programlı olarak yönetilebilir.
Ayrıca Azure [CLI'yi kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-firewall-issues"></a>Sorun giderme güvenlik duvarı sorunları
PostgreSQL Server hizmeti için Microsoft Azure Veritabanı'na erişim beklediğiniz gibi görünmüyorsa aşağıdaki noktaları göz önünde bulundurun:

* **İzin listesindeyapılan değişiklikler henüz yürürlüğe ekolmadı:** PostgreSQL Server güvenlik duvarı yapılandırması için Azure Veritabanı'nda yapılan değişikliklerin etkili olması için beş dakika kadar gecikme olabilir.

* **Giriş yetkisi ne de yanlış bir parola kullanılmıştır:** Bir girişte PostgreSQL sunucusu için Azure Veritabanı'nda izin yoksa veya kullanılan parola yanlışsa, PostgreSQL sunucusu için Azure Veritabanı'na bağlantı reddedilir. Güvenlik duvarı ayarı oluşturmak, istemcilere yalnızca sunucunuza bağlanmayı deneme fırsatı sağlar; her istemci hala gerekli güvenlik kimlik bilgilerini sağlamalıdır.

   Örneğin, bir JDBC istemcisi kullanarak aşağıdaki hata görünebilir.
   > java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: password authentication kullanıcı "kullanıcı adı" için başarısız oldu

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

   * Internet Servis Sağlayıcınızdan (ISS) PostgreSQL Server için Azure Veritabanı'na erişen istemci bilgisayarlarınıza atanan IP adresi aralığını isteyin ve ardından IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.

   * İstemci bilgisayarlarınız için statik IP adresi alın ve ardından statik IP adresini güvenlik duvarı kuralı olarak ekleyin.

* **Sunucunun IP'si herkese açık gibi görünür:** PostgreSQL sunucusu için Azure Veritabanı'na bağlantılar, herkese açık bir Azure ağ geçidi üzerinden yönlendirilir. Bununla birlikte gerçek sunucu IP'si güvenlik duvarı tarafından korunur. Daha fazla bilgi için [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin. 

## <a name="next-steps"></a>Sonraki adımlar
Sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma yla ilgili makaleler için bkz:
* [Azure portalını kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-using-portal.md)
* [Azure CLI'yi kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-using-cli.md)
- [PostgreSQL için Azure Veritabanı'ndaki VNet hizmet bitiş noktaları](./concepts-data-access-and-security-vnet.md)
