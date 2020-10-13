---
title: Ağa genel bakış-PostgreSQL için Azure veritabanı-esnek sunucu
description: PostgreSQL için Azure veritabanı için esnek sunucu dağıtım seçeneğinde bağlantı ve ağ seçenekleri hakkında bilgi edinin
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: 38dd103189446e287f3d62d93344ed89a364d238
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708790"
---
# <a name="networking-overview---azure-database-for-postgresql---flexible-server"></a>Ağa genel bakış-PostgreSQL için Azure veritabanı-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

Bu makalede PostgreSQL için Azure veritabanı 'nın bağlantı ve ağ kavramları (esnek sunucu) açıklanmaktadır. 

## <a name="choosing-a-networking-option"></a>Ağ seçeneği seçme
PostgreSQL için Azure veritabanı-esnek sunucu için iki ağ seçeneğiniz vardır. Seçenekler **özel erişim (VNET tümleştirmesi)** ve **genel erişim (izin verilen IP adresleri)**. Sunucu oluşturma sırasında bir seçenek seçmeniz gerekir. 

> [!NOTE]
> Ağ seçeneğiniz, sunucu oluşturulduktan sonra değiştirilemez. 

* **Özel erişim (VNET tümleştirmesi)** – esnek sunucunuzu [Azure sanal ağınıza](../../virtual-network/virtual-networks-overview.md)dağıtabilirsiniz. Azure sanal ağları özel ve güvenli ağ iletişimi sağlar. Bir sanal ağdaki kaynaklar özel IP adresleri üzerinden iletişim kurabilir.

   Aşağıdaki özellikleri istiyorsanız VNet tümleştirme seçeneğini belirleyin:
   * Aynı sanal ağdaki Azure kaynaklarından özel IP adresleri kullanarak esnek sunucunuza bağlanma
   * Azure olmayan kaynaklardan esnek sunucunuza bağlanmak için VPN veya ExpressRoute kullanın
   * Esnek sunucuda genel uç nokta yok

* **Genel erişim (izin VERILEN IP adresleri)** – esnek sunucunuza ortak bir uç nokta üzerinden erişilir. Genel uç nokta, genel olarak çözümlenebilen bir DNS adresidir. "İzin verilen IP adresleri" ifadesi sunucunuza erişim izni vermek için seçtiğiniz bir IP aralığı anlamına gelir. Bu izinler, **güvenlik duvarı kuralları**olarak adlandırılır. 

   Aşağıdaki özellikleri istiyorsanız ortak erişim yöntemini seçin:
   * Sanal ağları desteklemeyen Azure kaynaklarından Bağlan
   * VPN veya ExpressRoute ile bağlı olmayan bir Azure dışındaki kaynaklardan bağlanma 
   * Esnek sunucu ortak bir uç noktaya sahip

Aşağıdaki özellikler, özel erişimi mi yoksa genel erişim seçeneğini mi kullanacağınızı tercih etmeksizin geçerlidir:
* İzin verilen IP adreslerinden gelen bağlantıların, geçerli kimlik bilgileriyle PostgreSQL sunucusunda kimlik doğrulaması yapması gerekir
* Ağ trafiğiniz için [bağlantı şifreleme](#tls-and-ssl) kullanılabilir
* Sunucunun tam etki alanı adı (FQDN) vardır. Bağlantı dizelerindeki hostname özelliği için bir IP adresi yerine FQDN kullanmanızı öneririz.
* Her iki seçenek de, veritabanı veya tablo düzeyinde değil, sunucu düzeyinde erişimi denetler. Veritabanı, tablo ve diğer nesne erişimini denetlemek için PostgreSQL 'in rol özelliklerini kullanacaksınız.


## <a name="private-access-vnet-integration"></a>Özel erişim (Sanal Ağ Tümleştirmesi)
Sanal ağ (VNet) Tümleştirmesi ile özel erişim, PostgreSQL esnek sunucunuz için özel ve güvenli iletişim sağlar.

### <a name="virtual-network-concepts"></a>Sanal ağ kavramları
PostgreSQL esnek sunucularıyla sanal ağları kullanırken bilmeniz için bazı kavramlar aşağıda verilmiştir.

* **Sanal ağ** -bir Azure sanal ağı (VNet), kullanım için yapılandırılmış özel bir IP adresi alanı içerir. Azure sanal ağ hakkında daha fazla bilgi edinmek için [Azure sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md) makalesini ziyaret edin.

    Sanal ağınız, esnek sunucunuz ile aynı Azure bölgesinde olmalıdır.


* **Temsilci alt ağ** -sanal ağ alt ağlar (alt ağlar) içerir. Alt ağlar, Sanal ağınızı daha küçük adres alanlarına segmentlerinizi sağlar. Azure kaynakları bir sanal ağ içindeki belirli alt ağlara dağıtılır. 

   PostgreSQL esnek sunucunuzun, PostgreSQL için yalnızca esnek sunucu kullanımı için **temsilci atanmış** bir alt ağda olması gerekir. Bu temsili, yalnızca PostgreSQL için Azure veritabanı 'nın esnek sunucularının bu alt ağı kullanabileceği anlamına gelir. Temsilci alt ağında diğer türdeki Azure kaynakları bulunamaz. Temsilci özelliğini Microsoft. DBforPostgreSQL/Flexibtaservers olarak atayarak bir alt ağ temsilcissiniz.

* **Ağ güvenlik grupları (NSG)** Ağ güvenlik gruplarındaki güvenlik kuralları, sanal ağ alt ağları ve ağ arabirimlerini akacak ve dışı bir ağ trafiği türünü filtrelemenizi sağlar. Daha fazla bilgi için [ağ güvenlik grubuna genel bakış](../../virtual-network/network-security-groups-overview.md) konusunu gözden geçirin.


### <a name="unsupported-virtual-network-scenarios"></a>Desteklenmeyen sanal ağ senaryoları
* Ortak uç nokta (veya genel IP veya DNS)-sanal ağa dağıtılan esnek bir sunucu ortak bir uç noktaya sahip olamaz
* Esnek sunucu bir sanal ağa ve alt ağa dağıtıldıktan sonra, başka bir sanal ağa veya alt ağa taşıyamazsınız. Sanal ağı başka bir kaynak grubuna veya aboneliğine taşıyamazsınız.
* Alt ağda bulunan kaynaklar bir kez alt ağ boyutu (adres alanları) artırılamıyor
* Bölgeler arasında VNET eşlemesi desteklenmiyor

[Azure Portal](how-to-manage-virtual-network-portal.md) veya [Azure CLI](how-to-manage-virtual-network-cli.md)'de özel erişim (VNET tümleştirmesi) ile esnek bir sunucu oluşturmayı öğrenin.

## <a name="public-access-allowed-ip-addresses"></a>Genel erişim (izin verilen IP adresleri)
Ortak erişim yönteminin özellikleri şunlardır:
* Yalnızca izin verilen IP adreslerinin PostgreSQL esnek sunucunuza erişme izni vardır. Varsayılan olarak, IP adresine izin verilmez. Sunucu oluşturma sırasında veya daha sonra IP adresleri ekleyebilirsiniz.
* PostgreSQL sunucunuzun genel olarak çözümlenebilen bir DNS adı vardır
* Esnek sunucunuz Azure sanal ağlarınızdan birinde değil
* Sunucunuza gelen ve giden ağ trafiği özel bir ağ üzerinden geçmiyor. Trafik Genel internet yol yollarını kullanır.

### <a name="firewall-rules"></a>Güvenlik duvarı kuralları
Bir IP adresine izin verilmesi, güvenlik duvarı kuralı olarak adlandırılır. Bir bağlantı girişimi izin verilmeyen bir IP adresinden geliyorsa, kaynak istemci bir hata görür.

[Azure Portal](how-to-manage-firewall-portal.md) veya [Azure CLI](how-to-manage-firewall-cli.md)'de ortak ERIŞIME (izin verilen IP adresleri) sahip esnek bir sunucu oluşturmayı öğrenin.


### <a name="allowing-all-azure-ip-addresses"></a>Tüm Azure IP adreslerine izin verme
Azure hizmetiniz için sabit bir giden IP adresi yoksa, tüm Azure veri merkezi IP adreslerinden gelen bağlantıları etkinleştirmeyi deneyebilirsiniz.

> [!IMPORTANT]
> Azure **hizmetlerinden ve Azure 'daki kaynaklardan ortak erişime Izin ver** seçeneği, güvenlik duvarını diğer müşterilerin aboneliklerinden gelen bağlantılar da dahil olmak üzere Azure 'daki tüm bağlantılara izin verecek şekilde yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

### <a name="troubleshooting-public-access-issues"></a>Genel erişim sorunlarını giderme
PostgreSQL için Microsoft Azure veritabanına erişim beklendiği gibi davranmıyorsa aşağıdaki noktaları göz önünde bulundurun:

* **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:** PostgreSQL için Azure veritabanı sunucu güvenlik duvarı yapılandırmasının etkili olması için beş dakikalık bir gecikme olabilir.

* **Kimlik doğrulama başarısız oldu:** Bir kullanıcının PostgreSQL için Azure veritabanı sunucusunda izinleri yoksa veya kullanılan parola yanlışsa, PostgreSQL için Azure veritabanı sunucusuna bağlantı reddedilir. Bir güvenlik duvarı ayarı oluşturmak, istemcilere yalnızca sunucunuza bağlanmayı denemek için bir fırsat sağlar. Her istemci yine de gerekli güvenlik kimlik bilgilerini sağlamalıdır.

* **Dinamik ISTEMCI IP adresi:** Dinamik IP adresleme ile Internet bağlantınız varsa ve güvenlik duvarından geçiş konusunda sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:

   * PostgreSQL için Azure veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanan IP adresi aralığı için Internet servis sağlayıcınıza (ISS) sorun ve IP adresi aralığını bir güvenlik duvarı kuralı olarak ekleyin.
   * İstemci bilgisayarlarınız yerine statik IP adresleme alın ve statik IP adresini bir güvenlik duvarı kuralı olarak ekleyin.


## <a name="hostname"></a>Konak adı
Seçtiğiniz ağ seçeneğinden bağımsız olarak, esnek sunucunuza bağlanırken her zaman konak adı olarak tam etki alanı adı (FQDN) kullanmanızı öneririz. Sunucunun IP adresinin statik kalması garanti edilmez. FQDN 'nin kullanılması, bağlantı dizeniz üzerinde değişiklik yapmaktan kaçınmanıza yardımcı olur. 

Örnek
* Önerilen `hostname = servername.postgres.database.azure.com`
* Mümkün olduğunda, kullanmaktan kaçının `hostname = 10.0.0.4` (özel bir adres) veya `hostname = 40.2.45.67` (genel bir adres)


## <a name="tls-and-ssl"></a>TLS ve SSL
PostgreSQL için Azure veritabanı-esnek sunucu, Aktarım Katmanı Güvenliği (TLS) kullanarak istemci uygulamalarınızın PostgreSQL hizmetine bağlanmasını destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında şifrelenmiş ağ bağlantıları sağlayan bir endüstri standardı protokolüdür. TLS, SSL 'nin güncelleştirilmiş bir protokolüdür (Güvenli Yuva Katmanı).

PostgreSQL için Azure veritabanı-esnek sunucu yalnızca Aktarım Katmanı Güvenliği kullanılarak şifrelenmiş bağlantıları destekler. TLS 1,0 ve TLS 1,1 ile gelen tüm bağlantılar reddedilir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Portal](how-to-manage-virtual-network-portal.md) veya [Azure CLI](how-to-manage-virtual-network-cli.md)'de **özel erişim (VNET tümleştirmesi)** ile esnek bir sunucu oluşturmayı öğrenin.
* [Azure Portal](how-to-manage-firewall-portal.md) veya [Azure CLI](how-to-manage-firewall-cli.md)'de **ortak erişime (izin verilen IP adresleri)** sahip esnek bir sunucu oluşturmayı öğrenin.
