---
title: Azure Data Factory bağlayıcı sorunlarını giderme
description: Azure Data Factory 'deki bağlayıcı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 12/09/2020
ms.author: jingwang
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: a7a81a742922d45be965c7f73e3cb910d0ef989a
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97109304"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Azure Data Factory bağlayıcı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki bağlayıcılar için genel sorun giderme yöntemleri incelenmektedir.
  
## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="error-code--azurebloboperationfailed"></a>Hata kodu: AzureBlobOperationFailed

- **İleti**: `Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Neden**: BLOB depolama işlemi isabet sorunu.

- **Öneri**: ayrıntıdaki hatayı denetleyin. Blob yardım belgesine başvurun: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes . Yardım gerekirse depolama ekibine başvurun.


### <a name="error-code--azureblobservicenotreturnexpecteddatalength"></a>Hata kodu: AzureBlobServiceNotReturnExpectedDataLength

- **İleti**: `Error occurred when trying to fetch the blob '%name;'. This could be a transient issue and you may rerun the job. If it fails again continuously, contact customer support.`


### <a name="error-code--azureblobnotsupportmultiplefilesintosingleblob"></a>Hata kodu: AzureBlobNotSupportMultipleFilesIntoSingleBlob

- **İleti**: `Transferring multiple files into a single Blob is not supported. Currently only single file source is supported.`


### <a name="error-code--azurestorageoperationfailedconcurrentwrite"></a>Hata kodu: AzureStorageOperationFailedConcurrentWrite

- **İleti**: `Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="invalid-property-during-copy-activity"></a>Kopyalama etkinliği sırasında geçersiz özellik

- **İleti**:  `Copy activity <Activity Name> has an invalid "source" property. The source type is not compatible with the dataset <Dataset Name> and its linked service <Linked Service Name>. Please verify your input against.`

- **Neden**: veri kümesinde tanımlanan tür, kopyalama etkinliğinde tanımlanan kaynak/havuz türüyle tutarsız.

- **Çözüm**: türleri tutarlı hale getirmek ve dağıtımı yeniden çalıştırmak için veri kümesini veya Işlem hattı JSON tanımını düzenleyin.


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

- **İleti**: `ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Neden**: ADLS 2. işlem başarısız olduğunu gösteren hatayı oluşturur.

- **Öneri**: ADLS 2. tarafından oluşturulan ayrıntılı hata iletisini denetleyin. Geçici bir hata nedeniyle, lütfen yeniden deneyin. Daha fazla yardıma ihtiyacınız varsa lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.

- **Neden**: hata Iletisi ' yasak ' içerdiğinde, kullandığınız hizmet sorumlusu veya yönetilen kimlik ADLS 2. erişmek için yeterli izne sahip olmayabilir.

- **Öneri**: yardım belgesine bakın: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication .

- **Neden**: hata Iletisi ' ınternalservererror ' içerdiğinde hata ADLS 2. tarafından döndürülür.

- **Öneri**: geçici bir hatadan kaynaklanıyor olabilir, lütfen yeniden deneyin. Sorun devam ederse lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.


### <a name="error-code--adlsgen2invalidurl"></a>Hata kodu: AdlsGen2InvalidUrl

- **İleti**: `Invalid url '%url;' provided, expecting http[s]://<accountname>.dfs.core.windows.net.`


### <a name="error-code--adlsgen2invalidfolderpath"></a>Hata kodu: AdlsGen2InvalidFolderPath

- **İleti**: `The folder path is not specified. Cannot locate the file '%name;' under the ADLS Gen2 account directly. Please specify the folder path instead.`


### <a name="error-code--adlsgen2operationfailedconcurrentwrite"></a>Hata kodu: AdlsGen2OperationFailedConcurrentWrite

- **İleti**: `Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="error-code-adlsgen2timeouterror"></a>Hata kodu: AdlsGen2TimeoutError

- **İleti**: `Request to ADLS Gen2 account '%account;' met timeout error. It is mostly caused by the poor network between the Self-hosted IR machine and the ADLS Gen2 account. Check the network to resolve such error.`


### <a name="request-to-adls-gen2-account-met-timeout-error"></a>ADLS 2. hesabı için istek zaman aşımı hatası oluştu

- **İleti**: hata kodu = `UserErrorFailedBlobFSOperation` , hata iletisi = `BlobFS operation failed for: A task was canceled` .

- **Neden**: sorun, genellikle şirket IÇINDE barındırılan IR makinesinde gerçekleşen ADLS 2. havuz zaman aşımı hatası nedeniyle oluşur.

- **Öneri**: 

    1. Şirket içinde barındırılan IR makinenizi ve hedef ADLS 2. hesabınızı mümkünse aynı bölgeye yerleştirin. Bu, rastgele zaman aşımı hatasından kaçınmanıza ve daha iyi performans sağlayabilir.

    1. ExpressRoute gibi özel bir ağ ayarı olup olmadığını denetleyin ve ağın yeterli bant genişliğine sahip olduğundan emin olun. Genel bant genişliği düşük olduğunda şirket içinde barındırılan IR eş zamanlı işleri ayarının daha az olması önerilir, bu da birden çok eş zamanlı iş genelinde ağ kaynağı yarışmasını önleyebilir.

    1. Dosya boyutu orta veya küçük olduğunda bu zaman aşımı hatasını azaltmak için ikili olmayan kopya için daha küçük blok boyutu kullanın. Lütfen [BLOB depolama yerleştirme bloğuna](https://docs.microsoft.com/rest/api/storageservices/put-block)bakın.

       Özel blok boyutunu belirtmek için,. JSON düzenleyicisinde özelliği düzenleyebilirsiniz:
    ```
    "sink": {
        "type": "DelimitedTextSink",
        "storeSettings": {
            "type": "AzureBlobFSWriteSettings",
            "blockSizeInMB": 8
        }
    }
    ```


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-underlying-connection-was-closed-could-not-establish-trust-relationship-for-the-ssltls-secure-channel"></a>Hata iletisi: temel alınan bağlantı kapatıldı: SSL/TLS güvenli kanalı için güven ilişkisi kurulamadı.

- **Belirtiler**: kopyalama etkinliği aşağıdaki hatayla başarısız olur: 

    ```
    Message: Failure happened on 'Sink' side. ErrorCode=UserErrorFailedFileOperation,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Upload file failed at path STAGING/PLANT/INDIARENEWABLE/LiveData/2020/01/14\\20200114-0701-oem_gibtvl_mannur_data_10min.csv.,Source=Microsoft.DataTransfer.ClientLibrary,''Type=System.Net.WebException,Message=The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.,Source=System,''Type=System.Security.Authentication.AuthenticationException,Message=The remote certificate is invalid according to the validation procedure.,Source=System,'.
    ```

- **Neden**: TLS el sıkışması sırasında sertifika doğrulaması başarısız oldu.

- **Çözüm**: geçici çözüm: ADLS 1. için TLS doğrulamasını atlamak üzere hazırlanan kopyayı kullanın. Bu sorunu yeniden oluşturup Netmon Trace 'i toplamanız ve sonra yerel ağ yapılandırmasını denetlemek için ağ ekibinize de sahip olmanız gerekir.

    ![ADLS 1. sorunlarını giderme](./media/connector-troubleshoot-guide/adls-troubleshoot.png)


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
                  

## <a name="azure-synapse-analyticsazure-sql-databasesql-server"></a>Azure SYNAPSE Analytics/Azure SQL veritabanı/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Hata kodu: SqlFailedToConnect

- **İleti**: `Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: daha fazla ayrıntı için bu başvuru BELGESI içindeki SQL hata kodu ile ara: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata ILETISI "IP adresi olan istemci '... ' içeriyorsa) sunucusuna erişim izni verilmez "ve Azure SQL veritabanı 'na bağlanmaya çalışıyorsunuz, genellikle Azure SQL veritabanı güvenlik duvarı sorunu nedeniyle oluşur.

- **Öneri**: mantıksal SQL Server güvenlik duvarı yapılandırması ' nda, "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine izin ver" seçeneğini etkinleştirin. Başvuru belgesi: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure .


### <a name="error-code--sqloperationfailed"></a>Hata kodu: SqlOperationFailed

- **İleti**: `A database operation failed. Please search error to get more details.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: SQL hatası net değilse, lütfen veritabanını en son uyumluluk düzeyi olan ' 150 ' olarak değiştirmeyi deneyin. En son sürüm SQL hatalarını oluşturabilir. [Belge ayrıntısı](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level#backwardCompat)bölümüne bakın.

    SQL sorunlarını gidermek için lütfen daha fazla ayrıntı için bu başvuru belgesi içindeki SQL hata kodu ile arama yapın: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata Iletisi "PdwManagedToNativeInteropException" içeriyorsa, genellikle kaynak ve havuz sütun boyutları arasında uyuşmazlık olur.

- **Öneri**: hem kaynak hem de havuz sütunlarının boyutunu denetleyin. Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: hata Iletisi "InvalidOperationException" içeriyorsa, genellikle geçersiz giriş verileri oluşur.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance .



### <a name="error-code--sqlunauthorizedaccess"></a>Hata kodu: SqlUnauthorizedAccess

- **İleti**: `Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Neden**: kimlik bilgisi yanlış veya oturum açma hesabı SQL veritabanına erişemiyor.

- **Öneri**: oturum açma hesabının SQL veritabanına erişmek için yeterli izni olup olmadığını denetleyin.


### <a name="error-code--sqlopenconnectiontimeout"></a>Hata kodu: SqlOpenConnectionTimeout

- **İleti**: `Open connection to database timeout after '%timeoutValue;' seconds.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: bağlı hizmet bağlantı dizesini daha büyük bağlantı zaman aşımı değeri ile güncelleştirmeyi yeniden deneyin.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Hata kodu: SqlAutoCreateTableTypeMapFailed

- **İleti**: `Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **Neden**: otomatik oluşturma tablosu kaynak gereksinimini karşılamaz.

- **Öneri**: ' eşlemeler ' içindeki sütun türünü güncelleştirin veya hedef sunucuda havuz tablosunu el ile oluşturun.


### <a name="error-code--sqldatatypenotsupported"></a>Hata kodu: SqlDataTypeNotSupported

- **İleti**: `A database operation failed. Check the SQL errors.`

- **Neden**: sorun SQL kaynağında olursa ve hata SqlDateTime overflow ile ilgiliyse, veri değeri Logic Type aralığı üzerinden (1/1/1753 12:00:00 ÖÖ-12/31/9999 11:59:59 PM) oluşur.

- **Öneri**: türü kaynak SQL sorgusunda dizeye atayın veya kopyalama etkinliği sütun eşlemesinde sütun türünü ' String ' olarak değiştirin.

- **Neden**: sorun SQL havuzunda gerçekleşirse ve hata SqlDateTime overflow ile ilgiliyse, veri değeri havuz tablosu 'nda izin verilen aralığın üzerinde olur.

- **Öneri**: havuz tablosunda karşılık gelen sütun türü ' datetime2 ' türüne güncelleştirin.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Hata kodu: SqlInvalidDbStoredProcedure

- **İleti**: `The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Neden**: belirtilen saklı yordam geçerli değil. Bu, saklı yordamın herhangi bir veri döndürmemesinin nedeni olabilir.

- **Öneri**: SAKLı yordamı SQL araçları ile doğrulayın. Saklı yordamın verileri döndüre, emin olun.


### <a name="error-code--sqlinvaliddbquerystring"></a>Hata kodu: SqlInvalidDbQueryString

- **İleti**: `The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Neden**: belirtilen SQL sorgusu geçerli değil. Sorgunun herhangi bir veri döndürmediğinden kaynaklanabilir

- **Öneri**: SQL sorgusunu SQL araçları ile doğrulayın. Sorgunun verileri döndüre, emin olun.


### <a name="error-code--sqlinvalidcolumnname"></a>Hata kodu: SqlInvalidColumnName

- **İleti**: `Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Neden**: sütun bulunamıyor. Olası yapılandırma yanlış.

- **Öneri**: sorgudaki sütunu, veri kümesindeki ' Structure ' öğesini ve etkinlik içindeki ' eşlemeler ' doğrulamasını doğrulayın.


### <a name="error-code--sqlcolumnnamemismatchbycasesensitive"></a>Hata kodu: SqlColumnNameMismatchByCaseSensitive

- **İleti**: `Column '%column;' in DataSet '%dataSetName;' cannot be found in physical SQL Database. Column matching is case-sensitive. Column '%columnInTable;' appears similar. Check the DataSet(s) configuration to proceed further.`


### <a name="error-code--sqlbatchwritetimeout"></a>Hata kodu: SqlBatchWriteTimeout

- **İleti**: `Timeouts in SQL write operation.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: yeniden dene. Sorun yeniden üretme varsa Azure SQL desteği 'ne başvurun.


### <a name="error-code--sqlbatchwritetransactionfailed"></a>Hata kodu: SqlBatchWriteTransactionFailed

- **İleti**: `SQL transaction commits failed`

- **Neden**: özel durum ayrıntıları işlem zaman aşımını sürekli söyledi, tümleştirme çalışma zamanı ve veritabanı arasındaki ağ gecikmesi varsayılan eşikten 30 saniye daha yüksektir.

- **Öneri**: SQL bağlantılı hizmet bağlantı dizesini ' bağlantı zaman aşımı ' değeri 120 veya üzeri olarak güncelleştirin ve etkinliği yeniden çalıştırın.

- **Neden**: özel durum ayrıntıları SqlConnection 'ı kopuk olarak söylüyor, yalnızca geçici ağ ARıZASı veya SQL veritabanı tarafı sorunu olabilir

- **Öneri**: etkinliği yeniden deneyın ve SQL veritabanı tarafı ölçümlerini inceleyin.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Hata kodu: Sqlbulkcopyınvalidcolumnlength

- **İleti**: `SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **Neden**: bcp istemcisinden geçersiz bir sütun uzunluğu aldığından SQL toplu kopyalama başarısız oldu.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance .


### <a name="error-code--sqlconnectionisclosed"></a>Hata kodu: Sqlconnectionısclosed

- **İleti**: `The connection is closed by SQL Database.`

- **Neden**: yüksek eşzamanlı çalıştırma ve sunucu bağlantısı sona erdiğinde SQL bağlantısı SQL veritabanı tarafından kapalıdır.

- **Öneri**: uzak sunucu SQL bağlantısını kapattı. Retry. Sorun yeniden üretme varsa Azure SQL desteği 'ne başvurun.


### <a name="error-code--sqlcreatetablefailedunsupportedtype"></a>Hata kodu: SqlCreateTableFailedUnsupportedType

- **İleti**: `Type '%type;' in source side cannot be mapped to a type that supported by sink side(column name:'%name;') in autocreate table.`


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Hata iletisi: bir karakter dizesinden uniqueidentifier öğesine dönüştürülürken dönüştürme başarısız oldu

- **Belirtiler**: bir tablo veri kaynağından (örneğin, SQL Server) verileri, hazırlanan kopya ve PolyBase kullanarak Azure SYNAPSE Analytics 'e kopyaladığınızda, şu hatayı alırsınız:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Neden**: Azure SYNAPSE Analytics PolyBase boş dizeyi GUID 'ye dönüştüremiyor.

- **Çözüm**: kopyalama etkinliği havuzunda, PolyBase ayarları altında "**tür Varsayılanı kullan**" seçeneğini false olarak ayarlayın.


### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Hata iletisi: beklenen veri türü: DECIMAL (x, x), sorunlu değer

- **Belirtiler**: bir tablo veri kaynağından (örneğin, SQL Server) verileri, hazırlanan kopya ve PolyBase kullanarak Azure SYNAPSE Analytics 'e kopyaladığınızda, şu hatayı alırsınız:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Neden**: Azure SYNAPSE Analytics PolyBase, decimal sütununa boş dize (null değer) ekleyemiyor.

- **Çözüm**: kopyalama etkinliği havuzunda, PolyBase ayarları altında "**tür Varsayılanı kullan**" seçeneğini false olarak ayarlayın.


### <a name="error-message-java-exception-message-hdfsbridgecreaterecordreader"></a>Hata iletisi: Java özel durum iletisi: HdfsBridge:: CreateRecordReader

- **Belirtiler**: PolyBase kullanarak Azure SYNAPSE Analytics 'e veri kopyalama ve aşağıdaki hatayı isabet alırsınız:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Neden**: olası neden şemanın (Toplam sütun genişliği) çok büyük (1 MB 'den büyük) olması olabilir. Tüm sütunların boyutunu ekleyerek hedef Azure SYNAPSE Analytics tablosunun şemasını denetleyin:

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

- **Belirtiler**: SQL sorgusunu kullanarak Azure SYNAPSE Analytics 'ten veri çekin ve şu hatayı alırsınız:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Neden**: Azure SYNAPSE Analytics 'ten dış tabloyu sorgulama sorunu.

- **Çözüm**: aynı sorguyu SSMS 'de çalıştırın ve aynı sonucu görüp gör, kontrol edin. Yanıt Evet ise, Azure SYNAPSE Analytics 'e yönelik bir destek bileti açın ve daha fazla sorun gidermek için Azure SYNAPSE Analytics sunucunuzu ve veritabanı adınızı sağlayın.
            

### <a name="low-performance-when-load-data-into-azure-sql"></a>Azure SQL 'e veri yüklerken düşük performans

- **Belirtiler**: VERILERI Azure SQL 'e kopyalama yavaş olur.

- **Neden**: sorunun kök nedeni çoğunlukla Azure SQL tarafında performans sorunu tetikleniyor. Olası bazı nedenler şunlardır:

    1. Azure DB katmanı yeterince yüksek değil.

    1. Azure DB DTU kullanımı %100 ' e yakın. [Performansı izleyebilir](https://docs.microsoft.com/azure/azure-sql/database/monitor-tune-overview) ve DB katmanını yükseltmeyi göz önünde bulundurun.

    1. Dizinler düzgün ayarlanmadı. Lütfen veri yüklemeden önce tüm dizinleri kaldırın ve yükleme tamamlandıktan sonra yeniden oluşturun.

    1. WriteBatchSize, şema satır boyutuna sığacak kadar büyük değil. Lütfen sorunun özelliğini genişletmenize çalışın.

    1. Toplu iç içe değil, saklı yordam kullanılıyor, bu da daha kötü performansa sahip olması beklenir. 

- **Çözüm**: [kopyalama etkinliği performansı](https://docs.microsoft.com/azure/data-factory/copy-activity-performance-troubleshooting) için lütfen TSG 'ye başvurun


### <a name="performance-tier-is-low-and-leads-to-copy-failure"></a>Performans katmanı düşük ve kopyalamanın başarısız olmasına yol açar

- **Belirtiler**: Azure SQL 'e veri kopyalanırken aşağıdaki hata iletisi oluştu: `Database operation failed. Error message from database execution : ExecuteNonQuery requires an open and available Connection. The connection's current state is closed.`

- **Neden**: Azure SQL S1 kullanılıyor, bu durum bu durumda GÇ sınırlarına ulaştı.

- **Çözüm**: sorunu çözmek Için lütfen Azure SQL performans katmanını yükseltin. 


### <a name="sql-table-cannot-be-found"></a>SQL tablosu bulunamıyor 

- **Belirtiler**: veriler karma 'ten şirket içi SQL Server tablosuna kopyalanırken hata oluştu:`Cannot find the object "dbo.Contoso" because it does not exist or you do not have permissions.`

- **Neden**: geçerli SQL hesabının .net SqlBulkCopy. WriteToServer tarafından verilen istekleri yürütmek için yeterli izni yok.

- **Çözüm**: lütfen daha AYRıCALıKLı bir SQL hesabına geçin.


### <a name="string-or-binary-data-would-be-truncated"></a>Dize veya ikili veriler kesilecek

- **Belirtiler**: veriler şirket Içi/Azure SQL Server tablosuna kopyalanırken hata oluştu: 

- **Neden**: CX SQL tablo şeması tanımında beklentiden daha az uzunluğa sahip bir veya daha fazla sütun vardır.

- **Çözüm**: sorunu çözmek için lütfen aşağıdaki adımları deneyin:

    1. Hangi satırlardaki soruna sahip olan sorunları gidermek için, özellikle "Redirectıncompatiblerowsettings" [hata toleransını](https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance)uygulayın.

    1. Hangi sütunların güncelleştirileceğini görmek için SQL tablo şeması sütun uzunluğu ile yeniden yönlendirilen verileri iki kez kontrol edin.

    1. Tablo şemasını buna göre güncelleştirin.


## <a name="delimited-text-format"></a>Sınırlandırılmış metin biçimi

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Hata kodu: Delimitedtextcolumnsüs Otallownull

- **İleti**: `The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Neden**: etkinlikte ' firstRowAsHeader ' ayarlandığında, ilk satır sütun adı olarak kullanılır. Bu hata, ilk satırın boş değer içerdiği anlamına gelir. Örneğin: ' ColumnA, ColumnB '.

- **Öneri**: ilk satırı denetleyin ve boş değer varsa değeri düzeltir.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Hata kodu: Delimitedtextmorecolumnsıısdefined tanımlandı

- **İleti**: `Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %columnCount;.`

- **Neden**: sorunlu satırın sütun sayısı, ilk satırın sütun sayısından daha büyük. Bu durum, veri sorunu ya da yanlış sütun sınırlayıcısı/quote char ayarlarından kaynaklanıyor olabilir.

- **Öneri**: hata iletisinde satır sayısını alın, satırın sütununu kontrol edin ve verileri onarın.

- **Neden**: beklenen sütun sayısı hata iletisinde "1" ise yanlış sıkıştırma veya Biçim ayarları belirttiniz olabilir. Bu nedenle, ADF Dosyanızı yanlış bir şekilde ayrıştırdı.

- **Öneri**: kaynak dosya (lar) ile eşleştiğinden emin olmak için biçim ayarlarını kontrol edin.

- **Neden**: kaynağınız bir klasörysa, belirtilen klasörde bulunan dosyaların farklı bir şeması olması mümkündür.

- **Öneri**: belirtilen klasörde bulunan dosyaların aynı şemaya sahip olduğundan emin olun.


### <a name="error-code--delimitedtextincorrectrowdelimiter"></a>Hata kodu: DelimitedTextIncorrectRowDelimiter

- **İleti**: `The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--delimitedtexttoolargecolumncount"></a>Hata kodu: DelimitedTextTooLargeColumnCount

- **İleti**: `Column count reaches limitation when deserializing csv file. Maximum size is '%size;'. Check the column delimiter and row delimiter provided. (Column delimiter: '%columnDelimiter;', Row delimiter: '%rowDelimiter;')`


### <a name="error-code--delimitedtextinvalidsettings"></a>Hata kodu: DelimitedTextInvalidSettings

- **İleti**: `%settingIssues;`



## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Hata kodu: DynamicsCreateServiceClientError

- **İleti**: `This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Neden**: Bu, Dynamics Server tarafında geçici bir sorundur.

- **Öneri**: işlem hattını yeniden çalıştırın. Başarısız olursa paralelliği azaltmayı deneyin. Hala başarısız olduysa, lütfen Dynamics destek birimine başvurun.


### <a name="columns-are-missing-when-previewingimporting-schema"></a>Şema önizleniyor/içeri aktarılırken sütunlar eksik

- **Belirtiler**: şemayı içeri aktarırken veya verileri önizlerken bazı sütunlardan bazıları eksik olur. Hata iletisi: `The valid structure information (column name and type) are required for Dynamics source.`

- **Neden**: ADF, ilk 10 kayıtta hiçbir değere sahip olmayan sütunları gösteremediğinden bu sorun temel olarak tasarıma göre yapılır. Lütfen eklediğiniz sütunların doğru biçimde olduğundan emin olun. 

- **Öneri**: eşleme sekmesine sütunları el ile ekleyin.


## <a name="excel-format"></a>Excel biçimi

### <a name="timeout-or-slow-performance-when-parsing-large-excel-file"></a>Büyük Excel dosyası ayrıştırılırken zaman aşımı veya yavaş performans

- **Belirtiler**:

    1. Excel veri kümesi oluşturduğunuzda ve şemayı bağlantı/depolama alanından içeri aktardığınızda, verileri Önizle, çalışma sayfalarını Listele veya Yenile, Excel dosyası boyutta büyükse zaman aşımı hatası ile karşılaşabilirsiniz.

    1. Büyük Excel dosyasından (>= 100MB) verileri başka veri deposuna kopyalamak için kopyalama etkinliğini kullandığınızda, yavaş performans veya OOM sorunu yaşayabilirsiniz.

- **Neden**: 

    1. Excel veri kümesindeki şemayı içeri aktarma, verileri görüntüleme ve çalışma sayfalarını listeleme gibi işlemler için zaman aşımı, 100s ve statiktir. Büyük Excel dosyası için, bu işlemler zaman aşımı değeri içinde bitmeyebilir.

    2. ADF kopyalama etkinliği tüm Excel dosyasını belleğe okur ve verileri okumak için belirtilen çalışma sayfasını ve hücreleri bulur. Bu davranış, temel alınan ADF tarafından kullanılan SDK 'nın kullanımından kaynaklanır.

- **Çözüm**: 

    1. Şemayı içeri aktarmak için, özgün dosyanın bir alt kümesi olan daha küçük bir örnek dosya oluşturabilir ve "şemayı bağlantı/depodan içeri aktar" yerine "örnek dosyadan şemayı içeri aktar" seçeneğini belirleyebilirsiniz.

    2. Çalışma sayfası listesinde, çalışma sayfası açılan menüsünde "Düzenle" düğmesine tıklayabilir ve bunun yerine sayfa adını/dizinini girebilirsiniz.

    3. Büyük Excel dosyasını (>100MB) başka bir depoya kopyalamak için, spor akışı okuma ve daha iyi şekilde gerçekleştirdiğiniz veri akışı Excel kaynağını kullanabilirsiniz.


## <a name="hdinsight"></a>HDInsight

### <a name="ssl-error-when-adf-linked-service-using-hdinsight-esp-cluster"></a>HDInsight ESP kümesi kullanılarak ADF bağlantılı hizmet sırasında SSL hatası

- **İleti**: `Failed to connect to HDInsight cluster: 'ERROR [HY000] [Microsoft][DriverSupport] (1100) SSL certificate verification failed because the certificate is missing or incorrect.`

- **Neden**: sorun büyük olasılıkla sistem güven deposuyla ilgilidir.

- **Çözüm**: ayarı değiştirmek Için **Microsoft Integration Runtime\4.0\Shared\ODBC DRIVERS\MICROSOFT Hive ODBC driver\lib** yoluna gidebilir ve DriverConfiguration64.exe açabilirsiniz.

    ![Sistem güven deposunun kullan işaretini kaldır](./media/connector-troubleshoot-guide/system-trust-store-setting.png)


## <a name="json-format"></a>JSON biçimi

### <a name="error-code--jsoninvalidarraypathdefinition"></a>Hata kodu: Jsonınvalidarraypathdefinition

- **İleti**: `Error occurred when deserializing source JSON data. Check whether the JsonPath in JsonNodeReference and JsonPathDefintion is valid.`


### <a name="error-code--jsonemptyjobjectdata"></a>Hata kodu: JsonEmptyJObjectData

- **İleti**: `The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--jsonnullvalueinpathdefinition"></a>Hata kodu: Jsonnullvalueınpathdefinition

- **İleti**: `Null JSONPath detected in JsonPathDefinition.`


### <a name="error-code--jsonunsupportedhierarchicalcomplexvalue"></a>Hata kodu: JsonUnsupportedHierarchicalComplexValue

- **İleti**: `The retrieved type of data %data; with value %value; is not supported yet. Please either remove the targeted column '%name;' or enable skip incompatible row to skip the issue rows.`


### <a name="error-code--jsonconflictpartitiondiscoveryschema"></a>Hata kodu: JsonConflictPartitionDiscoverySchema

- **İleti**: `Conflicting partition column names detected.'%schema;', '%partitionDiscoverySchema;'`


### <a name="error-code--jsoninvaliddataformat"></a>Hata kodu: Jsonınvaliddataformat

- **İleti**: `Error occurred when deserializing source JSON file '%fileName;'. Check if the data is in valid JSON object format.`


### <a name="error-code--jsoninvaliddatamixedarrayandobject"></a>Hata kodu: Jsonınvaliddatamixedarrayandobject

- **İleti**: `Error occurred when deserializing source JSON file '%fileName;'. The JSON format doesn't allow mixed arrays and objects.`


## <a name="oracle"></a>Oracle

### <a name="error-code-argumentoutofrangeexception"></a>Hata kodu: ArgumentOutOfRangeException

- **İleti**: `Hour, Minute, and Second parameters describe an un-representable DateTime.`

- **Neden**: ADF 'de, tarih saat değerleri 0001-01-01 00:00:00 ile 9999-12-31 23:59:59 arasında desteklenir. Ancak, Oracle, ADF 'de hataya neden olan BC yüzyıl veya min/sn>59) daha geniş aralıktaki DateTime değerlerini destekler.

- **Öneri**: 

    `select dump(<column name>)`Oracle 'daki DEĞERIN ADF 'nin aralığında olup olmadığını kontrol etmek için lütfen öğesini çalıştırın. 

    Sonuç olarak bayt sırasını biliyorsanız lütfen kontrol edin https://stackoverflow.com/questions/13568193/how-are-dates-stored-in-oracle .


## <a name="parquet-format"></a>Parquet biçimi

### <a name="error-code--parquetjavainvocationexception"></a>Hata kodu: Parquetjavaınvocationexception

- **İleti**: `An error occurred when invoking java, message: %javaException;.`

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ', ' Java yığın alanı ' ve ' doublecapacity ' içerdiğinde, genellikle tümleştirme çalışma zamanının eski sürümünde bir bellek yönetimi sorunudur.

- **Öneri**: şirket içinde barındırılan Integration Runtime kullanıyorsanız ve sürüm 3.20.7159.1 'den daha eski ise en son sürüme yükseltmeniz önerilir.

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ' içerdiğinde, tümleştirme çalışma zamanı dosyaları işlemek için yeterli kaynağa sahip değil.

- **Öneri**: Tümleştirme çalışma zamanında eşzamanlı çalıştırmaları sınırlayın. Şirket içinde barındırılan Integration Runtime için, 8 GB 'a eşit veya daha büyük belleği olan güçlü bir makineye kadar ölçeklendirin.

- **Neden**: hata Iletisi ' NullPointerReference ' içerdiğinde, geçici bir hata olabilir.

- **Öneri**: yeniden dene. Sorun devam ederse lütfen desteğe başvurun.


### <a name="error-code--parquetinvalidfile"></a>Hata kodu: Parquetınvalidfile

- **İleti**: `File is not a valid Parquet file.`

- **Neden**: Parquet dosyası sorunu.

- **Öneri**: girişin geçerli bir Parquet dosyası olduğunu denetleyin.


### <a name="error-code--parquetnotsupportedtype"></a>Hata kodu: ParquetNotSupportedType

- **İleti**: `Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **Neden**: Parquet biçimi Azure Data Factory desteklenmiyor.

- **Öneri**: kaynak verileri iki kez kontrol edin. Belgeye başvurun: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs .


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>Hata kodu: ParquetMissedDecimalPrecisionScale

- **İleti**: `Decimal Precision or Scale information is not found in schema for column: %column;.`

- **Neden**: duyarlık ve ölçek sayısını ayrıştırmayı deneyin, ancak böyle bir bilgi sağlanmaz.

- **Öneri**: ' kaynak ' doğru duyarlık ve ölçek döndürmüyor. Sorun sütununun duyarlığını ve ölçeğini denetleyin.


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>Hata kodu: ParquetInvalidDecimalPrecisionScale

- **İleti**: `Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **Neden**: şema geçersiz.

- **Öneri**: sorun sütununun duyarlılığını ve ölçeğini denetleyin.


### <a name="error-code--parquetcolumnnotfound"></a>Hata kodu: ParquetColumnNotFound

- **İleti**: `Column %column; does not exist in Parquet file.`

- **Neden**: kaynak şemasının havuz şeması ile eşleşmemesi.

- **Öneri**: ' Activity ' içindeki ' eşlemeleri ' öğesini işaretleyin. Kaynak sütunun doğru havuz sütunuyla eşlenip eşlendiğinden emin olun.


### <a name="error-code--parquetinvaliddataformat"></a>Hata kodu: Parquetınvaliddataformat

- **İleti**: `Incorrect format of %srcValue; for converting to %dstType;.`

- **Neden**: veriler eşlemeler. Source içinde belirtilen türe dönüştürülemiyor

- **Öneri**: kaynak verileri iki kez kontrol edin veya kopyalama etkinliği sütun eşlemesinde bu sütun için doğru veri türünü belirtin. Belgeye başvurun: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs .


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>Hata kodu: ParquetDataCountNotMatchColumnCount

- **İleti**: `The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **Neden**: kaynak sütun sayısı ve havuz sütun sayısı uyuşmazlığı

- **Öneri**: çift Check kaynak sütunu sayısı ' Mapping ' içindeki havuz sütun sayısıyla aynı.


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>Hata kodu: ParquetDataTypeNotMatchColumnType

- **İleti**:% srctype; veri türü Belirtilen% dstType; sütun türü ile eşleşmiyor '% columnIndex; ' sütununda.

- **Neden**: kaynaktaki veri, havuzda tanımlı olarak dönüştürülemez

- **Öneri**: Mapping. Sink içinde doğru bir tür belirtin.


### <a name="error-code--parquetbridgeinvaliddata"></a>Hata kodu: Parquetköprüınvaliddata

- **İleti**: `%message;`

- **Neden**: sınırlama üzerinde veri değeri

- **Öneri**: yeniden dene. Sorun devam ederse lütfen bizimle iletişime geçin.


### <a name="error-code--parquetunsupportedinterpretation"></a>Hata kodu: Parquetunsupportedyorumlama

- **İleti**: `The given interpretation '%interpretation;' of Parquet format is not supported.`

- **Neden**: desteklenmeyen senaryo

- **Öneri**: ' Parquetınterpotfor ' ' mini SQL ' olmamalıdır.


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>Hata kodu: ParquetUnsupportFileLevelCompressionOption

- **İleti**: `File level compression is not supported for Parquet.`

- **Neden**: desteklenmeyen senaryo

- **Öneri**: yükte ' CompressionType ' öğesini kaldırın.


### <a name="error-code--usererrorjniexception"></a>Hata kodu: UserErrorJniException

- **İleti**: `Cannot create JVM: JNI return code [-6][JNI call failed: Invalid arguments.]`

- **Neden**: bazı geçersiz (genel) bağımsız değişkenler ayarlandığından JVM oluşturulamıyor.

- **Öneri**: kendınden konak IR 'nizin **her bir düğümünü** barındıran makinede oturum açın. Sistem değişkeninin şu şekilde doğru şekilde ayarlandığından emin olun: `_JAVA_OPTIONS "-Xms256m -Xmx16g" with memory bigger than 8 G` . Tüm IR düğümlerini yeniden başlatın ve ardından işlem hattını yeniden çalıştırın.


### <a name="arithmetic-overflow"></a>Aritmetik taşma

- **Belirtiler**: Parquet dosyaları kopyalanırken hata iletisi oluştu: `Message = Arithmetic Overflow., Source = Microsoft.DataTransfer.Common`

- **Neden**: Oracle 'Dan Parquet 'e dosya kopyalarken Şu anda yalnızca duyarlık <= 38 ve tamsayı bölümü <= 20 ' nin uzunluğu desteklenir. 

- **Çözüm**: Bu soruna sahip sütunları, geçici çözüm olarak VARCHAR2 'e dönüştürebilirsiniz.


### <a name="no-enum-constant"></a>Sabit Listesi sabiti yok

- **Belirtiler**: veriler Parquet biçimine kopyalanırken hata iletisi oluştu: `java.lang.IllegalArgumentException:field ended by &apos;;&apos;` , ya da: `java.lang.IllegalArgumentException:No enum constant org.apache.parquet.schema.OriginalType.test` .

- **Neden**: 

    Sorun, boşluk veya desteklenmeyen karakterler (örneğin,;) nedeniyle olabilir. {} (), Parquet bu biçimi desteklemediği için sütun adında () \n\t =). 

    Örneğin, *contoso (test)* gibi sütun adı, [Koddan](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/MessageTypeParser.java) köşeli parantez içinde bulunan türü ayrıştıracaktır `Tokenizer st = new Tokenizer(schemaString, " ;{}()\n\t");` . Böyle bir "test" türü olmadığından hata oluşur.

    Desteklenen türleri denetlemek için, bunları [burada](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/OriginalType.java)kontrol edebilirsiniz.

- **Çözüm**: 

    1. Havuz sütun adında boşluk olup olmadığını iki kez denetleyin.

    1. Boşluk içeren ilk satırın sütun adı olarak kullanıldığını iki kez işaretleyin.

    1. OriginalType türünün desteklenip desteklenmediğini iki kez denetleyin. Bu özel simgelerden kaçınmaya çalışın `,;{}()\n\t=` . 


## <a name="rest"></a>REST

### <a name="unexpected-network-response-from-rest-connector"></a>REST bağlayıcısından beklenmeyen ağ yanıtı

- **Belirtiler**: uç nokta bazen Rest bağlayıcısından beklenmedik yanıt (400/401/403/500) alır.

- **Neden**: Rest kaynak Bağlayıcısı, bir http isteği oluştururken bağlı hizmetten/veri kümesinden/kopya kaynağından parametre olarak URL ve http yöntemi/üst bilgisi/gövdesi kullanır. Bu sorun büyük olasılıkla belirtilen bir veya daha fazla parametrede bazı hatalardan kaynaklanır.

- **Çözüm**: 
    - Parametrenin neden olup olmadığını denetlemek için cmd penceresinde ' KIVI ' kullanın (**kabul etme** ve **Kullanıcı Aracısı** üstbilgileri her zaman eklenmelidir):
        ```
        curl -i -X <HTTP method> -H <HTTP header1> -H <HTTP header2> -H "Accept: application/json" -H "User-Agent: azure-data-factory/2.0" -d '<HTTP body>' <URL>
        ```
      Komut, aynı beklenmeyen yanıtı döndürürse, lütfen beklenen yanıtı döndürünceye kadar, yukarıdaki parametreleri ' kıvrımlı ' ile onarın. 

      Ayrıca, komutun daha gelişmiş kullanımı için ' kıvrık--help ' kullanabilirsiniz.

    - Yalnızca ADF REST Bağlayıcısı beklenmeyen yanıt döndürürse, daha fazla sorun giderme için lütfen Microsoft desteğine başvurun.
    
    - Lütfen ' kıvır ', SSL sertifikası doğrulama sorununu yeniden oluşturmak için uygun olamayacağını unutmayın. Bazı senaryolarda, hiçbir SSL sertifikası doğrulama sorunu çıkmadan ' kıvır ' komutu başarıyla yürütüldü. Ancak aynı URL tarayıcıda yürütüldüğünde, istemcinin sunucu ile güven kurması için ilk yere hiçbir SSL sertifikası döndürülmez.

      Yukarıdaki durum için **Postman** ve **Fiddler** gibi araçlar önerilir.


## <a name="sftp"></a>SFTP

### <a name="invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>' SSHPublicKey ' kimlik doğrulama türü için geçersiz SFTP kimlik bilgisi belirtildi

- **Belirtiler**: ' sshpublickey ' kimlik doğrulama türü için geçersiz SFTP kimlik bilgisi sağlandığı için SSH ortak anahtar kimlik doğrulaması kullanılıyor.

- **Neden**: Bu hata üç olası nedenden kaynaklanabilir:

    1. Özel anahtar içeriği AKV/SDK 'dan getirilir, ancak doğru kodlanmamış.

    1. Yanlış anahtar içerik biçimi seçildi.

    1. Geçersiz kimlik bilgisi veya özel anahtar içeriği.

- **Çözüm**: 

    1. **Nedeni 1**:

       Özel anahtar içeriği AKV 'den ise ve müşteri, SFTP bağlantılı hizmetine doğrudan yükleme yaparken özgün anahtar dosyası işinize

       https://docs.microsoft.com/azure/data-factory/connector-sftp#using-ssh-public-key-authenticationBkz. PrivateKey Içeriği Base64 KODLAMALı SSH özel anahtar içeridir.

       Lütfen **özgün özel anahtar dosyasının tüm içeriğini** Base64 kodlaması ile kodlayın ve kodlanmış dizeyi Akv 'ye depolayın. Özgün özel anahtar dosyası, dosyadan karşıya yükle ' ye tıkladığınızda SFTP bağlantılı hizmetinde çalışabileceğiniz bir dosyadır.

       Dizeyi oluşturmak için kullanılan bazı örnekler şunlardır:

       - C# kodu kullan:
       ```
       byte[] keyContentBytes = File.ReadAllBytes(Private Key Path);
       string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
       ```

       - Python kodu kullan:
       ```
       import base64
       rfd = open(r'{Private Key Path}', 'rb')
       keyContent = rfd.read()
       rfd.close()
       print base64.b64encode(Key Content)
       ```

       - Üçüncü taraf Base64 dönüştürme aracını kullanma

         Gibi Araçlar https://www.base64encode.org/ önerilir.

    1. **Nedeni 2**:

       PKCS # 8 biçiminde SSH özel anahtarı kullanılıyorsa

       PKCS # 8 biçimlendirme SSH özel anahtarı ("-----ŞIFRELENMIŞ özel anahtar-----" ile başlar) Şu anda ADF 'de SFTP sunucusuna erişmek için desteklenmiyor. 

       Anahtarı geleneksel SSH anahtar biçimine dönüştürmek için aşağıdaki komutları çalıştırın ("-----BEGIN RSA özel anahtarı-----" ile başlayın):

       ```
       openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file
       chmod 600 traditional_format_key_file
       ssh-keygen -f traditional_format_key_file -p
       ```
    1. **Nedeni 3**:

       Anahtar dosyanızın veya parolanızın doğru olup olmadığını görmek için lütfen WinSCP gibi araçlarla çift kontrol edin.


### <a name="incorrect-linked-service-type-is-used"></a>Yanlış bağlı hizmet türü kullanıldı

- **Belirtiler**: FTP/SFTP sunucusuna ulaşılamıyor.

- **Neden**: FTP veya SFTP sunucusu için yanlış bağlantılı hizmet türü, BIR SFTP sunucusuna bağlanmak için veya ters bir bağlantı kurmak Için FTP bağlantılı hizmetini kullanma gibi kullanılır.

- **Çözüm**: Lütfen hedef sunucunun bağlantı noktasını denetleyin. FTP varsayılan olarak 21 numaralı bağlantı noktasını kullanır ve SFTP bağlantı noktası 22 kullanır.


### <a name="sftp-copy-activity-failed"></a>SFTP kopyalama etkinliği başarısız oldu

- **Belirtiler**: hata kodu: Usererrorınvalidcolumnmappingcolumnnotfound. Hata iletisi: `Column &apos;AccMngr&apos; specified in column mapping cannot be found in source data.`

- **Neden**: kaynak, "AccMngr" adlı bir sütun içermiyor.

- **Çözüm**: Bu tür "AccMngr" sütununun olup olmadığını doğrulamak için veri kümesinin hedef veri kümesi sütununu eşleyerek nasıl yapılandırıldığını iki kez denetleyin.


### <a name="sftp-server-connection-throttling"></a>SFTP sunucusu bağlantısını daraltma

- **Belirtiler**: Sunucu yanıtı SSH protokol kimliğini içermez ve kopyalanamadı.

- Neden: ADF, SFTP sunucusundan paralel olarak indirilecek birden çok bağlantı oluşturacak ve bazen SFTP sunucusu **azaltmasına yol açacaktır**. Pratikte, farklı sunucu, vuruş azaltma sırasında farklı bir hata döndürür.

- **Çözüm**: 

    Lütfen SFTP veri kümesinin en fazla eşzamanlı bağlantısını 1 olarak belirtin ve kopyayı yeniden çalıştırın. Başarılı olursa, azaltma işleminin nedeni olduğundan emin olabilirsiniz.

    Düşük aktarım hızını yükseltmek istiyorsanız, lütfen eş zamanlı bağlantı sayısı sınırını artırmak için SFTP yöneticisiyle iletişime geçin veya aşağıdaki IP 'yi izin verilenler listesine ekleyin:

    - Yönetilen IR kullanıyorsanız lütfen [Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)ekleyin.
      Ya da SFTP sunucusu izin verilenler listesine büyük IP aralıklarının listesini eklemek istemiyorsanız şirket içinde barındırılan IR 'yi de yükleyebilirsiniz.

    - Şirket içinde barındırılan IR kullanıyorsanız, lütfen Shar 'ın yüklü olduğu makine IP 'sini izin verilenler listesine ekleyin.


### <a name="error-code-sftprenameoperationfail"></a>Hata kodu: SftpRenameOperationFail

- **Belirtiler**: işlem hattı, blob 'DAN SFTP 'ye veri kopyalayamadı ve şu hatayla başarısız oldu: `Operation on target Copy_5xe failed: Failure happened on 'Sink' side. ErrorCode=SftpRenameOperationFail,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException` .

- **Neden**: useTempFileRename seçeneği, verileri kopyalarken doğru olarak ayarlandı. Bu işlem, işlemin geçici dosyaları kullanmasına izin verir. Tüm veriler kopyalanmadan önce bir veya daha fazla geçici dosya silinirse hata tetiklenir.

- **Çözüm**: useTempFileName seçeneğini false olarak ayarlayın.


## <a name="general-copy-activity-error"></a>Genel kopyalama etkinliği hatası

### <a name="error-code--jrenotfound"></a>Hata kodu: JreNotFound

- **İleti**: `Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Neden**: şirket içinde barındırılan tümleştirme çalışma zamanı Java çalışma zamanı bulamıyor. Java çalışma zamanı, belirli bir kaynağı okumak için gereklidir.

- **Öneri**: Tümleştirme çalışma zamanı ortamınızı, belge başvurusu: https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>Hata kodu: Yavaya Cardpathsinyatsupported

- **İleti**: `Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Neden**: havuz veri kümesi joker karakteri desteklemiyor.

- **Öneri**: havuz veri kümesini denetleyin ve Joker değer olmadan yolu düzeltir.


### <a name="error-code--mappinginvalidpropertywithemptyvalue"></a>Hata kodu: MappingInvalidPropertyWithEmptyValue

- **İleti**: `One or more '%sourceOrSink;' in copy activity mapping doesn't point to any data. Choose one of the three properties 'name', 'path' and 'ordinal' to reference columns/fields.`


### <a name="error-code--mappinginvalidpropertywithnamepathandordinal"></a>Hata kodu: Mappingınvalidpropertywithnamepathandordinal

- **İleti**: `Mixed properties are used to reference '%sourceOrSink;' columns/fields in copy activity mapping. Please only choose one of the three properties 'name', 'path' and 'ordinal'. The problematic mapping setting is 'name': '%name;', 'path': '%path;','ordinal': '%ordinal;'.`


### <a name="error-code--mappingduplicatedordinal"></a>Hata kodu: MappingDuplicatedOrdinal

- **İleti**: `Copy activity 'mappings' has duplicated ordinal value "%Ordinal;". Fix the setting in 'mappings'.`


### <a name="error-code--mappinginvalidordinalforsinkcolumn"></a>Hata kodu: MappingInvalidOrdinalForSinkColumn

- **İleti**: `Invalid 'ordinal' property for sink column under 'mappings' property. Ordinal: %Ordinal;.`


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
