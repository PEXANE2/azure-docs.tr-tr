---
title: MariaDB için Azure veritabanı-tek sunucu güvenliği
description: MariaDB için Azure veritabanı-tek sunucu güvenlik özelliklerine genel bakış.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: 1a2d7ac449c131519473afaa80eb3a57c287eb28
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74485191"
---
# <a name="security-in-azure-database-for-mariadb---single-server"></a>MariaDB için Azure veritabanı-tek sunucu güvenliği

MariaDB sunucusu için Azure veritabanınızdaki verileri korumak üzere kullanılabilen birden çok güvenlik katmanı vardır. Bu makalede bu güvenlik seçenekleri özetlenmektedir.

## <a name="information-protection-and-encryption"></a>Bilgi koruması ve şifreleme

### <a name="in-transit"></a>Aktarım yerinde
MariaDB için Azure veritabanı, Aktarım Katmanı Güvenliği ile verileri aktarım sırasında şifreleyerek veri güvenliğini sağlar. Şifreleme (SSL/TLS) varsayılan olarak zorlanır.

### <a name="at-rest"></a>REST
MariaDB hizmeti için Azure veritabanı, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar hariç olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.


## <a name="network-security"></a>Ağ güvenliği
MariaDB sunucusu için Azure veritabanı 'na olan bağlantılar önce bölgesel bir ağ geçidiyle yönlendirilir. Ağ geçidinin genel olarak erişilebilir bir IP 'si vardır, ancak sunucu IP adresleri korunur. Ağ Geçidi hakkında daha fazla bilgi için, [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin.  

MariaDB sunucusu için yeni oluşturulmuş bir Azure veritabanı, tüm dış bağlantıları engelleyen bir güvenlik duvarına sahiptir. Ağ geçidine ulaştıkları halde sunucuya bağlanmasına izin verilmez. 

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları, her isteğin kaynak IP adresini temel alarak sunuculara erişim izni verir. Daha fazla bilgi için [güvenlik duvarı kurallarına genel bakış](concepts-firewall-rules.md) bölümüne bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları
Sanal ağ hizmeti uç noktaları, sanal ağ bağlantınızı Azure omurgası üzerinden genişletmelidir. Sanal ağ kurallarını kullanarak, bir sanal ağdaki seçili alt ağlardan gelen bağlantılara izin vermek için MariaDB sunucusu için Azure veritabanınızı etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktasına genel bakış](concepts-data-access-security-vnet.md).


## <a name="access-management"></a>Erişim yönetimi

MariaDB sunucusu için Azure veritabanı oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek MariaDB kullanıcıları oluşturmak için kullanılabilir.


## <a name="threat-protection"></a>Tehdit koruması

Sunucu erişimine yönelik olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılayan [Gelişmiş tehdit koruması](concepts-data-access-and-security-threat-protection.md) 'nı tercih edebilirsiniz.

Veritabanında etkinlik izlemek için [Denetim günlüğü](concepts-audit-logs.md) kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
- [IP](concepts-firewall-rules.md) veya [sanal ağlar](concepts-data-access-security-vnet.md) için güvenlik duvarı kurallarını etkinleştirme