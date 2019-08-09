---
title: Azure SQL veri ambarı sürüm notları Haziran 2018 | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/23/2018
ms.author: anjangsh
ms.reviewer: jrasnick
ms.openlocfilehash: 4348a634fd5b2b33f36d8e79f28caf659b82ccf4
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "67626145"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>Azure SQL veri ambarı 'ndaki yenilikler nelerdir? Haziran 2018
Azure SQL veri ambarı geliştirmeleri sürekli olarak alır. Bu makalede Haziran 2018 ' de sunulan yeni özellikler ve değişiklikler açıklanmaktadır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="user-defined-restore-points"></a>Kullanıcı tanımlı geri yükleme noktaları
SQL veri ambarı, sekiz saatlik bir kurtarma noktası hedefini (RPO) garanti etmek için her 8 saatte bir veri ambarınızın anlık görüntülerini otomatik olarak alır. Bu otomatik anlık görüntüler, veri Ambarınızı çalıştırmanın yönetim yükünü kolaylaştırırken, iş gereksiniminizi temel alarak kritik zamanlarda anlık görüntü alma ihtiyacı vardır. Örneğin, bir anlık görüntüyü, önemli bir veri yükünün önüne veya yeni betikler, işlemden önce bir geri yükleme noktasını etkinleştirmek için veri ambarına dağıtımından önce yapılır. 

SQL veri ambarı artık [New-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint) cmdlet 'i aracılığıyla [Kullanıcı tanımlı geri yükleme noktalarını](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) desteklemektedir.

```powershell
New-AzSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Sütun düzeyinde güvenlik
Veri ambarınızdaki veri erişimini ve güvenliğini yönetmek, müşterileriniz ve iş Ortaklığınızla güven oluşturmak için kritik öneme sahiptir. SQL veri ambarı artık, veri Ambarınızı yeniden tasarlamanız gerekmeden gizli verileri görüntüleme izinlerini ayarlamanıza olanak tanıyan [sütun düzeyi güvenliği 'ni (CLS) desteklemektedir](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/) .

CLS, standart [Grant](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security) T-SQL ifadesini kullanarak, kullanıcının yürütme bağlamına veya grup üyeliklerine göre tablo sütunlarına erişimi denetlemenize olanak sağlar. Erişim kısıtlama mantığı, başka bir uygulamadaki verilerden uzakta olmak yerine veritabanı katmanının kendisinde bulunur ve genel güvenlik uygulamasını basitleştirir.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="object_schema_name"></a>OBJECT_SCHEMA_NAME
[OBJECT_SCHEMA_NAME ()](https://docs.microsoft.com/sql/t-sql/functions/object-schema-name-transact-sql) işlevi, şema kapsamlı nesneler için veritabanı şeması adını döndürür. Bu işlev, nesne şeması doğrulaması yapılırken ETL araçlarında yaygın hale geldi. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Örnek sonuçlar**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systime_zone_info-view"></a>Sys. time_zone_info görünümü desteği
[Sys. time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) görünümü, Azure SQL veri ambarı 'nda desteklenen Saat dilimleriyle ilgili bilgileri döndürür.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Örnek sonuçlar**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdm_pdw_exec_requests-behavior-change"></a>Otomatik Istatistikler işlemleri sys. DM _pdw_exec_requests (davranış değişikliği) içinde görünür

[Otomatik oluşturma istatistiklerinin](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistic)kullanıma sunulmasıyla birlikte, Azure SQL veri ambarı sorgu yürütmeyi iyileştirmek için istatistikler oluşturacaktır. Haziran 2018 sürümü, her bir [Istatistik oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql) işlemi yürütüldüğünde [sys. DM _pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) görünümüne bir kayıt eklenerek istatistiklerin otomatik olarak ne zaman oluşturulduğunu izleme özelliği ekler.

```sql
SELECT
    [start_time]
    , [end_time]
    , [command]
FROM
    [sys].[dm_pdw_exec_requests]
WHERE
    [command] LIKE 'CREATE STATISTICS _WA_Sys%';
```
**Örnek sonuçlar**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilgi sahibi olduğunuza göre, hızlı [bir şekılde SQL veri ambarı oluşturmayı][create a SQL Data Warehouse]öğrenin. Azure’da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
