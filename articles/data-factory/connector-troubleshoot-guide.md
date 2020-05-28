---
title: Azure Data Factory bağlayıcı sorunlarını giderme
description: Azure Data Factory 'deki bağlayıcı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 01/09/2020
ms.author: jingwang
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: a1b2f74af02db1560dbcdd0bf0c72976dc6dcea8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84022342"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Azure Data Factory bağlayıcı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki bağlayıcılar için genel sorun giderme yöntemleri incelenmektedir.
  

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="error-code--azurebloboperationfailed"></a>Hata kodu: AzureBlobOperationFailed

- **İleti**:`Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Neden**: BLOB depolama işlemi isabet sorunu.

- **Öneri**: ayrıntıdaki hatayı denetleyin. Blob yardım belgesine başvurun: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes . Yardım gerekirse depolama ekibine başvurun.


### <a name="error-code--azureblobservicenotreturnexpecteddatalength"></a>Hata kodu: AzureBlobServiceNotReturnExpectedDataLength

- **İleti**:`Error occurred when trying to fetch the blob '%name;'. This could be a transient issue and you may rerun the job. If it fails again continuously, contact customer support.`


### <a name="error-code--azureblobnotsupportmultiplefilesintosingleblob"></a>Hata kodu: AzureBlobNotSupportMultipleFilesIntoSingleBlob

- **İleti**:`Transferring multiple files into a single Blob is not supported. Currently only single file source is supported.`


### <a name="error-code--azurestorageoperationfailedconcurrentwrite"></a>Hata kodu: AzureStorageOperationFailedConcurrentWrite

- **İleti**:`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Hata iletisi: Istek boyutu çok büyük

- **Belirtiler**: verileri varsayılan yazma toplu işi boyutuyla Azure Cosmos DB kopyalar ve *"**istek boyutu çok büyük**"* hatası ile karşılaşırsınız.

- **Neden**: Cosmos DB tek bir isteğin boyutunu 2 MB olarak sınırlar. Formül, Istek boyutu = tek belge boyutu * yazma toplu Iş boyutu ' dür. Belge boyutunuz büyükse, varsayılan davranış çok büyük istek boyutuna neden olur. Yazma toplu iş boyutunu ayarlayabilirsiniz.

- **Çözüm**: kopyalama etkinliği havuzunda ' toplu Iş boyutunu yaz ' değerini küçültün (varsayılan değer 10000 ' dir).

### <a name="error-message-unique-index-constraint-violation"></a>Hata iletisi: benzersiz dizin kısıtlaması ihlali

- **Belirtiler**: Cosmos DB veri kopyalarken şu hatayla karşılaşırsınız:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception = Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Neden**: olası iki neden vardır:

    - **Insert** as Write davranışını kullanırsanız, bu hata kaynak VERILERDE aynı kimliğe sahip satırlar/nesneler olduğu anlamına gelir.

    - Yazma davranışı olarak **upsert** kullanırsanız ve kapsayıcıya başka bir benzersiz anahtar ayarlarsanız, bu hata kaynak verilerde farklı kimliklere sahip satırlar/nesneler ve tanımlanan benzersiz anahtar için aynı değere sahip olduğunuz anlamına gelir.

- **Çözüm**: 

    - Cause1 için, **büyük sert** yazma davranışı olarak ayarlayın.
    - Neden 2 için, her belgenin tanımlanmış benzersiz anahtar için farklı bir değere sahip olduğundan emin olun.

### <a name="error-message-request-rate-is-large"></a>Hata iletisi: Istek hızı büyük

- **Belirtiler**: Cosmos DB veri kopyalarken şu hatayla karşılaşırsınız:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Neden**: kullanılan istek birimleri Cosmos DB yapılandırılan ru 'dan daha büyük. Cosmos DB RU 'yi [buradan](../cosmos-db/request-units.md#request-unit-considerations)nasıl hesaplayacağını öğrenin.

- **Çözüm**: iki çözüm vardır:

    1. Cosmos DB, kopyalama etkinliği performansını iyileştirecek, ancak Cosmos DB daha fazla ücret ödemesine neden olacak şekilde, 1. kapsayıcıyı daha büyük bir değere **yükseltin** . 

    2. **Writebatchsize** değerini daha küçük bir değere (1000 gibi) düşürün ve **parallelcopy** değerlerini 1 gibi daha küçük bir değere ayarlayın. Bu, kopya çalıştırma performansının geçerli olmasını sağlar ancak Cosmos DB daha fazla ücret vermez.

### <a name="column-missing-in-column-mapping"></a>Sütun eşlemesinde sütun eksik

- **Belirtiler**: sütun eşleme için Cosmos DB şemayı içeri aktardığınızda bazı sütunlar eksiktir. 

- **Neden**: ADF ilk 10 Cosmos DB belgeden şemayı anlar. Bazı sütunlarda/özelliklerde bu belgelerde değer yoksa, ADF tarafından algılanmaz ve bu nedenle gösterilmez.

- **Çözüm**: sütunu boş değere sahip sonuç kümesinde göstermek üzere zorlamak için aşağıdaki gibi bir sorgu ayarlayabilirsiniz: (ilk 10 belgede "imkansız" sütunu eksik). Alternatif olarak, eşleme için sütunu el ile ekleyebilirsiniz.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Hata iletisi: okuyucu için Guidtemsili CSharpLegacy

- **Belirtiler**: UUID alanı Ile Cosmos DB MongoAPI/MongoDB 'den veri kopyalarken şu hatayla karşılaşırsınız:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Neden**: bSon-UuidStardard ve UUIDLEGACY 'de UUID 'yi göstermenin iki yolu vardır. Varsayılan olarak, UuidLegacy veri okumak için kullanılır. MongoDB 'deki UUID verileriniz UuidStandard ise hatayla karşılaşacaktır.

- **Çözüm**: MongoDB bağlantı dizesinde "**uuidRepresentation = Standard**" seçeneğini ekleyin. Daha fazla bilgi için bkz. [MongoDB bağlantı dizesi](connector-mongodb.md#linked-service-properties).
            

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code--adlsgen2operationfailed"></a>Hata kodu: AdlsGen2OperationFailed

- **İleti**:`ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Neden**: ADLS 2. işlem başarısız olduğunu gösteren hatayı oluşturur.

- **Öneri**: ADLS 2. tarafından oluşturulan ayrıntılı hata iletisini denetleyin. Geçici bir hata nedeniyle, lütfen yeniden deneyin. Daha fazla yardıma ihtiyacınız varsa lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.

- **Neden**: hata Iletisi ' yasak ' içerdiğinde, kullandığınız hizmet sorumlusu veya yönetilen kimlik ADLS 2. erişmek için yeterli izne sahip olmayabilir.

- **Öneri**: yardım belgesine bakın: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication .

- **Neden**: hata Iletisi ' ınternalservererror ' içerdiğinde hata ADLS 2. tarafından döndürülür.

- **Öneri**: geçici bir hatadan kaynaklanıyor olabilir, lütfen yeniden deneyin. Sorun devam ederse lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.


### <a name="error-code--adlsgen2invalidurl"></a>Hata kodu: AdlsGen2InvalidUrl

- **İleti**:`Invalid url '%url;' provided, expecting http[s]://<accountname>.dfs.core.windows.net.`


### <a name="error-code--adlsgen2invalidfolderpath"></a>Hata kodu: AdlsGen2InvalidFolderPath

- **İleti**:`The folder path is not specified. Cannot locate the file '%name;' under the ADLS Gen2 account directly. Please specify the folder path instead.`


### <a name="error-code--adlsgen2operationfailedconcurrentwrite"></a>Hata kodu: AdlsGen2OperationFailedConcurrentWrite

- **İleti**:`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="error-code--adlsgen2timeouterror"></a>Hata kodu: AdlsGen2TimeoutError

- **İleti**:`Request to ADLS Gen2 account '%account;' met timeout error. It is mostly caused by the poor network between the Self-hosted IR machine and the ADLS Gen2 account. Check the network to resolve such error.`


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Hata iletisi: uzak sunucu bir hata döndürdü: (403) yasak

- **Belirtiler**: kopyalama etkinliği aşağıdaki hatayla başarısız oldu: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Neden**: olası bir neden, kullandığınız hizmet sorumlusu veya yönetilen kimliğin belirli klasöre/dosyaya erişim izni olmaması olabilir.

- **Çözüm**: kopyalamanız gereken tüm klasörler ve alt klasörlerde ilgili izinleri verin. [Bu belgeye](connector-azure-data-lake-store.md#linked-service-properties)başvurun.

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Hata iletisi: hizmet sorumlusu kullanılarak erişim belirteci alınamadı. ADAL hatası: service_unavailable

- **Belirtiler**: kopyalama etkinliği aşağıdaki hatayla başarısız oldu:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Neden**: Azure Active Directory sahip olduğu hizmet belirteci sunucusu (STS) kullanılamıyor, yani istekleri işlemek için çok meşgul OLMADıĞıNDA, HTTP hatası 503 döndürür. 

- **Çözüm**: birkaç dakika sonra kopyalama etkinliğini yeniden çalıştırın.
                  

## <a name="azure-sql-data-warehouseazure-sql-databasesql-server"></a>Azure SQL veri ambarı/Azure SQL veritabanı/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Hata kodu: SqlFailedToConnect

- **İleti**:`Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: lütfen daha fazla ayrıntı için bu başvuru BELGESI içindeki SQL hata kodu ile arama yapın: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata ILETISI "IP adresi olan istemci '... ' içeriyorsa) sunucusuna erişim izni verilmez "ve Azure SQL veritabanı 'na bağlanmaya çalışıyorsunuz, genellikle Azure SQL veritabanı güvenlik duvarı sorunu nedeniyle oluşur.

- **Öneri**: mantıksal SQL Server güvenlik duvarı yapılandırması ' nda, "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine izin ver" seçeneğini etkinleştirin. Başvuru belgesi: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure .


### <a name="error-code--sqloperationfailed"></a>Hata kodu: SqlOperationFailed

- **İleti**:`A database operation failed. Please search error to get more details.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: SQL hatası net değilse, lütfen veritabanını en son uyumluluk düzeyi olan ' 150 ' olarak değiştirmeyi deneyin. En son sürüm SQL hatalarını oluşturabilir. Lütfen belge: Ayrıntılar bölümüne bakın https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level?view=sql-server-ver15#backwardCompat .
        SQL sorunlarını gidermek için lütfen daha fazla ayrıntı için bu başvuru belgesi içindeki SQL hata kodu ile arama yapın: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata Iletisi "PdwManagedToNativeInteropException" içeriyorsa, genellikle kaynak ve havuz sütun boyutları arasında uyuşmazlık olur.

- **Öneri**: hem kaynak hem de havuz sütunlarının boyutunu denetleyin. Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata Iletisi "InvalidOperationException" içeriyorsa, genellikle geçersiz giriş verileri oluşur.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance .


### <a name="error-code--sqlunauthorizedaccess"></a>Hata kodu: SqlUnauthorizedAccess

- **İleti**:`Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Neden**: kimlik bilgisi yanlış veya oturum açma hesabı SQL veritabanına erişemiyor.

- **Öneri**: oturum açma hesabının SQL veritabanına erişmek için yeterli izni olup olmadığını denetleyin.


### <a name="error-code--sqlopenconnectiontimeout"></a>Hata kodu: SqlOpenConnectionTimeout

- **İleti**:`Open connection to database timeout after '%timeoutValue;' seconds.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: lütfen bağlı hizmet bağlantı dizesini daha büyük bağlantı zaman aşımı değeri ile güncelleştirmeyi yeniden deneyin.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Hata kodu: SqlAutoCreateTableTypeMapFailed

- **İleti**:`Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **Neden**: otomatik oluşturma tablosu kaynak gereksinimini karşılamaz.

- **Öneri**: ' eşlemeler ' içindeki sütun türünü güncelleştirin veya hedef sunucuda havuz tablosunu el ile oluşturun.


### <a name="error-code--sqldatatypenotsupported"></a>Hata kodu: SqlDataTypeNotSupported

- **İleti**:`A database operation failed. Check the SQL errors.`

- **Neden**: sorun SQL kaynağında olursa ve hata SqlDateTime overflow ile ilgiliyse, veri değeri Logic Type aralığı üzerinden (1/1/1753 12:00:00 ÖÖ-12/31/9999 11:59:59 PM) oluşur.

- **Öneri**: türü kaynak SQL sorgusunda dizeye atayın veya kopyalama etkinliği sütun eşlemesinde sütun türünü ' String ' olarak değiştirin.

- **Neden**: sorun SQL havuzunda gerçekleşirse ve hata SqlDateTime overflow ile ilgiliyse, veri değeri havuz tablosu 'nda izin verilen aralığın üzerinde olur.

- **Öneri**: havuz tablosunda karşılık gelen sütun türü ' datetime2 ' türüne güncelleştirin.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Hata kodu: SqlInvalidDbStoredProcedure

- **İleti**:`The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Neden**: belirtilen saklı yordam geçerli değil. Bu, saklı yordamın herhangi bir veri döndürmemesinin nedeni olabilir.

- **Öneri**: SAKLı yordamı SQL araçları ile doğrulayın. Saklı yordamın verileri döndüre, emin olun.


### <a name="error-code--sqlinvaliddbquerystring"></a>Hata kodu: SqlInvalidDbQueryString

- **İleti**:`The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Neden**: belirtilen SQL sorgusu geçerli değil. Sorgunun herhangi bir veri döndürmediğinden kaynaklanabilir

- **Öneri**: SQL sorgusunu SQL araçları ile doğrulayın. Sorgunun verileri döndüre, emin olun.


### <a name="error-code--sqlinvalidcolumnname"></a>Hata kodu: SqlInvalidColumnName

- **İleti**:`Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Neden**: sütun bulunamıyor. Olası yapılandırma yanlış.

- **Öneri**: sorgudaki sütunu, veri kümesindeki ' Structure ' öğesini ve etkinlik içindeki ' eşlemeler ' doğrulamasını doğrulayın.


### <a name="error-code--sqlcolumnnamemismatchbycasesensitive"></a>Hata kodu: SqlColumnNameMismatchByCaseSensitive

- **İleti**:`Column '%column;' in DataSet '%dataSetName;' cannot be found in physical SQL Database. Column matching is case-sensitive. Column '%columnInTable;' appears similar. Check the DataSet(s) configuration to proceed further.`


### <a name="error-code--sqlbatchwritetimeout"></a>Hata kodu: SqlBatchWriteTimeout

- **İleti**:`Timeouts in SQL write operation.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: Lütfen yeniden deneyin. Sorun yeniden üretme varsa Azure SQL desteği 'ne başvurun.


### <a name="error-code--sqlbatchwritetransactionfailed"></a>Hata kodu: SqlBatchWriteTransactionFailed

- **İleti**:`SQL transaction commits failed`

- **Neden**: özel durum ayrıntıları işlem zaman aşımını sürekli söyledi, tümleştirme çalışma zamanı ve veritabanı arasındaki ağ gecikmesi varsayılan eşikten 30 saniye daha yüksektir.

- **Öneri**: SQL bağlantılı hizmet bağlantı dizesini ' bağlantı zaman aşımı ' değeri 120 veya üzeri olarak güncelleştirin ve etkinliği yeniden çalıştırın.

- **Neden**: özel durum ayrıntıları SqlConnection 'ı kopuk olarak söylüyor, yalnızca geçici ağ ARıZASı veya SQL veritabanı tarafı sorunu olabilir

- **Öneri**: lütfen etkinliği yeniden deneyın ve SQL veritabanı tarafı ölçümlerini inceleyin.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Hata kodu: Sqlbulkcopyınvalidcolumnlength

- **İleti**:`SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **Neden**: bcp istemcisinden geçersiz bir sütun uzunluğu aldığından SQL toplu kopyalama başarısız oldu.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance .


### <a name="error-code--sqlconnectionisclosed"></a>Hata kodu: Sqlconnectionısclosed

- **İleti**:`The connection is closed by SQL Database.`

- **Neden**: yüksek eşzamanlı çalıştırma ve sunucu bağlantısı sona erdiğinde SQL bağlantısı SQL veritabanı tarafından kapalıdır.

- **Öneri**: uzak sunucu SQL bağlantısını kapattı. Lütfen yeniden deneyin. Sorun yeniden üretme varsa Azure SQL desteği 'ne başvurun.


### <a name="error-code--sqlcreatetablefailedunsupportedtype"></a>Hata kodu: SqlCreateTableFailedUnsupportedType

- **İleti**:`Type '%type;' in source side cannot be mapped to a type that supported by sink side(column name:'%name;') in autocreate table.`


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Hata iletisi: bir karakter dizesinden uniqueidentifier öğesine dönüştürülürken dönüştürme başarısız oldu

- **Belirtiler**: tablo veri kaynağından (örneğin, SQL Server) verileri, hazırlanan Copy ve PolyBase kullanarak Azure SQL veri ambarı 'na kopyaladığınızda, aşağıdaki hataya ulaşırsınız:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Neden**: Azure SQL veri ambarı PolyBase boş dizeyi GUID 'ye dönüştüremiyor.

- **Çözüm**: kopyalama etkinliği havuzunda, PolyBase ayarları altında "**tür Varsayılanı kullan**" seçeneğini false olarak ayarlayın.

### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Hata iletisi: beklenen veri türü: DECIMAL (x, x), sorunlu değer

- **Belirtiler**: bir tablo veri kaynağından (örneğin, SQL Server) verileri, hazırlanan kopya ve PolyBase kullanarak SQL DW 'ye kopyaladığınızda aşağıdaki hataya ulaşırsınız:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Neden**: Azure SQL veri ambarı PolyBase, decimal sütununa boş dize (null değer) ekleyemiyor.

- **Çözüm**: kopyalama etkinliği havuzunda, PolyBase ayarları altında "**tür Varsayılanı kullan**" seçeneğini false olarak ayarlayın.

### <a name="error-message-java-exception-messagehdfsbridgecreaterecordreader"></a>Hata iletisi: Java özel durum iletisi: HdfsBridge:: CreateRecordReader

- **Belirtiler**: PolyBase 'ı kullanarak Azure SQL veri ambarı 'na veri kopyalar ve şu hatayı alırsınız:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Neden**: olası neden şemanın (Toplam sütun genişliği) çok büyük (1 MB 'den büyük) olması olabilir. Tüm sütunların boyutunu ekleyerek hedef SQL DW tablosunun şemasını denetleyin:

    - Int-> 4 bayt
    - Büyük tamsayı-> 8 bayt
    - Varchar (n), char (n), binary (n), varbinary (n)-> n bayt
    - Nvarchar (n), nchar (n)-> n * 2 bayt
    - Tarih-> 6 bayt
    - DateTime/(2), smalldatetime-> 16 bayt
    - DateTimeOffset-> 20 bayt
    - Decimal-> 19 bayt
    - Kayan > 8 bayt
    - Para-> 8 bayt
    - Küçük para-> 4 bayt
    - Gerçek > 4 bayt
    - Küçük tamsayı-> 2 bayt
    - Zaman-> 12 bayt
    - Tinyint-> 1 bayt

- **Çözüm**: sütun GENIŞLIĞINI 1 MB 'tan küçük olacak şekilde azalt

- Veya PolyBase 'i devre dışı bırakarak toplu ekleme yaklaşımını kullanın

### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Hata iletisi: HTTP koşullu üst bilgi (ler) i kullanılarak belirtilen koşul karşılanmadı

- **Belirtiler**: SQL sorgusunu kullanarak Azure SQL veri ambarı 'ndan veri çekin ve şu hatayı alırsınız:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Neden**: Azure SQL veri ambarı, Azure depolama 'daki dış tabloyu sorgulama sorunu ile karşılaştı.

- **Çözüm**: aynı sorguyu SSMS 'de çalıştırın ve aynı sonucu görüp gör, kontrol edin. Alıyorsanız sorun gidermek için Azure SQL Veri Ambarı için bir destek bileti açın ve SQL DW sunucunuzla veritabanınızın adını belirtin.
            

## <a name="delimited-text-format"></a>Sınırlandırılmış metin biçimi

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Hata kodu: Delimitedtextcolumnsüs Otallownull

- **İleti**:`The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Neden**: etkinlikte ' firstRowAsHeader ' ayarlandığında, ilk satır sütun adı olarak kullanılır. Bu hata, ilk satırın boş değer içerdiği anlamına gelir. Örneğin: ' ColumnA,, ColumnB '.

- **Öneri**: ilk satırı denetleyin ve boş değer varsa değeri düzeltir.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Hata kodu: Delimitedtextmorecolumnsıısdefined tanımlandı

- **İleti**:`Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %columnCount;.`

- **Neden**: sorunlu satırın sütun sayısı, ilk satırın sütun sayısından büyük. Bu durum, veri sorunu ya da yanlış sütun sınırlayıcısı/quote char ayarlarından kaynaklanıyor olabilir.

- **Öneri**: lütfen satır sayısını hata iletisinde alın, satırın sütununu kontrol edin ve verileri onarın.

- **Neden**: beklenen sütun sayısı "1" ise hata ILETISINDE, ADF 'nin dosya (lar) yanlışlıkla ayrıştırmasına neden olan yanlış sıkıştırma veya biçim ayarlarını belirtmeiyorduk.

- **Öneri**: kaynak dosya (lar) ile eşleştiğinden emin olmak için biçim ayarlarını kontrol edin.

- **Neden**: kaynağınız bir klasörysa, belirtilen klasörde bulunan dosyaların farklı bir şeması olması mümkündür.

- **Öneri**: belirtilen klasörde bulunan dosyaların aynı şemaya sahip olduğundan emin olun.


### <a name="error-code--delimitedtextincorrectrowdelimiter"></a>Hata kodu: DelimitedTextIncorrectRowDelimiter

- **İleti**:`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--delimitedtexttoolargecolumncount"></a>Hata kodu: DelimitedTextTooLargeColumnCount

- **İleti**:`Column count reaches limitation when deserializing csv file. Maximum size is '%size;'. Check the column delimiter and row delimiter provided. (Column delimiter: '%columnDelimiter;', Row delimiter: '%rowDelimiter;')`


### <a name="error-code--delimitedtextinvalidsettings"></a>Hata kodu: DelimitedTextInvalidSettings

- **İleti**:`%settingIssues;`



## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Hata kodu: DynamicsCreateServiceClientError

- **İleti**:`This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Neden**: Bu, Dynamics Server tarafında geçici bir sorundur.

- **Öneri**: işlem hattını yeniden çalıştırın. Başarısız olursa paralelliği azaltmayı deneyin. Hala başarısız olduysa, lütfen Dynamics destek birimine başvurun.



## <a name="json-format"></a>JSON biçimi

### <a name="error-code--jsoninvalidarraypathdefinition"></a>Hata kodu: Jsonınvalidarraypathdefinition

- **İleti**:`Error occurred when deserializing source JSON data. Check whether the JsonPath in JsonNodeReference and JsonPathDefintion is valid.`


### <a name="error-code--jsonemptyjobjectdata"></a>Hata kodu: JsonEmptyJObjectData

- **İleti**:`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--jsonnullvalueinpathdefinition"></a>Hata kodu: Jsonnullvalueınpathdefinition

- **İleti**:`Null JSONPath detected in JsonPathDefinition.`


### <a name="error-code--jsonunsupportedhierarchicalcomplexvalue"></a>Hata kodu: JsonUnsupportedHierarchicalComplexValue

- **İleti**:`The retrieved type of data %data; with value %value; is not supported yet. Please either remove the targeted column '%name;' or enable skip incompatible row to skip the issue rows.`


### <a name="error-code--jsonconflictpartitiondiscoveryschema"></a>Hata kodu: JsonConflictPartitionDiscoverySchema

- **İleti**:`Conflicting partition column names detected.'%schema;', '%partitionDiscoverySchema;'`


### <a name="error-code--jsoninvaliddataformat"></a>Hata kodu: Jsonınvaliddataformat

- **İleti**:`Error occurred when deserializing source JSON file '%fileName;'. Check if the data is in valid JSON object format.`


### <a name="error-code--jsoninvaliddatamixedarrayandobject"></a>Hata kodu: Jsonınvaliddatamixedarrayandobject

- **İleti**:`Error occurred when deserializing source JSON file '%fileName;'. The JSON format doesn't allow mixed arrays and objects.`



## <a name="parquet-format"></a>Parquet biçimi

### <a name="error-code--parquetjavainvocationexception"></a>Hata kodu: Parquetjavaınvocationexception

- **İleti**:`An error occurred when invoking java, message: %javaException;.`

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ', ' Java yığın alanı ' ve ' doublecapacity ' içerdiğinde, genellikle tümleştirme çalışma zamanının eski sürümünde bir bellek yönetimi sorunudur.

- **Öneri**: şirket içinde barındırılan Integration Runtime kullanıyorsanız ve sürüm 3.20.7159.1 'den daha eski ise en son sürüme yükseltmeyi önerin.

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ' içerdiğinde, tümleştirme çalışma zamanı dosyaları işlemek için yeterli kaynağa sahip değil.

- **Öneri**: Tümleştirme çalışma zamanında eşzamanlı çalıştırmaları sınırlayın. Şirket içinde barındırılan Integration Runtime için, 8 GB 'a eşit veya daha büyük belleği olan güçlü bir makineye kadar ölçeklendirin.

- **Neden**: hata Iletisi ' NullPointerReference ' içerdiğinde, geçici bir hata olabilir.

- **Öneri**: Lütfen yeniden deneyin. Sorun devam ederse lütfen desteğe başvurun.


### <a name="error-code--parquetinvalidfile"></a>Hata kodu: Parquetınvalidfile

- **İleti**:`File is not a valid parquet file.`

- **Neden**: Parquet dosyası sorunu.

- **Öneri**: girişin geçerli bir Parquet dosyası olduğunu denetleyin.


### <a name="error-code--parquetnotsupportedtype"></a>Hata kodu: ParquetNotSupportedType

- **İleti**:`Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **Neden**: Parquet biçimi Azure Data Factory desteklenmiyor.

- **Öneri**: kaynak verileri iki kez kontrol edin. Belgeye başvurun: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs .


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>Hata kodu: ParquetMissedDecimalPrecisionScale

- **İleti**:`Decimal Precision or Scale information is not found in schema for column: %column;.`

- **Neden**: duyarlık ve ölçek sayısını ayrıştırmayı deneyin, ancak böyle bir bilgi sağlanmaz.

- **Öneri**: ' kaynak ' doğru duyarlık ve ölçek döndürmüyor. Sorun sütununun duyarlığını ve ölçeğini denetleyin.


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>Hata kodu: ParquetInvalidDecimalPrecisionScale

- **İleti**:`Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **Neden**: şema geçersiz.

- **Öneri**: sorun sütununun duyarlılığını ve ölçeğini denetleyin.


### <a name="error-code--parquetcolumnnotfound"></a>Hata kodu: ParquetColumnNotFound

- **İleti**:`Column %column; does not exist in Parquet file.`

- **Neden**: kaynak şemasının havuz şeması ile eşleşmemesi.

- **Öneri**: ' Activity ' içindeki ' eşlemeleri ' öğesini işaretleyin. Kaynak sütunun doğru havuz sütunuyla eşlenip eşlendiğinden emin olun.


### <a name="error-code--parquetinvaliddataformat"></a>Hata kodu: Parquetınvaliddataformat

- **İleti**:`Incorrect format of %srcValue; for converting to %dstType;.`

- **Neden**: veriler eşlemeler. Source içinde belirtilen türe dönüştürülemiyor

- **Öneri**: kaynak verileri iki kez kontrol edin veya kopyalama etkinliği sütun eşlemesinde bu sütun için doğru veri türünü belirtin. Belgeye başvurun: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs .


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>Hata kodu: ParquetDataCountNotMatchColumnCount

- **İleti**:`The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **Neden**: kaynak sütun sayısı ve havuz sütun sayısı uyuşmazlığı

- **Öneri**: çift Check kaynak sütunu sayısı ' Mapping ' içindeki havuz sütun sayısıyla aynı.


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>Hata kodu: ParquetDataTypeNotMatchColumnType

- **İleti**:% srctype; veri türü Belirtilen% dstType; sütun türü ile eşleşmiyor '% columnIndex; ' sütununda.

- **Neden**: kaynaktaki veri, havuzda tanımlı olarak dönüştürülemez

- **Öneri**: lütfen Mapping. Sink içinde doğru bir tür belirtin.


### <a name="error-code--parquetbridgeinvaliddata"></a>Hata kodu: Parquetköprüınvaliddata

- **İleti**:`%message;`

- **Neden**: sınırlama üzerinde veri değeri

- **Öneri**: Lütfen yeniden deneyin. Sorun devam ederse lütfen bizimle iletişime geçin.


### <a name="error-code--parquetunsupportedinterpretation"></a>Hata kodu: Parquetunsupportedyorumlama

- **İleti**:`The given interpretation '%interpretation;' of parquet format is not supported.`

- **Neden**: desteklenmeyen senaryo

- **Öneri**: ' Parquetınterpotfor ' ' mini SQL ' olmamalıdır.


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>Hata kodu: ParquetUnsupportFileLevelCompressionOption

- **İleti**:`File level compression is not supported for Parquet.`

- **Neden**: desteklenmeyen senaryo

- **Öneri**: yükte ' CompressionType ' öğesini kaldırın.



## <a name="general-copy-activity-error"></a>Genel kopyalama etkinliği hatası

### <a name="error-code--jrenotfound"></a>Hata kodu: JreNotFound

- **İleti**:`Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Neden**: şirket içinde barındırılan tümleştirme çalışma zamanı Java çalışma zamanı bulamıyor. Java çalışma zamanı, belirli bir kaynağı okumak için gereklidir.

- **Öneri**: Tümleştirme çalışma zamanı ortamınızı, belge başvurusu:https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>Hata kodu: Yavaya Cardpathsinyatsupported

- **İleti**:`Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Neden**: havuz veri kümesi joker karakteri desteklemiyor.

- **Öneri**: havuz veri kümesini denetleyin ve Joker değer olmadan yolu düzeltir.


### <a name="error-code--mappinginvalidpropertywithemptyvalue"></a>Hata kodu: MappingInvalidPropertyWithEmptyValue

- **İleti**:`One or more '%sourceOrSink;' in copy activity mapping doesn't point to any data. Choose one of the three properties 'name', 'path' and 'ordinal' to reference columns/fields.`


### <a name="error-code--mappinginvalidpropertywithnamepathandordinal"></a>Hata kodu: Mappingınvalidpropertywithnamepathandordinal

- **İleti**:`Mixed properties are used to reference '%sourceOrSink;' columns/fields in copy activity mapping. Please only choose one of the three properties 'name', 'path' and 'ordinal'. The problematic mapping setting is 'name': '%name;', 'path': '%path;','ordinal': '%ordinal;'.`


### <a name="error-code--mappingduplicatedordinal"></a>Hata kodu: MappingDuplicatedOrdinal

- **İleti**:`Copy activity 'mappings' has duplicated ordinal value "%Ordinal;". Fix the setting in 'mappings'.`


### <a name="error-code--mappinginvalidordinalforsinkcolumn"></a>Hata kodu: MappingInvalidOrdinalForSinkColumn

- **İleti**:`Invalid 'ordinal' property for sink column under 'mappings' property. Ordinal: %Ordinal;.`


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
            
