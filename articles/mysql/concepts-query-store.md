---
title: Sorgu Mağazası - MySQL için Azure Veritabanı
description: Zaman içinde performansı izlemenize yardımcı olmak için MySQL için Azure Veritabanı'ndaki Sorgu Mağazası özelliği hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: d138c2fb8ed667d5b3c961c9f567264fa40edaee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537049"
---
# <a name="monitor-azure-database-for-mysql-performance-with-query-store"></a>Query Store ile MySQL performansı için Azure Veritabanını Izleyin

**Aşağıdakiler için geçerlidir:** MySQL 5.7 için Azure Veritabanı

MySQL için Azure Veritabanı'ndaki Sorgu Mağazası özelliği, zaman içinde sorgu performansını izlemenin bir yolunu sağlar. Sorgu Mağazası, en uzun süre çalışan ve en yoğun kaynak sorgularını hızla bulmanıza yardımcı olarak performans sorun giderme olayını kolaylaştırır. Sorgu Mağazası sorguların ve çalışma zamanı istatistiklerinin geçmişini otomatik olarak yakalar ve bunları incelemeniz için saklar. Veritabanı kullanım desenlerini görebilmeniz için verileri zaman pencerelerine göre ayırır. Tüm kullanıcılar, veritabanları ve sorgular için veriler **MySQL** örneği için Azure Veritabanı'ndaki mysql şema veritabanında depolanır.

## <a name="common-scenarios-for-using-query-store"></a>Sorgu Deposu'nun kullanımı için sık karşılaşılan senaryolar

Sorgu deposu aşağıdakiler de dahil olmak üzere bir dizi senaryoda kullanılabilir:

- Gerilen sorguları algılama
- Belirli bir zaman penceresinde sorgunun kaç kez yürütüldedildiğini belirleme
- Büyük deltaları görmek için zaman pencerelerinde bir sorgunun ortalama yürütme süresini karşılaştırma

## <a name="enabling-query-store"></a>Sorgu Deposu'nı Etkinleştirme

Query Store bir opt-in özelliğidir, bu nedenle varsayılan olarak bir sunucuda etkin değildir. Sorgu deposu, belirli bir sunucudaki tüm veritabanları için etkin veya genel olarak devre dışı bırakılır ve veritabanı başına açılamaz veya kapatılamaz.

### <a name="enable-query-store-using-the-azure-portal"></a>Azure portalını kullanarak Sorgu Mağazası'nı etkinleştirme

1. Azure portalında oturum açın ve MySQL sunucusu için Azure Veritabanınızı seçin.
1. Menünün **Ayarlar** bölümünde **Sunucu Parametreleri'ni** seçin.
1. query_store_capture_mode parametresini arayın.
1. Tüm değeri ayarlayın ve **kaydedin.**

Sorgu Deposu'nda bekleme istatistiklerini etkinleştirmek için:

1. query_store_wait_sampling_capture_mode parametresini arayın.
1. Tüm değeri ayarlayın ve **kaydedin.**

Mysql veritabanında devam etmek için ilk veri toplu işlemi için 20 dakikaya kadar bekleyin.

## <a name="information-in-query-store"></a>Sorgu Deposu'ndaki Bilgiler

Sorgu Mağazası'nın iki mağazası vardır:

- Sorgu yürütme istatistikleri bilgilerini kalıcı olarak sürdürmek için bir çalışma zamanı istatistik deposu.
- Kalıcı bekleme istatistikleri bilgileri için bekleme istatistikleri deposu.

Alan kullanımını en aza indirmek için, çalışma zamanı istatistikleri deposundaki çalışma zamanı yürütme istatistikleri sabit, yapılandırılabilir bir zaman penceresi üzerinde toplanır. Bu mağazalardaki bilgiler sorgu deposu görünümlerini sorgulayarak görünür.

Aşağıdaki sorgu Sorgu Deposu'ndaki sorgularla ilgili bilgileri döndürür:

```sql
SELECT * FROM mysql.query_store;
```

Veya bekleme istatistikleri için bu sorgu:

```sql
SELECT * FROM mysql.query_store_wait_stats;
```

## <a name="finding-wait-queries"></a>Bekleme sorgularını bulma

> [!NOTE]
> Bekleme istatistikleri en yoğun iş yükü saatlerinde etkinleştirilmemeli veya hassas iş yükleri için süresiz olarak açık olmamalıdır. <br>Yüksek CPU kullanımıyla veya daha düşük vCore'larla yapılandırılan sunucularda çalışan iş yükleri için bekleme istatistiklerini etkinleştirirken dikkatli olun. Süresiz olarak açık lanmamalıdır. 

Bekleme olay türleri, farklı bekleme olaylarını benzerliğe göre kovalarda birleştirir. Sorgu Deposu bekleme olay türünü, belirli bekleme olay adını ve söz konusu sorguyu sağlar. Bu bekleme bilgilerini sorgu çalışma zamanı istatistikleriyle ilişkilendirebilmek, sorgu performansı özelliklerine neyin katkıda bulundugun daha derin bir anlayış kazanabileceğiniz anlamına gelir.

Sorgu Deposu'ndaki bekleme istatistiklerini kullanarak iş yükünüz hakkında daha fazla bilgi edinmenize ilişkin bazı örnekler aşağıda verilmiştir:

| **Gözlem** | **Eylem** |
|---|---|
|Yüksek Kilit bekler | Etkilenen sorgular için sorgu metinlerini denetleyin ve hedef varlıkları tanımlayın. Sık sık yürütülen ve/veya yüksek süreye sahip aynı varlığı değiştiren diğer sorgular için Sorgu Deposu'na bakın. Bu sorguları tanımladıktan sonra, eşzamanlılığı geliştirmek için uygulama mantığını değiştirmeyi düşünün veya daha az kısıtlayıcı bir yalıtım düzeyi kullanın. |
|Yüksek Arabellek IO bekler | Sorgu Deposu'nda çok sayıda fiziksel okuma içeren sorguları bulun. Sorguları yüksek IO beklemeleriyle eşleşirlerse, taramalar yerine arama yapmak için temel varlık üzerinde bir dizin sunmayı düşünün. Bu, sorguların IO genel havasını en aza indirir. Bu sunucu için sorguları en iyi duruma getirecek dizin önerileri olup olmadığını görmek için portaldaki sunucunuzun **Performans Önerileri'ni** denetleyin. |
|Yüksek Bellek bekler | Sorgu Deposu'nda en iyi bellek tüketen sorguları bulun. Bu sorgular büyük olasılıkla etkilenen sorguların daha fazla ilerlemesini geciktiriyor. Bu sorguları en iyi duruma getirecek dizin önerileri olup olmadığını görmek için portaldaki sunucunuzun **Performans Önerileri'ni** denetleyin.|

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Sorgu Deposu etkinleştirildiğinde, 15 dakikalık toplama pencerelerinde, pencere başına 500'e kadar farklı sorguda veri kaydeder.

Sorgu Deposu parametrelerini yapılandırmak için aşağıdaki seçenekler kullanılabilir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı** |
|---|---|---|---|
| query_store_capture_mode | Değere göre sorgu deposu özelliğiNI AÇ/KAPALI olarak açın. Not: performance_schema KAPALI ise, query_store_capture_mode açmak performance_schema ve bu özellik için gerekli performans şeması araçlarının bir alt kümesini açar. | TÜMÜ | YOK, HEPSI |
| query_store_capture_interval | Sorgu deposu yakalama aralığı dakika içinde. Sorgu ölçümlerinin toplandığı aralığın belirtilmesine izin verir | 15 | 5 - 60 |
| query_store_capture_utility_queries | Sistemde çalıştırıyorum tüm yardımcı program sorgularını yakalamak için Açma veya KAPALI. | NO | EVET, HAYıR. |
| query_store_retention_period_in_days | Sorgu deposundaki verileri korumak için gün içinde zaman penceresi. | 7 | 1 - 30 |

Aşağıdaki seçenekler özellikle bekleme istatistikleri için geçerlidir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı** |
|---|---|---|---|
| query_store_wait_sampling_capture_mode | Bekleme istatistiklerini AÇMA /KAPAMA'yı sağlar. | Hiçbiri | YOK, HEPSI |
| query_store_wait_sampling_frequency | Bekleme örnekleme sıklığını saniyeler içinde değiştirir. 5 ila 300 saniye arası. | 30 | 5-300 |

> [!NOTE]
> Şu anda **query_store_capture_mode** bu yapılandırmanın yerini alacaktır, bu da **hem query_store_capture_mode** hem de **query_store_wait_sampling_capture_mode** bekleme istatistiklerinin çalışması için TÜM'e etkinleştirilmesi gerektiği anlamına gelir. **query_store_capture_mode** kapatılırsa, bekleme istatistikleri etkinleştirilen performance_schema kullandığından ve sorgu deposu tarafından yakalanan query_text kullandığından bekleme istatistikleri de kapatılır.

Bir parametre için farklı bir değer elde etmek veya ayarlamak için [Azure portalını](howto-server-parameters.md) veya [Azure CLI'yi](howto-configure-server-parameters-using-cli.md) kullanın.

## <a name="views-and-functions"></a>Görünümler ve işlevler

Aşağıdaki görünümleri ve işlevleri kullanarak Sorgu Mağazası'nı görüntüleyin ve yönetin. [Seçili ayrıcalık ortak rolündeki](howto-create-users.md#how-to-create-additional-admin-users-in-azure-database-for-mysql) herkes Sorgu Deposu'ndaki verileri görmek için bu görünümleri kullanabilir. Bu görünümler yalnızca **mysql** veritabanında kullanılabilir.

Sorgular, edebi ve sabitleri kaldırdıktan sonra yapılarına bakılarak normale döner. İki sorgu, gerçek değerler dışında aynıysa, aynı karmaya sahip olurlar.

### <a name="mysqlquery_store"></a>mysql.query_store

Bu görünüm, Sorgu Deposu'ndaki tüm verileri döndürür. Her farklı veritabanı kimliği, kullanıcı kimliği ve sorgu kimliği için bir satır vardır.

| **Adı** | **Veri Türü** | **IS_NULLABLE** | **Açıklama** |
|---|---|---|---|
| `schema_name`| varchar(64) | NO | Şema adı |
| `query_id`| bigint(20) | NO| Belirli bir sorgu için oluşturulan benzersiz kimlik, aynı sorgu farklı şemada yürütülürse, yeni bir kimlik oluşturulur |
| `timestamp_id` | timestamp| NO| Sorgunun yürütüldüğü zaman damgası. Bu, query_store_interval yapılandırmasını temel|
| `query_digest_text`| Longtext| NO| Tüm literals kaldırıldıktan sonra normalleştirilmiş sorgu metni|
| `query_sample_text` | Longtext| NO| Gerçek sorgunun literals ile ilk görünümü|
| `query_digest_truncated` | bit| EVET| Sorgu metninin kesildimi. Sorgu 1 KB'den uzunsa değer Evet olacaktır|
| `execution_count` | bigint(20)| NO| Yapılandırılan aralık döneminde bu zaman damgası kimliği / için sorgunun kaç kez yürütüldürün sayısı|
| `warning_count` | bigint(20)| NO| Bu sorgunun iç|
| `error_count` | bigint(20)| NO| Bu sorgunun aralık sırasında oluşturduğu hata sayısı|
| `sum_timer_wait` | double| EVET| Bu sorgunun aralıktaki toplam yürütme süresi|
| `avg_timer_wait` | double| EVET| Bu sorgunun aralıktaki ortalama yürütme süresi|
| `min_timer_wait` | double| EVET| Bu sorgu için minimum yürütme süresi|
| `max_timer_wait` | double| EVET| Maksimum yürütme süresi|
| `sum_lock_time` | bigint(20)| NO| Bu süre zarfında bu sorgu yürütmesi için tüm kilitler için harcanan toplam süre|
| `sum_rows_affected` | bigint(20)| NO| Etkilenen satır sayısı|
| `sum_rows_sent` | bigint(20)| NO| İstemciye gönderilen satır sayısı|
| `sum_rows_examined` | bigint(20)| NO| İncelenen satır sayısı|
| `sum_select_full_join` | bigint(20)| NO| Tam birleştirme sayısı|
| `sum_select_scan` | bigint(20)| NO| Seçili tarama sayısı |
| `sum_sort_rows` | bigint(20)| NO| Sıralanmış satır sayısı|
| `sum_no_index_used` | bigint(20)| NO| Sorgunun herhangi bir dizin kullanmadığı kaç kez|
| `sum_no_good_index_used` | bigint(20)| NO| Sorgu yürütme altyapısının iyi dizinler kullanmadığı kaç kez|
| `sum_created_tmp_tables` | bigint(20)| NO| Oluşturulan toplam geçici tablo sayısı|
| `sum_created_tmp_disk_tables` | bigint(20)| NO| Diskte oluşturulan toplam geçici tablo sayısı (G/Ç oluşturur)|
| `first_seen` | timestamp| NO| Toplama penceresi sırasında sorgunun ilk oluşumu (UTC)|
| `last_seen` | timestamp| NO| Bu toplama penceresi sırasında sorgunun son oluşumu (UTC)|

### <a name="mysqlquery_store_wait_stats"></a>mysql.query_store_wait_stats

Bu görünüm, Sorgu Deposu'ndaki bekleme olayları verilerini döndürür. Her farklı veritabanı kimliği, kullanıcı kimliği, sorgu kimliği ve olay için bir satır vardır.

| **Adı**| **Veri Türü** | **IS_NULLABLE** | **Açıklama** |
|---|---|---|---|
| `interval_start` | timestamp | NO| Aralığın başlangıcı (15 dakikalık artış)|
| `interval_end` | timestamp | NO| Aralığın sonu (15 dakikalık artış)|
| `query_id` | bigint(20) | NO| Normalleştirilmiş sorguda oluşturulan benzersiz kimlik (sorgu deposundan)|
| `query_digest_id` | varchar(32) | NO| Tüm literalleri kaldırdıktan sonra normalleştirilmiş sorgu metni (sorgu deposundan) |
| `query_digest_text` | Longtext | NO| Gerçek sorgunun literals ile ilk görünümü (sorgu deposundan) |
| `event_type` | varchar(32) | NO| Bekleme olayının kategorisi |
| `event_name` | varchar(128) | NO| Bekleme olayının adı |
| `count_star` | bigint(20) | NO| Sorgu için aralık ta örnekalınan bekleme olaylarının sayısı |
| `sum_timer_wait_ms` | double | NO| Aralık ta bu sorgunun toplam bekleme süresi (milisaniye cinsinden) |

### <a name="functions"></a>İşlevler

| **Adı**| **Açıklama** |
|---|---|
| `mysql.az_purge_querystore_data(TIMESTAMP)` | Verilen zaman damgası önce tüm sorgu depolama verileri temizleme |
| `mysql.az_procedure_purge_querystore_event(TIMESTAMP)` | Verilen zaman damgası önce tüm bekleme olay verileri temizleme |
| `mysql.az_procedure_purge_recommendation(TIMESTAMP)` | Verilen zaman damgası önce sona erme leri olan önerileri tasfiye |

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

- MySQL sunucusunda parametre `default_transaction_read_only` varsa, Sorgu Deposu veri yakalayamaz.
- Sorgu Deposu işlevi, uzun Unicode sorguları (=\>6000 bayt) ile karşılaşırsa kesintiye uğrayabilir.
- Bekleme istatistikleri için bekletme süresi 24 saattir.
- Bekleme istatistikleri olayların bir kısmını yakalamak için örnek kullanır. Frekans parametresi `query_store_wait_sampling_frequency`kullanılarak değiştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu Performans Öngörüleri](concepts-query-performance-insight.md) hakkında daha fazla bilgi edinin
