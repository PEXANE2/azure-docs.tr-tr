---
title: SYNAPSE SQL sorunlarını giderme
description: Azure SYNAPSE Analytics 'te SYNAPSE SQL sorunlarını giderme.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: b2ae3aa77383888c320ed58e03e73b2e306feeba
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213780"
---
# <a name="troubleshooting-synapse-sql-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL sorunlarını giderme

Bu makalede, SYNAPSE SQL 'de genel sorun giderme sorunları listelenmektedir.

## <a name="connecting"></a>Bağlanmada

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 'NT AUTHORITY\ANONYMOUS LOGON' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, hata: 18456) | Bu hata, bir Azure AD kullanıcısı ana veritabanına bağlanmaya çalıştığında ancak ana veritabanında bir kullanıcı olmadığında oluşur.  Bu sorunu düzeltmek için, bağlantı sırasında bağlanmak istediğiniz SQL havuzunu belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla bilgi için [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| Sunucu sorumlusu "MyUserName" geçerli güvenlik bağlamında "asıl" veritabanına erişemiyor. Kullanıcının varsayılan veritabanı açılamıyor. Oturum açılamadı. 'MyUserName' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, hata: 916) | Bu hata, bir Azure AD kullanıcısı ana veritabanına bağlanmaya çalıştığında ancak ana veritabanında bir kullanıcı olmadığında oluşur.  Bu sorunu düzeltmek için, bağlantı sırasında bağlanmak istediğiniz SQL havuzunu belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla bilgi için [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| CTAıP hatası                                                  | Bu hata, SQL Server ana veritabanında bir oturum oluşturulduğunda, ancak SQL veritabanında olmadığında ortaya çıkabilir.  Bu hatayla karşılaşırsanız, [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine göz atın.  Bu makalede, ana bilgisayarda bir oturum açma ve Kullanıcı oluşturma ve ardından SQL veritabanında bir Kullanıcı oluşturma işlemleri açıklanmaktadır. |
| Güvenlik duvarı tarafından engellendi                                          | SQL havuzları, yalnızca bilinen IP adreslerinin bir veritabanına erişimi olduğundan emin olmak için güvenlik duvarları tarafından korunur. Güvenlik duvarları varsayılan olarak güvenli hale gelir. Bu, bağlanmadan önce açıkça etkinleştirmeniz ve IP adresi veya adres aralığı yapmanız gerekir.  Güvenlik duvarınızı erişim için yapılandırmak üzere, [sağlama yönergelerindeki](create-data-warehouse-portal.md) [istemci IP 'niz için sunucu güvenlik duvarı erişimini yapılandırma](create-data-warehouse-portal.md) bölümündeki adımları izleyin. |
| Araçla veya sürücüyle bağlantı yapılamaz                           | SYNAPSE SQL havuzu, [SMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)'Yi, [Visual Studio için SSDT](sql-data-warehouse-install-visual-studio.md)'yi veya verilerinizi sorgulamak için [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) kullanılmasını önerir. Sürücüler hakkında daha fazla bilgi ve Azure SYNAPSE 'a bağlanma hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Için sürücüler](sql-data-warehouse-connection-strings.md) ve [Azure SYNAPSE makalelerine bağlanma](sql-data-warehouse-connect-overview.md) . |

## <a name="tools"></a>Araçlar

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Visual Studio Nesne Gezgini 'nde Azure AD kullanıcıları eksik           | Bu bilinen bir sorundur.  Geçici bir çözüm olarak, [sys. database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)içindeki kullanıcıları görüntüleyin.  SYNAPSE SQL havuzu ile Azure Active Directory kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure SYNAPSE kimlik doğrulaması](sql-data-warehouse-authentication.md) . |
| El ile betik oluşturma, betik Sihirbazı 'nı kullanma veya SSMS aracılığıyla bağlanma yavaş, yanıt vermiyor veya hata üretmiyor | Ana veritabanında kullanıcıların oluşturulduğundan emin olun. Komut dosyası seçeneklerinde Ayrıca, Engine sürümünün "Microsoft Azure SQL Veri Ambarı Edition" olarak ayarlandığından ve altyapı türünün "Microsoft Azure SQL Veritabanı" olduğundan emin olun. |
| SSMS 'de betik oluşturma başarısız oluyor                               | "Bağımlı nesneler için betik oluştur" seçeneği "true" olarak ayarlandıysa, SYNAPSE SQL havuzu için betik oluşturma işlemi başarısız olur. Geçici bir çözüm olarak, kullanıcıların **Araçlar-> seçenekler->SQL Server Nesne Gezgini-> bağımlı seçenekler için betik oluştur ve yanlış olarak ayarla** seçeneğine el ile gitmesi gerekir |

## <a name="performance"></a>Performans

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Sorgu performansı sorunlarını giderme                            | Belirli bir sorgu sorunlarını gidermeye çalışıyorsanız, [sorgularınızı nasıl izleyebileceğiniz hakkında bilgi edinmeye](sql-data-warehouse-manage-monitor.md#monitor-query-execution)başlayın. |
| TempDB alanı sorunları | [Tempdb](sql-data-warehouse-manage-monitor.md#monitor-tempdb) alan kullanımını izleyin.  TempDB alanını çalıştırmanın yaygın nedenleri şunlardır:<br>-Sorguya ayrılan yeterli kaynak yok, verilerin TempDB 'ye taşımasına neden olur.  Bkz. [Iş yükü yönetimi](resource-classes-for-workload-management.md) <br>-Çok fazla veri hareketine neden olan istatistikler eksik veya güncel değil.  İstatistik oluşturma hakkında ayrıntılı bilgi için bkz. [tablo Istatistiklerini koruma](sql-data-warehouse-tables-statistics.md)<br>-TempDB alanı hizmet düzeyi başına ayrılır.  [SQL havuzunuzu](sql-data-warehouse-manage-compute-overview.md#scaling-compute) daha yüksek bir DWU ayarına ölçeklendirirken daha fazla tempdb alanı ayırır.|
| Kötü sorgu performansı ve planları genellikle eksik istatistiklerin bir sonucudur | Düşük performansın en yaygın nedeni, tablolarınızdaki istatistiklerin olmamasıdır.  İstatistiklerin nasıl oluşturulacağı ve performanstan ne kadar önemli olduğunu gösteren Ayrıntılar için bkz. [tablo Istatistiklerini koruma](sql-data-warehouse-tables-statistics.md) . |
| Sıraya alınan düşük eşzamanlılık/sorgu sayısı                             | Bellek ayırmayı eşzamanlılık ile dengelemeye anlamak için [Iş yükü yönetimini](resource-classes-for-workload-management.md) anlamak önemlidir. |
| En iyi yöntemleri uygulama                              | Sorgu performansını artırmanın yollarını öğrenmek için başlamak için en iyi yer [SYNAPSE SQL Pool en iyi yöntemler](sql-data-warehouse-best-practices.md) makaleleridir. |
| Ölçeklendirmeyle performansı artırma                      | Bazen, performansı iyileştirmeye yönelik çözüm, [SQL havuzunuzu ölçeklendirerek](sql-data-warehouse-manage-compute-overview.md)sorgularınıza daha fazla işlem gücü eklemektir. |
| Yetersiz Dizin kalitesinin sonucu olarak sorgu performansı yetersiz     | Bazı süreler sorguları, [düşük columnstore dizin kalitesi](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality)nedeniyle yavaşlayabilir.  Daha fazla bilgi ve [bölüm kalitesini artırmak için dizinleri yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)hakkında daha fazla bilgi için bu makaleye bakın. |

## <a name="system-management"></a>Sistem Yönetimi

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: sunucu, izin verilen veritabanı Işlem birimi kotasını (45000) aşacağından işlem gerçekleştirilemedi. | Oluşturmaya çalıştığınız veritabanının [DWU](what-is-a-data-warehouse-unit-dwu-cdwu.md) değerini azaltın ya da [bir kota artışı isteyin](sql-data-warehouse-get-started-create-support-ticket.md). |
| Alan kullanımını araştırma                              | Sisteminizin alan kullanımını anlamak için bkz. [Tablo boyutları](sql-data-warehouse-tables-overview.md#table-size-queries) . |
| Tabloları yönetmeyle ilgili yardım                                    | Tablolarınızı yönetme hakkında yardım almak için [tabloya genel bakış](sql-data-warehouse-tables-overview.md) makalesine bakın.  Bu makale ayrıca [tablo veri türleri](sql-data-warehouse-tables-data-types.md), tablo [dağıtma](sql-data-warehouse-tables-distribute.md), tablo [dizini oluşturma](sql-data-warehouse-tables-index.md) [, tablo](sql-data-warehouse-tables-partition.md)istatistikleri ve [geçici tabloları](sql-data-warehouse-tables-temporary.md) [koruma](sql-data-warehouse-tables-statistics.md) gibi daha ayrıntılı konulara bağlantılar içerir. |
| Saydam veri şifrelemesi (TDE) ilerleme çubuğu Azure portal güncelleştirilmiyor | TDE [PowerShell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)ile durumunu görüntüleyebilirsiniz. |

## <a name="differences-from-sql-database"></a>SQL veritabanı farklılıkları

| Sorun                                 | Çözüm                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Desteklenmeyen SQL veritabanı özellikleri     | [Desteklenmeyen tablo özelliklerine](sql-data-warehouse-tables-overview.md#unsupported-table-features)bakın. |
| Desteklenmeyen SQL veritabanı veri türleri   | [Desteklenmeyen veri türlerine](sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types)bakın.        |
| SILME ve GÜNCELLEŞTIRME sınırlamaları         | [Desteklenmeyen güncelleştirme ve silme sözdizimini çözmek için](sql-data-warehouse-develop-ctas.md)bkz. [geçici çözümleri güncelleştirme](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements), [geçici çözümleri silme](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements) ve CTAS kullanma. |
| MERGE deyimleri desteklenmiyor      | Bkz. [birleştirme geçici çözümleri](sql-data-warehouse-develop-ctas.md#replace-merge-statements).                  |
| Saklı yordam sınırlamaları          | Saklı yordamların bazı sınırlamalarını anlamak için [saklı yordam sınırlamaları](sql-data-warehouse-develop-stored-procedures.md#limitations) bölümüne bakın. |
| UDF 'ler SELECT deyimlerini desteklemez | Bu, UDF 'lerimizin geçerli bir sınırlamasıdır.  Destekliyoruz sözdizimi için bkz. [oluşturma işlevi](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) . |

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzla ilgili çözüm bulma konusunda daha fazla yardım için, deneyebileceğiniz bazı diğer kaynaklar aşağıda verilmiştir.

* [Bloglar](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Destek bileti oluşturun](sql-data-warehouse-get-started-create-support-ticket.md)
* [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html)
* [Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)
