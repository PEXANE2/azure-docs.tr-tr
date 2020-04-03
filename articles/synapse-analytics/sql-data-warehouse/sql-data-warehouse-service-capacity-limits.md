---
title: Kapasite sınırları - Azure Synapse Analytics (eski adıyla SQL DW)
description: Azure Synapse'deki Synapse SQL havuzunun çeşitli bileşenleri için izin verilen maksimum değerler.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 2/19/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: f35a5da15ca1a672046844282626a6cb7b8ecbdf
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80583526"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-capacity-limits"></a>Azure Synapse Analytics (eski adıyla SQL DW) kapasite sınırları

Azure Synapse'nin çeşitli bileşenleri için izin verilen maksimum değerler.

## <a name="workload-management"></a>İş yükü yönetimi

| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| [Veri Ambarı Birimleri (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Tek bir SQL havuzu (veri ambarı) birimi için Max DWU | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Veri Ambarı Birimleri (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Sunucu başına varsayılan DTU |54,000<br></br>Varsayılan olarak, her SQL sunucusunda (örneğin, myserver.database.windows.net) DW5000c'ye kadar izin veren 54.000 DTU Kotası vardır. Bu kota yalnızca bir güvenlik sınırıdır. [Destek bileti oluşturarak](sql-data-warehouse-get-started-create-support-ticket.md) ve istek türü olarak *Kota* seçerek kotanızı artırabilirsiniz.  DTU ihtiyaçlarınızı hesaplamak için 7,5'i gereken toplam DWU ile çarpın veya 9,5'i gereken toplam cDWU ile çarpın. Örnek:<br></br>DW6000 x 7.5 = 45.000 DUS<br></br>DW5000c x 9.5 = 47.500 DUS.<br></br>Geçerli DTU tüketiminizi portaldaki SQL sunucu seçeneğinden görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. |
| Veritabanı bağlantısı |Maksimum Eşzamanlı açık oturumlar |1024<br/><br/>Eşzamanlı açık oturumların sayısı seçilen DWU'ya göre değişir. DWU600c ve üzeri maksimum 1024 açık oturumu destekler. DWU500c ve altında, 512 maksimum eşzamanlı açık oturum sınırını destekler. Unutmayın, aynı anda yürütülebilir sorgu sayısı sınırları vardır. Eşzamanlılık sınırı aşıldığında, istek işlenmeyi beklediği bir iç kuyruğa girer. |
| Veritabanı bağlantısı |Hazırlanan ifadeler için maksimum bellek |20 MB |
| [İş yükü yönetimi](resource-classes-for-workload-management.md) |Maksimum eşzamanlı sorgular |128<br/><br/>  En fazla 128 eşzamanlı sorgu yürütülür ve kalan sorgular sıraya alınır.<br/><br/>Kullanıcılar daha yüksek kaynak sınıflarına atandığında veya [veri ambarı birimi](memory-concurrency-limits.md) ayarı düşürüldüğünde eşzamanlı sorgu sayısı azalabilir. DMV sorguları gibi bazı sorguların çalışmasına her zaman izin verilir ve eşzamanlı sorgu sınırını etkilemez. Eşzamanlı sorgu yürütmehakkında daha fazla bilgi için [eşzamanlı lık maksimumları](memory-concurrency-limits.md) makalesine bakın. |
| [Tempdb](sql-data-warehouse-tables-temporary.md) |Maksimum GB |DW100c başına 399 GB. Bu nedenle DWU1000c'de tempdb 3,99 TB'ye kadar boyutlandırılır. |
||||

## <a name="database-objects"></a>Veritabanı nesneleri

| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Veritabanı |Maksimum boyut | Gen1: 240 TB diske sıkıştırılmış. Bu alan tempdb veya günlük alanı bağımsızdır ve bu nedenle bu alan kalıcı tablolara ayrılmıştır.  Kümelenmiş sütun deposu sıkıştırma 5X olarak tahmin edilmektedir.  Bu sıkıştırma, tüm tablolar sütun deposu (varsayılan tablo türü) kümelenmiş olduğunda veritabanının yaklaşık 1 PB'ye kadar büyümesine olanak tanır. <br/><br/> Gen2: Sütun mağaza tabloları için sınırsız depolama.  Veritabanının Rowstore bölümü hala diskte sıkıştırılmış 240 TB ile sınırlıdır. |
| Tablo |Maksimum boyut |Sütun mağaza tabloları için sınırsız boyut. <br>Diske sıkıştırılmış rowstore tabloları için 60 TB. |
| Tablo |Veritabanı başına tablolar | 100.000 |
| Tablo |Tablo başına sütunlar |1024 sütun |
| Tablo |Sütun başına bayt |Sütun [veri türüne](sql-data-warehouse-tables-data-types.md)bağlıdır. Karakter veri türleri için MAX Limit, sayfa dışında (satır taşma) depolama alanında en fazla 2 GB depolayabilir.  Char veya varchar limiti gibi Unicode olmayan karakterler bir veri sayfasında 8000, nchar veya nvarchar limiti gibi Unicode karakterleri bir veri sayfasında 4000'dir.  Performansı artırmak için veri sayfası depolama boyutlarını kullanın. |
| Tablo |Satır başına bayt, tanımlanmış boyut |8060 bayt<br/><br/>Satır başına bayt sayısı, sayfa sıkıştırmalı SQL Server için olduğu gibi hesaplanır. SQL Server gibi, değişken **uzunluktasütunların** satır dışına itilmesini sağlayan satır taşma depolama sı desteklenir. Değişken uzunluk satırları satır dışına itildiğinde, ana kayıtta yalnızca 24 baytlık kök depolanır. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms186981.aspx) |
| Tablo |Tablo başına bölümler |15.000<br/><br/>Yüksek performans için, iş gereksinimlerinizi desteklerken ihtiyacınız olan bölüm sayısını en aza indirmenizi öneririz. Bölüm sayısı arttıkça, Veri Tanım Dili (DDL) ve Veri Düzenleme Dili (DML) işlemlerinin yükü büyür ve performansın düşmesine neden olur. |
| Tablo |Bölüm sınır değeri başına karakterler. |4000 |
| Dizin oluşturma |Tablo başına kümelenmiş olmayan dizinler. |50<br/><br/>Yalnızca rowstore tabloları için geçerlidir. |
| Dizin oluşturma |Tablo başına kümelenmiş dizinler. |1<br><br/>Hem satır mağazası hem de sütun mağazası tabloları için geçerlidir. |
| Dizin oluşturma |Dizin anahtar boyutu. |900 bayt.<br/><br/>Yalnızca rowstore dizinleri için geçerlidir.<br/><br/>Dizin oluşturulduğunda, sütunlarda varolan veriler 900 bayt'ı geçmezse, en fazla 900 bayt boyutuna sahip varchar sütunlarında dizinler oluşturulabilir. Ancak, toplam boyutun 900 baytı aşmasını sağlayan sütunlar üzerindeki daha sonra INSERT veya UPDATE eylemleri başarısız olur. |
| Dizin oluşturma |Dizin başına anahtar sütunlar. |16<br/><br/>Yalnızca rowstore dizinleri için geçerlidir. Kümelenmiş sütun deposu dizinleri tüm sütunları içerir. |
| İstatistikler |Birleştirilmiş sütun değerlerinin boyutu. |900 bayt. |
| İstatistikler |İstatistik nesnesi başına sütunlar. |32 |
| İstatistikler |Tablo başına sütunlar üzerinde oluşturulan istatistikler. |30,000 |
| Saklı Yordamlar |Maksimum yuvalama seviyesi. |8 |
| Görüntüle |Görünüm başına sütunlar |1,024 |
||||

## <a name="loads"></a>Yükler

| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Polibase Yükler |Satır başına MB |1<br/><br/>Polybase, 1 MB'dan küçük satırları yükler. KÜMELENMIŞ Sütun Deposu Dizini (CCI) ile tablolara LOB veri türleri yükleme desteklenmez.<br/><br/> |
||||

## <a name="queries"></a>Sorgular

| Kategori | Açıklama | Maksimum |
|:--- |:--- |:--- |
| Sorgu |Kullanıcı tablolarında sıralanmış sorgular. |1000 |
| Sorgu |Sistem görünümlerinde eşzamanlı sorgular. |100 |
| Sorgu |Sistem görünümlerinde sıralanmış sorgular |1000 |
| Sorgu |Maksimum parametreler |2098 |
| Batch |Maksimum boyut |65,536*4096 |
| SELECT sonuçları |Satır başına sütunlar |4096<br/><br/>SELECT sonucunda satır başına 4096'dan fazla sütun olamaz. Her zaman 4096'ya sahip olabileceğinin garantisi yok. Sorgu planı geçici bir tablo gerektiriyorsa, tablo başına maksimum 1024 sütun uygulanabilir. |
| SELECT |İç içe alt sorgular |32<br/><br/>SELECT deyiminde 32'den fazla iç içe alt sorgunuz olamaz. Her zaman 32'ye sahip olabileceğinin garantisi yoktur. Örneğin, JOIN sorgu planına bir alt sorgu getirebilir. Alt sorgu sayısı da kullanılabilir bellek ile sınırlı olabilir. |
| SELECT |JOIN başına sütunlar |1024 sütun<br/><br/>JOIN'te hiçbir zaman 1024'ten fazla sütuna sahip olamazsınız. Her zaman 1024'e sahip olabileceğinin garantisi yoktur. JOIN planı, JOIN sonucundan daha fazla sütuniçeren geçici bir tablo gerektiriyorsa, 1024 sınırı geçici tabloiçin geçerlidir. |
| SELECT |GRUP BY sütunbaşına bayt. |8060<br/><br/>GROUP BY yan tümcesindeki sütunlar en fazla 8060 bayt olabilir. |
| SELECT |Order By sütunbaşına bayt |8060 bayt<br/><br/>ORDER BY yan tümcesindeki sütunlar en fazla 8060 bayt olabilir |
| İfade başına tanımlayıcılar |Başvurulan tanımlayıcısayısı |65,535<br/><br/> Sorgunun tek bir ifadesinde bulunabilecek tanımlayıcı sayısı sınırlıdır. Bu sayının aşılması SQL Server hatası 8632 ile sonuçlanır. Daha fazla bilgi için [bkz: İç hata: İfade hizmetleri sınırına ulaşıldı.](https://support.microsoft.com/help/913050/error-message-when-you-run-a-query-in-sql-server-2005-internal-error-a) |
| Dize sabit değerleri | Bir deyimdeki dize gerçek sayısı | 20.000 <br/><br/>Sorgunun tek bir ifadesinde dize sabitlerinin sayısı sınırlıdır. Bu sayının aşılması SQL Server hatası 8632 ile sonuçlanır.|
||||

## <a name="metadata"></a>Meta Veriler

| Sistem görünümü | Maksimum satırlar |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |En son 1000 SQL isteği için toplam DMS çalışanı sayısı. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |sys.dm_pdw_exec_requests'da depolanan en son 1000 SQL isteği için toplam adım sayısı. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |Sys.dm_pdw_exec_requests'da depolanan en son 1000 SQL isteği. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure Synapse'yi kullanma yla ilgili öneriler için [Hile Sayfası'na](cheat-sheet.md)bakın.
