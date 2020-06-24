---
title: Sunucu parametreleri-MariaDB için Azure veritabanı
description: Bu konuda, MariaDB için Azure veritabanı 'nda sunucu parametrelerini yapılandırmaya yönelik yönergeler sağlanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/8/2020
ms.openlocfilehash: 5ea5a94c73adda1328b97c3e985c4b6ac614d1b9
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85255033"
---
# <a name="server-parameters-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda sunucu parametreleri

Bu makalede, MariaDB için Azure veritabanı 'nda sunucu parametrelerini yapılandırmaya yönelik konular ve yönergeler sağlanmaktadır.

## <a name="what-are-server-parameters"></a>Sunucu parametreleri nelerdir? 

MariaDB altyapısı, altyapının davranışını yapılandırmak ve ayarlamak için kullanılabilecek birçok farklı sunucu değişkeni/parametresi sağlar. Bazı parametreler, çalışma zamanında dinamik olarak ayarlanabilir, diğerleri ise "statik" olarak ayarlanabilir, bu da bir sunucu yeniden başlatılmasını gerektirir.

MariaDB için Azure veritabanı, iş yükünüzün ihtiyaçlarına uyması için [Azure Portal](./howto-server-parameters.md), [Azure CLI](./howto-configure-server-parameters-cli.md)ve [PowerShell](./howto-configure-server-parameters-using-powershell.md) 'i kullanarak çeşitli MariaDB sunucu parametrelerinin değerini değiştirme özelliğini kullanıma sunar.

## <a name="configurable-server-parameters"></a>Yapılandırılabilir sunucu parametreleri

Desteklenen sunucu parametrelerinin listesi sürekli olarak büyüyordur. Tam listeyi görüntülemek ve sunucu parametreleri değerlerini yapılandırmak için Azure portal sunucu parametreleri sekmesini kullanın.

Yaygın olarak güncellenen çeşitli sunucu parametrelerinin sınırları hakkında daha fazla bilgi edinmek için aşağıdaki bölümlere bakın. Sınırlar, sunucunun fiyatlandırma katmanı ve sanal çekirdekleri tarafından belirlenir.

### <a name="innodb_buffer_pool_size"></a>innodb_buffer_pool_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/innodb-system-variables/#innodb_buffer_pool_size) gözden geçirin.

#### <a name="servers-supporting-up-to-4-tb-storage"></a>4 TB 'a kadar depolamayı destekleyen sunucular

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
|---|---|---|---|---|
|Temel|1|872415232|134217728|872415232|
|Temel|2|2684354560|134217728|2684354560|
|Genel Amaçlı|2|3758096384|134217728|3758096384|
|Genel Amaçlı|4|8053063680|134217728|8053063680|
|Genel Amaçlı|8|16106127360|134217728|16106127360|
|Genel Amaçlı|16|32749125632|134217728|32749125632|
|Genel Amaçlı|32|66035122176|134217728|66035122176|
|Genel Amaçlı|64|132070244352|134217728|132070244352|
|Bellek İçin İyileştirilmiş|2|7516192768|134217728|7516192768|
|Bellek İçin İyileştirilmiş|4|16106127360|134217728|16106127360|
|Bellek İçin İyileştirilmiş|8|32212254720|134217728|32212254720|
|Bellek İçin İyileştirilmiş|16|65498251264|134217728|65498251264|
|Bellek İçin İyileştirilmiş|32|132070244352|134217728|132070244352|

#### <a name="servers-support-up-to-16-tb-storage"></a>Sunucular 16 TB 'a kadar depolamayı destekler

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
|---|---|---|---|---|
|Temel|1|872415232|134217728|872415232|
|Temel|2|2684354560|134217728|2684354560|
|Genel Amaçlı|2|7516192768|134217728|7516192768|
|Genel Amaçlı|4|16106127360|134217728|16106127360|
|Genel Amaçlı|8|32212254720|134217728|32212254720|
|Genel Amaçlı|16|65498251264|134217728|65498251264|
|Genel Amaçlı|32|132070244352|134217728|132070244352|
|Genel Amaçlı|64|264140488704|134217728|264140488704|
|Bellek İçin İyileştirilmiş|2|15032385536|134217728|15032385536|
|Bellek İçin İyileştirilmiş|4|32212254720|134217728|32212254720|
|Bellek İçin İyileştirilmiş|8|64424509440|134217728|64424509440|
|Bellek İçin İyileştirilmiş|16|130996502528|134217728|130996502528|
|Bellek İçin İyileştirilmiş|32|264140488704|134217728|264140488704|

### <a name="innodb_file_per_table"></a>innodb_file_per_table

> [!NOTE]
> `innodb_file_per_table`yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarında güncelleştirilebilen bir şekilde yapılandırılabilir.

MariaDB, InnoDB tablosunu tablo oluşturma sırasında verdiğiniz yapılandırmaya göre farklı Tablespaces içinde depolar. [Sistem tablo](https://mariadb.com/kb/en/innodb-system-tablespaces/) alanı, InnoDB veri sözlüğü için depolama alanıdır. [Tablo başına dosya tablosu](https://mariadb.com/kb/en/innodb-file-per-table-tablespaces/) , tek bir InnoDB tablosunun verilerini ve dizinlerini içerir ve dosya sisteminde kendi veri dosyasında depolanır. Bu davranış, `innodb_file_per_table` sunucu parametresi tarafından denetlenir. `innodb_file_per_table`İçin ayarı `OFF` , InnoDB 'in, sistem tablo tablosu 'nda tablo oluşturmasına neden olur. Aksi halde, InnoDB tablo başına tabloalanları içinde tablo oluşturur.

MariaDB için Azure veritabanı, tek bir veri dosyasında en büyük, **1 TB**'yi destekler. Veritabanınızın boyutu 1 TB 'den büyükse, tabloyu [innodb_file_per_table](https://mariadb.com/kb/en/innodb-system-variables/#innodb_file_per_table) tablo alanında oluşturmanız gerekir. 1 TB 'tan büyük tek bir tablo boyutunuz varsa, bölüm tablosunu kullanmanız gerekir.

### <a name="join_buffer_size"></a>join_buffer_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
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
|Bellek İçin İyileştirilmiş|2|625|10|1250|
|Bellek İçin İyileştirilmiş|4|1250|10|2500|
|Bellek İçin İyileştirilmiş|8|2500|10|5000|
|Bellek İçin İyileştirilmiş|16|5000|10|10000|
|Bellek İçin İyileştirilmiş|32|10000|10|20000|

Bağlantılar sınırı aştığında, şu hatayı alabilirsiniz:
> HATA 1040 (08004): çok fazla bağlantı

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek üzere ProxySQL gibi bir bağlantı havuzlayıcı kullanmanızı öneririz.

MariaDB 'ye yeni istemci bağlantıları oluşturma zaman alır ve bir kez kurulduktan sonra bile bu bağlantılar veritabanı kaynaklarını kaplar. Çoğu uygulama, bu durumu çözer birçok kısa süreli bağlantı ister. Sonuç olarak gerçek iş yükünüz için daha az kaynak kullanılabilir ve performansı azaltıldı. Boştaki bağlantıları azaltan ve var olan bağlantıları yeniden kullanan bir bağlantı havuzlayıcı bunun önlenmesine yardımcı olur. ProxySQL 'i ayarlama hakkında bilgi edinmek için [Blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)ziyaret edin.

>[!Note]
>ProxySQL açık kaynaklı bir topluluk aracıdır. En iyi çaba temelinde Microsoft tarafından desteklenir. Yetkili kılavuzlarla üretime yönelik destek almak için, [Proxysql ürün desteğini](https://proxysql.com/services/support/)değerlendirebilir ve bunlarla iletişime ulaşabilirsiniz.

### <a name="max_heap_table_size"></a>max_heap_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
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

### <a name="query_cache_size"></a>query_cache_size

Sorgu önbelleği, MariaDB içinde parametresiyle varsayılan olarak etkindir `have_query_cache` . 

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|* * En büyük değer * *|
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

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
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

### <a name="tmp_table_size"></a>tmp_table_size

Bu parametre hakkında daha fazla bilgi edinmek için [MariaDB belgelerini](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) gözden geçirin.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**Varsayılan değer (bayt)**|**En az değer (bayt)**|**En büyük değer (bayt)**|
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

## <a name="non-configurable-server-parameters"></a>Yapılandırılamayan sunucu parametreleri

Aşağıdaki sunucu parametreleri hizmette yapılandırılamaz:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|6/E|
|innodb_log_files_in_group|2|

Burada listelenmeyen diğer sunucu parametreleri, [MariaDB](https://mariadb.com/kb/en/server-system-variables/)'nin varsayılan dışı varsayılan değerlerine ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal kullanarak sunucu parametrelerini yapılandırmayı](./howto-server-parameters.md) öğrenin
- [Azure CLI kullanarak sunucu parametrelerini yapılandırmayı](./howto-configure-server-parameters-cli.md) öğrenin
- [PowerShell kullanarak sunucu parametrelerini yapılandırmayı](./howto-configure-server-parameters-using-powershell.md) öğrenin