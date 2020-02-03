---
title: Sorun giderme
description: Azure SQL veri ambarı sorunlarını giderme.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/25/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: ce57c48e568e840f3a651a5530f3fba6c0be60b7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721056"
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL veri ambarı sorunlarını giderme
Bu makalede, yaygın sorun giderme sorusu listelenmektedir.

## <a name="connecting"></a>Bağlanmada
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 'NT AUTHORITY\ANONYMOUS LOGON' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, hata: 18456) | Bu hata bir AAD kullanıcısı ana veritabanına bağlanmaya çalıştığında, ancak asıl veritabanında kullanıcı olmadığında meydana gelir.  Bu sorunu düzeltmek için, bağlantı zamanında bağlanmak istediğiniz SQL Veri Ambarı’nı belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla bilgi için [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| Sunucu sorumlusu "MyUserName" geçerli güvenlik bağlamında "asıl" veritabanına erişemiyor. Kullanıcının varsayılan veritabanı açılamıyor. Oturum açılamadı. 'MyUserName' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, hata: 916) | Bu hata bir AAD kullanıcısı ana veritabanına bağlanmaya çalıştığında, ancak asıl veritabanında kullanıcı olmadığında meydana gelir.  Bu sorunu düzeltmek için, bağlantı zamanında bağlanmak istediğiniz SQL Veri Ambarı’nı belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla bilgi için [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| CTAıP hatası                                                  | Bu hata, SQL Server ana veritabanında bir oturum oluşturulduğunda, ancak SQL veri ambarı veritabanında olmadığında ortaya çıkabilir.  Bu hatayla karşılaşırsanız, [güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine göz atın.  Bu makalede, ana bilgisayarda bir oturum açma ve Kullanıcı oluşturma ve ardından SQL veri ambarı veritabanında bir Kullanıcı oluşturma işlemleri açıklanmaktadır. |
| Güvenlik duvarı tarafından engellendi                                          | Azure SQL veritabanları, yalnızca bilinen IP adreslerinin bir veritabanına erişimi olduğundan emin olmak için sunucu ve veritabanı düzeyi güvenlik duvarları tarafından korunur. Güvenlik duvarları varsayılan olarak güvenli hale gelir. Bu, bağlanmadan önce açıkça etkinleştirmeniz ve IP adresi veya adres aralığı yapmanız gerekir.  Güvenlik duvarınızı erişim için yapılandırmak üzere, [sağlama yönergelerindeki](sql-data-warehouse-get-started-provision.md) [istemci IP 'niz için sunucu güvenlik duvarı erişimini yapılandırma](sql-data-warehouse-get-started-provision.md) bölümündeki adımları izleyin. |
| Araçla veya sürücüyle bağlantı yapılamaz                           | SQL veri ambarı, [SMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)'Yi, [Visual Studio için SSDT](sql-data-warehouse-install-visual-studio.md)'yi veya verilerinizi sorgulamak için [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) kullanılmasını önerir. Sürücüler hakkında daha fazla bilgi ve SQL veri ambarı 'na bağlanma hakkında daha fazla bilgi için bkz. [Azure SQL veri ambarı Için sürücüler](sql-data-warehouse-connection-strings.md) ve [Azure SQL veri ambarı makalelerine bağlanma](sql-data-warehouse-connect-overview.md) . |

## <a name="tools"></a>Araçlar
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Visual Studio Nesne Gezgini 'nde AAD kullanıcıları eksik           | Bu bilinen bir sorundur.  Geçici bir çözüm olarak, [sys. database_principals](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?view=sql-server-ver15)içindeki kullanıcıları görüntüleyin.  SQL veri ambarı ile Azure Active Directory kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veri ambarı 'Nda kimlik doğrulama](sql-data-warehouse-authentication.md) . |
| El ile betik oluşturma, betik Sihirbazı 'nı kullanma veya SSMS aracılığıyla bağlanma yavaş, yanıt vermiyor veya hata üretmiyor | Ana veritabanında kullanıcıların oluşturulduğundan emin olun. Komut dosyası seçeneklerinde Ayrıca, Engine sürümünün "Microsoft Azure SQL Veri Ambarı Edition" olarak ayarlandığından ve altyapı türünün "Microsoft Azure SQL Veritabanı" olduğundan emin olun. |
| SSMS 'de betik oluşturma başarısız oluyor                               | "Bağımlı nesneler için betik oluştur" seçeneği "true" olarak ayarlandıysa SQL veri ambarı için betik oluşturma işlemi başarısız olur. Geçici bir çözüm olarak, kullanıcıların **Araçlar-> Seçenekler-> SQL Server Nesne Gezgini-> bağımlı seçenekler için betik oluştur ve yanlış olarak ayarla** seçeneğine el ile gitmesi gerekir |

## <a name="performance"></a>Performans
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Sorgu performansı sorunlarını giderme                            | Belirli bir sorgu sorunlarını gidermeye çalışıyorsanız, [sorgularınızı nasıl izleyebileceğiniz hakkında bilgi edinmeye](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-query-execution)başlayın. |
| TempDB alanı sorunları | [Tempdb](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-tempdb) alan kullanımını izleyin.  TempDB alanını çalıştırmanın yaygın nedenleri şunlardır:<br>-Sorguya ayrılan yeterli kaynak yok, verilerin TempDB 'ye taşımasına neden olur.  Bkz. [Iş yükü yönetimi](resource-classes-for-workload-management.md) <br>-Çok fazla veri hareketine neden olan istatistikler eksik veya güncel değil.  İstatistik oluşturma hakkında ayrıntılı bilgi için bkz. [tablo Istatistiklerini koruma](sql-data-warehouse-tables-statistics.md)<br>-TempDB alanı hizmet düzeyi başına ayrılır.  [SQL veri Ambarınızı](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#scaling-compute) daha yüksek bir DWU ayarına ölçeklendirirken daha fazla tempdb alanı ayırır.|
| Kötü sorgu performansı ve planları genellikle eksik istatistiklerin bir sonucudur | Düşük performansın en yaygın nedeni, tablolarınızdaki istatistiklerin olmamasıdır.  İstatistiklerin nasıl oluşturulacağı ve performanstan ne kadar önemli olduğunu gösteren Ayrıntılar için bkz. [tablo Istatistiklerini koruma](sql-data-warehouse-tables-statistics.md) . |
| Sıraya alınan düşük eşzamanlılık/sorgu sayısı                             | Bellek ayırmayı eşzamanlılık ile dengelemeye anlamak için [Iş yükü yönetimini](resource-classes-for-workload-management.md) anlamak önemlidir. |
| En iyi yöntemleri uygulama                              | Sorgu performansını artırmak için en iyi başlangıç [SQL veri ambarı en iyi yöntemler](sql-data-warehouse-best-practices.md) makaleleridir. |
| Ölçeklendirmeyle performansı artırma                      | Bazen, performansı iyileştirmeye yönelik çözüm, [SQL veri Ambarınızı ölçeklendirerek](sql-data-warehouse-manage-compute-overview.md)sorgularınıza daha fazla işlem gücü eklemektir. |
| Yetersiz Dizin kalitesinin sonucu olarak sorgu performansı yetersiz     | Bazı süreler sorguları, [düşük columnstore dizin kalitesi](../sql-data-warehouse/sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality)nedeniyle yavaşlayabilir.  Daha fazla bilgi ve [bölüm kalitesini artırmak için dizinleri yeniden oluşturma](../sql-data-warehouse/sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)hakkında daha fazla bilgi için bu makaleye bakın. |

## <a name="system-management"></a>Sistem Yönetimi
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: sunucu, izin verilen veritabanı Işlem birimi kotasını (45000) aşacağından işlem gerçekleştirilemedi. | Oluşturmaya çalıştığınız veritabanının [DWU](what-is-a-data-warehouse-unit-dwu-cdwu.md) değerini azaltın ya da [bir kota artışı isteyin](sql-data-warehouse-get-started-create-support-ticket.md). |
| Alan kullanımını araştırma                              | Sisteminizin alan kullanımını anlamak için bkz. [Tablo boyutları]( ../sql-data-warehouse/sql-data-warehouse-tables-overview.md#table-size-queries) . |
| Tabloları yönetmeyle ilgili yardım                                    | Tablolarınızın yönetimiyle ilgili yardım için [tabloya genel bakış] [Genel Bakış] makalesine bakın.  Bu makale ayrıca [tablo veri türleri](sql-data-warehouse-tables-data-types.md), tablo [dağıtma](sql-data-warehouse-tables-distribute.md), tablo [dizini oluşturma](sql-data-warehouse-tables-index.md) [, tablo](sql-data-warehouse-tables-partition.md)istatistikleri ve [geçici tabloları](sql-data-warehouse-tables-temporary.md) [koruma](sql-data-warehouse-tables-statistics.md) gibi daha ayrıntılı konulara bağlantılar içerir. |
| Saydam veri şifrelemesi (TDE) ilerleme çubuğu Azure portal güncelleştirilmiyor | TDE [PowerShell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption)ile durumunu görüntüleyebilirsiniz. |


## <a name="differences-from-sql-database"></a>SQL veritabanı farklılıkları
| Sorun                                 | Çözüm                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Desteklenmeyen SQL veritabanı özellikleri     | [Desteklenmeyen tablo özelliklerine](../sql-data-warehouse/sql-data-warehouse-tables-overview.md#unsupported-table-features)bakın. |
| Desteklenmeyen SQL veritabanı veri türleri   | [Desteklenmeyen veri türlerine](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types)bakın.        |
| SILME ve GÜNCELLEŞTIRME sınırlamaları         | [Desteklenmeyen güncelleştirme ve silme sözdizimini çözmek için](sql-data-warehouse-develop-ctas.md)bkz. [geçici çözümleri güncelleştirme](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements), [geçici çözümleri silme](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements) ve CTAS kullanma. |
| MERGE deyimleri desteklenmiyor      | Bkz. [birleştirme geçici çözümleri](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md#replace-merge-statements).                  |
| Saklı yordam sınırlamaları          | Saklı yordamların bazı sınırlamalarını anlamak için [saklı yordam sınırlamaları](../sql-data-warehouse/sql-data-warehouse-develop-stored-procedures.md#limitations) bölümüne bakın. |
| UDF 'ler SELECT deyimlerini desteklemez | Bu, UDF 'lerimizin geçerli bir sınırlamasıdır.  Destekliyoruz sözdizimi için bkz. [oluşturma işlevi](https://docs.microsoft.com/sql/t-sql/statements/create-function-sql-data-warehouse?view=aps-pdw-2016-au7) . |

## <a name="next-steps"></a>Sonraki adımlar
Sorununuzla ilgili çözüm bulma konusunda daha fazla yardım için, deneyebileceğiniz bazı diğer kaynaklar aşağıda verilmiştir.

* [Bloglar](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse)
* [Stack Overflow forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)