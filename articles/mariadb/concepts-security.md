---
title: Güvenlik - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı'ndaki güvenlik özelliklerine genel bakış.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8f41fe1005e96b428337bc73b9d468962a079596
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527835"
---
# <a name="security-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda güvenlik

MariaDB sunucusu için Azure Veritabanınızdaki verileri korumak için kullanılabilen birden çok güvenlik katmanı vardır. Bu makalede, bu güvenlik seçenekleri özetleri.

## <a name="information-protection-and-encryption"></a>Bilgi koruma ve şifreleme

### <a name="in-transit"></a>Transit
MariaDB için Azure Veritabanı, Aktarım Katmanı Güvenliği ile aktarım sırasında verilerinizi şifreleyerek verilerinizi güvence altına alar. Şifreleme (SSL/TLS) varsayılan olarak uygulanır.

### <a name="at-rest"></a>Istirahat
MariaDB için Azure Veritabanı hizmeti, fips 140-2 onaylı şifreleme modüllerini kullanarak verilerin depolanması nda kullanılır. Yedeklemeler de dahil olmak üzere veriler, sorguları çalıştırırken oluşturulan geçici dosyalar dışında diskte şifrelenir. Hizmet, Azure depolama şifrelemesinde yer alan AES 256 bit şifresini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.


## <a name="network-security"></a>Ağ güvenliği
MariaDB sunucusu için Azure Veritabanına bağlantılar ilk olarak bölgesel bir ağ geçidiüzerinden yönlendirilir. Ağ geçidinde herkese açık bir IP vardır, sunucu IP adresleri ise korunur. Ağ geçidi hakkında daha fazla bilgi için [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin.  

MariaDB sunucusu için yeni oluşturulan Azure Veritabanı'nda tüm dış bağlantıları engelleyen bir güvenlik duvarı vardır. Ağ geçidine erişseler de sunucuya bağlanmalarına izin verilmez. 

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları, her isteğin menşeli IP adresine dayalı olarak sunuculara erişim sağlar. Daha fazla bilgi için [güvenlik duvarı kurallarına genel bakışa](concepts-firewall-rules.md) bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları
Sanal ağ hizmeti bitiş noktaları, sanal ağ bağlantınızı Azure omurgası üzerinden genişletir. Sanal ağ kurallarını kullanarak MariaDB sunucusu için Azure Veritabanınızın sanal ağdaki seçili alt ağlardan bağlantılara izin vermesini sağlayabilirsiniz. Daha fazla bilgi için [sanal ağ hizmeti bitiş noktasına genel bakış](concepts-data-access-security-vnet.md)abakın.


## <a name="access-management"></a>Erişim yönetimi

MariaDB sunucusu için Azure Veritabanı oluştururken, yönetici bir kullanıcı için kimlik bilgileri sağlarsınız. Bu yönetici, ek MariaDB kullanıcıları oluşturmak için kullanılabilir.


## <a name="threat-protection"></a>Tehdit koruması

Sunuculara erişmek veya sunuculardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılayan [Gelişmiş Tehdit Koruması'nı](concepts-data-access-and-security-threat-protection.md) tercih edebilirsiniz.

[Denetim günlüğü](concepts-audit-logs.md) veritabanlarınızdaki etkinliği izlemek için kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
- [IP'ler](concepts-firewall-rules.md) veya sanal ağlar için güvenlik duvarı kurallarını [etkinleştirme](concepts-data-access-security-vnet.md)