---
title: Query Store - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Sorgu Deposu özelliği açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/14/2019
ms.openlocfilehash: ccc503e6718ee8f516920cfbea3ad86e7ed81d84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768274"
---
# <a name="monitor-performance-with-the-query-store"></a>Sorgu Deposu ile performansı izleme

**Aşağıdakiler için geçerlidir:** PostgreSQL için Azure Veritabanı - Tek Sunucu sürümleri 9.6, 10, 11

PostgreSQL için Azure Veritabanı'ndaki Sorgu Mağazası özelliği, zaman içinde sorgu performansını izlemenin bir yolunu sağlar. Sorgu Mağazası, en uzun süre çalışan ve en yoğun kaynak sorgularını hızla bulmanıza yardımcı olarak performans sorun giderme olayını kolaylaştırır. Sorgu Mağazası sorguların ve çalışma zamanı istatistiklerinin geçmişini otomatik olarak yakalar ve bunları incelemeniz için saklar. Veritabanı kullanım desenlerini görebilmeniz için verileri zaman pencerelerine göre ayırır. Tüm kullanıcılar, veritabanları ve sorgular için veriler, PostgreSQL örneği için Azure Veritabanı'nda **azure_sys** adlı bir veritabanında depolanır.

> [!IMPORTANT]
> **azure_sys** veritabanını veya şemalarını değiştirmeyin. Bunu yapmak Sorgu Mağazası ve ilgili performans özelliklerinin düzgün çalışmasını engeller.

## <a name="enabling-query-store"></a>Sorgu Deposu'nı Etkinleştirme
Query Store bir opt-in özelliğidir, bu nedenle varsayılan olarak bir sunucuda etkin değildir. Mağaza, belirli bir sunucudaki tüm veritabanları için etkin veya genel olarak devre dışı bırakılır ve veritabanı başına açılamaz veya kapatılamaz.

### <a name="enable-query-store-using-the-azure-portal"></a>Azure portalını kullanarak Sorgu Mağazası'nı etkinleştirme
1. Azure portalında oturum açın ve PostgreSQL sunucusu için Azure Veritabanınızı seçin.
2. Menünün **Ayarlar** bölümünde **Sunucu Parametreleri'ni** seçin.
3. Parametreyi `pg_qs.query_capture_mode` arayın.
4. Değeri ayarla `TOP` ve **Kaydet.**

Sorgu Deposu'nda bekleme istatistiklerini etkinleştirmek için: 
1. Parametreyi `pgms_wait_sampling.query_capture_mode` arayın.
1. Değeri ayarla `ALL` ve **Kaydet.**


Alternatif olarak, bu parametreleri Azure CLI'yi kullanarak ayarlayabilirsiniz.
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
az postgres server configuration set --name pgms_wait_sampling.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value ALL
```

İlk toplu iş verilerinin azure_sys veritabanında kalıcı olması için 20 dakikaya izin verin.

## <a name="information-in-query-store"></a>Sorgu Deposu'ndaki Bilgiler
Sorgu Mağazası'nın iki mağazası vardır:
- Sorgu yürütme istatistikleri bilgilerini kalıcı olarak sürdürmek için bir çalışma zamanı istatistikleri deposu.
- Bekleme istatistikleri bilgilerinin kalıcılığı için bekleme istatistikleri deposu.

Query Store'u kullanmak için sık karşılaşılan senaryolar şunlardır:
- Belirli bir zaman penceresinde sorgunun kaç kez yürütüldedildiğini belirleme
- Büyük deltaları görmek için zaman pencerelerinde bir sorgunun ortalama yürütme süresini karşılaştırma
- Son X saatiçinde en uzun süre çalışan sorguları tanımlama
- Kaynaklarda bekleyen en iyi N sorgularını tanımlama
- Belirli bir sorgu için bekleme yapısını anlama

Alan kullanımını en aza indirmek için, çalışma zamanı istatistikleri deposundaki çalışma zamanı yürütme istatistikleri sabit, yapılandırılabilir bir zaman penceresi üzerinde toplanır. Bu mağazalardaki bilgiler sorgu deposu görünümlerini sorgulayarak görünür.

## <a name="access-query-store-information"></a>Erişim Sorgu Deposu bilgileri

Sorgu Deposu verileri Postgres sunucunuzdaki azure_sys veritabanında depolanır. 

Aşağıdaki sorgu Sorgu Deposu'ndaki sorgularla ilgili bilgileri döndürür:
```sql
SELECT * FROM query_store.qs_view; 
``` 

Veya bekleme istatistikleri için bu sorgu:
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

Ayrıca, analitik ve uyarı için Sorgu Deposu verilerini [Azure Monitor Günlükleri'ne,](../azure-monitor/log-query/log-query-overview.md) akış için Olay Hub'larına ve arşivleme için Azure Depolama'ya da yatabilirsiniz. Yapılandırmak için günlük **kategorileri QueryStoreRuntimeİstatistik** ve **QueryStoreWaitStatistics**vardır. Kurulum hakkında bilgi edinmek için [Azure Monitörü tanı ayarları](../azure-monitor/platform/diagnostic-settings.md) makalesini ziyaret edin.


## <a name="finding-wait-queries"></a>Bekleme sorgularını bulma
Bekleme olay türleri, farklı bekleme olaylarını benzerliğe göre kovalarda birleştirir. Sorgu Deposu bekleme olay türünü, belirli bekleme olay adını ve söz konusu sorguyu sağlar. Bu bekleme bilgilerini sorgu çalışma zamanı istatistikleriyle ilişkilendirebilmek, sorgu performansı özelliklerine neyin katkıda bulundugun daha derin bir anlayış kazanabileceğiniz anlamına gelir.

Sorgu Deposu'ndaki bekleme istatistiklerini kullanarak iş yükünüz hakkında daha fazla bilgi edinmenize ilişkin bazı örnekler aşağıda verilmiştir:

| **Gözlem** | **Eylem** |
|---|---|
|Yüksek Kilit bekler | Etkilenen sorgular için sorgu metinlerini denetleyin ve hedef varlıkları tanımlayın. Sık sık yürütülen ve/veya yüksek süreye sahip aynı varlığı değiştiren diğer sorgular için Sorgu Deposu'na bakın. Bu sorguları tanımladıktan sonra, eşzamanlılığı geliştirmek için uygulama mantığını değiştirmeyi düşünün veya daha az kısıtlayıcı bir yalıtım düzeyi kullanın.|
| Yüksek Arabellek IO bekler | Sorgu Deposu'nda çok sayıda fiziksel okuma içeren sorguları bulun. Sorguları yüksek IO beklemeleriyle eşleşirlerse, taramalar yerine arama yapmak için temel varlık üzerinde bir dizin sunmayı düşünün. Bu, sorguların IO genel havasını en aza indirir. Bu sunucu için sorguları en iyi duruma getirecek dizin önerileri olup olmadığını görmek için portaldaki sunucunuzun **Performans Önerileri'ni** denetleyin.|
| Yüksek Bellek bekler | Sorgu Deposu'nda en iyi bellek tüketen sorguları bulun. Bu sorgular büyük olasılıkla etkilenen sorguların daha fazla ilerlemesini geciktiriyor. Bu sorguları en iyi duruma getirecek dizin önerileri olup olmadığını görmek için portaldaki sunucunuzun **Performans Önerileri'ni** denetleyin.|

## <a name="configuration-options"></a>Yapılandırma seçenekleri
Sorgu Deposu etkinleştirildiğinde, 15 dakikalık toplama pencerelerinde, pencere başına 500'e kadar farklı sorguda veri kaydeder. 

Sorgu Deposu parametrelerini yapılandırmak için aşağıdaki seçenekler kullanılabilir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı**|
|---|---|---|---|
| pg_qs.query_capture_mode | Hangi ifadelerin izlendiği ne kadar ayarlar. | yok | yok, üst, tüm |
| pg_qs.max_query_text_length | Kaydedilebilen maksimum sorgu uzunluğunu ayarlar. Daha uzun sorgular kesilir. | 6000 | 100 - 10K arası |
| pg_qs.retention_period_in_days | Bekletme süresini ayarlar. | 7 | 1 - 30 |
| pg_qs.track_utility | Yardımcı program komutlarının izlenip izlenmediğini ayarlar | on | açık, kapama |

Aşağıdaki seçenekler özellikle bekleme istatistikleri için geçerlidir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | Bekleme istatistikleri için izlenen ifadeleri ayarlar. | yok | hiçbiri, tüm|
| Pgms_wait_sampling.history_period | Bekleme olaylarının örneklendiği sıklığı milisaniye cinsinden ayarlayın. | 100 | 1-600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** **pgms_wait_sampling.query_capture_mode**yerini. pg_qs.query_capture_mode NONE ise, pgms_wait_sampling,query_capture_mode ayarı hiçbir etkisi yoktur.


Bir parametre için farklı bir değer elde etmek veya ayarlamak için [Azure portalını](howto-configure-server-parameters-using-portal.md) veya [Azure CLI'yi](howto-configure-server-parameters-using-cli.md) kullanın.

## <a name="views-and-functions"></a>Görünümler ve işlevler
Aşağıdaki görünümleri ve işlevleri kullanarak Sorgu Mağazası'nı görüntüleyin ve yönetin. PostgreSQL ortak rolündeki herkes Sorgu Deposu'ndaki verileri görmek için bu görünümleri kullanabilir. Bu görünümler yalnızca **azure_sys** veritabanında kullanılabilir.

Sorgular, edebi ve sabitleri kaldırdıktan sonra yapılarına bakılarak normale döner. İki sorgu, gerçek değerler dışında aynıysa, aynı karmaya sahip olurlar.

### <a name="query_storeqs_view"></a>query_store.qs_view
Bu görünüm, Sorgu Deposu'ndaki tüm verileri döndürür. Her farklı veritabanı kimliği, kullanıcı kimliği ve sorgu kimliği için bir satır vardır. 

|**Adı**   |**Tür** | **Başvurular**  | **Açıklama**|
|---|---|---|---|
|runtime_stats_entry_id |bigint | | runtime_stats_entries tablosundan kimlik|
|user_id    |Oıd    |pg_authid.oid  |İfadeyi çalıştıran kullanıcının OID'si|
|Db_ıd  |Oıd    |pg_database.oid    |İfadenin yürütüldüğü veritabanınOID|
|query_id   |bigint  || İfadenin ayrışdıran ağacından hesaplanan dahili karma kodu|
|query_sql_text |Varchar(10000)  || Temsili bir ifadenin metni. Aynı yapıya sahip farklı sorgular birlikte kümelenir; bu metin kümedeki sorguların ilkinin metnidir.|
|plan_id    |bigint |   |Bu sorguya karşılık gelen planın kimliği, henüz kullanılamıyor|
|Start_time |timestamp  ||  Sorgular zaman kovalarına göre toplanır - bir kovanın süresi varsayılan olarak 15 dakikadır. Bu, bu giriş için zaman kovasına karşılık gelen başlangıç saatidir.|
|end_time   |timestamp  ||  Bu giriş için zaman kovasına karşılık gelen bitiş saati.|
|Aramalar  |bigint  || Sorgunun yürütülme sayısı|
|total_time |çift hassasiyet   ||  Milisaniye cinsinden toplam sorgu yürütme süresi|
|min_time   |çift hassasiyet   ||  Milisaniye cinsinden minimum sorgu yürütme süresi|
|max_time   |çift hassasiyet   ||  Milisaniye cinsinden maksimum sorgu yürütme süresi|
|mean_time  |çift hassasiyet   ||  Ortalama sorgu yürütme süresi, milisaniye cinsinden|
|stddev_time|   çift hassasiyet    ||  Sorgu yürütme süresinin standart sapma, milisaniye cinsinden |
|Satır   |bigint ||  İfadeden alınan veya etkilenen toplam satır sayısı|
|shared_blks_hit|   bigint  ||  İfadeye göre paylaşılan blok önbellek isabetlerinin toplam sayısı|
|shared_blks_read|  bigint  ||  Deyim tarafından okunan paylaşılan blokların toplam sayısı|
|shared_blks_dirtied|   bigint   || İfadeye göre kirlenmiş toplam paylaşılan blok sayısı |
|shared_blks_written|   bigint  ||  İfade tarafından yazılan toplam paylaşılan blok sayısı|
|local_blks_hit|    bigint ||   İfadeye göre toplam yerel blok önbelleği isabet sayısı|
|local_blks_read|   bigint   || Deyim tarafından okunan toplam yerel blok sayısı|
|local_blks_dirtied|    bigint  ||  İfadeye göre kirlenmiş toplam yerel blok sayısı|
|local_blks_written|    bigint  ||  İfade tarafından yazılan toplam yerel blok sayısı|
|temp_blks_read |bigint  || Deyim tarafından okunan toplam geçici blok sayısı|
|temp_blks_written| bigint   || İfade tarafından yazılan toplam geçici blok sayısı|
|blk_read_time  |çift hassasiyet    || İfadenin blokları milisaniye cinsinden okuması için harcanan toplam süre (track_io_timing etkinse, aksi takdirde sıfır)|
|blk_write_time |çift hassasiyet    || Ekstrenin blokları milisaniye cinsinden yazılması için harcanan toplam süre (track_io_timing etkinse, aksi takdirde sıfır)|
    
### <a name="query_storequery_texts_view"></a>query_store.query_texts_view
Bu görünüm Sorgu Deposu'nda sorgu metni verilerini döndürür. Her bir farklı query_text için bir satır vardır.

|**Adı**|  **Tür**|   **Açıklama**|
|---|---|---|
|query_text_id  |bigint     |query_texts tablosunun kimliği|
|query_sql_text |Varchar(10000)     |Temsili bir ifadenin metni. Aynı yapıya sahip farklı sorgular birlikte kümelenir; bu metin kümedeki sorguların ilkinin metnidir.|

### <a name="query_storepgms_wait_sampling_view"></a>query_store.pgms_wait_sampling_view
Bu görünüm, Sorgu Deposu'ndaki bekleme olayları verilerini döndürür. Her farklı veritabanı kimliği, kullanıcı kimliği, sorgu kimliği ve olay için bir satır vardır.

|**Adı**|  **Tür**|   **Başvurular**| **Açıklama**|
|---|---|---|---|
|user_id    |Oıd    |pg_authid.oid  |İfadeyi çalıştıran kullanıcının OID'si|
|Db_ıd  |Oıd    |pg_database.oid    |İfadenin yürütüldüğü veritabanınOID|
|query_id   |bigint     ||İfadenin ayrışdıran ağacından hesaplanan dahili karma kodu|
|Event_type |metin       ||Arka uç için bekleyen olay türü|
|event  |metin       ||Arka uç şu anda bekliyorsa bekleme olay adı|
|Aramalar  |Tamsayı        ||Yakalanan aynı olayın sayısı|


### <a name="functions"></a>İşlevler
Query_store.qs_reset() geçersiz döndürür

`qs_reset` Query Store tarafından şimdiye kadar toplanan tüm istatistikleri atar. Bu işlev yalnızca sunucu yöneticisi rolü tarafından yürütülebilir.

Query_store.staging_data_reset() geçersiz döndürür

`staging_data_reset` Query Store tarafından bellekte toplanan tüm istatistikleri atar (diğer bir deyişle, bellekte henüz veritabanına atılmamış veriler). Bu işlev yalnızca sunucu yöneticisi rolü tarafından yürütülebilir.

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar
- Bir PostgreSQL sunucusunda default_transaction_read_only parametresi varsa, Sorgu Deposu veri yakalayamaz.
- Sorgu Deposu işlevi, uzun Unicode sorguları (>= 6000 bayt) ile karşılaşırsa kesintiye uğrayabilir.
- [Okuma yinelemeleri,](concepts-read-replicas.md) sorgu deposu verilerini ana sunucudan çoğaltır. Bu, okundu çoğaltmanın Sorgu Deposu'nun okundu yinelemede çalıştırılabilen sorgularla ilgili istatistikler sağlamadığı anlamına gelir.


## <a name="next-steps"></a>Sonraki adımlar
- [Sorgu Mağazası'nın özellikle yararlı olabileceği senaryolar](concepts-query-store-scenarios.md)hakkında daha fazla bilgi edinin.
- [Sorgu Mağazası'nı kullanmak için en iyi uygulamalar](concepts-query-store-best-practices.md)hakkında daha fazla bilgi edinin.
