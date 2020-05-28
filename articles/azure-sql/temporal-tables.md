---
title: Zamana bağlı tabloları kullanmaya başlama
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde zamana bağlı tabloları kullanmaya başlama hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
ms.date: 06/26/2019
ms.openlocfilehash: e172c251f629dc53bd8f4479d63fe743bbe42095
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84046750"
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde zamana bağlı tabloları kullanmaya başlama
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

Zamana bağlı tablolar, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nin, özel kodlamaya gerek duymadan verilerinize ilişkin tüm değişiklikleri izlemenize ve analiz etmenize olanak tanıyan bir programlama özelliğidir. Zamana bağlı tablolar, depolanan olguların yalnızca belirli bir dönemde geçerli olarak yorumlanabilmesi için verileri zaman içeriğiyle yakından ilgili tutar. Zamana bağlı tabloların bu özelliği, verimli zaman tabanlı analizler ve veri gelişinden Öngörüler elde etmenizi sağlar.

## <a name="temporal-scenario"></a>Zamana bağlı senaryo

Bu makalede, bir uygulama senaryosunda zamana bağlı tabloları kullanma adımları gösterilmektedir. Kullanıcı etkinliğini, sıfırdan veya Kullanıcı etkinliği analiziyle genişletmek istediğiniz var olan bir Web sitesinde geliştirmekte olan yeni bir Web sitesinde izlemek istediğinizi varsayalım. Bu basitleştirilmiş örnekte, bir süre boyunca ziyaret edilen Web sayfaları sayısının Azure SQL veritabanı veya Azure SQL yönetilen örneği üzerinde barındırılan Web sitesi veritabanında yakalanmasının ve izlenmesi gereken bir gösterge olduğunu varsaytık. Kullanıcı etkinliğinin geçmiş analizinin amacı, Web sitesini yeniden tasarlayacağınız ve ziyaretçiler için daha iyi deneyim sağlayacak girişleri sağlamaktır.

Bu senaryonun veritabanı modeli çok basit-kullanıcı etkinliği ölçüsünün tek bir tamsayı alanı ile temsil edildiği, **Pageziyaret**edildiği ve kullanıcı profilindeki temel bilgilerle birlikte yakalanmasıdır. Bunlara ek olarak, zaman tabanlı analiz için her bir kullanıcı için her bir satır için belirli bir süre içinde belirli bir kullanıcının ziyaret ettiği sayfa sayısını temsil eden bir dizi satır tutabilirsiniz.

![Şema](./media/temporal-tables/AzureTemporal1.png)

Neyse ki, bu etkinlik bilgilerini korumak için uygulamanıza herhangi bir çaba koymanız gerekmez. Zamana bağlı tablolar sayesinde, bu süreç otomatikleştirilmiş hale, Web sitesi tasarımı sırasında ve veri analizinde odaklanmak için daha fazla zaman esnekliği sağlar. Yapmanız gereken tek şey, **Websiteınfo** tablosunun zamana bağlı [sistem sürümü](/sql/relational-databases/tables/temporal-tables#what-is-a-system-versioned-temporal-table)olarak yapılandırılmasını sağlamaktır. Bu senaryodaki zamana bağlı tabloları kullanmaya yönelik tam adımlar aşağıda açıklanmıştır.

## <a name="step-1-configure-tables-as-temporal"></a>1. Adım: tabloları zamana bağlı olarak yapılandırma

Yeni geliştirme yapıp başlatdığınıza veya var olan uygulamayı yükseltmeye bağlı olarak, zamana bağlı tablolar oluşturur veya geçici öznitelikler ekleyerek mevcut olanları değiştirirsiniz. Genel durumda, senaryonuz bu iki seçenekten oluşan bir karışımı olabilir. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS), [SQL Server veri araçları](/sql/ssdt/download-sql-server-data-tools-ssdt) (ssdt), [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio)veya başka bir Transact-SQL geliştirme aracını kullanarak bu eylemi gerçekleştirin.

> [!IMPORTANT]
> Azure SQL veritabanı ve Azure SQL yönetilen örneği güncelleştirmeleriyle eşitlenmiş olarak kalmak için Management Studio en son sürümünü her zaman kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](/sql/ssms/download-sql-server-management-studio-ssms).

### <a name="create-new-table"></a>Yeni tablo oluştur

Sorgu düzenleyicisini bir zamana bağlı tablo şablonu betiği ile açmak için SSMS Nesne Gezgini bağlam menüsü öğesini "yeni sistem sürümü tutulan tablo" ya da şablonu doldurmak için "şablon parametrelerinin değerlerini belirt" (Ctrl + Shift + M) kullanın:

![SSMSNewTable](./media/temporal-tables/AzureTemporal2.png)

SSDT 'de, veritabanı projesine yeni öğeler eklerken "zamana bağlı tablo (sistem sürümlü)" şablonunu seçin. Bu, tablo tasarımcısını açacak ve tablo yerleşimini kolayca belirtmenizi sağlayacak:

![SSDTNewTable](./media/temporal-tables/AzureTemporal3.png)

Aşağıdaki örnekte gösterildiği gibi doğrudan Transact-SQL deyimlerini belirterek de zamana bağlı tablo oluşturabilirsiniz. Her zamana bağlı tablonun zorunlu öğelerinin, geçmiş satır sürümlerini depolayacağı başka bir Kullanıcı tablosuna bir başvuruya sahip dönem tanımı ve SYSTEM_VERSIONING yan tümcesi olduğunu unutmayın:

```sql
CREATE TABLE WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
```

Sistem sürümü tutulan zamana bağlı tablo oluştururken, varsayılan yapılandırmayla birlikte bulunan geçmiş tablosu otomatik olarak oluşturulur. Varsayılan geçmiş tablosu, sayfa sıkıştırması etkinken dönem sütunlarında (bitiş, başlangıç) kümelenmiş B-ağaç dizinini içerir. Bu yapılandırma, özellikle [veri denetimi](/sql/relational-databases/tables/temporal-table-usage-scenarios#enabling-system-versioning-on-a-new-table-for-data-audit)için, zamana bağlı tabloların kullanıldığı senaryoların çoğunluğu için idealdir.

Bu durumda, daha uzun bir veri geçmişi ve daha büyük veri kümeleriyle zaman tabanlı eğilim analizi gerçekleştirmeyi hedefliyoruz. bu nedenle, geçmiş tablosu için depolama seçimi kümelenmiş bir columnstore dizinidir. Kümelenmiş bir columnstore, analitik sorgular için çok iyi sıkıştırma ve performans sağlar. Zamana bağlı tablolar, geçerli ve zamana bağlı tablolardaki dizinleri tamamen bağımsız olarak yapılandırma esnekliği sağlar.

> [!NOTE]
> Columnstore dizinleri İş Açısından Kritik, Genel Amaçlı ve Premium katmanlarda ve S3 ve üzeri Standart katmanda kullanılabilir.

Aşağıdaki betik, geçmiş tablosundaki varsayılan dizinin kümelenmiş columnstore olarak nasıl değiştirilebiliyorsa gösterir:

```sql
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

Zamana bağlı tablolar, daha kolay tanımlayıcı için Nesne Gezgini, geçmiş tablosu bir alt düğüm olarak görüntülenirken, belirli bir simgeyle temsil edilir.

![AlterTable](./media/temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>Mevcut tabloyu zamana bağlı olarak değiştir

Websiteuserınfo tablosunun zaten varolduğu, ancak değişiklik geçmişini tutmak üzere tasarlanmadığı alternatif senaryoyu ele alalım. Bu durumda, aşağıdaki örnekte gösterildiği gibi, var olan tabloyu geçici olacak şekilde genişletebilirsiniz.

```sql
ALTER TABLE WebsiteUserInfo
ADD
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

## <a name="step-2-run-your-workload-regularly"></a>2. Adım: iş yükünüzü düzenli olarak çalıştırın

Zamana bağlı tabloların başlıca avantajı, değişiklik izlemeyi gerçekleştirmek için Web sitenizi değiştirmenize veya ayarlamanıza gerek kalmaz. Oluşturulduktan sonra, veri üzerinde her değişiklik yaptığınızda zamana bağlı tablolar, önceki satır sürümlerini saydam bir şekilde kalıcı hale getirin.

Bu senaryonun otomatik değişiklik izleme özelliğinden yararlanmak için, bir Kullanıcı Web sitesinde oturumunu her sona erdirdiğinde yalnızca **pagescolumn** sütununu güncelleştirelim:

```sql
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5
WHERE [UserID] = 1;
```

Güncelleştirme sorgusunun, gerçek işlem gerçekleştiği zaman ve geçmiş verilerin gelecekteki analizler için nasıl korunduğuna ilişkin tam zamanı bildiğine dikkat etmeniz önemlidir. Her iki yön de otomatik olarak Azure SQL veritabanı ve Azure SQL yönetilen örneği tarafından işlenir. Aşağıdaki diyagramda, geçmiş verilerinin her güncelleştirmede nasıl oluşturulduğu gösterilmektedir.

![TemporalArchitecture](./media/temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>3. Adım: geçmiş veri analizini gerçekleştirme

Zamana bağlı sistem sürümü oluşturma özelliği etkin olduğunda, geçmiş veri analizi yalnızca bir sorgudur. Bu makalede, genel analiz senaryolarını ele alan birkaç örnek sağlayacağız. tüm ayrıntıları öğrenmek için [FOR SYSTEM_TIME](/sql/relational-databases/tables/temporal-tables#how-do-i-query-temporal-data) yan tümcesiyle sunulan çeşitli seçenekleri inceleyin.

Ziyaret edilen Web sayfası sayısına göre sıralanan ilk 10 kullanıcıyı bir saat önce görmek için şu sorguyu çalıştırın:

```sql
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
```

Site ziyaretlerini bir gün önce, bir ay önce veya istediğiniz herhangi bir noktada analiz etmek için bu sorguyu kolayca değiştirebilirsiniz.

Önceki güne yönelik temel istatistiksel analizler gerçekleştirmek için aşağıdaki örneği kullanın:

```sql
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
```

Belirli bir kullanıcının etkinliklerini bir süre içinde aramak için, kapsanan ın yan tümcesini kullanın:

```sql
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
```

Eğilimleri ve kullanım düzenlerini çok kolay bir şekilde gösterebilmeniz için grafik görselleştirmesi özellikle zamana bağlı sorgularda yararlıdır:

![TemporalGraph](./media/temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Gelişen tablo şeması

Genellikle, uygulama geliştirme yaparken zamana bağlı tablo şemasını değiştirmeniz gerekir. Bunun için, normal ALTER TABLE deyimlerini ve Azure SQL veritabanı 'nı ya da Azure SQL yönetilen örneğini, değişiklikleri geçmiş tablosuna uygun şekilde yaymış olmanız yeterlidir. Aşağıdaki betik, izleme için nasıl ek öznitelik ekleyebileceğiniz gösterilmektedir:

```sql
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
```

Benzer şekilde, iş yükünüz etkin durumdayken sütun tanımını da değiştirebilirsiniz:

```sql
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
```

Son olarak, artık gerekli olmayan bir sütunu kaldırabilirsiniz.

```sql
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo
    DROP COLUMN TemporaryColumn;
```

Alternatif olarak, veritabanına bağlıyken (çevrimiçi mod) veya veritabanı projesinin bir parçası olarak (çevrimdışı mod), zamana bağlı tablo şemasını değiştirmek için en son [SSDT](/sql/ssdt/download-sql-server-data-tools-ssdt) 'yi kullanın.

## <a name="controlling-retention-of-historical-data"></a>Geçmiş verilerin bekletilmesini denetleme

Sistem sürümü tutulan zamana bağlı tablolarla, geçmiş tablosu normal tablolardan daha fazla veritabanı boyutunu artırabilir. Büyük ve sürekli büyüyen bir geçmiş tablosu, hem saf depolama maliyetlerinden hem de zamana bağlı sorgulama üzerinde bir performans vergisini daha iyi hale gelmesine neden olabilir. Bu nedenle, geçmiş tablosundaki verileri yönetmek için veri saklama ilkesi geliştirmek, her zamana bağlı tablonun yaşam döngüsünün planlanmasına ve yönetilmesine ilişkin önemli bir yönüdür. Azure SQL veritabanı ve Azure SQL yönetilen örneği ile, zamana bağlı tabloda geçmiş verileri yönetmek için aşağıdaki yaklaşımlara sahipsiniz:

- [Tablo bölümleme](/sql/relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables#using-table-partitioning-approach)
- [Özel temizleme betiği](/sql/relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables#using-custom-cleanup-script-approach)

## <a name="next-steps"></a>Sonraki adımlar

- Zamana bağlı tablolar hakkında daha fazla bilgi için bkz. zamana bağlı [tabloları](/sql/relational-databases/tables/temporal-tables)kullanıma alma.
- Müşteri tarafından zamana bağlı bir [uygulama başarısını](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) dinlemek ve canlı bir zamana bağlı [tanıtım](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)izlemek için Channel 9 ' a gidin.
