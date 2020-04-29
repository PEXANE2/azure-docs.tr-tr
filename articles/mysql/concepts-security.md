---
title: Güvenlik-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'ndaki güvenlik özelliklerine genel bakış.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: aac2641913331095550c0e19cc587257a996fcce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79537032"
---
# <a name="security-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı’nda güvenlik

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

### <a name="private-ip"></a>Özel IP
Özel bağlantı, Azure 'da MySQL için Azure veritabanı 'na özel bir uç nokta aracılığıyla bağlanmanızı sağlar. Azure özel bağlantısı temel olarak Azure hizmetlerini özel sanal ağınız (VNet) içinde sunar. PaaS kaynaklarına, sanal ağ üzerindeki diğer kaynaklar gibi özel IP adresi kullanılarak erişilebilir. Daha fazla bilgi için bkz. [özel bağlantıya genel bakış](concepts-data-access-security-private-link.md)

## <a name="access-management"></a>Erişim yönetimi

MySQL sunucusu için Azure veritabanı oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek MySQL kullanıcıları oluşturmak için kullanılabilir.


## <a name="threat-protection"></a>Tehdit koruması

Sunucu erişimine yönelik olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılayan [Gelişmiş tehdit koruması](concepts-data-access-and-security-threat-protection.md) 'nı tercih edebilirsiniz.

Veritabanında etkinlik izlemek için [Denetim günlüğü](concepts-audit-logs.md) kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
- [IP](concepts-firewall-rules.md) veya [sanal ağlar](concepts-data-access-and-security-vnet.md) için güvenlik duvarı kurallarını etkinleştirme