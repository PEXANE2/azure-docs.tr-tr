---
title: Autovacuum optimize - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL için bir Azure Veritabanında otomatik boşluğu nasıl optimize edebileceğiniz açıklanmaktadır - Tek Sunucu
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 1917bd6744e100db54fe959292e29486f8a1784b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770195"
---
# <a name="optimize-autovacuum-on-an-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında otomatik vakum u optimize edin - Tek Sunucu
Bu makalede, PostgreSQL sunucusu için bir Azure Veritabanı'nda otomatik boşluğun etkin bir şekilde nasıl optimize edilebildiğini açıklanmaktadır.

## <a name="overview-of-autovacuum"></a>Otomatik vakuma genel bakış
PostgreSQL, daha fazla veritabanı eşzamanlılığı sağlamak için çoklu sürüm eşzamanlılık denetimi (MVCC) kullanır. Her güncelleştirme bir ekleme ve silme ile sonuçlanır ve her silme, satırsilme için yumuşak işaretlenmiş satırlarla sonuçlanır. Yumuşak işaretleme, daha sonra temizlenecek ölü tupülleri tanımlar. Bu görevleri gerçekleştirmek için PostgreSQL bir vakum işi çalıştırın.

Bir vakum işi el ile veya otomatik olarak tetiklenebilir. Veritabanı nda ağır güncelleştirme veya silme işlemleri olduğunda daha fazla ölü tupül bulunur. Veritabanı boşta yken daha az ölü tupül vardır. Veritabanı yükü ağır olduğunda daha sık vakum lamanız gerekir, bu da vakum işlerini *manuel olarak* çalıştırmayı rahatsız edici hale getirir.

Otomatik vakum yapılandırılabilir ve atoklama nın yararları. PostgreSQL'in gönderim yaptığı varsayılan değerler, ürünün her türlü cihazda çalıştığından emin olmaya çalışır. Bu cihazlar Ahududu içerir. İdeal yapılandırma değerleri aşağıdakideğerlere bağlıdır:
- SKU ve depolama boyutu gibi toplam kullanılabilir kaynak.
- Kaynak kullanımı.
- Bireysel nesne özellikleri.

## <a name="autovacuum-benefits"></a>Otovakum faydaları
Zaman zaman vakum yoksa, biriken ölü tuples neden olabilir:
- Daha büyük veritabanları ve tablolar gibi veri şişkinliği.
- Daha büyük suboptimal dizinler.
- Artırılmış G/Ç.

## <a name="monitor-bloat-with-autovacuum-queries"></a>Otovakum sorguları ile şişirme monitör
Aşağıdaki örnek sorgu, XYZ adlı bir tabloda ölü ve canlı tuples sayısını belirlemek için tasarlanmıştır:
 
    'SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup/ n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables WHERE relname = 'XYZ' order by n_dead_tup DESC;'

## <a name="autovacuum-configurations"></a>Otomatik vakum konfigürasyonları
Otomatik vakum denetimi yapılandırma parametreleri iki anahtar sorunun yanıtlarına dayanır:
- Ne zaman başlamalı?
- Başladıktan sonra ne kadar temizlemeli?

Aşağıda, önceki sorulara göre güncelleştirebileceğiniz bazı otomatik vakum yapılandırma parametrelerinin yanı sıra bazı kılavuzlar verilmiştir.

Parametre|Açıklama|Varsayılan değer
---|---|---
autovacuum_vacuum_threshold|Herhangi bir tabloda bir vakum işlemini tetiklemek için gereken en az güncelleştirilmiş veya silinmiş tuples sayısını belirtir. Varsayılan değer 50 tuples'tir. Bu parametreyi yalnızca postgresql.conf dosyasında veya sunucu komut satırında ayarlayın. Tabloların ayarını geçersiz kılmak için tablo depolama parametrelerini değiştirin.|50
autovacuum_vacuum_scale_factor|Bir vakum işlemini tetikleyip tetiklemeye karar verirken autovacuum_vacuum_threshold eklemek için tablo boyutunun bir kısmını belirtir. Varsayılan değer, tablo boyutunun yüzde 20'si olan 0,2'dir. Bu parametreyi yalnızca postgresql.conf dosyasında veya sunucu komut satırında ayarlayın. Tabloların ayarını geçersiz kılmak için tablo depolama parametrelerini değiştirin.|Yüzde 5
autovacuum_vacuum_cost_limit|Otomatik vakum işlemlerinde kullanılan maliyet sınır değerini belirtir. -1 belirtilirse, varsayılan değer, normal vacuum_cost_limit değeri kullanılır. Birden fazla işçi varsa, değer çalışan otovakum işçileri arasında orantılı olarak dağıtılır. Her işçi için sınırların toplamı bu değişkenin değerini aşmaz. Bu parametreyi yalnızca postgresql.conf dosyasında veya sunucu komut satırında ayarlayın. Tabloların ayarını geçersiz kılmak için tablo depolama parametrelerini değiştirin.|-1
autovacuum_vacuum_cost_delay|Otomatik vakum işlemlerinde kullanılan maliyet gecikme değerini belirtir. -1 belirtilirse, normal vacuum_cost_delay değeri kullanılır. Varsayılan değer 20 milisaniyedir. Bu parametreyi yalnızca postgresql.conf dosyasında veya sunucu komut satırında ayarlayın. Tabloların ayarını geçersiz kılmak için tablo depolama parametrelerini değiştirin.|20 ms
autovacuum_nap_time|Herhangi bir veritabanında otomatik vakum çalışır arasındaki minimum gecikme belirtir. Her turda, daemon veritabanını inceler ve bu veritabanındaki tablolar için gerektiği gibi VAKUM ve ANALYZE komutları yayınlar. Gecikme saniye cinsinden ölçülür ve varsayılan değer bir dakikadır (1 dk). Bu parametreyi yalnızca postgresql.conf dosyasında veya sunucu komut satırında ayarlayın.|15 s
autovacuum_max_workers|Herhangi bir anda çalıştırılabilen otomatik vakum başlatıcısı dışındaki maksimum otomatik vakum işlemi sayısını belirtir. Varsayılan değer üçeder. Bu parametreyi yalnızca sunucu başlangıcında ayarlayın.|3

Tabloların ayarlarını geçersiz kılmak için tablo depolama parametrelerini değiştirin. 

## <a name="autovacuum-cost"></a>Otovakum maliyeti
Burada bir vakum işlemi çalışan "maliyetleri" şunlardır:

- Vakumun çalıştığı veri sayfaları kilitli.
- Bir vakum işi çalışırken hesaplama ve bellek kullanılır.

Sonuç olarak, vakum işleri çok sık veya çok seyrek çalıştırmayın. Bir vakum işi iş yüküne uyum sağlamalı. Her birinin dengeleri nedeniyle tüm otomatik vakum parametre değişikliklerini test edin.

## <a name="autovacuum-start-trigger"></a>Otomatik vakum başlangıç tetikleyicisi
Ölü tuples sayısı autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor * reltuples aştığında otomatik vakum tetiklenir. Burada, reltuples bir sabittir.

Otomatik vakumdan temizleme veritabanı yüküne ayak uydurmalıdır. Aksi takdirde, depolama alanıtünuz tükenebilir ve sorgularda genel bir yavaşlama yaşayabilirsiniz. Zaman içinde amortismana tabi, bir vakum işleminin ölü tupülleri temizleme hızı, ölü tupüllerin oluşturulma hızına eşit olmalıdır.

Birçok güncelleştirme ve silme içeren veritabanlarında daha fazla ölü tuples vardır ve daha fazla alana ihtiyaç vardır. Genellikle, birçok güncelleştirmeve siler içeren veritabanları, autovacuum_vacuum_scale_factor ve autovacuum_vacuum_threshold düşük değerlerinden yararlanır. Düşük değerler ölü tuples uzun süreli birikimini önler. Vakumlama gereksinimi daha az acil olduğundan, daha küçük veritabanları ile her iki parametre için daha yüksek değerler kullanabilirsiniz. Sık vakumlama hesaplama ve bellek pahasına gelir.

Yüzde 20'lik varsayılan ölçek faktörü, ölü tuples düşük yüzdesi ile tablolarda iyi çalışır. Ölü tuples yüksek bir yüzdesi ile tablolarda iyi çalışmıyor. Örneğin, 20 GB'lık bir tabloda, bu ölçek faktörü 4 GB ölü tupül anlamına gelir. 1-TB masasında, 200 GB ölü tuples var.

PostgreSQL ile bu parametreleri tablo düzeyinde veya örnek düzeyinde ayarlayabilirsiniz. Bugün, bu parametreleri yalnızca PostgreSQL için Azure Veritabanı'nda tablo düzeyinde ayarlayabilirsiniz.

## <a name="estimate-the-cost-of-autovacuum"></a>Otovakum maliyetini tahmin edin
Otomatik vakum çalıştırmak "maliyetlidir" ve vakum işlemlerinin çalışma süresini denetlemek için parametreler vardır. Aşağıdaki parametreler vakum çalışma maliyetini tahmin yardımcı olur:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

Vakum işlemi fiziksel sayfaları okur ve ölü tuples için kontroleder. shared_buffers'daki her sayfanın maliyeti 1 (vacuum_cost_page_hit) olarak kabul edilir. Ölü tuples varsa, diğer tüm sayfaların 20 (vacuum_cost_page_dirty) veya ölü tuples yoksa 10 (vacuum_cost_page_miss) bir maliyeti olduğu kabul edilir. İşlem autovacuum_vacuum_cost_limit aştığında vakum işlemi durur. 

Sınıra ulaşıldıktan sonra, işlem yeniden başlamadan önce autovacuum_vacuum_cost_delay parametre tarafından belirtilen süre boyunca uyur. Sınıra ulaşılmazsa, otomatik vakum autovacuum_nap_time parametresi tarafından belirtilen değerden sonra başlar.

Özetle, autovacuum_vacuum_cost_delay ve autovacuum_vacuum_cost_limit parametreleri birim zaman başına ne kadar veri temizlemeye izin verildiğini denetler. Varsayılan değerlerin çoğu fiyatlandırma katmanı için çok düşük olduğunu unutmayın. Bu parametreler için en uygun değerler fiyatlandırma katmanına bağlıdır ve buna göre yapılandırılmalıdır.

autovacuum_max_workers parametresi, aynı anda çalıştırılabilen maksimum otomatik vakum işlemi sayısını belirler.

PostgreSQL ile bu parametreleri tablo düzeyinde veya örnek düzeyinde ayarlayabilirsiniz. Bugün, bu parametreleri yalnızca PostgreSQL için Azure Veritabanı'nda tablo düzeyinde ayarlayabilirsiniz.

## <a name="optimize-autovacuum-per-table"></a>Otosüpürgeyi masa başına optimize edin
Tablo başına önceki tüm yapılandırma parametrelerini yapılandırabilirsiniz. Bir örneği aşağıda verilmiştir:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Otomatik vakum, masa başına eşzamanlı bir işlemdir. Ölü tuples büyük yüzdesi bir tablo var, otovakum için yüksek "maliyet" . Yüksek güncelleştirme oranına sahip ve silen tabloları birden çok tabloya bölebilirsiniz. Tabloları nikiyelize etmek ve otomatik vakumu tek bir masada tamamlamak için "maliyeti" azaltmaya yardımcı olur. Ayrıca, işçilerin serbestçe zamanlanmasından emin olmak için paralel otovakum işçisi sayısını artırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Otomatik vakumun nasıl kullanılacağı ve ayarını yapmak hakkında daha fazla bilgi edinmek için aşağıdaki PostgreSQL belgelerine bakın:

 - [Bölüm 18, Sunucu yapılandırması](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - [Bölüm 24, Rutin veritabanı bakım görevleri](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
