---
title: PostgreSQL için Azure Veritabanında Güvenlik - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanı'ndaki güvenlik özelliklerine genel bakış.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 02dc9e1ad9ee46b1a400e44b6ef737e70571a17a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972596"
---
# <a name="security-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Güvenlik - Tek Sunucu

PostgreSQL sunucusu için Azure Veritabanınızdaki verileri korumak için kullanılabilen birden çok güvenlik katmanı vardır. Bu makalede, bu güvenlik seçenekleri özetleri.

## <a name="information-protection-and-encryption"></a>Bilgi koruma ve şifreleme

### <a name="in-transit"></a>Transit
PostgreSQL için Azure Veritabanı, Aktarım Katmanı Güvenliği ile aktarım sırasında verilerinizi şifreleyerek verilerinizi güvence altına alar. Şifreleme (SSL/TLS) varsayılan olarak uygulanır.

### <a name="at-rest"></a>Istirahat
PostgreSQL için Azure Veritabanı hizmeti, fips 140-2 onaylı şifreleme modüllerini kullanarak verilerin depolanması nda kullanılır. Yedeklemeler de dahil olmak üzere veriler, sorguları çalıştırırken oluşturulan geçici dosyalar dışında diskte şifrelenir. Hizmet, Azure depolama şifrelemesinde yer alan AES 256 bit şifresini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.


## <a name="network-security"></a>Ağ güvenliği
PostgreSQL sunucusu için Bir Azure Veritabanına bağlantılar ilk olarak bölgesel bir ağ geçidiüzerinden yönlendirilir. Ağ geçidinde herkese açık bir IP vardır, sunucu IP adresleri ise korunur. Ağ geçidi hakkında daha fazla bilgi için [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin.  

PostgreSQL için yeni oluşturulan Azure Veritabanı'nda tüm dış bağlantıları engelleyen bir güvenlik duvarı vardır. Ağ geçidine erişseler de sunucuya bağlanmalarına izin verilmez. 

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları, her isteğin menşeli IP adresine dayalı olarak sunuculara erişim sağlar. Daha fazla bilgi için [güvenlik duvarı kurallarına genel bakışa](concepts-firewall-rules.md) bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları
Sanal ağ hizmeti bitiş noktaları, sanal ağ bağlantınızı Azure omurgası üzerinden genişletir. Sanal ağ kurallarını kullanarak PostgreSQL için Azure Veritabanınızın sanal ağdaki seçili alt ağlardan bağlantılara izin vermesini sağlayabilirsiniz. Daha fazla bilgi için [sanal ağ hizmeti bitiş noktasına genel bakış](concepts-data-access-and-security-vnet.md)abakın.

### <a name="private-ip"></a>Özel IP
Private Link, Azure'daki PostgreSQL Tek sunucu için Azure Veritabanınıza özel bir bitiş noktası üzerinden bağlanmanızı sağlar. Azure Özel Bağlantı, Azure hizmetlerini aslında özel Sanal Ağınıza (VNet) getirir. PaaS kaynaklarına, VNet'teki diğer kaynaklar gibi özel IP adresi kullanılarak erişilebilir. Daha fazla bilgi için [özel bağlantıya genel bakış](concepts-data-access-and-security-private-link.md)


## <a name="access-management"></a>Erişim yönetimi

PostgreSQL sunucusu için Azure Veritabanı oluştururken, yönetici rolü için kimlik bilgileri sağlarsınız. Bu yönetici rolü ek [PostgreSQL rolleri](https://www.postgresql.org/docs/current/user-manag.html)oluşturmak için kullanılabilir.

[Azure Active Directory (AAD) kimlik doğrulaması](concepts-aad-authentication.md)kullanarak sunucuya da bağlanabilirsiniz.


## <a name="threat-protection"></a>Tehdit koruması

Sunuculara erişmek veya sunuculardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılayan [Gelişmiş Tehdit Koruması'nı](concepts-data-access-and-security-threat-protection.md) tercih edebilirsiniz.

[Denetim günlüğü](concepts-audit.md) veritabanlarınızdaki etkinliği izlemek için kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
- [IP'ler](concepts-firewall-rules.md) veya sanal ağlar için güvenlik duvarı kurallarını [etkinleştirme](concepts-data-access-and-security-vnet.md)
- PostgreSQL için Azure Veritabanı'nda [Azure Active Directory kimlik doğrulaması](concepts-aad-authentication.md) hakkında bilgi edinin
