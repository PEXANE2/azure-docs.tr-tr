---
title: Kaynak veritabanlarını bağlayan sorunlar
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kaynak veritabanlarına bağlamayla ilişkili bilinen sorunları/hataları nasıl gidereceklerini öğrenin.
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
ms.openlocfilehash: 3eef7476a729c7b83290b9d8d86ba06524bed72b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297091"
---
# <a name="troubleshoot-dms-errors-when-connecting-to-source-databases"></a>Kaynak veritabanlarına bağlanırken DMS hatalarını giderme

Aşağıdaki makalede, Azure Veritabanı Geçiş Hizmeti'ni (DMS) kaynak veritabanınıza bağlanırken karşılaşabileceğiniz olası sorunları nasıl giderilebilirsiniz hakkında ayrıntılı bilgi verilmektedir. Aşağıdaki her bölüm, bağlantının nasıl giderileceğine ilişkin bilgilere olan ayrıntılar ve bağlantılarla birlikte karşılaşabileceğiniz hatayı listeleyen belirli bir kaynak veritabanı türüyle ilgilidir.

## <a name="sql-server"></a>SQL Server

Kaynak SQL Server veritabanına bağlanma yla ilgili olası sorunlar ve bunların nasıl ele alınış ları aşağıdaki tabloda sağlanır.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğunu ve SQL Server'ın uzak bağlantılara izin verecek şekilde yapılandırıldığından doğrulayın.<br> | Hizmet kaynak sunucuyu bulamazsa bu hata oluşur. Sorunu gidermek için, [dinamik bağlantı noktası veya adlandırılmış örnek kullanırken kaynak SQL Server'a bağlanan hata](https://docs.microsoft.com/azure/dms/known-issues-troubleshooting-dms#error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance)makalesine bakın. |
| **Hata 53** - SQL bağlantısı başarısız oldu. (Ayrıca, hata kodları 1, 2, 5, 53, 233, 258, 1225, 11001 için)<br><br> | Hizmet kaynak sunucuya bağlanamazsa bu hata oluşur. Sorunu gidermek için aşağıdaki kaynaklara bakın ve sonra yeniden deneyin. <br><br>  [Bağlantı sorununu gidermek için etkileşimli kullanım kılavuzu](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)<br><br> [SQL Server'ı Azure SQL Veritabanına geçirmek için ön koşullar](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-azure-sql-database) <br><br> [SQL Server'ı Azure SQL Veritabanı'na geçirmek için ön koşullar yönetilen bir örnek](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance) |
| **Hata 18456** - Giriş başarısız oldu.<br> | Bu hata, hizmet sağlanan T-SQL kimlik bilgilerini kullanarak kaynak veritabanına bağlanamazsa oluşur. Sorunu gidermek için girilen kimlik bilgilerini doğrulayın. [Ayrıca MSSQLSERVER_18456'e](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurabilir ve sonra yeniden deneyebilirsiniz. |
| Malformed HesapName{0}değeri ' ' sağlanır. AccountName için beklenen biçim DomainName\UserName'dir<br> | Bu hata, kullanıcı Windows kimlik doğrulamasını seçerancak kullanıcı adını geçersiz biçimde sağlarsa oluşur. Sorunu gidermek için, Windows kimlik doğrulaması için doğru biçimde kullanıcı adı sağlayın veya **SQL Kimlik Doğrulaması'nı**seçin. |

## <a name="aws-rds-mysql"></a>AWS RDS MySQL

Kaynak AWS RDS MySQL veritabanına bağlanma yla ilişkili olası sorunlar ve bunların nasıl ele alınılabildiğini aşağıdaki tabloda bulabilirsiniz.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata [2003]**[HY000] - bağlantı başarısız oldu. HATA [HY000] [MySQL][ODBC x.x(w) sürücüsü] MySQL sunucusuna '{server}' (10060) üzerinden bağlanamıyor | MySQL ODBC sürücüsü kaynak sunucuya bağlanamazsa bu hata oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin.<br> |
| **Hata [2005]**[HY000] - bağlantı başarısız oldu. HATA [HY000] [MySQL][ODBC x.x(w) sürücüsü] Bilinmeyen MySQL sunucu ana bilgisayar '{server}' | Hizmet RDS'de kaynak ana bilgisayarı bulamazsa bu hata oluşur. Sorun, listelenen kaynak yok veya RDS altyapısı ile ilgili bir sorun var, çünkü olabilir. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin.<br> |
| **Hata [1045]**[HY000] - bağlantı başarısız oldu. HATA [HY000] [MySQL][ODBC x.x(w) sürücüsü] Erişim kullanıcı '{user}'@'{server}' (şifre kullanarak: EVET) için reddedildi | MySQL ODBC sürücüsü geçersiz kimlik bilgileri nedeniyle kaynak sunucuya bağlanamıyorsa bu hata oluşur. Girdiğiniz kimlik bilgilerini doğrulayın. Sorun devam ederse, kaynak bilgisayarın doğru kimlik bilgilerine sahip olduğunu doğrulayın. Konsoldaki parolayı sıfırlamanız gerekebilir. Sorunla hala karşılaşırsanız, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin.<br> |
| **Hata [9002]**[HY000] - bağlantı başarısız oldu. HATA [HY000] [MySQL][ODBC x.x(w) sürücüsü] Bağlantı dizesi doğru olmayabilir. Referanslar için portalı ziyaret edin.| Bağlantı dizesi ile ilgili bir sorun nedeniyle bağlantı başarısız olursa bu hata oluşur. Sağlanan bağlantı dizesinin geçerli olduğunu doğrulayın. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin.<br> |
| **İkili günlüğe kaydetmede hata. Değişken binlog_format '{value}' değeri vardır. Lütfen 'satır' olarak değiştirin.** | Bu hata, ikili günlüğe kaydetmede bir hata varsa oluşur; değişken binlog_format yanlış değere sahiptir. Sorunu gidermek için parametre grubundaki binlog_format 'ROW' olarak değiştirin ve ardından örneği yeniden başlatın. Daha fazla bilgi için, [İkili Günlük Seçenekleri ve Değişkenler](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html) veya [AWS RDS MySQL Veritabanı Günlüğü Dosyaları belgelerine](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html)bakın.<br> |

> [!NOTE]
> Kaynak AWS RDS MySQL veritabanına bağlanmayla ilgili sorun giderme sorunları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS Bağlantı sorunları için sorun giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Amazon RDS veritabanı örneğime bağlanan sorunları nasıl çözebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-postgresql"></a>AWS RDS PostgreSQL

Kaynak AWS RDS PostgreSQL veritabanına bağlanma yla ilgili olası sorunlar ve bunların nasıl ele alınılabildiğini aşağıdaki tabloda bulabilirsiniz.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata [101]**[08001] - bağlantı başarısız oldu. HATA [08001] zaman aşımı süresi doldu. | Bu hata, Postgres sürücüsü kaynak sunucuya bağlanamazsa oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin. |
| **Hata: Parametre wal_level değeri '{value}' vardır. Çoğaltmaya izin vermek için lütfen 'mantıksal' olarak değiştirin.** | Bu hata, wal_level parametresi yanlış değere sahipse oluşur. Sorunu gidermek için, parametre grubundards.logical_replication'yi 1 olarak değiştirin ve ardından örneği yeniden başlatın. Daha fazla bilgi için [Amazon RDS'de](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)DMS veya [PostgreSQL kullanarak Azure PostgreSQL'e geçiş için Ön koşullara](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#prerequisites) bakın. |

> [!NOTE]
> Kaynak AWS RDS PostgreSQL veritabanına bağlanmayla ilgili sorun giderme sorunları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS Bağlantı sorunları için sorun giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Amazon RDS veritabanı örneğime bağlanan sorunları nasıl çözebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-sql-server"></a>AWS RDS SQL Server

Kaynak AWS RDS SQL Server veritabanına bağlanma yla ilişkili olası sorunlar ve bunların nasıl ele alınılabildiğini aşağıdaki tabloda sağlanır.

| Hata         | Neden ve sorun giderme ayrıntısı |
| ------------- | ------------- |
| **Hata 53** - SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilemiyordu. Örnek adının doğru olduğunu ve SQL Server'ın uzak bağlantılara izin verecek şekilde yapılandırıldığından doğrulayın. (sağlayıcı: Adlandırılmış Borular Sağlayıcısı, hata: 40 - SQL Server bağlantısı açamadı | Hizmet kaynak sunucuya bağlanamazsa bu hata oluşur. Sorunu gidermek için, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin. |
| **Hata 18456** - Giriş başarısız oldu. Kullanıcı '{user}' için giriş başarısız oldu | Bu hata, hizmet sağlanan T-SQL kimlik bilgileriyle kaynak veritabanına bağlanamazsa oluşur. Sorunu gidermek için girilen kimlik bilgilerini doğrulayın. [Ayrıca MSSQLSERVER_18456'e](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurabilir ve yeniden deneyebilirsiniz. |
| **Hata 87** - Bağlantı dizesi geçerli değildir. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğunu ve SQL Server'ın uzak bağlantılara izin verecek şekilde yapılandırıldığından doğrulayın. (sağlayıcı: SQL Ağ Arabirimleri, hata: 25 - Bağlantı dizesi geçerli değildir) | Bu hata, hizmet geçersiz bir bağlantı dizesi nedeniyle kaynak sunucuya bağlanamazsa oluşur. Sorunu gidermek için sağlanan bağlantı dizesini doğrulayın. Sorun devam ederse, bu tablonun altındaki notta listelenen sorun giderme belgelerine bakın ve sonra yeniden deneyin. |
| **Hata - Sunucu sertifikasına güvenbelgesi yok.** Sunucuyla başarılı bir bağlantı kuruldu, ancak oturum açma işlemi sırasında bir hata oluştu. (sağlayıcı: SSL Sağlayıcı, hata: 0 - Sertifika zinciri güvenilir olmayan bir otorite tarafından verildi.) | Bu hata, kullanılan sertifika güvenilen değilse oluşur. Sorunu gidermek için güvenilebilen bir sertifika bulmanız ve ardından sunucuda etkinleştirmeniz gerekir. Alternatif olarak, bağlanırken Güven Sertifikası seçeneğini seçebilirsiniz. Bu eylemi yalnızca kullanılan sertifikayı biliyorsanız ve sertifikaya güveniyorsanız gerçeklenin. <br> Kendi imzalı bir sertifika kullanılarak şifrelenen TLS bağlantıları güçlü bir güvenlik sağlamaz , ortadaki adam saldırılarına karşı hassastırlar. Üretim ortamında veya internete bağlı sunucularda kendi imzalı sertifikalar kullanarak TLS'ye güvenmeyin. <br> Daha fazla bilgi için Microsoft SQL Server DB Örneği veya Öğretici [ile SSL kullanma:](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Concepts.General.SSL.Using.html) [RDS SQL Server'ı DMS kullanarak Azure'a geçirin.](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) |
| **Hata 300** - Kullanıcının gerekli izinleri yoktur. VIEW SERVER STATE izni nesne '{server}', veritabanı '{database}' üzerinde reddedildi | Bu hata, kullanıcıgeçiş gerçekleştirmek için izin yoksa oluşur. Sorunu gidermek için [GRANT Server Permissions - Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql?view=sql-server-2017) veya Tutorial: Daha fazla ayrıntı için [DMS kullanarak RDS SQL Server'ı Azure'a geçirin.](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) |

> [!NOTE]
> Bir kaynak AWS RDS SQL Server'a bağlanmayla ilgili sorun giderme sorunları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [SQL Server’a bağlantı hatalarını çözme](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)
> * [Amazon RDS veritabanı örneğime bağlanan sorunları nasıl çözebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="known-issues"></a>Bilinen sorunlar

* [Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [MySQL için Azure Veritabanına çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [PostgreSQL için Azure Veritabanına çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Sonraki adımlar

* Makaleyi görüntüleyin [Azure Veritabanı Geçiş Hizmeti PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Makaleyi görüntüleyin [Azure portalını kullanarak MySQL için Azure Veritabanı'ndaki sunucu parametreleri nasıl yapılandırılabilirsiniz.](https://docs.microsoft.com/azure/mysql/howto-server-parameters)
* Makaleyi [görüntüleyin Azure Veritabanı Geçiş Hizmeti'ni kullanmak için ön koşullara genel bakış.](https://docs.microsoft.com/azure/dms/pre-reqs)
* Azure [Veritabanı Geçiş Hizmeti'ni kullanma hakkında SSS'ye](https://docs.microsoft.com/azure/dms/faq)bakın.
