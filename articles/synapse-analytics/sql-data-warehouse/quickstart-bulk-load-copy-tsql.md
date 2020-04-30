---
title: 'Hızlı başlangıç: tek bir T-SQL ifadesini kullanarak verileri toplu yükleme'
description: COPY ifadesini kullanarak verileri toplu yükleme
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d39b3085a802ca0ff745ab1f63f4a8fba966ea48
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81115012"
---
# <a name="quickstart-bulk-load-data-using-the-copy-statement"></a>Hızlı başlangıç: COPY ifadesini kullanarak verileri toplu yükleme

Bu hızlı başlangıçta, yüksek aktarım hızı veri alımı için basit ve esnek [kopyalama ifadesini](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) kullanarak SQL havuzunuza verileri toplu olarak yükleyebilirsiniz. KOPYALAMA deyimleri, aşağıdakileri yapmak için işlevler sağlayarak, verileri sorunsuzca ve esnek bir şekilde yüklemeyi sağlayan önerilen yükleme yardımcı programıdır:

- Veri ambarında kesin DENETIM izinlerine gerek duymadan daha düşük ayrıcalıklı kullanıcıların yüklenmesine izin ver
- Ek veritabanı nesneleri oluşturmak zorunda kalmadan yalnızca tek bir T-SQL ifadesiyle yararlanın
- Erişim Imzalarını paylaşma (SAS) kullanarak depolama hesabı anahtarlarını kullanıma açmadan daha hassas bir izin modelinden yararlanın
- ERRORFILE konumu (REJECTED_ROW_LOCATION) için farklı bir depolama hesabı belirtin
- Her bir hedef sütun için varsayılan değerleri özelleştirin ve belirli hedef sütunlara yüklenecek kaynak veri alanlarını belirtin
- CSV dosyaları için özel bir satır Sonlandırıcı belirtin
- CSV dosyaları için kaçış dizesi, alan ve satır sınırlayıcıları
- CSV dosyaları için SQL Server Tarih biçimlerinden yararlanın
- Depolama konumu yolunda joker karakter ve birden çok dosya belirtin

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, zaten bir SQL havuzunuz olduğunu varsayar. Bir SQL havuzu oluşturulmadıysa, [oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) hızlı başlangıcı ' nı kullanın.

## <a name="create-the-target-table"></a>Hedef tabloyu oluşturma

Bu örnekte, New York TAXI veri kümesinden veri yükleyeceğiz. Tek bir yıl içinde alınan TAXI 'yi temsil eden seyahat adlı bir tablo yükleyeceğiz. Tabloyu oluşturmak için aşağıdakileri çalıştırın:

```sql
CREATE TABLE [dbo].[Trip]
(
    [DateID] int NOT NULL,
    [MedallionID] int NOT NULL,
    [HackneyLicenseID] int NOT NULL,
    [PickupTimeID] int NOT NULL,
    [DropoffTimeID] int NOT NULL,
    [PickupGeographyID] int NULL,
    [DropoffGeographyID] int NULL,
    [PickupLatitude] float NULL,
    [PickupLongitude] float NULL,
    [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [DropoffLatitude] float NULL,
    [DropoffLongitude] float NULL,
    [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [PassengerCount] int NULL,
    [TripDurationSeconds] int NULL,
    [TripDistanceMiles] float NULL,
    [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
    [FareAmount] money NULL,
    [SurchargeAmount] money NULL,
    [TaxAmount] money NULL,
    [TipAmount] money NULL,
    [TollsAmount] money NULL,
    [TotalAmount] money NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    CLUSTERED COLUMNSTORE INDEX
);
```

## <a name="run-the-copy-statement"></a>COPY ifadesini Çalıştır

Azure Blob depolama hesabından seyahat tablosuna veri yükleyecek aşağıdaki kopyalama ifadesini çalıştırın.

```sql
COPY INTO [dbo].[Trip] FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/'
WITH (
   FIELDTERMINATOR='|',
   ROWTERMINATOR='0x0A'
) OPTION (LABEL = 'COPY: dbo.trip');
```

## <a name="monitor-the-load"></a>Yükü izleme

Yükün düzenli olarak aşağıdaki sorguyu çalıştırarak ilerleme yapıp yapmadığını denetleyin:

```sql
SELECT  r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command
,       sum(bytes_processed) AS bytes_processed
,       sum(rows_processed) AS rows_processed
FROM    sys.dm_pdw_exec_requests r
              JOIN sys.dm_pdw_dms_workers w
                     ON r.[request_id] = w.request_id
WHERE [label] = 'COPY: dbo.trip' and session_id <> session_id() and type = 'WRITER'
GROUP BY r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command;

```

## <a name="next-steps"></a>Sonraki adımlar

- Veri yükleme konusunda en iyi uygulamalar için bkz. [veri yükleme Için En Iyi uygulamalar](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/guidance-for-loading-data).
- Veri yüklerinizin kaynaklarını yönetme hakkında daha fazla bilgi için bkz. [Iş yükü yalıtımı](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-configure-workload-isolation-tsql). 
