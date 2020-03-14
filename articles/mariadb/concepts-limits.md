---
title: Sınırlamalar-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda bağlantı ve depolama motoru seçeneklerinin sayısı gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: c982181dee34a7eb0715d5e1271ef5ed794f3809
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79296753"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı sınırlamaları
Aşağıdaki bölümlerde, kapasitesi, depolama altyapısı desteği, destek ayrıcalığına, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır.

## <a name="server-parameters"></a>Sunucu parametreleri

Çeşitli popüler sunucu parametrelerinin en düşük ve en yüksek değerleri, fiyatlandırma katmanı ve sanal çekirdekler tarafından belirlenir. Sınırlar için aşağıdaki tablolara bakın.

### <a name="max_connections"></a>max_connections

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|50|10|50|
|Temel|2|100|10|100|
|Genel Amaçlı|2|300|10|600|
|Genel Amaçlı|4|625|10|1250|
|Genel Amaçlı|8|1250|10|2500|
|Genel Amaçlı|16|2500|10|5000|
|Genel Amaçlı|32|5000|10|10000|
|Genel Amaçlı|64|10000|10|20000|
|Bellek için İyileştirilmiş|2|600|10|800|
|Bellek için İyileştirilmiş|4|1250|10|2500|
|Bellek için İyileştirilmiş|8|2500|10|5000|
|Bellek için İyileştirilmiş|16|5000|10|10000|
|Bellek için İyileştirilmiş|32|10000|10|20000|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> 1040 (08004). hata: Çok fazla sayıda bağlantı

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek üzere ProxySQL gibi bir bağlantı havuzlayıcı kullanmanızı öneririz.

MariaDB 'ye yeni istemci bağlantıları oluşturma zaman alır ve bir kez kurulduktan sonra bile bu bağlantılar veritabanı kaynaklarını kaplar. Çoğu uygulama, bu durumu çözer birçok kısa süreli bağlantı ister. Sonuç olarak gerçek iş yükünüz için daha az kaynak kullanılabilir ve performansı azaltıldı. Boştaki bağlantıları azaltan ve var olan bağlantıları yeniden kullanan bir bağlantı havuzlayıcı bunun önlenmesine yardımcı olur. ProxySQL 'i ayarlama hakkında bilgi edinmek için [Blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)ziyaret edin.

## <a name="query_cache_size"></a>query_cache_size

Sorgu önbelleği varsayılan olarak kapalıdır. Sorgu önbelleğini etkinleştirmek için `query_cache_type` parametresini yapılandırın. 

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|0|0|16777216|
|Genel Amaçlı|4|0|0|33554432|
|Genel Amaçlı|8|0|0|67108864|
|Genel Amaçlı|16|0|0|134217728|
|Genel Amaçlı|32|0|0|134217728|
|Genel Amaçlı|64|0|0|134217728|
|Bellek için İyileştirilmiş|2|0|0|33554432|
|Bellek için İyileştirilmiş|4|0|0|67108864|
|Bellek için İyileştirilmiş|8|0|0|134217728|
|Bellek için İyileştirilmiş|16|0|0|134217728|
|Bellek için İyileştirilmiş|32|0|0|134217728|

## <a name="sort_buffer_size"></a>sort_buffer_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|524288|32768|4194304|
|Genel Amaçlı|4|524288|32768|8388608|
|Genel Amaçlı|8|524288|32768|16777216|
|Genel Amaçlı|16|524288|32768|33554432|
|Genel Amaçlı|32|524288|32768|33554432|
|Genel Amaçlı|64|524288|32768|33554432|
|Bellek için İyileştirilmiş|2|524288|32768|8388608|
|Bellek için İyileştirilmiş|4|524288|32768|16777216|
|Bellek için İyileştirilmiş|8|524288|32768|33554432|
|Bellek için İyileştirilmiş|16|524288|32768|33554432|
|Bellek için İyileştirilmiş|32|524288|32768|33554432|

## <a name="join_buffer_size"></a>join_buffer_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|262144|128|268435455|
|Genel Amaçlı|4|262144|128|536870912|
|Genel Amaçlı|8|262144|128|1073741824|
|Genel Amaçlı|16|262144|128|2147483648|
|Genel Amaçlı|32|262144|128|4294967295|
|Genel Amaçlı|64|262144|128|4294967295|
|Bellek için İyileştirilmiş|2|262144|128|536870912|
|Bellek için İyileştirilmiş|4|262144|128|1073741824|
|Bellek için İyileştirilmiş|8|262144|128|2147483648|
|Bellek için İyileştirilmiş|16|262144|128|4294967295|
|Bellek için İyileştirilmiş|32|262144|128|4294967295|

## <a name="max_heap_table_size"></a>max_heap_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|16777216|16384|268435455|
|Genel Amaçlı|4|16777216|16384|536870912|
|Genel Amaçlı|8|16777216|16384|1073741824|
|Genel Amaçlı|16|16777216|16384|2147483648|
|Genel Amaçlı|32|16777216|16384|4294967295|
|Genel Amaçlı|64|16777216|16384|4294967295|
|Bellek için İyileştirilmiş|2|16777216|16384|536870912|
|Bellek için İyileştirilmiş|4|16777216|16384|1073741824|
|Bellek için İyileştirilmiş|8|16777216|16384|2147483648|
|Bellek için İyileştirilmiş|16|16777216|16384|4294967295|
|Bellek için İyileştirilmiş|32|16777216|16384|4294967295|

## <a name="tmp_table_size"></a>tmp_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer**|**En düşük değer**|**En büyük değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|16777216|1024|67108864|
|Genel Amaçlı|4|16777216|1024|134217728|
|Genel Amaçlı|8|16777216|1024|268435456|
|Genel Amaçlı|16|16777216|1024|536870912|
|Genel Amaçlı|32|16777216|1024|1073741824|
|Genel Amaçlı|64|16777216|1024|1073741824|
|Bellek için İyileştirilmiş|2|16777216|1024|134217728|
|Bellek için İyileştirilmiş|4|16777216|1024|268435456|
|Bellek için İyileştirilmiş|8|16777216|1024|536870912|
|Bellek için İyileştirilmiş|16|16777216|1024|1073741824|
|Bellek için İyileştirilmiş|32|16777216|1024|1073741824|

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Destekleniyor
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [BELLEK](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Desteklenmeyen
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [KARA DELIK](https://mariadb.com/kb/en/library/blackhole/)
- [ARŞIVLIYORSANıZ](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- DBA rol: birçok sunucu parametreleri ve ayarları yanlışlıkla sunucu performansının düşmesine neden veya DBMS ACID özelliklerini negate. Bu nedenle, bir ürün düzeyinde SLA ve hizmet bütünlüğü korumak için bu hizmeti DBA rol kullanıma sunmuyor. Yeni bir veritabanı örneği oluşturulduğunda bu oluşturulur, varsayılan kullanıcı hesabı, veritabanı yönetilen örneğine DDL ve DML deyimleri çoğunu gerçekleştirmek bu kullanıcı sağlar.
- Süper ayrıcalık: benzer [süper ayrıcalık](https://mariadb.com/kb/en/library/grant/#global-privileges) da kısıtlıdır.
- DEFINER: oluşturmak için süper ayrıcalıklar gerektirir ve kısıtlıdır. Bir yedekleme kullanarak veri içeri aktardıysanız, bir mysqldump gerçekleştirirken `CREATE DEFINER` komutlarını el ile veya `--skip-definer` komutunu kullanarak kaldırın.

## <a name="data-manipulation-statement-support"></a>Veri işleme ifadesi desteği

### <a name="supported"></a>Destekleniyor
- `LOAD DATA INFILE` desteklenir, ancak `[LOCAL]` parametresi belirtilmelidir ve bir UNC yoluna (SMB üzerinden bağlanmış Azure depolama) yönlendirilmelidir.

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusuyla aynı yapılandırmaları ile oluşturulur.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları, abonelik ve kaynak grubu genelinde dinamik olarak taşıma işlemi şu anda desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- MariaDB sunucu örneği, bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntülüyor. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MariaDB veritabanı sürümleri](concepts-supported-versions.md)
