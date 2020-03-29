---
title: Geçici Tablolarla Başlarken
description: Azure SQL Veritabanı'nda Geçici Tablolar'ı kullanmaya nasıl başlayacaksınız öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
ms.date: 06/26/2019
ms.openlocfilehash: 98fd2658f3fbcb0e7e29114d29f8dc6ed39eedf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820717"
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Azure SQL Veritabanında Geçici Tablolarla Başlarken

Zamansal Tablolar, Azure SQL Veritabanı'nın özel kodlamaya gerek kalmadan verilerinizdeki değişikliklerin tüm geçmişini izlemenize ve çözümlemenize olanak tanıyan yeni bir programlanabilirlik özelliğidir. Zamansal Tablolar, depolanan gerçeklerin yalnızca belirli bir süre içinde geçerli olarak yorumlanabilmesi için verileri zaman bağlamıyla yakından ilişkili tutar. Zamansal Tablolar'ın bu özelliği, verimli zaman tabanlı analizve veri evriminden öngörüler elde etmenizi sağlar.

## <a name="temporal-scenario"></a>Zamansal Senaryo

Bu makalede, bir uygulama senaryosunda Zamansal Tablolar kullanmak için adımları göstermektedir. Sıfırdan geliştirilen yeni bir web sitesindeki veya kullanıcı etkinliği analitiğiyle genişletmek istediğiniz mevcut bir web sitesindeki kullanıcı etkinliğini izlemek istediğinizi varsayalım. Bu basitleştirilmiş örnekte, belirli bir süre içinde ziyaret edilen web sayfası sayısının Azure SQL Veritabanı'nda barındırılan web sitesi veritabanında yakalanması ve izlenmesi gereken bir gösterge olduğunu varsayıyoruz. Kullanıcı etkinliğinin tarihsel analizinin amacı, web sitesini yeniden tasarlamak ve ziyaretçiler için daha iyi bir deneyim sağlamak için girdiler elde etmektir.

Bu senaryo için veritabanı modeli çok basittir - kullanıcı etkinlik ölçümü tek bir tamsayı alanı yla temsil edilir, **PageVisited**, ve kullanıcı profilindeki temel bilgilerle birlikte yakalanır. Ayrıca, zaman tabanlı çözümleme için, her satırın belirli bir kullanıcının belirli bir süre içinde ziyaret ettiği sayfa sayısını temsil ettiği her kullanıcı için bir dizi satır tutarsınız.

![Şema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Neyse ki, bu etkinlik bilgilerini korumak için uygulamanızda herhangi bir çaba göstermeniz gerekmez. Zamansal Tablolar ile bu süreç otomatiktir - web sitesi tasarımı sırasında tam esneklik ve veri analizinin kendisine odaklanmanız için daha fazla zaman sağlar. Yapmanız gereken tek **şey, WebSiteInfo** tablosunun [zamansal sistem sürümü](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0)olarak yapılandırıldığından emin olmaktır. Bu senaryoda Zamansal Tabloları kullanmak için tam adımlar aşağıda açıklanmıştır.

## <a name="step-1-configure-tables-as-temporal"></a>Adım 1: Tabloları zamansal olarak yapılandırın
Yeni geliştirme başlatma veya varolan uygulamayı yükseltme nize bağlı olarak, zamansal tablolar oluşturur veya zamansal öznitelikleri ekleyerek varolanları değiştirirsiniz. Genel olarak, senaryonuz bu iki seçeneğin bir karışımı olabilir. Bu işlemleri [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) veya diğer Transact-SQL geliştirme araçlarını kullanarak gerçekleştirin.

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı sürümde olmak için her zaman en güncel Management Studio sürümünü kullanmanız önerilir. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Yeni tablo oluşturma
Sorgu düzenleyicisini geçici bir tablo şablon komut dosyasıyla açmak için SSMS Object Explorer'da bağlam menü öğesi "Yeni Sistem Sürümlü Tablo"yu kullanın ve ardından şablonu doldurmak için "Şablon Parametreleri için Değerleri Belirt" (Ctrl+Shift+M) kullanın:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

SSDT'de, veritabanı projesine yeni öğeler eklerken "Geçici Tablo (Sistem Sürümünde)" şablonu'nu seçin. Bu, tablo tasarımcısını açar ve tablo düzenini kolayca belirtmenizi sağlar:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Aşağıdaki örnekte gösterildiği gibi, doğrudan Transact-SQL deyimlerini belirterek zamansal tablo da oluşturabilirsiniz. Her zamansal tablonun zorunlu öğelerinin DÖNEM tanımı ve SYSTEM_VERSIONING yan tümcesi ve geçmiş satır sürümlerini depolayacak başka bir kullanıcı tablosuna atıfta bulunulan yan tümcesi olduğunu unutmayın:

```
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

Sistem sürümüyle sürülen zamançizelgesi tablosu oluşturduğunuzda, varsayılan yapılandırmaiçeren eşlik eden geçmiş tablosu otomatik olarak oluşturulur. Varsayılan geçmiş tablosu, sayfa sıkıştırma etkin olan dönem sütunlarında (bitiş, başlangıç) kümelenmiş bir B-ağacı dizini içerir. Bu yapılandırma, özellikle [veri denetimi](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0)için zamansal tabloların kullanıldığı senaryoların çoğu için en uygun uyguluyor. 

Bu özel durumda, daha uzun bir veri geçmişi üzerinde ve daha büyük veri kümeleri ile zaman tabanlı eğilim çözümlemesi gerçekleştirmeyi hedefliyoruz, bu nedenle geçmiş tablosunun depolama seçeneği kümelenmiş bir sütun deposu dizinidir. Kümelenmiş bir sütun mağazası, analitik sorgular için çok iyi sıkıştırma ve performans sağlar. Zamansal Tablolar, geçerli ve zamansal tablolardaki dizinleri tamamen bağımsız olarak yapılandırma esnekliği sağlar. 

> [!NOTE]
> Sütun mağazası dizinleri Premium katmanda ve Standart katman, S3 ve üzeri yerlerde kullanılabilir.
>

Aşağıdaki komut dosyası, geçmiş tablosundaki varsayılan dizinkümelenmiş sütun deposuolarak nasıl değiştirilebileceğini gösterir:

```
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
```

Zamansal Tablolar Nesne Gezgini'nde daha kolay tanımlama için belirli bir simgeyle gösterilirken, geçmiş tablosu alt düğüm olarak görüntülenir.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-to-temporal"></a>Varolan tabloyu zamansal olarak değiştirin
WebsiteUserInfo tablosunun zaten var olduğu, ancak değişikliklerin geçmişini korumak için tasarlanmadığı alternatif senaryoyu ele alalım. Bu durumda, aşağıdaki örnekte gösterildiği gibi, varolan tabloyu zamansal hale getirmek için genişletebilirsiniz:

```
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

## <a name="step-2-run-your-workload-regularly"></a>Adım 2: İş yükünüzü düzenli olarak çalıştırın
Zamansal Tablolar'ın en büyük avantajı, değişiklik izleme gerçekleştirmek için web sitenizi herhangi bir şekilde değiştirmenize veya ayarlamanız gerekmemesidir. Oluşturulduktan sonra, Zamansal Tablolar verilerinizde her değişiklik yaptığınızda önceki satır sürümlerini saydam olarak devam eder. 

Bu özel senaryo için otomatik değişiklik takibinden yararlanmak için, bir kullanıcı nın web sitesindeki oturumunu her sona erdirisinde **Ziyaret edilen** sütunu güncelleyelim:

```
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
```

Güncelleştirme sorgusunun gerçek işlemin tam olarak ne zaman gerçekleştiğini veya geçmiş verilerin gelecekteki çözümleme için nasıl korunacağını bilmesi gerekmediğini fark etmek önemlidir. Her iki yönü de Azure SQL Veritabanı tarafından otomatik olarak işlenir. Aşağıdaki diyagram, her güncelleştirmede geçmiş verilerinin nasıl oluşturulduğunu göstermektedir.

![Zamansal Mimari](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>Adım 3: Geçmiş veri analizi yapma
Zamansal sistem sürümü etkinleştirildiğinde, geçmiş veri analizi sizden yalnızca bir sorgu uzaktadır. Bu makalede, tüm ayrıntıları öğrenmek, [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) maddesi ile tanıtılan çeşitli seçenekleri keşfetmek için ortak analiz senaryolarını ele alan birkaç örnek sunacağız.

Bir saat önce ziyaret edilen web sayfası sayısına göre sipariş edilen ilk 10 kullanıcıyı görmek için şu sorguyu çalıştırın:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
```

Bu sorguyu, bir gün önce, bir ay önce veya geçmişte istediğiniz herhangi bir noktada site ziyaretlerini analiz etmek için kolayca değiştirebilirsiniz.

Önceki güne ait temel istatistiksel analizleri gerçekleştirmek için aşağıdaki örneği kullanın:

```
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
```

Belirli bir kullanıcının faaliyetlerini belirli bir süre içinde aramak için, İçERMe yan tümcesini kullanın:

```
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
```

Eğilimleri ve kullanım modellerini sezgisel bir şekilde kolayca gösterebildiğiniz için grafik görselleştirme özellikle zamansal sorgular için uygundur:

![Zamansal Grafi](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Gelişen tablo şeması
Genellikle, uygulama geliştirme yaparken zamansal tablo şemasını değiştirmeniz gerekir. Bunun için normal ALTER TABLE deyimlerini çalıştırmanız yeterlidir ve Azure SQL Veritabanı geçmiş tablosundaki değişiklikleri uygun şekilde yayacaktır. Aşağıdaki komut dosyası, izleme için nasıl ek öznitelik ekleyebileceğinizi gösterir:

```
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
```

Benzer şekilde, iş yükünüz etkinken sütun tanımını değiştirebilirsiniz:

```
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
```

Son olarak, artık gerekmeyen bir sütunu kaldırabilirsiniz.

```
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
```

Alternatif olarak, veritabanına (çevrimiçi mod) bağlıyken veya veritabanı projesinin (çevrimdışı mod) bir parçası yken zamansal tablo şemasını değiştirmek için en son [SSDT'yi](https://msdn.microsoft.com/library/mt204009.aspx) kullanın.

## <a name="controlling-retention-of-historical-data"></a>Geçmiş verilerin tutulmasını denetleme
Sistem sürümlü zamansal tablolarda, geçmiş tablosu veritabanı boyutunu normal tablolardan daha fazla artırabilir. Büyük ve sürekli büyüyen geçmiş tablosu, hem saf depolama maliyetleri hem de zamansal sorgulama ya da performans vergisi uygulama nedeniyle bir sorun haline gelebilir. Bu nedenle, geçmiş tablosundaki verileri yönetmek için bir veri saklama ilkesi geliştirmek, her zamansal tablonun yaşam döngüsünü planlamanın ve yönetmenin önemli bir yönüdür. Azure SQL Veritabanı ile, zamansal tabloda geçmiş verileri yönetmek için aşağıdaki yaklaşımlara sahipsiniz:

* [Tablo Bölümleme](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Özel Temizleme Komut Dosyası](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Sonraki adımlar

- Geçici Tablolar hakkında daha fazla bilgi için, [Bkz. Geçici Tablolar.](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables)
- Gerçek bir müşteri [geçici uygulama başarı öyküsü](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) duymak ve canlı bir [zamansal gösteri](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)izlemek için Kanal 9 ziyaret edin.

