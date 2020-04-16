---
title: Azure Data Factory bağlayıcı sorunlarını giderme
description: Azure Veri Fabrikası'nda bağlayıcı sorunlarını nasıl gidereceklerini öğrenin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 01/09/2020
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 62ad337646cf3fc0bbe4305dccad5adb56f8ee15
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410221"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Azure Data Factory bağlayıcı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Veri Fabrikası'ndaki bağlayıcılar için sık karşılaşılan sorun giderme yöntemleri inceleilmiştir.
  

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="error-code--azurebloboperationfailed"></a>Hata kodu: AzureBlobOperationFailed

- **Mesaj**:`Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Neden**: Blob depolama işlemi sorunu vurdu.

- **Öneri**: Hatayı ayrıntılarda kontrol edin. Blob yardım belgesine https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codesbakın: . Yardıma ihtiyaç duyarsanız depolama ekibine başvurun.


### <a name="error-code--azureblobservicenotreturnexpecteddatalength"></a>Hata kodu: AzureBlobServiceNotReturnExpectedDataLength

- **Mesaj**:`Error occurred when trying to fetch the blob '%name;'. This could be a transient issue and you may rerun the job. If it fails again continuously, contact customer support.`


### <a name="error-code--azureblobnotsupportmultiplefilesintosingleblob"></a>Hata kodu: AzureBlobNotSupportMultipleFilesIntoSingleBlob

- **Mesaj**:`Transferring multiple files into a single Blob is not supported. Currently only single file source is supported.`


### <a name="error-code--azurestorageoperationfailedconcurrentwrite"></a>Hata kodu: AzureStorageOperationFailedConcurrentWrite

- **Mesaj**:`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Hata iletisi: İstek boyutu çok büyük

- **Belirtiler**: Verileri varsayılan yazma toplu iş boyutuyla Azure Cosmos DB'ye kopyalarsınız ve " İstek boyutu çok büyüktür " hatasına ***basılırsınız.***

- **Neden**: Cosmos DB tek bir isteğin boyutunu 2 MB ile sınırlar. Formül, İstek Boyutu = Tek Belge Boyutu * Toplu Boyut Yaz. Belge boyutunuz büyükse, varsayılan davranış çok büyük istek boyutuna neden olur. Yazma toplu iş boyutunu ayarlayabilirsiniz.

- **Çözünürlük**: Kopyalama etkinliği lavabosunda ,'Toplu iş boyutu yaz' değerini azaltın (varsayılan değer 10000'dir).

### <a name="error-message-unique-index-constraint-violation"></a>Hata iletisi: Benzersiz dizin kısıtlaması ihlali

- **Belirtiler**: Verileri Cosmos DB'ye kopyalarken aşağıdaki hataya girersiniz:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception = Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Neden**: İki olası nedeni vardır:

    - **Insert'i** yazma davranışı olarak kullanıyorsanız, bu hata kaynak verilerinizde aynı kama sahip satırların/nesneleriniz olduğu anlamına gelir.

    - **Yukarı sezor'u** yazma davranışı olarak kullanırsanız ve kapsayıcıya başka bir benzersiz anahtar ayarlarsanız, bu hata kaynak verilerinizde farklı dislere sahip satırlar/nesneler ancak tanımlanan benzersiz anahtar için aynı değer anlamına gelir.

- **Çözünürlük**: 

    - Cause1 **için, Upsert'i** yazma davranışı olarak ayarlayın.
    - Cause 2 için, tanımlanan benzersiz anahtar için her belgenin farklı değeri olduğundan emin olun.

### <a name="error-message-request-rate-is-large"></a>Hata iletisi: İstek oranı büyük

- **Belirtiler**: Verileri Cosmos DB'ye kopyalarken aşağıdaki hataya girersiniz:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Neden**: Kullanılan istek birimleri Cosmos DB'de yapılandırılan mevcut RU'dan daha büyüktür. Cosmos DB'nin RU'yı nasıl hesapladığını [buradan](../cosmos-db/request-units.md#request-unit-considerations)öğrenin.

- **Çözünürlük**: İşte iki çözüm:

    1. **Cosmos DB'de** daha fazla maliyete yol açacak olsa da, kopya etkinliği performansını artıracak olan COSmos DB'de RU konteynerini daha büyük bir değere yükseltin. 

    2. **WriteBatchSize'ı** daha küçük bir değere (1000 gibi) azaltın ve **paralel Kopyaları** 1 gibi daha küçük bir değere ayarlayın, bu da kopya çalıştırma performansını geçerliden daha kötü hale getirecek, ancak Cosmos DB'de daha fazla maliyete neden olmaz.

### <a name="column-missing-in-column-mapping"></a>Sütun eşlemede eksik sütun

- **Belirtiler**: Sütun eşleme için Cosmos DB için şema aldığınızda, bazı sütunlar eksik. 

- **Neden**: ADF şema ilk 10 Cosmos DB belgelerinden çıkar. Bazı sütunların/özelliklerin bu belgelerde değeri yoksa, ADF tarafından algılanmazlar, bu nedenle gösterilmez.

- **Çözünürlük**: Boş değerle ayarlanmış sonuç kümesinde göstermek için sütunu zorlamak için sorguyu aşağıdaki gibi ayarlayabilirsiniz: (varyalım: "imkansız" sütun ilk 10 belgede eksik). Alternatif olarak, eşleme için sütunu el ile ekleyebilirsiniz.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Hata iletisi: Okuyucu için GuidRepresentation CSharpLegacy olduğunu

- **Semptomlar**: Cosmos DB MongoAPI/MongoDB'den UUID alanı ile veri kopyalarken aşağıdaki hataya ulaşabilirsiniz:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Neden**: UuidStardard ve UuidLegacy - BSON UUID temsil etmek için iki yolu vardır. Varsayılan olarak, UuidLegacy verileri okumak için kullanılır. MongoDB'deki UUID verileriniz UuidStandard ise hata yapacaksınız.

- **Çözünürlük**: MongoDB bağlantı dizesinde "**uuidRepresentation=standard**" seçeneğini ekleyin. Daha fazla bilgi için [MongoDB bağlantı dizesi'ne](connector-mongodb.md#linked-service-properties)bakın.
            

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code--adlsgen2operationfailed"></a>Hata kodu: AdlsGen2OperationFailed

- **Mesaj**:`ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Neden**: ADLS Gen2 işlemi başarısız olduğunu belirten hata atar.

- **Öneri**: ADLS Gen2 tarafından atılan ayrıntılı hata iletisini kontrol edin. Eğer geçici bir arızadan kaynaklanıyorsa, lütfen yeniden deneyin. Daha fazla yardıma ihtiyacınız varsa, lütfen Azure Depolama desteğine başvurun ve hata iletisinde istek kimliğini sağlayın.

- **Neden**: Hata iletisi 'Yasak' içeriyorsa, kullandığınız hizmet sorumlusu veya yönetilen kimliği ADLS Gen2'ye erişmek için yeterli izne sahip olmayabilir.

- **Öneri**: Yardım belgesine https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authenticationbakın: .

- **Neden**: Hata iletisi 'InternalServerError' içeriyorsa, hata ADLS Gen2 tarafından döndürülür.

- **Öneri**: Geçici bir arızadan kaynaklanabilir, lütfen yeniden deneyin. Sorun devam ederse, lütfen Azure Depolama desteğine başvurun ve hata iletisinde istek kimliğini sağlayın.


### <a name="error-code--adlsgen2invalidurl"></a>Hata kodu: AdlsGen2InvalidUrl

- **Mesaj**:`Invalid url '%url;' provided, expecting http[s]://<accountname>.dfs.core.windows.net.`


### <a name="error-code--adlsgen2invalidfolderpath"></a>Hata kodu: AdlsGen2InvalidFolderPath

- **Mesaj**:`The folder path is not specified. Cannot locate the file '%name;' under the ADLS Gen2 account directly. Please specify the folder path instead.`


### <a name="error-code--adlsgen2operationfailedconcurrentwrite"></a>Hata kodu: AdlsGen2OperationFailedConcurrentWrite

- **Mesaj**:`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="error-code--adlsgen2timeouterror"></a>Hata kodu: AdlsGen2TimeoutError

- **Mesaj**:`Request to ADLS Gen2 account '%account;' met timeout error. It is mostly caused by the poor network between the Self-hosted IR machine and the ADLS Gen2 account. Check the network to resolve such error.`


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Hata iletisi: Uzak sunucu bir hata döndü: (403) Yasak

- **Belirtiler**: Kopyalama etkinliği aşağıdaki hataile başarısız olur: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Neden**: Olası nedenlerden biri, kullandığınız hizmet sorumlusunun veya yönetilen kimliğin belirli klasöre/dosyaya erişim izniolmamasıdır.

- **Çözüm**: Kopyalamanız gereken tüm klasör ve alt klasörlerde ilgili izinleri verir. Bu [dokümana](connector-azure-data-lake-store.md#linked-service-properties)bakın.

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Hata iletisi: Hizmet ilkesini kullanarak tokene erişim alınamadı. ADAL Hatası: service_unavailable

- **Belirtiler**: Kopyalama etkinliği aşağıdaki hataile başarısız olur:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Neden**: Azure Active Directory'ye ait Hizmet Belirteç Sunucusu (STS) kullanılamadığında, yani istekleri işlemek için çok meşgulse, bir HTTP hatası 503 döndürür. 

- **Çözüm**: Kopyalama etkinliğini birkaç dakika sonra yeniden çalıştırın.
                  

## <a name="azure-sql-data-warehouseazure-sql-databasesql-server"></a>Azure SQL Veri Ambarı/Azure SQL Veritabanı/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Hata kodu: SqlFailedToConnect

- **Mesaj**:`Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Neden**: Hata iletisi "SqlException" içeriyorsa, SQL Veritabanı bazı belirli işlemin başarısız olduğunu belirten hatayı atar.

- **Öneri**: Daha fazla bilgi için bu başvuru https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errorsdokümanında LÜTFEN SQL hata koduna göre arama yapın: . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteğine başvurun.

- **Neden**: Hata iletisi "IP adresi '...' olan istemci yi içeriyorsa sunucuya erişmesine izin verilmez", ve Azure SQL Veritabanı'na bağlanmaya çalışıyorsunuz, genellikle bunun nedeni Azure SQL Veritabanı güvenlik duvarı sorunundan kaynaklanıyor.

- **Öneri**: Azure SQL Server güvenlik duvarı yapılandırmasında "Azure hizmetlerine ve kaynaklarına bu sunucuya erişmesine izin verin" seçeneğini etkinleştirin. Referans doc: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure.


### <a name="error-code--sqloperationfailed"></a>Hata kodu: SqlOperationFailed

- **Mesaj**:`A database operation failed. Please search error to get more details.`

- **Neden**: Hata iletisi "SqlException" içeriyorsa, SQL Veritabanı bazı belirli işlemin başarısız olduğunu belirten hatayı atar.

- **Öneri**: SQL hatası açık değilse, lütfen veritabanını en son uyumluluk düzeyi '150' olarak değiştirmeyi deneyin. En son sürüm SQL hataları atabilir. Lütfen ayrıntılı dokümanbakın: https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level?view=sql-server-ver15#backwardCompat.
        SQL sorunlarını gidermek için lütfen bu başvuru dokümanında daha https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errorsfazla ayrıntı için SQL hata koduna göre arama yapın: . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteğine başvurun.

- **Neden**: Hata iletisi "PdwManagedToNativeInteropException" içeriyorsa, genellikle kaynak ve lavabo sütun boyutları arasındaki uyumsuzlurluk neden olur.

- **Öneri**: Hem kaynak hem de lavabo sütunlarının boyutunu kontrol edin. Daha fazla yardıma ihtiyacınız varsa Azure SQL desteğine başvurun.

- **Neden**: Hata iletisi "Geçersiz Çalışma Özel Durum" içeriyorsa, genellikle geçersiz giriş verilerinden kaynaklanır.

- **Öneri**: Sorunla hangi satırın karşılaştığını belirlemek için, lütfen daha fazla araştırma için sorunlu satır(lar)ı depolamaalanına yönlendirebilecek kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Referans doc: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlunauthorizedaccess"></a>Hata kodu: SqlUnauthorizedAccess

- **Mesaj**:`Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Neden**: Kimlik bilgisi yanlıştır veya oturum açma hesabı SQL Veritabanı'na erişemez.

- **Öneri**: Sql Veritabanı'na erişmek için yeterli izne sahip olan giriş hesabını kontrol edin.


### <a name="error-code--sqlopenconnectiontimeout"></a>Hata kodu: SqlOpenConnectionTimeout

- **Mesaj**:`Open connection to database timeout after '%timeoutValue;' seconds.`

- **Neden**: SQL Veritabanı geçici hatası olabilir.

- **Öneri**: Lütfen bağlantılı servis bağlantı dizesini daha büyük bağlantı zaman değeriyle güncelleştirmeyi yeniden deneyin.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Hata kodu: SqlAutoCreateTableTypeMapFailed

- **Mesaj**:`Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **Neden**: Otomatik oluşturma tablosu kaynak gereksinimini karşılayamıyor.

- **Öneri**: 'Eşlemeler'deki sütun türünü güncelleştirin veya hedef sunucuda lavabo tablosunu el ile oluşturun.


### <a name="error-code--sqldatatypenotsupported"></a>Hata kodu: SqlDataTypeNotSupported

- **Mesaj**:`A database operation failed. Check the SQL errors.`

- **Neden**: Sorun SQL kaynağında gerçekleşirse ve hata SqlDateTime taşma ile ilgiliyse, veri değeri mantık türü aralığının üzerindedir (1/1/1753 12:00:00 - 12/31/9999 11:59:59 PM).

- **Öneri**: Kaynak SQL sorgusunda string'e yazıyı döküm veya kopya etkinliği sütun eşlemesinde sütun türünü 'String' olarak değiştirin.

- **Neden**: Sorun SQL lavaboda gerçekleşirse ve hata SqlDateTime taşmaile ilgiliyse, veri değeri lavabo tablosunda izin verilen aralığın üzerindedir.

- **Öneri**: İlgili sütun türünü lavabo tablosundaki 'datetime2' türüne güncelleştirin.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Hata kodu: SqlInvalidDbStoredProcedure

- **Mesaj**:`The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Neden**: Belirtilen Depolanan Yordam geçerli değildir. Depolanan yordamın herhangi bir veri döndürmemesi neden olabilir.

- **Öneri**: SQL Tools tarafından depolanan yordamı doğrulayın. Depolanan yordamın verileri döndürediğinden emin olun.


### <a name="error-code--sqlinvaliddbquerystring"></a>Hata kodu: SqlInvalidDbQueryString

- **Mesaj**:`The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Neden**: Belirtilen SQL Sorgusu geçerli değildir. Sorgunun herhangi bir veri döndürmemesi neden olabilir

- **Öneri**: SQL Sorgusunu SQL Tools ile doğrulayın. Sorgunun verileri döndürediğinden emin olun.


### <a name="error-code--sqlinvalidcolumnname"></a>Hata kodu: SqlInvalidColumnName

- **Mesaj**:`Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Neden**: Sütun bulamıyor. Olası yapılandırma yanlış.

- **Öneri**: Sorgudaki sütunu, veri kümesindeki 'yapı'yı ve etkinlikteki 'eşlemeleri' doğrulayın.


### <a name="error-code--sqlcolumnnamemismatchbycasesensitive"></a>Hata kodu: SqlColumnNameMismatchByCaseSensitive

- **Mesaj**:`Column '%column;' in DataSet '%dataSetName;' cannot be found in physical SQL Database. Column matching is case-sensitive. Column '%columnInTable;' appears similar. Check the DataSet(s) configuration to proceed further.`


### <a name="error-code--sqlbatchwritetimeout"></a>Hata kodu: SqlBatchWriteTimeout

- **Mesaj**:`Timeouts in SQL write operation.`

- **Neden**: SQL Veritabanı geçici hatası olabilir.

- **Öneri**: Lütfen yeniden deneyin. Sorun yeniden çoğaltılırsa Azure SQL desteğine başvurun.


### <a name="error-code--sqlbatchwritetransactionfailed"></a>Hata kodu: SqlBatchWriteTransactionFailed

- **Mesaj**:`SQL transaction commits failed`

- **Neden**: Özel durum ayrıntıları sürekli olarak işlem zaman ağını söylüyorsa, tümleştirme çalışma süresi ile veritabanı arasındaki ağ gecikmesi varsayılan eşikten 30 saniye olarak yüksektir.

- **Öneri**: Sql bağlantılı servis bağlantı dizesini 'bağlantı zaman amı' değeriyle güncelleştirin ve etkinliği 120 veya daha yüksek olarak yeniden çalıştırın.

- **Neden**: Özel durum ayrıntıları zaman zaman sqlconnection bozuk söylerseniz, sadece geçici ağ hatası veya SQL Veritabanı yan sorunu olabilir

- **Öneri**: Lütfen etkinliği yeniden deneyin ve SQL Veritabanı yan ölçümlerini gözden geçirin.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Hata kodu: SqlBulkCopyInvalidColumnLength

- **Mesaj**:`SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **Neden**: SQL Toplu Kopya bcp istemcisinden geçersiz bir sütun uzunluğu almak nedeniyle başarısız oldu.

- **Öneri**: Sorunla hangi satırın karşılaştığını belirlemek için, lütfen daha fazla araştırma için sorunlu satır(lar)ı depolamaalanına yönlendirebilecek kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Referans doc: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlconnectionisclosed"></a>Hata kodu: SqlConnectionIsClosed

- **Mesaj**:`The connection is closed by SQL Database.`

- **Neden**: Sql bağlantısı, yüksek eşzamanlı çalıştırma ve sunucu bağlantısını sonlandırdığinde SQL Veritabanı tarafından kapatılır.

- **Öneri**: Uzak sunucu SQL bağlantısını kapattı. Lütfen yeniden deneyin. Sorun yeniden çoğaltılırsa Azure SQL desteğine başvurun.


### <a name="error-code--sqlcreatetablefailedunsupportedtype"></a>Hata kodu: SqlCreateTableFailedUnsupportedType

- **Mesaj**:`Type '%type;' in source side cannot be mapped to a type that supported by sink side(column name:'%name;') in autocreate table.`


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Hata iletisi: Bir karakter dizesinden benzersiz tanımlayıcıya dönüştürme başarısız oldu

- **Belirtiler**: Aşamalı kopya ve PolyBase kullanarak veri kaynağından (SQL Server gibi) Azure SQL Veri Ambarı'na kopyaladiğinizde aşağıdaki hataya girersiniz:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Neden**: Azure SQL Veri Ambarı PolyBase boş dizeyi GUID'e dönüştüremez.

- **Çözünürlük**: Kopya etkinliği lavabo, Polybase ayarları altında,**"kullanım türü varsayılan**" seçeneğini false ayarlayın.

### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Hata iletisi: Beklenen veri türü: DECIMAL(x,x), Rahatsız edici değer

- **Belirtiler**: Aşamalı kopya ve PolyBase kullanarak veri kaynağından (SQL Server gibi) SQL DW'ye kopyaladiğinizde aşağıdaki hataya girersiniz:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Neden**: Azure SQL Veri Ambarı Çok temel ibare sütununa boş dize (null değer) ekleyemez.

- **Çözünürlük**: Kopya etkinliği lavabo, Polybase ayarları altında,**"kullanım türü varsayılan**" seçeneğini false ayarlayın.

### <a name="error-message-java-exception-messagehdfsbridgecreaterecordreader"></a>Hata iletisi: Java özel durum iletisi:HdfsBridge::CreateRecordReader

- **Belirtiler**: Verileri PolyBase kullanarak Azure SQL Veri Ambarı'na kopyalar sınız ve aşağıdaki hataya girersiniz:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Neden**: Olası neden şema (toplam sütun genişliği) çok büyük olmasıdır (1 MB'dan büyük). Tüm sütunların boyutunu ekleyerek hedef SQL DW tablosunun şema kontrol edin:

    - Int -> 4 bayt
    - Bigint -> 8 bayt
    - Varchar(n),char(n),ikili(n), varbinary(n) -> n bayt
    - Nvarchar(n), nchar(n) -> n*2 bayt
    - Tarih -> 6 bayt
    - Datetime/(2), smalldatetime -> 16 bayt
    - Datetimeoffset -> 20 bayt
    - Ondalık -> 19 bayt
    - Float -> 8 bayt
    - Para -> 8 bayt
    - Smallmoney -> 4 bayt
    - Gerçek -> 4 bayt
    - Smallint -> 2 bayt
    - Zaman -> 12 bayt
    - Tinyint -> 1 bayt

- **Çözünürlük**: Sütun genişliğini 1 MB'dan az olacak şekilde azaltın

- Veya Polybase devre dışı bırakarak toplu ekleme yaklaşımı kullanın

### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Hata iletisi: HTTP koşullu üstbilgi(ler) kullanılarak belirtilen koşul karşılanmadı

- **Belirtiler**: Azure SQL Veri Ambarı'ndan veri çekmek ve aşağıdaki hatayı vurmak için SQL sorgusunu kullanıyorsunuz:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Neden**: Azure SQL Veri Ambarı, Azure Depolama'daki dış tabloyu sorgulayan soruna isabet etti.

- **Çözünürlük**: Aynı sorguyu SSMS'te çalıştırın ve aynı sonucu görüp görmediğinizkontrol edin. Alıyorsanız sorun gidermek için Azure SQL Veri Ambarı için bir destek bileti açın ve SQL DW sunucunuzla veritabanınızın adını belirtin.
            

## <a name="delimited-text-format"></a>Sınırlı Metin Biçimi

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Hata kodu: DelimitedTextColumnNameNotAllowNull

- **Mesaj**:`The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Neden**: Etkinlikte 'firstRowAsHeader' ayarlandığında, ilk satır sütun adı olarak kullanılır. Bu hata, ilk satırBoş değer içerdiği anlamına gelir. Örneğin: 'ColumnA,ColumnB'.

- **Öneri**: İlk satırı işaretleyin ve boş değer varsa değeri düzeltin.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Hata kodu: DelimitedTextMoreColumnsThanDefined

- **Mesaj**:`Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %columnCount;.`

- **Neden**: Sorunlu satırın sütun sayısı, ilk satırın sütun sayısından büyüktür. Veri sorunu veya yanlış sütun delimiter/teklif char ayarlarını kaynaklanabilir.

- **Öneri**: Lütfen hata iletisindeki satır sayısını alın, satırın sütununa bakın ve verileri düzeltin.

- **Neden**: Beklenen sütun sayısı hata iletisinde "1" ise, ADF'nin dosyanızı (lar) yanlış ayrıştırmasına neden olan yanlış sıkıştırma veya biçim ayarlarını belirtmiş olabilirsiniz.

- **Öneri**: Kaynak dosya (lar) ile eşleştiğinden emin olmak için biçim ayarlarını kontrol edin.

- **Neden**: Kaynağınız bir klasörse, belirtilen klasörün altındaki dosyaların farklı şemaları olabilir.

- **Öneri**: Verilen klasörün altındaki dosyaların aynı şema olduğundan emin olun.


### <a name="error-code--delimitedtextincorrectrowdelimiter"></a>Hata kodu: DelimitedTextIncorrectRowDelimiter

- **Mesaj**:`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--delimitedtexttoolargecolumncount"></a>Hata kodu: DelimitedTextTooLargeColumnCount

- **Mesaj**:`Column count reaches limitation when deserializing csv file. Maximum size is '%size;'. Check the column delimiter and row delimiter provided. (Column delimiter: '%columnDelimiter;', Row delimiter: '%rowDelimiter;')`


### <a name="error-code--delimitedtextinvalidsettings"></a>Hata kodu: DelimitedTextInvalidSettings

- **Mesaj**:`%settingIssues;`



## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Hata kodu: DynamicsCreateServiceClientError

- **Mesaj**:`This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Neden**: Bu dinamikler sunucu tarafında geçici bir sorundur.

- **Öneri**: Boru hattını yeniden çalıştırın. Başarısız olmaya devam ederseniz, paralelliği azaltmaya çalışın. Hala başarısız olursa, lütfen dinamikler desteğine başvurun.



## <a name="json-format"></a>JSON Formatı

### <a name="error-code--jsoninvalidarraypathdefinition"></a>Hata kodu: JsonInvalidArrayPathDefinition

- **Mesaj**:`Error occurred when deserializing source JSON data. Check whether the JsonPath in JsonNodeReference and JsonPathDefintion is valid.`


### <a name="error-code--jsonemptyjobjectdata"></a>Hata kodu: JsonEmptyJObjectData

- **Mesaj**:`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--jsonnullvalueinpathdefinition"></a>Hata kodu: JsonNullValueInPathDefinition

- **Mesaj**:`Null JSONPath detected in JsonPathDefinition.`


### <a name="error-code--jsonunsupportedhierarchicalcomplexvalue"></a>Hata kodu: JsonUnsupportedHiyerarşikComplexValue

- **Mesaj**:`The retrieved type of data %data; with value %value; is not supported yet. Please either remove the targeted column '%name;' or enable skip incompatible row to skip the issue rows.`


### <a name="error-code--jsonconflictpartitiondiscoveryschema"></a>Hata kodu: JsonConflictPartitionDiscoverySchema

- **Mesaj**:`Conflicting partition column names detected.'%schema;', '%partitionDiscoverySchema;'`


### <a name="error-code--jsoninvaliddataformat"></a>Hata kodu: JsonInvalidDataFormat

- **Mesaj**:`Error occurred when deserializing source JSON file '%fileName;'. Check if the data is in valid JSON object format.`


### <a name="error-code--jsoninvaliddatamixedarrayandobject"></a>Hata kodu: JsonInvalidDataMixedArrayAndObject

- **Mesaj**:`Error occurred when deserializing source JSON file '%fileName;'. The JSON format doesn't allow mixed arrays and objects.`



## <a name="parquet-format"></a>Parke Formatı

### <a name="error-code--parquetjavainvocationexception"></a>Hata kodu: ParkquetJavaInvocationException

- **Mesaj**:`An error occurred when invoking java, message: %javaException;.`

- **Neden**: Hata iletisi 'java.lang.OutOfMemory', 'Java yığın alanı' ve 'doubleCapacity' içeriyorsa, genellikle tümleştirme çalışma zamanının eski sürümünde bir bellek yönetimi sorunudur.

- **Öneri**: Kendi kendine barındırılan Tümleştirme Çalışma Süresi kullanıyorsanız ve sürüm 3.20.7159.1'den daha erkenyse, en son sürüme yükseltmenizi öneririz.

- **Neden**: Hata iletisi 'java.lang.OutOfMemory' içeriyorsa, tümleştirme çalışma zamanı dosyayı işlemek için yeterli kaynağa sahip değildir.

- **Öneri**: Tümleştirme çalışma zamanında eşzamanlı çalıştırmaları sınırlayın. Kendi kendine barındırılan Tümleştirme Çalışma Süresi için, 8 GB'a eşit veya daha büyük belleklere sahip güçlü bir makineye ölçeklendirin.

- **Neden**: Hata iletisi 'NullPointerReference' içeriyorsa, bu geçici bir hata olabilir.

- **Öneri**: Lütfen yeniden deneyin. Sorun devam ederse, lütfen desteğe başvurun.


### <a name="error-code--parquetinvalidfile"></a>Hata kodu: ParkquetInvalidFile

- **Mesaj**:`File is not a valid parquet file.`

- **Neden**: Parke dosyası sorunu.

- **Öneri**: Girişi kontrol edin geçerli bir parke dosyasıdır.


### <a name="error-code--parquetnotsupportedtype"></a>Hata kodu: ParkquetNotSupportedType

- **Mesaj**:`Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **Neden**: Parke biçimi Azure Veri Fabrikası'nda desteklenmez.

- **Öneri**: Kaynak verileri iki kez kontrol edin. Dokümana bakın: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>Hata kodu: ParkquetMissedDecimalPrecisionScale

- **Mesaj**:`Decimal Precision or Scale information is not found in schema for column: %column;.`

- **Neden**: Sayı kesinliğini ve ölçeğiayrıştırmaya çalışın, ancak böyle bir bilgi sağlanmaz.

- **Öneri**: 'Kaynak' doğru Hassas ve ölçek döndürmez. Sorun sütunu kesinliğini ve ölçeğini denetleyin.


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>Hata kodu: ParkquetInvalidDecimalPrecisionScale

- **Mesaj**:`Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **Neden**: Şema geçersizdir.

- **Öneri**: Sorun sütununa kesinlik ve ölçek denetimi.


### <a name="error-code--parquetcolumnnotfound"></a>Hata kodu: ParkquetColumnNotFound

- **Mesaj**:`Column %column; does not exist in Parquet file.`

- **Neden**: Kaynak şeması lavabo şeması ile uyumsuzluktur.

- **Öneri**: 'etkinlik'teki 'eşlemeleri' kontrol edin. Kaynak sütunun sağ lavabo sütununa eşlenebileceğinden emin olun.


### <a name="error-code--parquetinvaliddataformat"></a>Hata kodu: ParkquetInvalidDataFormat

- **Mesaj**:`Incorrect format of %srcValue; for converting to %dstType;.`

- **Neden**: Veriler mappings.source'da belirtilen türe dönüştürülemez.

- **Öneri**: Kaynak verileri çift olarak denetleyin veya kopyalama etkinliği sütun eşlemesinde bu sütun için doğru veri türünü belirtin. Dokümana bakın: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>Hata kodu: ParkquetDataCountNotMatchColumnCount

- **Mesaj**:`The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **Neden**: Kaynak sütun sayısı ve lavabo sütun sayısı uyuşmazlığı

- **Öneri**: Çift onay kaynak sütun sayısı 'haritalama'daki lavabo sütun sayısıyla aynıdır.


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>Hata kodu: ParkquetDataTypeNotMatchColumnType

- **İleti**: Veri türü %srcType; verilen sütun türü %dstType eşleşmez; sütununda '%columnIndex;'.

- **Neden**: Kaynaktan gelen veriler lavaboda tanımlanan dakti-sid'ye dönüştürülemez

- **Öneri**: Lütfen mapping.sink'te doğru bir tür belirtin.


### <a name="error-code--parquetbridgeinvaliddata"></a>Hata kodu: ParkquetBridgeInvalidData

- **Mesaj**:`%message;`

- **Neden**: Sınırlama üzerinden veri değeri

- **Öneri**: Lütfen yeniden deneyin. Sorun devam ederse, lütfen bize ulaşın.


### <a name="error-code--parquetunsupportedinterpretation"></a>Hata kodu: ParquetUnsupportedInterpretation

- **Mesaj**:`The given interpretation '%interpretation;' of parquet format is not supported.`

- **Neden**: Desteklenmeyen senaryo

- **Öneri**: 'ParkquetInterpretFor' 'sparkSql' olmamalıdır.


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>Hata kodu: ParkquetUnsupportFileLevelCompressionOption

- **Mesaj**:`File level compression is not supported for Parquet.`

- **Neden**: Desteklenmeyen senaryo

- **Öneri**: Yükteki 'Sıkıştırma Tipi'ni kaldırın.



## <a name="general-copy-activity-error"></a>Genel Kopyalama Etkinlik Hatası

### <a name="error-code--jrenotfound"></a>Hata kodu: JreNotFound

- **Mesaj**:`Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Neden**: Kendi kendine barındırılan tümleştirme çalışma zamanı Java Runtime'ı bulamıyor. Java Runtime belirli bir kaynak okumak için gereklidir.

- **Öneri**: Entegrasyon çalışma zamanı ortamınızı, başvuru dokümanını kontrol edin:https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>Hata kodu: WildcardPathSinkNotSupported

- **Mesaj**:`Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Neden**: Sink dataset joker karakter desteklemez.

- **Öneri**: Lavabo veri kümesini kontrol edin ve joker karakter değeri olmadan yolu düzeltin.


### <a name="error-code--mappinginvalidpropertywithemptyvalue"></a>Hata kodu: MappingInvalidPropertyWithEmptyValue

- **Mesaj**:`One or more '%sourceOrSink;' in copy activity mapping doesn't point to any data. Choose one of the three properties 'name', 'path' and 'ordinal' to reference columns/fields.`


### <a name="error-code--mappinginvalidpropertywithnamepathandordinal"></a>Hata kodu: MappingInvalidPropertyWithNamePathAndOrdinal

- **Mesaj**:`Mixed properties are used to reference '%sourceOrSink;' columns/fields in copy activity mapping. Please only choose one of the three properties 'name', 'path' and 'ordinal'. The problematic mapping setting is 'name': '%name;', 'path': '%path;','ordinal': '%ordinal;'.`


### <a name="error-code--mappingduplicatedordinal"></a>Hata kodu: MappingDuplicatedOrdinal

- **Mesaj**:`Copy activity 'mappings' has duplicated ordinal value "%Ordinal;". Fix the setting in 'mappings'.`


### <a name="error-code--mappinginvalidordinalforsinkcolumn"></a>Hata kodu: MappingInvalidOrdinalForSinkColumn

- **Mesaj**:`Invalid 'ordinal' property for sink column under 'mappings' property. Ordinal: %Ordinal;.`


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme yardımı için şu kaynakları deneyin:

*  [Veri Fabrikası günlüğü](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Veri Fabrikası özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Veri Fabrikası için Yığın Taşma forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Veri Fabrikası hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
            
