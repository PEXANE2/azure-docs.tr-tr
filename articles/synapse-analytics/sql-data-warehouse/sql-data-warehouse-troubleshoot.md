---
title: Sorun giderme
description: Azure Synapse Analytics sorun giderme.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: b24706943cdf59fba89a8007c4914b628b9e34d5
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632975"
---
# <a name="troubleshooting-sql-analytics-in-azure-synapse"></a>Azure Synapse'de SQL Analytics'te Sorun Giderme

Bu makalede, sık karşılaşılan sorun giderme sorusu listelenebis.

## <a name="connecting"></a>Bağlanma

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 'NT AUTHORITY\ANONYMOUS LOGON' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, Hata: 18456) | Bu hata, bir Azure AD kullanıcısı ana veritabanına bağlanmaya çalıştığında, ancak ana veritabanında bir kullanıcı olmadığında oluşur.  Bu sorunu düzeltmek için, bağlantı sırasında bağlanmak istediğiniz SQL havuzunu belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla ayrıntı için [Güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| Sunucu sorumlusu "MyUserName" geçerli güvenlik bağlamında "asıl" veritabanına erişemiyor. Kullanıcının varsayılan veritabanı açılamıyor. Oturum açılamadı. 'MyUserName' kullanıcısı için oturum açma başarısız oldu. (Microsoft SQL Server, Hata: 916) | Bu hata, bir Azure AD kullanıcısı ana veritabanına bağlanmaya çalıştığında, ancak ana veritabanında bir kullanıcı olmadığında oluşur.  Bu sorunu düzeltmek için, bağlantı sırasında bağlanmak istediğiniz SQL havuzunu belirtin veya kullanıcıyı ana veritabanına ekleyin.  Daha fazla ayrıntı için [Güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bakın. |
| CTAIP hatası                                                  | Bu hata, SQL server ana veritabanında bir giriş oluşturulduğunda oluşabilir, ancak SQL veritabanında oluşturulmaz.  Bu hatayla karşılaşırsanız, Güvenlik [genel bakış](sql-data-warehouse-overview-manage-security.md) makalesine bir göz atın.  Bu makalede, master üzerinde bir giriş ve kullanıcı oluşturmak için nasıl ve daha sonra nasıl SQL veritabanında bir kullanıcı oluşturmak için açıklar. |
| Güvenlik Duvarı tarafından engellendi                                          | SQL havuzları, yalnızca bilinen IP adreslerinin bir veritabanına erişebilmesini sağlamak için güvenlik duvarları tarafından korunur. Güvenlik duvarları varsayılan olarak güvenlidir, bu da bağlanabilmeniz için açıkça etkinleştirmeniz ve IP adresi veya adres aralığı anlamına gelir.  Erişim için güvenlik duvarınızı yapılandırmak için, Sağlama yönergelerinde [istemci IP'niz için sunucu güvenlik duvarı erişimini yapılandırma](create-data-warehouse-portal.md) [adımlarını](create-data-warehouse-portal.md)izleyin. |
| Araç veya sürücüyle bağlanamıyor                           | Synapse SQL havuzu, Visual Studio için [SSMS](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15), [Visual Studio için SSDT](sql-data-warehouse-install-visual-studio.md)veya verilerinizi sorgulamak için [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) kullanmanızı önerir. Sürücüler ve Azure Synapse'ye bağlanma hakkında daha fazla bilgi için Azure [Synapse sürücüleri](sql-data-warehouse-connection-strings.md) ve Azure [Synapse makalelerine bağlan'a](sql-data-warehouse-connect-overview.md) bakın. |

## <a name="tools"></a>Araçlar

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Visual Studio nesne gezgini Azure AD kullanıcıları eksik           | Bu bilinen bir sorundur.  Geçici çözüm olarak, [sys.database_principals'deki](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?view=sql-server-ver15)kullanıcıları görüntüleyin.  Synapse SQL havuzuile Azure Active Directory'yi kullanma hakkında daha fazla bilgi edinmek için [Azure Synapse'ye kimlik doğrulama](sql-data-warehouse-authentication.md) bilgisini edinin. |
| Komut dosyası oluşturma, komut dosyası oluşturma sihirbazını kullanma veya SSMS üzerinden bağlanma yavaştır, yanıt vermez veya hata üretir | Kullanıcıların ana veritabanında oluşturulduğundan emin olun. Komut dosyası oluşturma seçeneklerinde, motor sürümünün "Microsoft Azure SQL Veri Ambarı Sürümü" ve motor türü "Microsoft Azure SQL Veritabanı" olarak ayarlandıklarında da emin olun. |
| SSMS'te komut dosyaları oluşturma başarısız olur                               | Synapse SQL havuzu için komut dosyası oluşturma seçeneği "Bağımlı nesneler için komut dosyası oluşturma" seçeneği "True" olarak ayarlanırsa başarısız olur. Geçici çözüm olarak, kullanıcılar araçlara el ile gitmeli **-> Seçenekleri -sql server object explorer >-> Bağımlı seçenekler için komut dosyası oluşturma ve yanlış olarak ayarlanmalıdır** |

## <a name="performance"></a>Performans

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Performans sorun giderme sorgula                            | Belirli bir sorguyu gidermeye çalışıyorsanız, [sorgularınızı nasıl izleyeceğinizi öğrenme](sql-data-warehouse-manage-monitor.md#monitor-query-execution)ile başlayın. |
| TempDB uzay sorunları | [TempDB](sql-data-warehouse-manage-monitor.md#monitor-tempdb) alan kullanımını izleyin.  TempDB alanının tükenmesinin yaygın nedenleri şunlardır:<br>- TempDB'ye veri dökülmesine neden olan sorguya ayrılan yeterli kaynak yok.  Bkz. [İş Yükü yönetimi](resource-classes-for-workload-management.md) <br>- İstatistikler eksik veya güncel olmayan aşırı veri hareketine neden olur.  Bkz. İstatistik oluşturma hakkında ayrıntılar için [tablo istatistiklerini koruma](sql-data-warehouse-tables-statistics.md)<br>- TempDB alanı hizmet düzeyine göre ayrılmıştır.  [SQL havuzunuzu](sql-data-warehouse-manage-compute-overview.md#scaling-compute) daha yüksek bir DWU ayarına ölçeklemek daha fazla TempDB alanı ayırır.|
| Kötü sorgu performansı ve planlar genellikle eksik istatistiklerin bir sonucudur | Düşük performansın en yaygın nedeni tablolarınızdaki istatistiklerin olmamasıdır.  Bkz. İstatistiklerin nasıl oluşturulup oluşturulabildikleri ve bunların performansınız için neden kritik olduğu hakkında ayrıntılar için [tablo istatistiklerini koruma](sql-data-warehouse-tables-statistics.md) konusu. |
| Düşük eşzamanlılık / sorgular sıraya                             | [İş Yükü yönetimini](resource-classes-for-workload-management.md) anlamak, bellek ayırmayı eşzamanlılıkla nasıl dengelenebildiğini anlamak için önemlidir. |
| En iyi uygulamalar nasıl uygulanır?                              | Sorgu performansını artırmanın yollarını öğrenmeye başlamak için en iyi yer [Synapse SQL havuzu en iyi uygulamalar](sql-data-warehouse-best-practices.md) makalesidir. |
| Ölçekleme ile performansı artırma                      | Bazen performansı artırmanın çözümü, [SQL havuzunuzu ölçekleyerek](sql-data-warehouse-manage-compute-overview.md)sorgularınıza daha fazla bilgi işlem gücü eklemektir. |
| Düşük dizin kalitesi sonucu düşük sorgu performansı     | Bazı zamanlar sorguları Kötü [sütun deposu dizin kalitesi](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality)nedeniyle yavaşlayabilirsiniz.  Daha fazla bilgi ve segment kalitesini artırmak için [dizinleri nasıl yeniden oluştureceğiniz](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)için bu makaleye bakın. |

## <a name="system-management"></a>Sistem yönetimi

| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: Sunucu izin verilen 45000 Veritabanı İşlem Birimi kotasını aşacağı için işlemi gerçekleştiremedi. | Oluşturmaya çalıştığınız veritabanının [DWU'yu](what-is-a-data-warehouse-unit-dwu-cdwu.md) azaltın veya [kota artışı isteyin.](sql-data-warehouse-get-started-create-support-ticket.md) |
| Alan kullanımını araştırma                              | Sisteminizin alan kullanımını anlamak için [Tablo boyutlarına](sql-data-warehouse-tables-overview.md#table-size-queries) bakın. |
| Tabloları yönetme konusunda yardım                                    | Tablolarınızı yönetmekonusunda yardım için [Tabloya genel bakış](sql-data-warehouse-tables-overview.md) makalesine bakın.  Bu makalede ayrıca Tablo veri [türleri](sql-data-warehouse-tables-data-types.md)gibi daha ayrıntılı konulara bağlantılar içerir , [tablo dağıtma](sql-data-warehouse-tables-distribute.md), [tablo dizinleme](sql-data-warehouse-tables-index.md), [Tablo bölümleme](sql-data-warehouse-tables-partition.md), [tablo istatistikleri](sql-data-warehouse-tables-statistics.md) ve Geçici [tablolar](sql-data-warehouse-tables-temporary.md)bakımı . |
| Saydam veri şifreleme (TDE) ilerleme çubuğu Azure portalında güncelleştirmiyor | Powershell ile TDE durumunu [powershell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption)görüntüleyebilirsiniz. |

## <a name="differences-from-sql-database"></a>SQL Veritabanından Farklar

| Sorun                                 | Çözüm                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Desteklenmeyen SQL Veritabanı özellikleri     | [Desteklenmeyen tablo özelliklerine](sql-data-warehouse-tables-overview.md#unsupported-table-features)bakın. |
| Desteklenmeyen SQL Veritabanı veri türleri   | [Desteklenmeyen veri türlerine](sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types)bakın.        |
| DELETE ve UPDATE sınırlamaları         | Bkz. [UPDATE geçici işleri,](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements) [DELETE geçici işleri](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements) ve [desteklenmeyen UPDATE ve DELETE sözdizimini aşmak için CTAS'ı kullanma.](sql-data-warehouse-develop-ctas.md) |
| MERGE deyimi desteklenmiyor      | Bkz. [BİrLEŞTİrme geçici işleri.](sql-data-warehouse-develop-ctas.md#replace-merge-statements)                  |
| Depolanan yordam sınırlamaları          | Depolanan yordamların bazı sınırlamalarını anlamak için [Depolanan yordam sınırlamalarına](sql-data-warehouse-develop-stored-procedures.md#limitations) bakın. |
| UDF'ler SELECT ifadelerini destekletmiyor | Bu, UDF'lerimizin geçerli bir sınırlamasıdır.  Desteklediğimiz sözdizimi için [CREATE FONKSIYONU'na](https://docs.microsoft.com/sql/t-sql/statements/create-function-sql-data-warehouse?view=aps-pdw-2016-au7) bakın. |

## <a name="next-steps"></a>Sonraki adımlar

Sorununuza çözüm bulmakonusunda daha fazla yardım için, deneyebileceğiniz diğer bazı kaynaklar aşağıda veda edebilirsiniz.

* [Bloglar](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Özellik istekleri](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Destek bileti oluşturun](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse)
* [Stack Overflow forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)
