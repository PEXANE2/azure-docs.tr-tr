---
title: MySQL için Azure veritabanı 'nda güvenlik-tek sunucu
description: MySQL için Azure veritabanı 'nın güvenlik özelliklerine genel bakış-tek sunucu.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: 06f895d2e8cd7936141c9f35c5ed0c81fb7e3e15
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74485061"
---
# <a name="security-in-azure-database-for-mysql---single-server"></a>MySQL için Azure veritabanı 'nda güvenlik-tek sunucu

MySQL için Azure veritabanı sunucunuzdaki verileri korumak için kullanılabilen birden çok güvenlik katmanı vardır. Bu makalede bu güvenlik seçenekleri özetlenmektedir.

## <a name="information-protection-and-encryption"></a>Bilgi koruması ve şifreleme

### <a name="in-transit"></a>Aktarım yerinde
MySQL için Azure veritabanı, Aktarım Katmanı Güvenliği ile verileri aktarma sırasında şifreleyerek verilerinizi korur. Şifreleme (SSL/TLS) varsayılan olarak zorlanır.

### <a name="at-rest"></a>REST
MySQL için Azure veritabanı hizmeti, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar hariç olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.


## <a name="network-security"></a>Ağ güvenliği
MySQL için Azure veritabanı sunucusuna yapılan bağlantılar öncelikle bölgesel bir ağ geçidiyle yönlendirilir. Ağ geçidinin genel olarak erişilebilir bir IP 'si vardır, ancak sunucu IP adresleri korunur. Ağ Geçidi hakkında daha fazla bilgi için, [bağlantı mimarisi makalesini](concepts-connectivity-architecture.md)ziyaret edin.  

MySQL için yeni oluşturulmuş bir Azure veritabanı sunucusu, tüm dış bağlantıları engelleyen bir güvenlik duvarına sahiptir. Ağ geçidine ulaştıkları halde sunucuya bağlanmasına izin verilmez. 

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları, her isteğin kaynak IP adresini temel alarak sunuculara erişim izni verir. Daha fazla bilgi için [güvenlik duvarı kurallarına genel bakış](concepts-firewall-rules.md) bölümüne bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları
Sanal ağ hizmeti uç noktaları, sanal ağ bağlantınızı Azure omurgası üzerinden genişletmelidir. Sanal ağ kurallarını kullanarak, MySQL için Azure veritabanı sunucunuzu, bir sanal ağdaki seçili alt ağlardan gelen bağlantılara izin verecek şekilde etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktasına genel bakış](concepts-data-access-and-security-vnet.md).


## <a name="access-management"></a>Erişim yönetimi

MySQL sunucusu için Azure veritabanı oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek MySQL kullanıcıları oluşturmak için kullanılabilir.


## <a name="threat-protection"></a>Tehdit koruması

Sunucu erişimine yönelik olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılayan [Gelişmiş tehdit koruması](concepts-data-access-and-security-threat-protection.md) 'nı tercih edebilirsiniz.

Veritabanında etkinlik izlemek için [Denetim günlüğü](concepts-audit-logs.md) kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
- [IP](concepts-firewall-rules.md) veya [sanal ağlar](concepts-data-access-and-security-vnet.md) için güvenlik duvarı kurallarını etkinleştirme