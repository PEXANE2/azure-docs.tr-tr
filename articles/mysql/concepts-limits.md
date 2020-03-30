---
title: Sınırlamalar - MySQL için Azure Veritabanı
description: Bu makalede, MySQL için Azure Veritabanı'nda bağlantı sayısı ve depolama motoru seçenekleri gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: bc4694928eceed57692a0d4b0469543c1a8f9678
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532765"
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanında Sınırlamalar
Aşağıdaki bölümlerde veritabanı hizmetinde kapasite, depolama motoru desteği, ayrıcalık desteği, veri işleme bildirimi desteği ve işlevsel sınırlar açıklanmıştır. Ayrıca MySQL veritabanı altyapısı için geçerli [genel sınırlamalara](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) bakın.

## <a name="server-parameters"></a>Sunucu parametreleri

Birkaç popüler sunucu parametrelerinin minimum ve maksimum değerleri fiyatlandırma katmanı ve vCores tarafından belirlenir. Sınırlar için aşağıdaki tablolara bakın.

### <a name="max_connections"></a>max_connections

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|50|10|50|
|Temel|2|100|10|100|
|Genel Amaçlı|2|300|10|600|
|Genel Amaçlı|4|625|10|1250|
|Genel Amaçlı|8|1250|10|2500|
|Genel Amaçlı|16|2500|10|5000|
|Genel Amaçlı|32|5000|10|10000|
|Genel Amaçlı|64|10000|10|20000|
|Bellek İçin İyileştirilmiş|2|600|10|800|
|Bellek İçin İyileştirilmiş|4|1250|10|2500|
|Bellek İçin İyileştirilmiş|8|2500|10|5000|
|Bellek İçin İyileştirilmiş|16|5000|10|10000|
|Bellek İçin İyileştirilmiş|32|10000|10|20000|

Bağlantılar sınırı aştığında, aşağıdaki hatayı alabilirsiniz:
> HATA 1040 (08004): Çok fazla bağlantı

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek için ProxySQL gibi bir bağlantı havuzu kullanmanızı öneririz.

MySQL'e yeni istemci bağlantıları oluşturmak zaman alır ve kurulduktan sonra, bu bağlantılar boşta yken bile veritabanı kaynaklarını kaplar. Çoğu uygulama, bu durumu biraraya getiren birçok kısa süreli bağlantı isteğinde dir. Sonuç, performansın düşmesine yol açan gerçek iş yükünüz için daha az kaynak kullanılabilir. Boşta kalan bağlantıları azaltan ve varolan bağlantıları yeniden kullanan bir bağlantı havuzu bunu önlemeye yardımcı olur. ProxySQL kurulumu hakkında bilgi edinmek için [blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)ziyaret edin.

### <a name="query_cache_size"></a>query_cache_size

Sorgu önbelleği varsayılan olarak kapatılır. Sorgu önbelleğini etkinleştirmek için `query_cache_type` parametreyi yapılandırın. 

Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_query_cache_size) inceleyin.

> [!NOTE]
> Sorgu önbelleği MySQL 5.7.20 itibariyle amortismana kaldırıldı ve MySQL 8.0'da kaldırıldı

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|0|0|16777216|
|Genel Amaçlı|4|0|0|33554432|
|Genel Amaçlı|8|0|0|67108864|
|Genel Amaçlı|16|0|0|134217728|
|Genel Amaçlı|32|0|0|134217728|
|Genel Amaçlı|64|0|0|134217728|
|Bellek İçin İyileştirilmiş|2|0|0|33554432|
|Bellek İçin İyileştirilmiş|4|0|0|67108864|
|Bellek İçin İyileştirilmiş|8|0|0|134217728|
|Bellek İçin İyileştirilmiş|16|0|0|134217728|
|Bellek İçin İyileştirilmiş|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_sort_buffer_size) inceleyin.

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|524288|32768|4194304|
|Genel Amaçlı|4|524288|32768|8388608|
|Genel Amaçlı|8|524288|32768|16777216|
|Genel Amaçlı|16|524288|32768|33554432|
|Genel Amaçlı|32|524288|32768|33554432|
|Genel Amaçlı|64|524288|32768|33554432|
|Bellek İçin İyileştirilmiş|2|524288|32768|8388608|
|Bellek İçin İyileştirilmiş|4|524288|32768|16777216|
|Bellek İçin İyileştirilmiş|8|524288|32768|33554432|
|Bellek İçin İyileştirilmiş|16|524288|32768|33554432|
|Bellek İçin İyileştirilmiş|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_join_buffer_size) inceleyin.

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|262144|128|268435455|
|Genel Amaçlı|4|262144|128|536870912|
|Genel Amaçlı|8|262144|128|1073741824|
|Genel Amaçlı|16|262144|128|2147483648|
|Genel Amaçlı|32|262144|128|4294967295|
|Genel Amaçlı|64|262144|128|4294967295|
|Bellek İçin İyileştirilmiş|2|262144|128|536870912|
|Bellek İçin İyileştirilmiş|4|262144|128|1073741824|
|Bellek İçin İyileştirilmiş|8|262144|128|2147483648|
|Bellek İçin İyileştirilmiş|16|262144|128|4294967295|
|Bellek İçin İyileştirilmiş|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_max_heap_table_size) inceleyin.

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|16777216|16384|268435455|
|Genel Amaçlı|4|16777216|16384|536870912|
|Genel Amaçlı|8|16777216|16384|1073741824|
|Genel Amaçlı|16|16777216|16384|2147483648|
|Genel Amaçlı|32|16777216|16384|4294967295|
|Genel Amaçlı|64|16777216|16384|4294967295|
|Bellek İçin İyileştirilmiş|2|16777216|16384|536870912|
|Bellek İçin İyileştirilmiş|4|16777216|16384|1073741824|
|Bellek İçin İyileştirilmiş|8|16777216|16384|2147483648|
|Bellek İçin İyileştirilmiş|16|16777216|16384|4294967295|
|Bellek İçin İyileştirilmiş|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_tmp_table_size) inceleyin.

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**Varsayılan değer**|**Min değeri**|**Maksimum değer**|
|---|---|---|---|---|
|Temel|1|Temel katmanda yapılandırılamaz|Yok|Yok|
|Temel|2|Temel katmanda yapılandırılamaz|Yok|Yok|
|Genel Amaçlı|2|16777216|1024|67108864|
|Genel Amaçlı|4|16777216|1024|134217728|
|Genel Amaçlı|8|16777216|1024|268435456|
|Genel Amaçlı|16|16777216|1024|536870912|
|Genel Amaçlı|32|16777216|1024|1073741824|
|Genel Amaçlı|64|16777216|1024|1073741824|
|Bellek İçin İyileştirilmiş|2|16777216|1024|134217728|
|Bellek İçin İyileştirilmiş|4|16777216|1024|268435456|
|Bellek İçin İyileştirilmiş|8|16777216|1024|536870912|
|Bellek İçin İyileştirilmiş|16|16777216|1024|1073741824|
|Bellek İçin İyileştirilmiş|32|16777216|1024|1073741824|

## <a name="storage-engine-support"></a>Depolama motoru desteği

### <a name="supported"></a>Destekleniyor
- [ınnodb](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [Bellek](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmeyen
- [Myısam](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [Blackhole](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [Arşiv](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [Federe](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- DBA rolü: Birçok sunucu parametreve ayarı yanlışlıkla sunucu performansını düşürebilir veya DBMS'nin ACID özelliklerini inkâr edebilir. Bu nedenle, hizmet bütünlüğünü ve SLA'yı ürün düzeyinde korumak için bu hizmet DBA rolünü ortaya çıkarmaz. Yeni bir veritabanı örneği oluşturulduğunda oluşturulan varsayılan kullanıcı hesabı, kullanıcının yönetilen veritabanı örneğinde DDL ve DML ekstrelerinin çoğunu gerçekleştirmesine olanak tanır. 
- SÜPER ayrıcalık: Benzer super [ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) da sınırlıdır.
- DEFINER: Oluşturmak için süper ayrıcalıklar gerektirir ve sınırlıdır. Bir yedekleme kullanarak veri alma, `CREATE DEFINER` el ile veya bir `--skip-definer` mysqldump gerçekleştirirken komutu kullanarak komutları kaldırın.

## <a name="data-manipulation-statement-support"></a>Veri işleme bildirimi desteği

### <a name="supported"></a>Destekleniyor
- `LOAD DATA INFILE`desteklenir, ancak `[LOCAL]` parametre belirtilmeli ve bir UNC yoluna yönlendirilmelidir (SMB aracılığıyla monte edilmiş Azure depolama).

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Fonksiyonel sınırlamalar

### <a name="scale-operations"></a>Ölçek işlemleri
- Temel fiyatlandırma katmanlarına ve temel fiyatlandırma katmanlarından dinamik ölçeklendirme şu anda desteklenmez.
- Azalan sunucu depolama boyutu desteklenmez.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltmeleri
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmez. Bir sonraki ana sürüme yükseltmek istiyorsanız, [bir döküm](./concepts-migrate-dump-restore.md) alın ve yeni motor sürümü ile oluşturulan bir sunucuya geri yükleyin.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- PITR özelliğini kullanırken, yeni sunucu dayandığı sunucuyla aynı yapılandırmalarla oluşturulur.
- Silinen bir sunucunun geri dinlenmesi desteklenmez.

### <a name="vnet-service-endpoints"></a>Sanal Ağ hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Güncel bilinen sorunlar
- MySQL sunucu örneği bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntüler. Doğru sunucu örneği motoru sürümünü almak `select version();` için komutu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında neler mevcuttur?](concepts-pricing-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
