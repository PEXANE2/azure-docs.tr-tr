---
title: Azure SQL Edge 'de T-SQL akış işi oluşturma (Önizleme)
description: Azure SQL Edge 'de Stream Analytics işleri oluşturma hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 4b09df3110907d58badda2c389b9ee39a9b02532
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636198"
---
# <a name="create-stream-analytics-job-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de Stream Analytics işi oluşturma (Önizleme) 

Bu makalede, Azure SQL Edge 'de (Önizleme) bir T-SQL akış işi oluşturma işlemi açıklanmaktadır. SQL Edge 'de bir akış işi oluşturmak için aşağıdaki adımlar gereklidir

1. Dış akış girişi ve çıkış nesneleri oluşturma
2. Akış işi oluşturma işleminin parçası olarak akış işi sorgusunu tanımlayın.

> [!NOTE]
> Azure SQL Edge 'de T-SQL akış özelliğini etkinleştirmek için, TF 11515 ' i başlangıç seçeneği olarak etkinleştirin veya [DBCC TRACEON]( https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) komutunu kullanın. MSSQL. conf dosyasını kullanarak izleme bayraklarını etkinleştirme hakkında daha fazla bilgi için bkz. [MSSQL. conf dosyasını kullanarak yapılandırma](configure.md#configure-by-using-an-mssqlconf-file). Bu gereksinim, gelecekteki Azure SQL Edge güncelleştirmelerinde (Önizleme) kaldırılacaktır.

## <a name="configure-an-external-stream-input-and-output-object"></a>Dış akış girişi ve çıkış nesnesi yapılandırma

T-SQL akışı, akış işinin dış akış girişleri ve çıkışları ile ilişkili veri kaynaklarını tanımlamak için SQL Server dış veri kaynağı işlevselliğini kullanır. Aşağıdaki T-SQL komutları bir dış akış girişi veya çıkış nesnesi oluşturmak için gereklidir.

[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql)

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql)

[Dış AKıŞ oluşturma (Transact-SQL)](#example-create-an-external-stream-object-sql-database)

Ayrıca, SQL Edge (veya SQL Server, Azure SQL) bir çıkış akışı olarak kullanılıyorsa, SQL veritabanına erişmek için kimlik bilgilerini tanımlamak üzere [VERITABANı KAPSAMLı KIMLIK bilgileri (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) T-SQL komutu gerekir.

### <a name="supported-input-and-output-stream-data-sources"></a>Desteklenen giriş ve çıkış akışı veri kaynakları

Azure SQL Edge Şu anda yalnızca akış girişleri ve çıkışları olarak aşağıdaki veri kaynaklarını destekler.

| Veri Kaynağı Türü | Girdi | Çıktı | Açıklama |
|------------------|-------|--------|------------------|
| Azure IoT Edge hub 'ı | E | E | Azure IoT Edge hub 'ına akış verilerini okumak/yazmak için veri kaynağı. Azure IoT Edge hub hakkında daha fazla bilgi için [IoT Edge hub 'ına](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub) başvurun|
| SQL Veritabanı | N | E | SQL veritabanına akış verileri yazmak için veri kaynağı bağlantısı. SQL veritabanı, yerel bir SQL Edge veritabanı veya uzak SQL Server ya da Azure SQL veritabanı olabilir|
| Azure Blob Depolama | N | E | Azure depolama hesabındaki bir bloba veri yazmak için veri kaynağı. |
| Kafka | E | N | Bir Kafka konusunun akış verilerini okumak için veri kaynağı. Bu bağdaştırıcı Şu anda yalnızca Azure SQL Edge 'in Intel/AMD sürümünde kullanılabilir ve SQL Edge 'in ARM64 sürümünde kullanılamaz.|

### <a name="example-create-an-external-stream-inputoutput-object-for-azure-iot-edge-hub"></a>Örnek: Azure IoT Edge hub 'ı için dış akış giriş/çıkış nesnesi oluşturma

Aşağıdaki örnek, Edge hub 'ı için bir dış akış nesnesi oluşturur. Azure IoT Edge Hub için bir dış akış giriş/çıkış veri kaynağı oluşturmak için, ilk olarak, okunan/yazılan verilerin yerleşimini anlamak üzere SQL için bir dış dosya biçimi oluşturmanız gerekir.

1. JSON biçim türü ile harici bir dosya biçimi oluşturun.

    ```sql
    Create External file format InputFileFormat
    WITH (  
       format_type = JSON,
    )
    go
    ```

2. IoT Edge hub 'ı için bir dış veri kaynağı oluşturun. Aşağıdaki T-SQL betiği, SQL Edge ile aynı Docker ana bilgisayarında çalışan bir uç hub 'ına veri kaynağı bağlantısı oluşturur.

    ```sql
    CREATE EXTERNAL DATA SOURCE EdgeHubInput WITH (
    LOCATION = 'edgehub://'
    )
    go
    ```

3. IoT Edge hub 'ı için dış akış nesnesi oluşturun. Aşağıdaki T-SQL betiği Edge hub 'ı için bir Stream nesnesi oluşturur. Sınır hub 'ı akış nesnesi durumunda, konum parametresi, okunan veya yazılan uç hub konusunun/kanalının adıdır.

    ```sql
    CREATE EXTERNAL STREAM MyTempSensors WITH (
    DATA_SOURCE = EdgeHubInput,
    FILE_FORMAT = InputFileFormat,
    LOCATION = N'TemperatureSensors',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    go
    ```

### <a name="example-create-an-external-stream-object-sql-database"></a>Örnek: dış Stream nesnesi SQL veritabanı oluşturma

Aşağıdaki örnek, yerel SQL Edge veritabanına bir dış akış nesnesi oluşturur. 

1. Veritabanında bir ana anahtar oluşturun. Kimlik bilgisi gizliliğini şifrelemek için bu gereklidir.

    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<<Strong_Password_For_Master_Key_Encryption>>';
    ```

2. SQL Server kaynağına erişmek için bir veritabanı kapsamlı kimlik bilgisi oluşturun. Aşağıdaki örnek, KIMLIK = ' username ' ve SECRET = ' password ' olan dış veri kaynağına bir kimlik bilgisi oluşturur.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SQLCredential
    WITH IDENTITY = '<SQL_Login>', SECRET = '<SQL_Login_PASSWORD>'
    go
    ```

3. Dış VERI kaynağı oluştur ile bir dış veri kaynağı oluşturun. Aşağıdaki örnek:

    * LocalSQLOutput adlı bir dış veri kaynağı oluşturur
    * Dış veri kaynağını tanımlar (konum = ' <vendor> :// <server> [: <port> ] '). Örnekte, SQL Edge 'in yerel bir örneğine işaret eder.
    * Son olarak, örnek, daha önce oluşturulan kimlik bilgisini kullanır.

    ```sql
    CREATE EXTERNAL DATA SOURCE LocalSQLOutput WITH (
    LOCATION = 'sqlserver://tcp:.,1433'
    ,CREDENTIAL = SQLCredential
    )
    go
    ```

4. Dış akış nesnesini oluşturun. Aşağıdaki örnek, dbo tablosuna işaret eden bir dış akış nesnesi oluşturur *. TemperatureMeasurements* , *mysqldatabase*veritabanında.

    ```sql
    CREATE EXTERNAL STREAM TemperatureMeasurements WITH (
    DATA_SOURCE = LocalSQLOutput,
    LOCATION = N'MySQLDatabase.dbo.TemperatureMeasurements',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    ```

## <a name="create-the-streaming-job-and-the-streaming-queries"></a>Akış işini ve akış sorgularını oluşturma

Akış sorgularını tanımlamak ve akış işini oluşturmak için **sys. sp_create_streaming_job** sistem saklı yordamını kullanın. **Sp_create_streaming_job** saklı yordamı iki parametre alır

- job_name-akış işinin adı. Akış işi adları, örnek genelinde benzersizdir.
- deyim- [Stream Analytics sorgu Dil](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?) tabanlı akış sorgu deyimleri.

Aşağıdaki örnek, bir akış sorgusuyla basit bir akış işi oluşturur. Bu sorgu, Edge hub 'ından gelen girdileri okur ve dbo 'ya yazar *. Veritabanında TemperatureMeasurements* .

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob1',
@statement= N'Select * INTO TemperatureMeasurements from MyEdgeHubInput'
```

Aşağıdaki örnek, sıcaklık verilerinde bozuklukları belirlemek için yerleşik AnomalyDetection_ChangePoint işlevini kullanan bir sorgu da dahil olmak üzere birden çok farklı sorguya sahip daha karmaşık bir akış işi oluşturur.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob2', @statement=
N' Select * INTO TemperatureMeasurements1 from MyEdgeHubInput1

Select * Into TemperatureMeasurements2 from MyEdgeHubInput2

Select * Into TemperatureMeasurements3 from MyEdgeHubInput3

SELECT
Timestamp as [Time],
[Temperature] As [Temperature],
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''Score'') as ChangePointScore,
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''IsAnomaly'') as IsChangePointAnomaly,
INTO TemperatureAnomalies FROM MyEdgeHubInput2;
'
go
```

## <a name="start-stop-drop-and-monitor-streaming-jobs"></a>Akış işlerini başlatma, durdurma, bırakma ve izleme

SQL Edge 'de bir akış işi başlatmak için, **sys. sp_start_streaming_job** saklı yordamını yürütün. Saklı yordam, giriş olarak, akış işinin başlamasını gerektirir.

```sql
exec sys.sp_start_streaming_job @name=N'StreamingJob1'
go
```

SQL Edge 'de bir akış işini durdurmak için, **sys. sp_stop_streaming_job** saklı yordamını yürütün. Saklı yordam, giriş olarak, akış işinin durdurulmasını gerektirir.

```sql
exec sys.sp_stop_streaming_job @name=N'StreamingJob1'
go
```

SQL Edge 'de bir akış işini bırakmak (veya silmek) için, **sys. sp_drop_streaming_job** saklı yordamını yürütün. Saklı yordam, giriş olarak akış işinin aynı olmasını gerektirir.

```sql
exec sys.sp_drop_streaming_job @name=N'StreamingJob1'
go
```

SQL Edge 'de bir akış işinin geçerli durumunu almak için, **sys. sp_get_streaming_job** saklı yordamını yürütün. Saklı yordam, giriş olarak akış işinin aynı olmasını gerektirir ve akış işinin adını ve geçerli durumunu verir.

```sql
exec sys.sp_get_streaming_job @name=N'StreamingJob1'
        WITH RESULT SETS
(
       (
       name nvarchar(256),
       status nvarchar(256)
       )
)
```

Akış işi aşağıdaki durumlardan herhangi birinde olabilir

| Durum | Açıklama |
|--------| ------------|
| Oluşturulan | Akış işi oluşturuldu, ancak henüz başlatılmadı |
| Başlatılıyor | Akış işi başlangıç aşamasındadır |
| Boş | Akış işi çalışıyor, ancak işlenecek giriş yok |
| İşleniyor | Akış işi çalışıyor ve girişleri işliyor. Bu durum, akış işi için sağlıklı durumu gösterir |
| Düzeyi düşürüldü | Akış işi çalışıyor, ancak giriş işleme sırasında önemli olmayan giriş/çıkış serileştirme/seri hale getirme hataları vardı. Giriş işi çalışmaya devam edecek, ancak hatalarla karşılaşan girdileri bırakacak |
| Durduruldu | Akış işi durduruldu |
| Başarısız | Akış işi başarısız oldu. Bu genellikle işlem sırasında önemli bir hatanın göstergesidir |

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure SQL Edge 'de akış işleriyle ilişkili meta verileri görüntüleme (Önizleme)](streaming-catalog-views.md) 
- [Dış akış oluşturma](create-external-stream-transact-sql.md)
