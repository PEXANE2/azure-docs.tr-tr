---
title: Sınırlamalar-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda bağlantı ve depolama motoru seçeneklerinin sayısı gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: d4450689f6865c19436e437e09a3aa9f286c6e21
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653128"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı sınırlamaları
Aşağıdaki bölümlerde kapasiteyi, depolama altyapısı desteğini, ayrıcalık desteğini, veri işleme ekstresi desteğini ve veritabanı hizmetindeki işlev sınırlarını anlatmaktadır.

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
|Bellek İçin İyileştirilmiş|2|600|10|800|
|Bellek İçin İyileştirilmiş|4|1250|10|2500|
|Bellek İçin İyileştirilmiş|8|2500|10|5000|
|Bellek İçin İyileştirilmiş|16|5000|10|10000|
|Bellek İçin İyileştirilmiş|32|10000|10|20000|

Bağlantılar sınırı aştığında, şu hatayı alabilirsiniz:
> HATA 1040 (08004): çok fazla bağlantı

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek üzere ProxySQL gibi bir bağlantı havuzlayıcı kullanmanızı öneririz.

MariaDB 'ye yeni istemci bağlantıları oluşturma zaman alır ve bir kez kurulduktan sonra bile bu bağlantılar veritabanı kaynaklarını kaplar. Çoğu uygulama, bu durumu çözer birçok kısa süreli bağlantı ister. Sonuç olarak gerçek iş yükünüz için daha az kaynak kullanılabilir ve performansı azaltıldı. Boştaki bağlantıları azaltan ve var olan bağlantıları yeniden kullanan bir bağlantı havuzlayıcı bunun önlenmesine yardımcı olur. ProxySQL 'i ayarlama hakkında bilgi edinmek için [Blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)ziyaret edin.

### <a name="query_cache_size"></a>query_cache_size

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
|Bellek İçin İyileştirilmiş|2|0|0|33554432|
|Bellek İçin İyileştirilmiş|4|0|0|67108864|
|Bellek İçin İyileştirilmiş|8|0|0|134217728|
|Bellek İçin İyileştirilmiş|16|0|0|134217728|
|Bellek İçin İyileştirilmiş|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

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
|Bellek İçin İyileştirilmiş|2|524288|32768|8388608|
|Bellek İçin İyileştirilmiş|4|524288|32768|16777216|
|Bellek İçin İyileştirilmiş|8|524288|32768|33554432|
|Bellek İçin İyileştirilmiş|16|524288|32768|33554432|
|Bellek İçin İyileştirilmiş|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

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
|Bellek İçin İyileştirilmiş|2|262144|128|536870912|
|Bellek İçin İyileştirilmiş|4|262144|128|1073741824|
|Bellek İçin İyileştirilmiş|8|262144|128|2147483648|
|Bellek İçin İyileştirilmiş|16|262144|128|4294967295|
|Bellek İçin İyileştirilmiş|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

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
|Bellek İçin İyileştirilmiş|2|16777216|16384|536870912|
|Bellek İçin İyileştirilmiş|4|16777216|16384|1073741824|
|Bellek İçin İyileştirilmiş|8|16777216|16384|2147483648|
|Bellek İçin İyileştirilmiş|16|16777216|16384|4294967295|
|Bellek İçin İyileştirilmiş|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

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
|Bellek İçin İyileştirilmiş|2|16777216|1024|134217728|
|Bellek İçin İyileştirilmiş|4|16777216|1024|268435456|
|Bellek İçin İyileştirilmiş|8|16777216|1024|536870912|
|Bellek İçin İyileştirilmiş|16|16777216|1024|1073741824|
|Bellek İçin İyileştirilmiş|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Saat dilimi tabloları, `mysql.az_load_timezone` MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan saklı yordam çağırarak doldurulabilirler. Saklı yordamı çağırma ve küresel veya oturum düzeyi saat dilimlerini ayarlama hakkında [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) veya [Azure CLI](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter) makalelerine bakın.

### <a name="innodb_file_per_table"></a>innodb_file_per_table

MariaDB, InnoDB tablosunu tablo oluşturma sırasında verdiğiniz yapılandırmaya göre farklı Tablespaces içinde depolar. [Sistem tablo](https://mariadb.com/kb/en/innodb-system-tablespaces/) alanı, InnoDB veri sözlüğü için depolama alanıdır. [Tablo başına dosya tablosu](https://mariadb.com/kb/en/innodb-file-per-table-tablespaces/) , tek bir InnoDB tablosunun verilerini ve dizinlerini içerir ve dosya sisteminde kendi veri dosyasında depolanır. Bu davranış, `innodb_file_per_table` sunucu parametresi tarafından denetlenir. `innodb_file_per_table`İçin ayarı `OFF` , InnoDB 'in, sistem tablo tablosu 'nda tablo oluşturmasına neden olur. Aksi halde, InnoDB tablo başına tabloalanları içinde tablo oluşturur.

MariaDB için Azure veritabanı, tek bir veri dosyasında en büyük, **1 TB**'yi destekler. Veritabanınızın boyutu 1 TB 'den büyükse, tabloyu [innodb_file_per_table](https://mariadb.com/kb/en/innodb-system-variables/#innodb_file_per_table) tablo alanında oluşturmanız gerekir. 1 TB 'tan büyük tek bir tablo boyutunuz varsa, bölüm tablosunu kullanmanız gerekir.

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
- DBA rolü: çok sayıda sunucu parametresi ve ayarı, DBMS 'nin sunucu performansını veya Negate ACID özelliklerini yanlışlıkla düşürebilir. Bu nedenle, hizmet bütünlüğünü ve SLA 'yı bir ürün düzeyinde sürdürmek için, bu hizmet DBA rolünü kullanıma sunmaz. Yeni bir veritabanı örneği oluşturulduğunda oluşturulan varsayılan kullanıcı hesabı, bu kullanıcının yönetilen veritabanı örneğinde DDL ve DML deyimlerinin çoğunu gerçekleştirmesini sağlar.
- Süper ayrıcalık: benzer [süper ayrıcalık](https://mariadb.com/kb/en/library/grant/#global-privileges) da kısıtlıdır.
- DEFINER: oluşturmak için süper ayrıcalıklar gerektirir ve kısıtlıdır. Bir yedekleme kullanarak veri içeri aktardıysanız, `CREATE DEFINER` komutları el ile veya `--skip-definer` bir mysqldump gerçekleştirirken komutunu kullanarak kaldırın.

## <a name="data-manipulation-statement-support"></a>Veri işleme ekstresi desteği

### <a name="supported"></a>Destekleniyor
- `LOAD DATA INFILE`desteklenir, ancak `[LOCAL]` parametresi belirtilmelidir ve bır UNC yoluna (SMB üzerinden bağlanmış Azure Storage) yönlendirilmelidir.

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlevsel sınırlamalar

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanlarına ve bu katmanlara dinamik ölçeklendirme Şu anda desteklenmiyor.
- Sunucu depolama boyutunun düşürülmesi desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltmeleri
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş Şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- INR özelliği kullanılırken yeni sunucu, temel aldığı sunucuyla aynı yapılandırmalara sahip olarak oluşturulur.
- Silinen bir sunucunun geri yüklenmesi desteklenmez.

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları, abonelik ve kaynak grubu genelinde dinamik olarak taşıma işlemi şu anda desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal Ağ hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- MariaDB sunucu örneği, bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntülüyor. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MariaDB veritabanı sürümleri](concepts-supported-versions.md)
