---
title: 'Quickstart: Tek bir T-SQL deyimi kullanarak toplu yük verileri'
description: COPY deyimini kullanarak toplu yük verileri
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
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115012"
---
# <a name="quickstart-bulk-load-data-using-the-copy-statement"></a>Quickstart: COPY deyimini kullanarak toplu yük verileri

Bu hızlı başlatmada, yüksek işlem li veri alımı için basit ve esnek [COPY deyimini](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) kullanarak verileri SQL havuzunuza toplu yüklersiniz. COPY deyimi, aşağıdakilere işlevsellik sağlayarak verileri sorunsuz ve esnek bir şekilde yüklemenize olanak sağladığından önerilen yükleme yardımcı programıdır:

- Veri ambarında sıkı KONTROL izinlerine gerek kalmadan daha düşük ayrıcalıklı kullanıcıların yüklemesine izin ver
- Ek veritabanı nesneleri oluşturmak zorunda kalmadan yalnızca tek bir T-SQL deyiminden yararlanın
- Share Access İmzalarını (SAS) kullanarak depolama hesabı anahtarlarını ortaya çıkarmadan daha hassas bir izin modeline sahip olun
- ERRORFILE konumu için farklı bir depolama hesabı belirtin (REJECTED_ROW_LOCATION)
- Her hedef sütun için varsayılan değerleri özelleştirme ve belirli hedef sütunlara yüklenmesi için kaynak veri alanlarını belirtin
- CSV dosyaları için özel bir satır sonlandırıcı belirtin
- CSV dosyaları için kaçış dizesi, alan ve satır sınırlayıcıları
- CSV dosyaları için SQL Server Date biçimleriden yararlanın
- Depolama konumu yolunda joker karakterler ve birden çok dosya belirtin

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatma zaten bir SQL havuzu var varsayar. Bir SQL havuzu oluşturulmamışsa, [Oluştur ve Bağla - portal](create-data-warehouse-portal.md) quickstart'ını kullanın.

## <a name="create-the-target-table"></a>Hedef tabloyu oluşturma

Bu örnekte, New York taksi veri setinden veri yükleniyor olacağız. Trip adında bir masa yükleyeceğiz. Tablooluşturmak için aşağıdakileri çalıştırın:

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

## <a name="run-the-copy-statement"></a>COPY deyimini çalıştırma

Azure blob depolama hesabından gelen verileri Trip tablosuna yükleyecek aşağıdaki COPY deyimini çalıştırın.

```sql
COPY INTO [dbo].[Trip] FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/'
WITH (
   FIELDTERMINATOR='|',
   ROWTERMINATOR='0x0A'
) OPTION (LABEL = 'COPY: dbo.trip');
```

## <a name="monitor-the-load"></a>Yükü izleme

Aşağıdaki sorguyu düzenli aralıklarla çalıştırarak yükünüzün ilerleme kaydedip oluşturmadığını kontrol edin:

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

- Veri yükleme yle ilgili en iyi uygulamalar [için, Veri Yükleme için En İyi Uygulamalar'a](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/guidance-for-loading-data)bakın.
- Veri yükleriniz için kaynakları nasıl yöneteceksiniz hakkında bilgi için İş [yükü yalıtımı'na](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)bakın. 
