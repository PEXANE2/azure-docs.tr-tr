---
title: PostgreSQL için Azure veritabanı 'nda güvenlik-tek sunucu
description: PostgreSQL için Azure veritabanı 'nın güvenlik özelliklerine genel bakış-tek sunucu.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: be042a0ec076538cf0f0d155667acea6f1ae19cb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91710490"
---
# <a name="security-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda güvenlik-tek sunucu

PostgreSQL için Azure veritabanı sunucunuzdaki verileri korumak için kullanılabilen birden çok güvenlik katmanı vardır. Bu makalede bu güvenlik seçenekleri özetlenmektedir.

## <a name="information-protection-and-encryption"></a>Bilgi koruması ve şifreleme

### <a name="in-transit"></a>Aktarım yerinde
PostgreSQL için Azure veritabanı, Aktarım Katmanı Güvenliği ile verileri aktarım sırasında şifreleyerek veri güvenliğini sağlar. Şifreleme (SSL/TLS) varsayılan olarak zorlanır.

### <a name="at-rest"></a>REST
PostgreSQL için Azure veritabanı hizmeti, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar dahil olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.


## <a name="network-security"></a>Ağ güvenliği
PostgreSQL için Azure veritabanı sunucusuna yapılan bağlantılar ilk olarak bölgesel ağ geçidiyle yönlendirilir. Ağ geçidinin genel olarak erişilebilir bir IP 'si vardır, ancak sunucu IP adresleri korunur. Ağ Geçidi hakkında daha fazla bilgi için, [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin.  

PostgreSQL için yeni oluşturulan Azure veritabanı sunucusuna tüm dış bağlantıları engelleyen bir güvenlik duvarı vardır. Ağ geçidine ulaştıkları halde sunucuya bağlanmasına izin verilmez. 

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları, her isteğin kaynak IP adresini temel alarak sunuculara erişim izni verir. Daha fazla bilgi için [güvenlik duvarı kurallarına genel bakış](concepts-firewall-rules.md) bölümüne bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları
Sanal ağ hizmeti uç noktaları, sanal ağ bağlantınızı Azure omurgası üzerinden genişletmelidir. Sanal ağ kurallarını kullanarak, PostgreSQL için Azure veritabanınızı, bir sanal ağdaki seçili alt ağlardan gelen bağlantılara izin verecek şekilde etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktasına genel bakış](concepts-data-access-and-security-vnet.md).

### <a name="private-ip"></a>Özel IP
Özel bağlantı, özel bir uç nokta aracılığıyla Azure 'daki PostgreSQL için Azure veritabanı 'na bağlanmanızı sağlar. Azure Özel Bağlantı, Azure hizmetlerini özel sanal ağınıza (VNet) getirir. Sanal ağdaki diğer kaynaklar gibi PaaS kaynaklarına da özel IP adresini kullanarak erişebilirsiniz. Daha fazla bilgi için bkz. [özel bağlantıya genel bakış](concepts-data-access-and-security-private-link.md)


## <a name="access-management"></a>Erişim yönetimi

PostgreSQL için Azure veritabanı sunucusu oluştururken, bir yönetici rolü için kimlik bilgileri sağlarsınız. Bu yönetici rolü, ek [PostgreSQL rolleri](https://www.postgresql.org/docs/current/user-manag.html)oluşturmak için kullanılabilir.

Sunucuya [Azure Active Directory (AAD) kimlik doğrulaması](concepts-aad-authentication.md)kullanarak da bağlanabilirsiniz.


## <a name="threat-protection"></a>Tehdit koruması

Sunucu erişimine yönelik olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılayan [Gelişmiş tehdit koruması](concepts-data-access-and-security-threat-protection.md) 'nı tercih edebilirsiniz.

Veritabanında etkinlik izlemek için [Denetim günlüğü](concepts-audit.md) kullanılabilir. 

## <a name="migrating-from-oracle"></a>Oracle 'dan geçiş

Oracle, tablo ve veri alanı verilerini şifrelemek için Saydam Veri Şifrelemesi (TDE) destekler. PostgreSQL için Azure 'da, veriler çeşitli katmanlarda otomatik olarak şifrelenir. Bu sayfanın "rest" bölümüne bakın ve ayrıca [müşterinin yönettiği anahtarlar](./concepts-data-encryption-postgresql.md) ve [altyapı çift şifrelemesi](./concepts-infrastructure-double-encryption.md)gibi çeşitli güvenlik konularına bakın. Ayrıca, [PostgreSQL Için Azure](./concepts-extensions.md)'da desteklenen [pgşifre](https://www.postgresql.org/docs/11/pgcrypto.html) uzantısını kullanmayı da düşünebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [IP](concepts-firewall-rules.md) veya [sanal ağlar](concepts-data-access-and-security-vnet.md) için güvenlik duvarı kurallarını etkinleştirme
- PostgreSQL için Azure veritabanı 'nda [Azure Active Directory kimlik doğrulaması](concepts-aad-authentication.md) hakkında bilgi edinin
