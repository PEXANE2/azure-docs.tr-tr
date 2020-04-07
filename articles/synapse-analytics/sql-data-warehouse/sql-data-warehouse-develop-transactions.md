---
title: Synapse SQL havuzunda hareketleri kullanma
description: Bu makalede, Synapse SQL havuzunda hareketleri uygulama ve çözüm geliştirme ipuçları içerir.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/22/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d9578653ff8074fee8336df447caf119f79febe0
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745258"
---
# <a name="use-transactions-in-synapse-sql-pool"></a>Synapse SQL havuzunda hareketleri kullanma

Bu makalede, sql havuzunda hareketleri uygulama ve çözüm geliştirme ipuçları içerir.

## <a name="what-to-expect"></a>Ne beklenebilir

Beklediğiniz gibi, SQL havuzu veri ambarı iş yükünün bir parçası olarak hareketleri destekler. Ancak, SQL havuzunun ölçekte tutulmasını sağlamak için, bazı özellikler SQL Server ile karşılaştırıldığında sınırlıdır. Bu makalede, farklar vurgulamaktadır.

## <a name="transaction-isolation-levels"></a>İşlem yalıtım düzeyleri

SQL havuzu ACID hareketlerini uygular. İşlemsel desteğin yalıtım düzeyi, TAAHHÜTEDİlMEYEN READ için varsayılandır.  Ana veritabanına bağlandığında kullanıcı veritabanı için READ_COMMITTED_SNAPSHOT veritabanı seçeneğini açarak COMMITTED SNAPSHOT ISOLATION'ı OKUMA olarak değiştirebilirsiniz.  

Etkinleştirildiğinde, bu veritabanındaki tüm işlemler READ COMMITTED SNAPSHOT Isolation altında yürütülür ve READ UNCOMMITTED'un oturum düzeyinde ayarlanması onurlandırılmaz. Ayrıntılar için [ALTER DATABASE SET seçeneklerini (Transact-SQL)](/sql/t-sql/statements/alter-database-transact-sql-set-options?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kontrol edin.

## <a name="transaction-size"></a>İşlem boyutu

Tek bir veri modifikasyon hareketinin boyutu sınırlıdır. Sınır dağıtım başına uygulanır. Bu nedenle, toplam ayırma, sınır dağılımı sayısı ile çarpılarak hesaplanabilir.

İşlemdeki en fazla satır sayısını yaklaşık olarak tahmin etmek için dağıtım kapağını her satırın toplam boyutuna bölün. Değişken uzunluktaki sütunlar için, maksimum boyutu kullanmak yerine ortalama bir sütun uzunluğu almayı düşünün.

Aşağıdaki tabloda, iki varsayım yapılmıştır:

* Verilerin eşit dağılımı oluştu
* Ortalama satır uzunluğu 250 bayt

## <a name="gen2"></a>Gen2

| [DWU](sql-data-warehouse-overview-what-is.md) | Dağıtım başına kap (GB) | Dağılım Sayısı | MAX işlem boyutu (GB) | # Dağıtım başına satırlar | İşlem başına Maksimum Satırlar |
| --- | --- | --- | --- | --- | --- |
| DW100c |1 |60 |60 |4,000,000 |240,000,000 |
| DW200c |1,5 |60 |90 |6,000,000 |360,000,000 |
| DW300c |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400c |3 |60 |180 |12,000,000 |720,000,000 |
| DW500c'yi seçin |3,75 |60 |225 |15,000,000 |900,000,000 |
| DW1000c |7,5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1500c |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000c |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW2500c |18.75 |60 |1125 |75,000,000 |4,500,000,000 |
| DW3000c |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW5000c |37.5 |60 |2,250 |150,000,000 |9,000,000,000 |
| DW6000c |45 |60 |2,700 |180,000,000 |10,800,000,000 |
| DW7500c |56.25 |60 |3,375 |225,000,000 |13,500,000,000 |
| DW10000c |75 |60 |4.500 |300,000,000 |18,000,000,000 |
| DW15000c |112.5 |60 |6,750 |450,000,000 |27,000,000,000 |
| DW30000c |225 |60 |13,500 |900,000,000 |54,000,000,000 |

## <a name="gen1"></a>Gen1

| [DWU](sql-data-warehouse-overview-what-is.md) | Dağıtım başına kap (GB) | Dağılım Sayısı | MAX işlem boyutu (GB) | # Dağıtım başına satırlar | İşlem başına Maksimum Satırlar |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1,5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3,75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4,5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7,5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

Hareket boyutu sınırı hareket veya işlem başına uygulanır. Tüm eşzamanlı hareketlerde uygulanmaz. Bu nedenle, her işlemin bu miktarda veriyi günlüğe yazmasına izin verilir.

Günlüğe yazılan veri miktarını en iyi duruma getirmek ve en aza indirmek için lütfen [En İyi İşlemler](sql-data-warehouse-develop-best-practices-transactions.md) makalesine bakın.

> [!WARNING]
> Maksimum işlem boyutu yalnızca verilerin yayılmasının eşit olduğu HASH veya ROUND_ROBIN dağıtılmış tablolar için elde edilebilir. Hareket, verileri dağıtımlara çarpık bir şekilde yazıyorsa, sınıra maksimum işlem boyutundan önce ulaşılması olasıdır.
> <!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>İşlem durumu

SQL havuzu -2 değerini kullanarak başarısız bir hareketi bildirmek için XACT_STATE() işlevini kullanır. Bu değer, hareketin başarısız olduğu ve yalnızca geri alma için işaretlendiğini gösterir.

> [!NOTE]
> -2'nin XACT_STATE işlevi tarafından başarısız bir işlemi belirtmek için kullanılması SQL Server'a farklı bir davranışı temsil eder. SQL Server, -1 değerini işitilemez bir işlemi temsil etmek için kullanır. SQL Server, bir işlem içinde bazı hataları, taahhüt edilemez olarak işaretlemek zorunda kalmadan tolere edebilir. Örneğin, `SELECT 1/0` bir hataya neden olur, ancak bir hareketi işlenmemiş bir duruma zorlamaz.

SQL Server ayrıca taahhüt edilemeyen işlemde okumalara da izin verir. Ancak, SQL havuzu bunu yapmanıza izin vermez. Bir SQL havuzu hareketinin içinde bir hata oluşursa, otomatik olarak -2 durumuna girer ve ekstre geri alınana kadar başka seçim ekstreleri yapamazsınız.

Bu nedenle, kod değişiklikleri yapmak için gerekebileceği gibi XACT_STATE() kullanıp kullanmadığını görmek için uygulama kodunuzu kontrol etmek önemlidir.

Örneğin, SQL Server'da aşağıdaki gibi görünen bir işlem görebilirsiniz:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            ROLLBACK TRAN;
            PRINT 'ROLLBACK';
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Önceki kod aşağıdaki hata iletisini verir:

Msg 111233, Düzey 16, Devlet 1, Hat 1 111233; Geçerli hareket iptal edildi ve bekleyen değişiklikler geri alındı. Bu sorunun nedeni, yalnızca geri alma durumundaki bir işlemin DDL, DML veya SELECT deyiminden önce açıkça geri alınmamasıdır.

ERROR_* işlevlerinin çıktısını alamazsınız.

SQL havuzunda kodun biraz değiştirilmesi gerekir:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            ROLLBACK TRAN;
            PRINT 'ROLLBACK';
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Beklenen davranış şimdi gözlenir. Hareketteki hata yönetilir ve ERROR_* işlevleri beklendiği gibi değerler sağlar.

Değişen tek şey, işlemin ROLLBACK'inin CATCH bloğundaki hata bilgilerinin okunmasından önce gerçekleşmesi gerektiğidir.

## <a name="error_line-function"></a>Error_Line() fonksiyonu

Ayrıca, SQL havuzunun ERROR_LINE() işlevini uygulamadığını veya desteklemediğini de belirtmekte yarar vardır. Kodunuzda bu varsa, SQL havuzuyla uyumlu olması için kodu kaldırmanız gerekir.

Eşdeğer işlevsellik uygulamak için kodunuzdaki sorgu etiketlerini kullanın. Daha fazla bilgi için [LABEL](sql-data-warehouse-develop-label.md) makalesine bakın.

## <a name="using-throw-and-raiserror"></a>THROW ve RAISERROR kullanma

THROW, SQL havuzunda özel durumları yükseltmek için daha modern bir uygulamadır ancak RAISERROR da desteklenir. Ancak dikkat değer birkaç farklılıklar vardır.

* Kullanıcı tanımlı hata iletileri numaraları THROW için 100.000 - 150.000 aralığında olamaz
* RAISERROR hata iletileri 50.000 sabit
* sys.messages kullanımı desteklenmiyor

## <a name="limitations"></a>Sınırlamalar

SQL havuzunun hareketle ilgili birkaç başka kısıtlaması vardır.

Bunlar şu şekildedir:

* Dağıtılmış hareket yok
* İç içe geçmeişlemine izin verilmiyor
* Kaydet puanına izin verilmiyor
* Adlandırılmış hareket yok
* İşaretli hareket yok
* Kullanıcı tanımlı bir işlemde CREATE TABLE gibi DDL desteği yok

## <a name="next-steps"></a>Sonraki adımlar

İşlemleri en iyi duruma getirmek hakkında daha fazla bilgi edinmek [için, Hareketler'in en iyi uygulamaları'na](sql-data-warehouse-develop-best-practices-transactions.md)bakın. Diğer SQL havuzu en iyi uygulamaları hakkında bilgi edinmek için, [SQL havuzu en iyi uygulamaları](sql-data-warehouse-best-practices.md)bakın.
