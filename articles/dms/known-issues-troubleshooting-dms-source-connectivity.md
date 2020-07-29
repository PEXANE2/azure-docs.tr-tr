---
title: Kaynak veritabanlarını bağlama sorunları
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'ni kaynak veritabanlarına bağlama ile ilişkili bilinen sorunların/hataların nasıl giderileceği hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 4421ad4260ff8eada87cdebc031d09922860324f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84196323"
---
# <a name="troubleshoot-dms-errors-when-connecting-to-source-databases"></a>Kaynak veritabanlarına bağlanırken DMS hatalarını giderme

Aşağıdaki makalede, Azure veritabanı geçiş hizmeti 'ni (DMS) kaynak veritabanınıza bağlarken karşılaşabileceğiniz olası sorunları nasıl ele geçirebileceği hakkında ayrıntılı bilgi verilmektedir. Aşağıdaki her bir bölüm, belirli bir kaynak veritabanı türüyle ilgilidir, ayrıntıları ve bağlantı sorunlarını gidermeye ilişkin bilgilerin bağlantılarıyla birlikte karşılaşabileceğiniz hatayı listeler.

## <a name="sql-server"></a>SQL Server

Kaynak SQL Server veritabanına bağlanma ile ilişkili olası sorunlar ve bunların nasıl ele alınacağını aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun.<br> | Bu hata, hizmet kaynak sunucuyu bulamıyorsa oluşur. Sorunu gidermek için [dinamik bağlantı noktası veya adlandırılmış örnek kullanılırken kaynak SQL Server bağlantı hatası](https://docs.microsoft.com/azure/dms/known-issues-troubleshooting-dms#error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance)makalesine bakın. |
| **Hata 53** -SQL bağlantısı başarısız oldu. (Ayrıca, hata kodları 1, 2, 5, 53, 233, 258, 1225, 11001 için)<br><br> | Hizmet kaynak sunucuya bağlanamıyorsa bu hata oluşur. Sorunu gidermek için aşağıdaki kaynaklara bakın ve sonra yeniden deneyin. <br><br>  [Bağlantı sorununu gidermek için etkileşimli Kullanıcı Kılavuzu](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)<br><br> [SQL Server Azure SQL veritabanı 'na geçirmek için Önkoşullar](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-azure-sql-database) <br><br> [SQL Server Azure SQL yönetilen örneği 'ne geçirme önkoşulları](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance) |
| **Hata 18456** -oturum açma başarısız.<br> | Bu hata, hizmet, kaynak veritabanına, belirtilen T-SQL kimlik bilgilerini kullanarak bağlanamıyorsa oluşur. Sorunu gidermek için, girilen kimlik bilgilerini doğrulayın. Ayrıca, bu tablonun altındaki notta listelenen [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya sorun giderme belgelerine de başvurabilirsiniz ve sonra yeniden deneyin. |
| Hatalı biçimlendirilmiş AccountName değeri ' {0} ' belirtildi. AccountName için beklenen biçim etkialanı \ Kullanıcı adı<br> | Kullanıcı Windows kimlik doğrulaması seçerse bu hata oluşur ancak kullanıcı adını geçersiz biçimde sağlar. Sorunu gidermek için, Windows kimlik doğrulaması için doğru biçimde Kullanıcı adı sağlayın ya da **SQL kimlik doğrulaması**' nı seçin. |

## <a name="aws-rds-mysql"></a>AWS RDS MySQL

Bir kaynak AWS RDS MySQL veritabanına bağlanma ile ilgili olası sorunlar ve bunların nasıl ele alınacağını aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata [2003]**[HY000]-bağlantı başarısız oldu. HATA [HY000] [MySQL] [ODBC x. x (w) sürücü], ' {Server} ' üzerinde MySQL sunucusuna bağlanamıyor (10060) | MySQL ODBC sürücüsü kaynak sunucuya bağlanamıyorsa bu hata oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin.<br> |
| **Hata [2005]**[HY000]-bağlantı başarısız oldu. HATA [HY000] [MySQL] [ODBC x. x (w) sürücü] bilinmeyen MySQL sunucu Konağı ' {Server} ' | Bu hata, hizmet RDS üzerinde kaynak Konağı bulamazsa oluşur. Sorun, listelenen kaynağın var olmaması ya da RDS altyapısıyla ilgili bir sorun olması olabilir. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin.<br> |
| **Hata [1045]**[HY000]-bağlantı başarısız oldu. HATA [HY000] [MySQL] [ODBC x. x (w) sürücü] ' {user} ' @ ' {Server} ' kullanıcısı için erişim reddedildi (parola kullanarak: Evet) | MySQL ODBC sürücüsü geçersiz kimlik bilgileri nedeniyle kaynak sunucuya bağlanamıyorsa bu hata oluşur. Girdiğiniz kimlik bilgilerini doğrulayın. Sorun devam ederse, kaynak bilgisayarın doğru kimlik bilgilerine sahip olduğunu doğrulayın. Konsolunda parolayı sıfırlamanız gerekebilir. Sorunla hala karşılaşıyorsanız, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin.<br> |
| **Hata [9002]**[HY000]-bağlantı başarısız oldu. HATA [HY000] [MySQL] [ODBC x. x (w) sürücü] bağlantı dizesi doğru olmayabilir. Başvurular için portalı ziyaret edin.| Bağlantı dizesindeki bir sorun nedeniyle bağlantı başarısız olursa bu hata oluşur. Belirtilen bağlantı dizesinin geçerli olduğunu doğrulayın. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin.<br> |
| **İkili günlüğe kaydetme hatası. Binlog_format değişkeni ' {Value} ' değerine sahip. Lütfen bunu ' Row ' olarak değiştirin.** | Bu hata, ikili günlüğe kaydetme sırasında bir hata oluşursa oluşur; binlog_format değişkeni yanlış değere sahip. Sorunu gidermek için, parametre grubundaki binlog_format ' ROW ' olarak değiştirin ve ardından örneği yeniden başlatın. Daha fazla bilgi için bkz. [Ikili günlük seçenekleri ve değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html) veya [AWS RDS MySQL veritabanı günlük dosyaları belgeleri](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html).<br> |

> [!NOTE]
> Kaynak AWS RDS MySQL veritabanına bağlanmayla ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS bağlantı sorunları sorunlarını giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Nasıl yaparım? Amazon RDS veritabanı örneğine bağlanma sorunları çözümlensin mi?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-postgresql"></a>AWS RDS PostgreSQL

Kaynak AWS RDS PostgreSQL veritabanına bağlanma ile ilişkili olası sorunlar ve bunların nasıl ele alınacağını aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata [101]**[08001]-bağlantı başarısız oldu. HATA [08001] zaman aşımı süresi doldu. | Postgres sürücüsü kaynak sunucuya bağlanamıyorsa bu hata oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin. |
| **Hata: wal_level parametresi ' {Value} ' değerine sahip. Çoğaltmaya izin vermek için lütfen bunu ' Logical ' olarak değiştirin.** | Parametrenin değeri yanlış wal_level bu hata oluşur. Sorunu gidermek için, parametre grubundaki RDS. logical_replication öğesini 1 olarak değiştirin ve ardından örneği yeniden başlatın. Daha fazla bilgi için bkz. [Amazon RDS ÜZERINDE DMS veya PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html) [kullanarak Azure PostgreSQL 'e geçiş için önkoşulları](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#prerequisites) . |

> [!NOTE]
> Kaynak AWS RDS PostgreSQL veritabanına bağlanmayla ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS bağlantı sorunları sorunlarını giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Nasıl yaparım? Amazon RDS veritabanı örneğine bağlanma sorunları çözümlensin mi?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-sql-server"></a>AWS RDS SQL Server

Kaynak AWS RDS SQL Server veritabanına bağlanma ile ilgili olası sorunlar ve bunların nasıl ele alınacağını aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata 53** -SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değil. Örnek adının doğru olduğundan ve SQL Server uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcı: adlandırılmış kanallar sağlayıcısı, hata: 40-SQL Server bağlantı açılamıyor | Hizmet kaynak sunucuya bağlanamıyorsa bu hata oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin. |
| **Hata 18456** -oturum açma başarısız. ' {User} ' kullanıcısı için oturum açma başarısız | Hizmet, kaynak veritabanına, belirtilen T-SQL kimlik bilgileriyle bağlanamıyorsa bu hata oluşur. Sorunu gidermek için, girilen kimlik bilgilerini doğrulayın. Ayrıca, bu tablonun altındaki notta listelenen [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya sorun giderme belgelerine de başvurabilirsiniz ve yeniden deneyin. |
| **Hata 87** -bağlantı dizesi geçerli değil. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcı: SQL ağ arabirimleri, hata: 25 bağlantı dizesi geçerli değil) | Bu hata, hizmet geçersiz bir bağlantı dizesi nedeniyle kaynak sunucuya bağlanamıyorsa oluşur. Sorunu gidermek için, belirtilen bağlantı dizesini doğrulayın. Sorun devam ederse, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve sonra yeniden deneyin. |
| **Hata-sunucu sertifikasına güvenilmiyor.** Sunucuyla başarıyla bağlantı kurdu, ancak oturum açma işlemi sırasında bir hata oluştu. (sağlayıcı: SSL sağlayıcısı, hata: 0-sertifika zinciri, güvenilmeyen bir yetkili tarafından verilmiş.) | Kullanılan sertifika güvenilir değilse bu hata oluşur. Sorunu gidermek için, güvenilir olabilecek bir sertifika bulmanız ve sonra sunucuda etkinleştirebilmeniz gerekir. Alternatif olarak, bağlanırken güven sertifikası seçeneğini de belirleyebilirsiniz. Bu eylemi yalnızca kullanılan sertifika hakkında bilginiz varsa ve ona güveniyorsanız yapın. <br> Kendinden imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü güvenlik sağlamaz; Bu kişiler, ortadaki adam saldırılarına maruz kalabilir. Bir üretim ortamında veya internet 'e bağlı sunuculardaki otomatik olarak imzalanan sertifikalar kullanarak TLS 'ye güvenmeyin. <br> Daha fazla bilgi için, bkz. [MICROSOFT SQL Server DB örneği veya öğreticiyle SSL kullanmak](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Concepts.General.SSL.Using.html) IÇIN, [DMS kullanarak RDS SQL Server 'Yi Azure 'a geçirin](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites). |
| **Hata 300** -Kullanıcı gerekli izinlere sahip değil. ' {Database} ' adlı veritabanındaki ' {Server} ' nesnesi üzerinde sunucu durumunu görüntüle izni reddedildi | Bu hata, kullanıcının geçiş gerçekleştirme izni yoksa oluşur. Sorunu gidermek için bkz. [sunucu Izinleri verme-Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql?view=sql-server-2017) veya öğretici: daha fazla ayrıntı için [DMS kullanarak RDS SQL Server Azure 'a geçirme](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) . |

> [!NOTE]
> Bir kaynak AWS RDS SQL Server bağlanmayla ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [SQL Server’a bağlantı hatalarını çözme](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)
> * [Nasıl yaparım? Amazon RDS veritabanı örneğine bağlanma sorunları çözümlensin mi?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="known-issues"></a>Bilinen sorunlar

* [Azure SQL veritabanı 'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [MySQL için Azure veritabanı 'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [PostgreSQL için Azure veritabanı 'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure veritabanı geçiş hizmeti PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration)makalesini görüntüleyin.
* [Azure Portal kullanarak MySQL Için Azure veritabanı 'nda sunucu parametrelerini yapılandırma](https://docs.microsoft.com/azure/mysql/howto-server-parameters)makalesini görüntüleyin.
* [Azure veritabanı geçiş hizmeti 'ni kullanmaya yönelik önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)makalesini görüntüleyin.
* [Azure veritabanı geçiş hizmeti 'ni kullanma hakkında SSS](https://docs.microsoft.com/azure/dms/faq)bölümüne bakın.
