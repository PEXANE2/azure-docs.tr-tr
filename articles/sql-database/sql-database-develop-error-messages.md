---
title: SQL hata kodları-veritabanı bağlantı hatası | Microsoft Docs
description: 'Yaygın veritabanı bağlantı hataları, veritabanı kopyalama sorunları ve genel hatalar gibi SQL veritabanı istemci uygulamaları için SQL hata kodları hakkında bilgi edinin. '
keywords: SQL hata kodu, erişim SQL, veritabanı bağlantı hatası, SQL hata kodları
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/06/2019
ms.openlocfilehash: 24bd2cca2e4ed053d51f618d90274e8988a09c26
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568892"
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>SQL veritabanı istemci uygulamaları için SQL hata kodları: Veritabanı bağlantı hataları ve diğer sorunlar

Bu makalede veritabanı bağlantı hataları, geçici hatalar (geçici hatalar da denir), kaynak idare hataları, veritabanı kopyalama sorunları, esnek havuz ve diğer hatalar da dahil olmak üzere SQL veritabanı istemci uygulamaları için SQL hata kodları listelenmektedir. Çoğu kategori Azure SQL veritabanı 'na özel ve Microsoft SQL Server için de geçerlidir. Ayrıca bkz. [sistem hata iletileri](https://technet.microsoft.com/library/cc645603(v=sql.105).aspx).

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Veritabanı bağlantı hataları, geçici hatalar ve diğer geçici hatalar

Aşağıdaki tabloda bağlantı kaybı hataları için SQL hata kodları ve uygulamanız SQL veritabanına erişmeye çalıştığında karşılaşabileceğiniz diğer geçici hatalar ele alınmaktadır. Azure SQL veritabanı 'na bağlanma hakkında Başlarken öğreticileri için bkz. [Azure SQL veritabanı 'Na bağlanma](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>En yaygın veritabanı bağlantı hataları ve geçici hata hataları

Azure altyapısının SQL Veritabanı hizmetinde ağır iş yükleri ortaya çıktığında sunucuları dinamik olarak yeniden yapılandırabilme özelliği vardır.  Bu dinamik davranış istemci programınızın SQL Veritabanı bağlantısını kaybetmesine neden olabilir. Bu tür bir hata koşuluna *geçici*bir hata denir.

İstemci programınızın kendisini düzeltmek için geçici hata süresi verdikten sonra bir bağlantıyı yeniden oluşturabilmesi için yeniden deneme mantığı olması önemle önerilir.  İlk yeniden denemeden önce 5 saniye gecikme yapmanızı öneririz. 5 saniyelik riskden daha kısa bir gecikme sonrasında bulut hizmeti 'nin yeniden denenmesi. Sonraki her yeniden deneme için gecikme, en fazla 60 saniyeye kadar üstel olarak artar.

Geçici hata hataları genellikle istemci programlarınızdan aşağıdaki hata iletilerinden biri olarak bildirim yapılır:

* &lt;&gt; SunucuAzure_instance&gt;üzerindeki veritabanı db_name Şu anda kullanılamıyor. &lt; Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse, müşteri desteğine başvurun ve session_id 'in &lt;oturum izleme kimliğini sağlayın&gt;
* &lt;&gt; SunucuAzure_instance&gt;üzerindeki veritabanı db_name Şu anda kullanılamıyor. &lt; Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse, müşteri desteğine başvurun ve session_id &lt;&gt;'in oturum izleme kimliğini sağlayın. (Microsoft SQL Server, Hata: 40613)
* Mevcut bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı.
* System. Data. Entity. Core. EntityCommandExecutionException: Komut tanımı yürütülürken bir hata oluştu. Ayrıntılar için iç özel duruma bakın. ---> System. Data. SqlClient. SqlException: Sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu. sağlayıcısını Oturum sağlayıcısı, hata: 19-fiziksel bağlantı kullanılamıyor)
* Veritabanı yeniden yapılandırma sürecinde olduğundan ve birincil veritabanındaki etkin bir işlemin ortasında yeni sayfalar uygulanırken, ikincil veritabanına bağlantı girişimi başarısız oldu. 

Yeniden deneme mantığına ait kod örnekleri için bkz.:

* [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) 
* [SQL veritabanı 'nda bağlantı hatalarını ve geçici hataları gidermeye yönelik eylemler](sql-database-connectivity-issues.md)

[SQL Server bağlantı havuzda (ADO.net)](https://msdn.microsoft.com/library/8xx3tyca.aspx), ADO.NET kullanan istemciler için *engelleme süresi* hakkında bir tartışma vardır.

### <a name="transient-fault-error-codes"></a>Geçici hata hata kodları

Aşağıdaki hatalar geçicidir ve uygulama mantığındaki yeniden denenmelidir: 

| Hata kodu | severity | Açıklama |
| ---:| ---:|:--- |
| 4060 |16 |Veritabanı açılamıyor. "%.&#x2a;ls" oturum açma tarafından istenen. Oturum açılamadı. Daha fazla bilgi için bkz. [hatalar 4000-4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999)|
| 40197 |17 |Hizmet, isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin. Hata kodu% d.<br/><br/>Yazılım veya donanım yükseltmeleri, donanım hataları veya diğer yük devretme sorunları nedeniyle bu hatayı alırsınız. 40197 hatası iletisi içinde gömülü hata kodu (% d), hata veya yük devretme türü hakkında ek bilgiler sağlar. Hata kodlarının bazı örnekleri 40020 40197, 40143, 40166 ve 40540 hata koduna katıştırılır.<br/><br/>SQL veritabanı sunucunuza yeniden bağlanmak veritabanınızın sağlıklı bir kopyasına otomatik olarak bağlanır. Uygulamanız hata 40197 ' i yakalamalı, sorun giderme için ileti içinde katıştırılmış hata kodunu (% d) günlüğe kaydedin ve kaynaklar kullanılabilir olana kadar SQL veritabanı 'na yeniden bağlanmayı deneyin ve bağlantınız yeniden oluşturulur. Daha fazla bilgi için bkz. [geçici hatalar](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 40501 |20 |Hizmet şu anda meşgul. 10 saniye sonra isteği yeniden deneyin. Olay KIMLIĞI:% ls. Kod:% d. Daha fazla bilgi için bkz. <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp;|
| 40613 |17 |Veritabanı '%.&#x2a;ls' sunucusundaki '%.&#x2a;ls' şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin '%.&#x2a;ls'.<br/><br/> Bu hata, veritabanında zaten var olan bir ayrılmış yönetici bağlantısı (DAC) varsa meydana gelebilir. Daha fazla bilgi için bkz. [geçici hatalar](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 49918 |16 |İsteği işlenemiyor. İsteği işlemek için yeterli kaynak yok.<br/><br/>Hizmet şu anda meşgul. Lütfen isteği daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp; |
| 49919 |16 |Oluşturma veya güncelleştirme isteği işlenemiyor. "% Ld" aboneliği için çok fazla sayıda oluşturma veya güncelleştirme işlemi sürüyor.<br/><br/>Hizmet, aboneliğiniz veya sunucunuz için birden çok oluşturma veya güncelleştirme isteğini işlemekle meşgul. İstekler Şu anda kaynak iyileştirmesi için engelleniyor. Bekleyen işlemler için [sys. DM _operation_status](https://msdn.microsoft.com/library/dn270022.aspx) sorgusunu sorgulayın. Bekleyen oluşturma veya güncelleştirme isteklerinin tamamlanmasını bekleyin veya bekleyen isteklerinizin birini silip isteğinizi daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp; |
| 49920 |16 |İsteği işlenemiyor. "% Ld" aboneliği için çok fazla işlem devam ediyor.<br/><br/>Hizmet, bu abonelik için birden çok isteği işlemekle meşgul. İstekler Şu anda kaynak iyileştirmesi için engelleniyor. İşlem durumu için [sys. DM _operation_status](https://msdn.microsoft.com/library/dn270022.aspx) sorgula. Bekleyen istekler tamamlanana kadar bekleyin veya bekleyen isteklerinizin birini silip isteğinizi daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp; |
| 4221 |16 |' HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING ' üzerinde uzun bekleme nedeniyle okuma-ikincil için oturum açma başarısız oldu. Çoğaltma geri dönüştürüldüğünde, uçuşdaki işlemler için satır sürümleri eksik olduğundan, çoğaltma oturum açma için kullanılamıyor. Bu sorun, birincil çoğaltmadaki etkin işlemler geri alınarak veya uygulanırken çözülebilir. Bu koşulun oluşumları, birincil üzerinde uzun yazma işlemlerinden kaçınılarak en aza indirgenebilir. |

## <a name="database-copy-errors"></a>Veritabanı kopyalama hataları

Azure SQL veritabanı 'nda bir veritabanı kopyalanırken aşağıdaki hatalarla karşılaşılabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md).

| Hata kodu | severity | Açıklama |
| ---:| ---:|:--- |
| 40635 |16 |İstemci IP adresi '%.&#x2a;ls' geçici olarak devre dışıdır. |
| 40637 |16 |Veritabanı kopyası oluşturma şu anda devre dışı. |
| 40561 |16 |Veritabanı kopyalama başarısız oldu. Kaynak ya da hedef veritabanı yok. |
| 40562 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı bırakılmış. |
| 40563 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı bırakılmış. |
| 40564 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40565 |16 |Veritabanı kopyalama başarısız oldu. Aynı kaynaktan 1 ' den fazla eşzamanlı veritabanı kopyası yapılmasına izin verilmez. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40566 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40567 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40568 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı kullanılamaz duruma geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40569 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı kullanılamaz duruma geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40570 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40571 |16 |Veritabanı kopyalama bir iç hata nedeniyle başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |

## <a name="resource-governance-errors"></a>Kaynak idare hataları

Aşağıdaki hatalar, Azure SQL veritabanı ile çalışırken kaynakların aşırı kullanımı nedeniyle oluşur. Örneğin:

* Bir işlem çok uzun bir süre açık.
* Bir işlem çok fazla kilit tutuyor.
* Bir uygulama çok fazla bellek kullanıyor.
* Bir uygulama çok fazla `TempDb` alan kullanıyor.

İlgili konu başlıkları:

* Daha fazla bilgi için bkz.
  * [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)
  * [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)
  * [Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md)
  * [tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)
  * [Elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md)
  * [Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). 

| Hata kodu | severity | Açıklama |
| ---:| ---:|:--- |
| 10928 |20 |Kaynak KIMLIĞI:% d. Veritabanı için% s sınırı% d ve bu sınıra ulaşıldı. Daha fazla bilgi için bkz. [tek ve havuza alınmış veritabanları Için SQL veritabanı kaynak sınırları](sql-database-resource-limits-database-server.md).<br/><br/>Kaynak KIMLIĞI, sınıra ulaşan kaynağı gösterir. Çalışan iş parçacıkları için kaynak KIMLIĞI = 1. Oturumlar için kaynak KIMLIĞI = 2.<br/><br/>Bu hata ve nasıl çözüleceği hakkında daha fazla bilgi için bkz.: <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp; |
| 10929 |20 |Kaynak KIMLIĞI:% d. % S en düşük garanti% d, maksimum sınır% d ve veritabanı için geçerli kullanım% d. Ancak, sunucu şu anda bu veritabanı için% d değerinden büyük istekleri desteklemeye yönelik çok meşgul. Kaynak KIMLIĞI, sınıra ulaşan kaynağı gösterir. Çalışan iş parçacıkları için kaynak KIMLIĞI = 1. Oturumlar için kaynak KIMLIĞI = 2. Daha fazla bilgi için bkz. <br/>&bull;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md) &nbsp;<br/>&bull;[Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md) &nbsp;<br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp; <br/>Aksi takdirde, lütfen daha sonra yeniden deneyin. |
| 40544 |20 |Veritabanı boyut kotasına ulaştı. Verileri bölümleyin veya silin, dizinleri bırakın veya olası çözümler için belgelere başvurun. Veritabanı Ölçeklendirme için bkz. [tek veritabanı kaynaklarını ölçeklendirme](sql-database-single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).|
| 40549 |16 |Uzun süre çalışan bir işlem olduğu için oturum sonlandırıldı. İşleminizi kısaltmayı deneyin. Toplu işleme hakkında bilgi için bkz. [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md).|
| 40550 |16 |Oturum çok fazla kilit elde ettiğinden sonlandırıldı. Tek bir işlemde daha az sayıda satır okumayı veya değiştirmeyi deneyin. Toplu işleme hakkında bilgi için bkz. [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md).|
| 40551 |16 |Aşırı `TEMPDB` kullanım nedeniyle oturum sonlandırıldı. Geçici tablo alanı kullanımını azaltmak için sorgunuzu değiştirmeyi deneyin.<br/><br/>Geçici nesneler kullanıyorsanız, geçici nesneleri oturum için artık gerekli olmadıklarında bırakarak `TEMPDB` veritabanında alandan tasarruf edin. SQL veritabanı 'nda tempdb kullanımı hakkında daha fazla bilgi için bkz. [SQL veritabanı 'Nda tempdb veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
| 40552 |16 |Aşırı işlem günlüğü alanı kullanımı nedeniyle oturum sonlandırıldı. Tek bir işlemde daha az sayıda satır değiştirmeyi deneyin. Toplu işleme hakkında bilgi için bkz. [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma](sql-database-use-batching-to-improve-performance.md).<br/><br/>`bcp.exe` Yardımcı programını `BatchSize` veya `System.Data.SqlClient.SqlBulkCopy` sınıfınıkullanaraktoplueklemeleryaparsanız,herbirişlemdesunucuyakopyalanmışsatır`-b batchsize` sayısını sınırlamak için veya seçeneklerini kullanmayı deneyin. `ALTER INDEX` İfadesiyle bir dizini yeniden oluşturuyorsanız `REBUILD WITH ONLINE = ON` seçeneğini kullanmayı deneyin. Sanal çekirdek satın alma modeli için işlem günlüğü boyutları hakkında bilgi için bkz.: <br/>&bull;[tek veritabanları için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) &nbsp;|
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Daha az satır işlemek için sorgunuzu değiştirmeyi deneyin.<br/><br/>Transact-SQL kodunuzda `ORDER BY` ve `GROUP BY` işlem sayısını azaltmak sorgunuzun bellek gereksinimlerini azaltır. Veritabanı Ölçeklendirme için bkz. [tek veritabanı kaynaklarını ölçeklendirme](sql-database-single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).|

## <a name="elastic-pool-errors"></a>Elastik havuz hataları

Aşağıdaki hatalar elastik havuzlar oluşturma ve kullanmayla ilgilidir:

| Hata kodu | severity | Açıklama | Düzeltici eylem |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |Elastik havuz, depolama sınırına ulaştı. Elastik havuzun depolama alanı kullanımı (% d) MB/s değerini aşamaz. Elastik havuzun depolama sınırına ulaşıldığında veritabanına veri yazmaya çalışılıyor. Kaynak limitleri hakkında bilgi için bkz.: <br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar.](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp; <br/> |Depolama sınırını artırmak, elastik havuzdaki ayrı veritabanları tarafından kullanılan depolamayı azaltmak veya elastik havuzdan veritabanlarını kaldırmak için mümkünse, depolama alanı sayısını ve/veya depolama alanını esnek havuza eklemeyi düşünün. Elastik havuz ölçekleme için bkz. [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).|
| 10929 | 16 |% S en düşük garanti% d, maksimum sınır% d ve veritabanı için geçerli kullanım% d. Ancak, sunucu şu anda bu veritabanı için% d değerinden büyük istekleri desteklemeye yönelik çok meşgul. Kaynak limitleri hakkında bilgi için bkz.: <br/>&bull;[Elastik havuzlar için DTU tabanlı sınırlar](sql-database-dtu-resource-limits-elastic-pools.md) &nbsp;<br/>&bull;[elastik havuzlar için sanal çekirdek tabanlı sınırlar.](sql-database-vcore-resource-limits-elastic-pools.md) &nbsp; <br/> Aksi takdirde, lütfen daha sonra yeniden deneyin. Veritabanı başına DTU/sanal çekirdek en az; Veritabanı başına DTU/sanal çekirdek maks. Elastik havuzdaki tüm veritabanları genelinde eş zamanlı çalışan (istek) toplam sayısı havuz sınırını aşmaya çalıştı. |Çalışan sınırını artırmak veya elastik havuzdan veritabanlarını kaldırmak için mümkünse, elastik havuzun DTU 'ları veya sanal çekirdekleri artırmayı düşünün. |
| 40844 | 16 |'% Ls ' sunucusundaki '% ls ' veritabanı, elastik havuzdaki bir '% ls ' sürüm veritabanıdır ve sürekli bir kopyalama ilişkisine sahip olamaz.  |Yok |
| 40857 | 16 |Sunucu: '% ls ', elastik havuz adı: '% ls ' için elastik havuz bulunamadı. Belirtilen elastik havuz belirtilen sunucuda yok. | Geçerli bir elastik havuz adı sağlayın. |
| 40858 | 16 |'% Ls ' esnek havuzu sunucuda zaten var: '% ls '. Belirtilen elastik havuz belirtilen SQL veritabanı sunucusunda zaten var. | Yeni elastik havuz adı sağlayın. |
| 40859 | 16 |Elastik havuz, '% ls ' hizmet katmanını desteklemiyor. Belirtilen hizmet katmanı elastik havuz sağlama için desteklenmiyor. |Varsayılan hizmet katmanını kullanmak için doğru sürümü sağlayın veya hizmet katmanını boş bırakın. |
| 40860 | 16 |Elastik havuz '% ls ' ve hizmet hedefi '% ls ' birleşimi geçersiz. Elastik havuz ve hizmet katmanı yalnızca kaynak türü ' Elakpool ' olarak belirtilmişse birlikte belirtilebilir. |Elastik havuz ve hizmet katmanının doğru birleşimini belirtin. |
| 40861 | 16 |Veritabanı sürümü '%. *ls ', '% ' olan esnek havuz hizmeti katmanından farklı olamaz.* ls '. Veritabanı sürümü elastik havuz hizmeti katmanından farklı. |Elastik havuz hizmeti katmanından farklı bir veritabanı sürümü belirtmeyin.  Veritabanı sürümünün belirtilmesi gerekmediğini unutmayın. |
| 40862 | 16 |Elastik havuz hizmeti hedefi belirtilmişse elastik havuz adı belirtilmelidir. Elastik havuz hizmeti hedefi, elastik havuzu benzersiz bir şekilde tanımlamaz. |Elastik havuz hizmeti hedefini kullanıyorsanız elastik havuz adını belirtin. |
| 40864 | 16 |Elastik havuz için DTU 'Lar '%. * ls ' hizmet katmanı için en az (% d) DTU olmalıdır. Elastik havuz için en düşük sınırın altındaki DTU 'Lar ayarlamaya çalışılıyor. |Elastik havuz için DTU 'ları en azından minimum sınıra ayarlamayı yeniden deneyin. |
| 40865 | 16 |Elastik havuz için DTU 'Lar, '%. * ls ' hizmet katmanının DTU değerini (% d) aşamaz. En yüksek sınırın üzerinde esnek havuz için DTU 'Lar ayarlamaya çalışılıyor. |Elastik havuz için DTU 'ları en yüksek sınırdan büyük olmayacak şekilde ayarlamayı yeniden deneyin. |
| 40867 | 16 |Veritabanı başına DTU Max, hizmet katmanı '%. * ls ' için en az (% d) olmalıdır. Desteklenen sınırın altında veritabanı başına DTU en fazla ayarı yapılmaya çalışılıyor. | İstenen ayarı destekleyen elastik havuz hizmet katmanını kullanmayı düşünün. |
| 40868 | 16 |Veritabanı başına DTU en yüksek değeri, '%. * ls ' hizmet katmanı için (% d) değerini aşamaz. Desteklenen sınırın ötesinde veritabanı başına DTU en fazla ayarı yapılmaya çalışılıyor. | İstenen ayarı destekleyen elastik havuz hizmet katmanını kullanmayı düşünün. |
| 40870 | 16 |Veritabanı başına DTU en düşük değeri, '%. * ls ' hizmet katmanı için (% d) değerini aşamaz. Veritabanı başına desteklenen sınırın ötesinde DTU en düşük değeri ayarlamaya çalışılıyor. | İstenen ayarı destekleyen elastik havuz hizmet katmanını kullanmayı düşünün. |
| 40873 | 16 |Veritabanı başına veritabanı (% d) ve DTU min sayısı (% d), elastik havuzun (% d) DTU sayısını aşamaz. Esnek havuzdaki veritabanları için en düşük değeri belirleme girişimi, elastik havuzun DTU 'ları aşıyor. | Elastik havuzun DTU 'larının arttırmasını veya veritabanı başına DTU boyutunu azaltmayı veya elastik havuzdaki veritabanlarının sayısını azaltmayı düşünün. |
| 40877 | 16 |Elastik havuz herhangi bir veritabanı içermediği takdirde silinemez. Elastik havuz bir veya daha fazla veritabanı içeriyor ve bu nedenle silinemez. |Veritabanlarını silmek için elastik havuzdan kaldırın. |
| 40881 | 16 |'%. * Ls ' esnek havuzu, veritabanı sayısı sınırına ulaştı.  Elastik havuz için veritabanı sayısı sınırı (% d) DTU 'lar (% d) ile esnek havuz için (% d) değerini aşamaz. Elastik havuzun veritabanı sayısı sınırına ulaşıldığında, elastik havuzda veritabanı oluşturulmaya veya eklemeye çalışılıyor. | Veritabanı sınırını artırmak veya elastik havuzdan veritabanlarını kaldırmak için mümkünse elastik havuzun DTU 'larının artırılmasını göz önünde bulundurun. |
| 40889 | 16 |'%. * Ls ' esnek havuzu için DTU 'Lar veya depolama sınırı, veritabanları için yeterli depolama alanı sağlamayacağından azaltılabilir. Elastik havuzun depolama sınırının altında depolama alanı kullanımını azaltma girişimi. | Esnek havuzda ayrı veritabanlarının depolama kullanımını azaltmayı düşünün veya DTU 'ları veya depolama sınırını azaltmak için havuzdan veritabanlarını kaldırın. |
| 40891 | 16 |Veritabanı başına DTU en az (% d), veritabanı başına en fazla DTU sayısını (% d) aşamaz. Veritabanı başına DTU en yüksek değerini veritabanı başına ayarlamaya çalışılıyor. |Veritabanları başına DTU en düşük değeri, veritabanı başına en fazla DTU sayısını aşmadığından emin olun. |
| TBD | 16 |Elastik havuzdaki tek bir veritabanı için depolama boyutu, '%. * ls ' hizmet katmanı elastik havuzu tarafından izin verilen en büyük boyutu aşamaz. Veritabanı için en büyük boyut, elastik havuz hizmeti katmanının izin verdiği en büyük boyutu aşıyor. |En büyük veritabanı boyutunu elastik havuz hizmeti katmanının izin verdiği en büyük boyut limitlerinin sınırları içinde ayarlayın. |

İlgili konu başlıkları:

* [Elastik havuz oluşturma (C#)](sql-database-elastic-pool-manage-csharp.md)
* [Elastik havuzu yönetme (C#)](sql-database-elastic-pool-manage-csharp.md)
* [Elastik havuz oluşturma (PowerShell)](sql-database-elastic-pool-manage-powershell.md)
* [Elastik havuzu izleme ve yönetme (PowerShell)](sql-database-elastic-pool-manage-powershell.md)

## <a name="general-errors"></a>Genel hatalar

Aşağıdaki hatalar önceki kategorilere düşmemektedir.

| Hata kodu | severity | Açıklama |
| ---:| ---:|:--- |
| [15006](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-15000-to-15999) |16 |(Tınlogin), geçersiz karakterler içerdiğinden geçerli bir ad değil.|
| [18452](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Oturum açma başarısız. Oturum açma güvenilmeyen bir etki alanından ve Windows authentication.%.&#x2a;ls ile kullanılamaz ls (Windows oturumu açma desteklenmez SQL Server'ın bu sürümünde.) |
| [18456](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |'% Kullanıcısı için oturum açma başarısız oldu. &#x2a;ls '.%. &#x2a;% ls. &#x2a;ls ("% kullanıcısı için oturum açma başarısız oldu&#x2a; . ls ".) |
| [18470](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Kullanıcı için oturum açma başarısız '%.&#x2a;ls'. Neden: Hesap devre dışı.%. &#x2a;ls |
| 40014 |16 |Aynı işlemde birden fazla veritabanı kullanılamaz. |
| 40054 |16 |Bu SQL Server sürümünde kümelenmiş dizini olmayan tablolar desteklenmez. Kümelenmiş bir dizin oluşturup yeniden deneyin. |
| 40133 |15 |Bu işlem bu SQL Server sürümünde desteklenmiyor. |
| 40506 |16 |Belirtilen SID bu SQL Server sürümü için geçersiz. |
| 40507 |16 |'%.&#x2a;ls' SQL Server'ın bu sürümünde parametrelerle çağrılacak olamaz. |
| 40508 |16 |Using ifadesinin veritabanları arasında geçiş yapmak için KULLANıLMASı desteklenmez. Farklı bir veritabanına bağlanmak için yeni bir bağlantı kullanın. |
| 40510 |16 |Deyimi '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor |
| 40511 |16 |Yerleşik işlevi '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40512 |16 |Kullanım dışı bırakılan '% ls ' özelliği bu SQL Server sürümünde desteklenmiyor. |
| 40513 |16 |Sunucu değişkeni '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40514 |16 |'% ls ' Bu SQL Server sürümünde desteklenmiyor. |
| 40515 |16 |Veritabanı ve/veya sunucu adına başvuru içinde '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40516 |16 |Genel geçici nesneler bu SQL Server sürümünde desteklenmiyor. |
| 40517 |16 |Anahtar sözcüğü veya deyim seçeneği '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40518 |16 |DBCC komutu '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40520 |16 |'% S_MSG ' güvenli kılınabilir sınıfı bu SQL Server sürümünde desteklenmiyor. |
| 40521 |16 |'% S_MSG ' güvenli kılınabilir sınıfı bu SQL Server sürümünde sunucu kapsamında desteklenmiyor. |
| 40522 |16 |Veritabanı asıl '%.&#x2a;ls' türü SQL Server'ın bu sürümünde desteklenmiyor. |
| 40523 |16 |'%.&#x2a;ls' örtük kullanıcı oluşturma, SQL Server'ın bu sürümünde desteklenmiyor. Kullanıcıyı kullanmadan önce açıkça oluşturun. |
| 40524 |16 |Veri türü '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40525 |16 |'%. Ls ' Ile bu SQL Server sürümünde desteklenmez. |
| 40526 |16 |'%.&#x2a;ls satır kümesi sağlayıcısı bu SQL Server sürümünde desteklenmiyor. |
| 40527 |16 |Bağlı sunucular bu SQL Server sürümünde desteklenmiyor. |
| 40528 |16 |Kullanıcılar bu SQL Server sürümünde sertifikalar, asimetrik anahtarlar veya Windows oturum açmaları ile eşlenemez. |
| 40529 |16 |Yerleşik işlevi '%.&#x2a;ls' kimliğe bürünme bağlam, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40532 |11 |Sunucu açamıyor "%.&#x2a;ls" oturum açma tarafından istenen. Oturum açılamadı. |
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Daha az satır işlemek için sorgunuzu değiştirmeyi deneyin.<br/><br/> Transact-SQL kodunuzda `ORDER BY` ve `GROUP BY` işlemlerinin sayısını azaltmak, sorgunuzun bellek gereksinimlerini azaltmaya yardımcı olur. |
| 40604 |16 |Sunucu kotasını aşacağından VERITABANı OLUŞTURULAMADı/DEĞIŞTIRILEMIYOR. |
| 40606 |16 |Bu SQL Server sürümünde veritabanı ekleme desteklenmiyor. |
| 40607 |16 |Windows oturumu açma bu SQL Server sürümünde desteklenmez. |
| 40611 |16 |Sunucular en fazla 128 güvenlik duvarı kuralı tanımlamış olabilir. |
| 40614 |16 |Güvenlik duvarı kuralının başlangıç IP adresi bitiş IP adresini aşamaz. |
| 40615 |16 |Oturum açma tarafından istenen{0}' ' sunucusu açılamıyor. '{1}' IP adresine sahip istemcinin sunucuya erişmesine izin verilmiyor.<br /><br />Erişimi etkinleştirmek için, SQL veritabanı portalını kullanın veya bu IP adresi\_veya\_adres aralığı için bir güvenlik duvarı kuralı oluşturmak üzere ana veritabanında SP kümesi güvenlik duvarını\_çalıştır kuralını çalıştırın. Bu değişikliğin etkili olması beş dakikaya kadar sürebilir. |
| 40617 |16 |(Kural adı) ile başlayan güvenlik duvarı kuralı adı çok uzun. Maksimum uzunluk 128 ' dir. |
| 40618 |16 |Güvenlik duvarı kuralı adı boş olamaz. |
| 40620 |16 |Kullanıcı için oturum açma başarısız "%.&#x2a;ls". Parola değiştirme başarısız oldu. Bu SQL Server sürümünde oturum açma sırasında parola değişikliği desteklenmiyor. |
| 40627 |20 |'{0}' Sunucusundaki ve '{1}' veritabanındaki işlem devam ediyor. Lütfen yeniden denemeden önce birkaç dakika bekleyin. |
| 40630 |16 |Parola doğrulama başarısız oldu. Parola çok kısa olduğundan ilke gereksinimlerini karşılamıyor. |
| 40631 |16 |Belirttiğiniz parola çok uzun. Parolada en fazla 128 karakter olmalıdır. |
| 40632 |16 |Parola doğrulama başarısız oldu. Parola yeterince karmaşık olmadığından ilke gereksinimlerini karşılamıyor. |
| 40636 |16 |Ayrılmış veritabanı adı kullanamazsınız '%.&#x2a;ls' Bu işlemde. |
| 40638 |16 |Geçersiz abonelik kimliği (abonelik kimliği). Abonelik yok. |
| 40639 |16 |İstek şemayla uyumlu değil: (şema hatası). |
| 40640 |20 |Sunucu beklenmeyen bir özel durumla karşılaştı. |
| 40641 |16 |Belirtilen konum geçersiz. |
| 40642 |17 |Sunucu şu anda çok meşgul. Lütfen daha sonra tekrar deneyin. |
| 40643 |16 |Belirtilen x-ms-version üstbilgi değeri geçersiz. |
| 40644 |14 |Belirtilen aboneliğe erişim yetkisi verilemedi. |
| 40645 |16 |ServerName (ServerName) boş veya null olamaz. Yalnızca küçük harf ' A'-'z ', 0-9 ve kısa çizgi olabilir. Kısa çizgi, adın içinde veya izlemeyebilir. |
| 40646 |16 |Abonelik KIMLIĞI boş olamaz. |
| 40647 |16 |Abonelikte (abonelik kimliği) sunucu (ServerName) yok. |
| 40648 |17 |Çok fazla istek gerçekleştirildi. Lütfen daha sonra yeniden deneyin. |
| 40649 |16 |Geçersiz içerik türü belirtildi. Yalnızca Application/XML destekleniyor. |
| 40650 |16 |Abonelik (abonelik kimliği) yok veya işlem için mevcut değil. |
| 40651 |16 |Abonelik (abonelik kimliği) devre dışı bırakıldığından sunucu oluşturulamadı. |
| 40652 |16 |Sunucu taşınamıyor veya oluşturulamıyor. Abonelik (abonelik kimliği), Sunucu kotasını aşacak. |
| 40671 |17 |Ağ geçidi ve yönetim hizmeti arasında iletişim hatası. Lütfen daha sonra yeniden deneyin. |
| 40852 |16 |'% Veritabanı açılamıyor. \*ls ', '% sunucusunda. \*ls ' oturum açma tarafından istendi. Veritabanına erişime yalnızca güvenlik özellikli bir bağlantı dizesi kullanılarak izin verilir. Bu veritabanına erişmek için, bağlantı dizelerinizi sunucu FQDN 'SI-' sunucu adı ' olan ' güvenli ' öğesini içerecek şekilde değiştirin. Database. Windows. net ' in ' Server Name '. Database olarak değiştirilmesi gerekir. `secure`. Windows.net. |
| 40914 | 16 | Oturum açma tarafından istenen ' *[sunucu-adı]* ' sunucusu açılamıyor. İstemcinin sunucuya erişmesine izin verilmiyor.<br /><br />Bu hatayı onarmak için bir [sanal ağ kuralı](sql-database-vnet-service-endpoint-rule-overview.md)eklemeyi göz önünde bulundurun. |
| 45168 |16 |SQL Azure sistemi yükleme aşamasındadır ve tek bir SQL veritabanı sunucusu için eşzamanlı DB CRUD işlemlerine bir üst sınır yerleştirmektedir (ör. veritabanı oluşturun). Hata iletisinde belirtilen sunucu, eşzamanlı bağlantı sayısı üst sınırını aştı. Daha sonra tekrar deneyin. |
| 45169 |16 |SQL Azure sistemi yükleme aşamasındadır ve tek bir abonelik için eşzamanlı sunucu CRUD işlemlerinin sayısına bir üst sınır yerleştirmektedir (ör. sunucu oluşturma). Hata iletisinde belirtilen abonelik, eşzamanlı bağlantı sayısı üst sınırını aştı ve istek reddedildi. Daha sonra tekrar deneyin. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SQL veritabanı özellikleri](sql-database-features.md)hakkında bilgi edinin.
* [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md)hakkında bilgi edinin.
* [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)hakkında bilgi edinin.

