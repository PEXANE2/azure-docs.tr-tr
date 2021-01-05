---
title: Azure Data Factory bağlayıcı sorunlarını giderme
description: Azure Data Factory 'deki bağlayıcı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 12/30/2020
ms.author: jingwang
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: e6591762ed6a7e2b462a209730276f3198d86ae8
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821477"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Azure Data Factory bağlayıcı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki bağlayıcılar için genel sorun giderme yöntemleri incelenmektedir.
  
## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="error-code-azurebloboperationfailed"></a>Hata kodu: AzureBlobOperationFailed

- **İleti**: `Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Neden**: BLOB depolama işlemi isabet sorunu.

- **Öneri**: ayrıntıdaki hatayı denetleyin. Blob yardım belgesine başvurun: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes . Yardım gerekirse depolama ekibine başvurun.


### <a name="invalid-property-during-copy-activity"></a>Kopyalama etkinliği sırasında geçersiz özellik

- **İleti**: `Copy activity <Activity Name> has an invalid "source" property. The source type is not compatible with the dataset <Dataset Name> and its linked service <Linked Service Name>. Please verify your input against.`

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

    - Cosmos DB, kopyalama etkinliği performansını iyileştirecek, ancak Cosmos DB daha fazla ücret ödemesine neden olacak şekilde, 1. kapsayıcıyı daha büyük bir değere **yükseltin** . 
    - **Writebatchsize** değerini daha küçük bir değere (1000 gibi) düşürün ve **parallelcopy** değerlerini 1 gibi daha küçük bir değere ayarlayın. Bu, kopya çalıştırma performansının geçerli olmasını sağlar ancak Cosmos DB daha fazla ücret vermez.

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
            
## <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB (SQL API)

### <a name="error-code--cosmosdbsqlapioperationfailed"></a>Hata kodu: CosmosDbSqlApiOperationFailed

- **İleti**: `CosmosDbSqlApi operation Failed. ErrorMessage: %msg;.`

- **Neden**: CosmosDbSqlApi işlemi isabet sorunu.

- **Öneri**: ayrıntıdaki hatayı denetleyin. [Cosmosdb yardım belgesi](https://docs.microsoft.com/azure/cosmos-db/troubleshoot-dot-net-sdk)' ne bakın. Yardım gerekirse CosmosDb ekibine başvurun.


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-underlying-connection-was-closed-could-not-establish-trust-relationship-for-the-ssltls-secure-channel"></a>Hata iletisi: temel alınan bağlantı kapatıldı: SSL/TLS güvenli kanalı için güven ilişkisi kurulamadı.

- **Belirtiler**: kopyalama etkinliği aşağıdaki hatayla başarısız olur: 

    ```
    Message: ErrorCode = `UserErrorFailedFileOperation`, Error Message = `The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel`.
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


## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code-adlsgen2operationfailed"></a>Hata kodu: ADLSGen2OperationFailed

- **İleti**: `ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Neden**: ADLS 2. işlem başarısız olduğunu gösteren hatayı oluşturur.

- **Öneri**: ADLS 2. tarafından oluşturulan ayrıntılı hata iletisini denetleyin. Geçici bir hata nedeniyle, lütfen yeniden deneyin. Daha fazla yardıma ihtiyacınız varsa lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.

- **Neden**: hata Iletisi ' yasak ' içerdiğinde, kullandığınız hizmet sorumlusu veya yönetilen kimlik ADLS 2. erişmek için yeterli izne sahip olmayabilir.

- **Öneri**: yardım belgesine bakın: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication .

- **Neden**: hata Iletisi ' ınternalservererror ' içerdiğinde hata ADLS 2. tarafından döndürülür.

- **Öneri**: geçici bir hatadan kaynaklanıyor olabilir, lütfen yeniden deneyin. Sorun devam ederse lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.

### <a name="request-to-adls-gen2-account-met-timeout-error"></a>ADLS 2. hesabı için istek zaman aşımı hatası oluştu

- **İleti**: hata kodu = `UserErrorFailedBlobFSOperation` , hata iletisi = `BlobFS operation failed for: A task was canceled` .

- **Neden**: sorun, genellikle şirket IÇINDE barındırılan IR makinesinde gerçekleşen ADLS 2. havuz zaman aşımı hatası nedeniyle oluşur.

- **Öneri**: 

    - Şirket içinde barındırılan IR makinenizi ve hedef ADLS 2. hesabınızı mümkünse aynı bölgeye yerleştirin. Bu, rastgele zaman aşımı hatasından kaçınmanıza ve daha iyi performans sağlayabilir.

    - ExpressRoute gibi özel bir ağ ayarı olup olmadığını denetleyin ve ağın yeterli bant genişliğine sahip olduğundan emin olun. Genel bant genişliği düşük olduğunda şirket içinde barındırılan IR eş zamanlı işleri ayarının daha az olması önerilir, bu da birden çok eş zamanlı iş genelinde ağ kaynağı yarışmasını önleyebilir.

    - Dosya boyutu orta veya küçük olduğunda bu zaman aşımı hatasını azaltmak için ikili olmayan kopya için daha küçük blok boyutu kullanın. [BLOB depolama yerleştirme bloğuna](https://docs.microsoft.com/rest/api/storageservices/put-block)bakın.

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

                  
## <a name="azure-file-storage"></a>Azure Dosya Depolama

### <a name="error-code--azurefileoperationfailed"></a>Hata kodu: AzureFileOperationFailed

- **İleti**: `Azure File operation Failed. Path: %path;. ErrorMessage: %msg;.`

- **Neden**: Azure dosya depolama işlemi isabet sorunu.

- **Öneri**: ayrıntıdaki hatayı denetleyin. Azure dosya yardım belgesine başvurun: https://docs.microsoft.com/rest/api/storageservices/file-service-error-codes . Yardıma ihtiyacınız varsa depolama ekibine başvurun.


## <a name="azure-synapse-analyticsazure-sql-databasesql-server"></a>Azure SYNAPSE Analytics/Azure SQL veritabanı/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Hata kodu: SqlFailedToConnect

- **İleti**: `Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Neden**: Azure SQL: hata Iletisinde "SqlErrorNumber = 47073" varsa, bağlantı ayarında genel ağ erişiminin reddedildiği anlamına gelir.

- **Öneri**: Azure SQL güvenlik duvarında "ortak ağ erişimini reddet" seçeneğini "Hayır" olarak ayarlayın. Daha fazla bilgi edinin https://docs.microsoft.com/azure/azure-sql/database/connectivity-settings#deny-public-network-access .

- **Neden**: Azure SQL: hata Iletisi "SqlErrorNumber = [ErrorCode]" gibi SQL hata kodu içeriyorsa, lütfen Azure SQL sorun giderme kılavuzu 'na bakın.

- **Öneri**: hakkında daha fazla bilgi edinin https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues .

- **Neden**: 1433 bağlantı noktasının güvenlik duvarı izin verilenler listesinde olup olmadığını denetleyin.

- **Öneri**: Bu referans belge: ile izleyin https://docs.microsoft.com/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access#ports-used-by- .

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: daha fazla ayrıntı için bu başvuru BELGESI içindeki SQL hata kodu ile ara: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors . Daha fazla yardıma ihtiyacınız varsa Azure SQL desteği ile iletişime geçin.

- **Neden**: Bu geçici bir sorundur (örn., Instable ağ bağlantısı), azaltmak için lütfen etkinlik ilkesini yeniden dene 'yi ekleyin.

- **Öneri**: Bu başvuru belgesini izleyin: https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities#activity-policy .

- **Neden**: hata ILETISI "IP adresi olan istemci '... ' içeriyorsa) sunucusuna erişim izni verilmez "ve Azure SQL veritabanı 'na bağlanmaya çalışıyorsunuz, genellikle Azure SQL veritabanı güvenlik duvarı sorunu nedeniyle oluşur.

- **Öneri**: Azure SQL Server güvenlik duvarı yapılandırması 'nda, "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine izin ver" seçeneğini etkinleştirin. Başvuru belgesi: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure .


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

- **Çözüm**: 
    - Sütun genişliğini 1 MB 'tan küçük olacak şekilde küçültün.
    - Alternatif olarak Polybase'i devre dışı bırakarak toplu ekleme yaklaşımını kullanabilirsiniz.


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

    - Azure DB katmanı yeterince yüksek değil.

    - Azure DB DTU kullanımı %100 ' e yakın. [Performansı izleyebilir](https://docs.microsoft.com/azure/azure-sql/database/monitor-tune-overview) ve DB katmanını yükseltmeyi göz önünde bulundurun.

    - Dizinler düzgün ayarlanmadı. Veri yüklemeden önce tüm dizinleri kaldırın ve yükleme tamamlandıktan sonra yeniden oluşturun.

    - WriteBatchSize, şema satır boyutuna sığacak kadar büyük değil. Sorunun özelliğini genişletmenize çalışın.

    - Toplu iç içe değil, saklı yordam kullanılıyor, bu da daha kötü performansa sahip olması beklenir. 

- **Çözüm**: [kopyalama etkinliği performansı](https://docs.microsoft.com/azure/data-factory/copy-activity-performance-troubleshooting) için TSG 'ye başvurun


### <a name="performance-tier-is-low-and-leads-to-copy-failure"></a>Performans katmanı düşük ve kopyalamanın başarısız olmasına yol açar

- **Belirtiler**: Azure SQL 'e veri kopyalanırken aşağıdaki hata iletisi oluştu: `Database operation failed. Error message from database execution : ExecuteNonQuery requires an open and available Connection. The connection's current state is closed.`

- **Neden**: Azure SQL S1 kullanılıyor, bu durum bu durumda GÇ sınırlarına ulaştı.

- **Çözüm**: sorunu çözmek IÇIN Azure SQL performans katmanını yükseltin. 


### <a name="sql-table-cannot-be-found"></a>SQL tablosu bulunamıyor 

- **Belirtiler**: veriler karma 'ten şirket içi SQL Server tablosuna kopyalanırken hata oluştu:`Cannot find the object "dbo.Contoso" because it does not exist or you do not have permissions.`

- **Neden**: geçerli SQL hesabının .net SqlBulkCopy. WriteToServer tarafından verilen istekleri yürütmek için yeterli izni yok.

- **Çözüm**: daha AYRıCALıKLı bir SQL hesabına geçiş yapın.


### <a name="error-message-string-or-binary-data-would-be-truncated"></a>Hata iletisi: dize veya ikili veriler kesilecek

- **Belirtiler**: veriler şirket Içi/Azure SQL Server tablosuna kopyalanırken hata oluştu: 

- **Neden**: CX SQL tablo şeması tanımında beklentiden daha az uzunluğa sahip bir veya daha fazla sütun vardır.

- **Çözüm**: sorunu çözmek için aşağıdaki adımları deneyin:

    1. Hangi satırların soruna sahip olduğunu gidermek için, özellikle "Redirectıncompatiblerowsettings" SQL havuzu [hata toleransı](https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance)uygulayın.

        > [!NOTE]
        > Lütfen hata toleransı ek yürütme süresi ortaya çıkarabilir ve bu da daha yüksek maliyetli olabilir.

    2. Hangi sütunların güncelleştirileceğini görmek için SQL tablo şeması sütun uzunluğu ile yeniden yönlendirilen verileri iki kez kontrol edin.

    3. Tablo şemasını buna göre güncelleştirin.


## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="error-code--azuretableduplicatecolumnsfromsource"></a>Hata kodu: AzureTableDuplicateColumnsFromSource

- **İleti**: `Duplicate columns with same name '%name;' are detected from source. This is NOT supported by Azure Table Storage sink`

- **Neden**: JOIN veya yapılandırılmamış CSV dosyaları SQL sorgusu için ortak olabilir

- **Öneri**: kaynak sütunları iki kez kontrol edin ve uygun şekilde onarın.


## <a name="db2"></a>DB2

### <a name="error-code--db2driverrunfailed"></a>Hata kodu: DB2DriverRunFailed

- **İleti**: `Error thrown from driver. Sql code: '%code;'`

- **Neden**: hata ILETISI "SQLSTATE = 51002 SQLCODE =-805" içeriyorsa, lütfen bu belgedeki ipucuna başvurun: https://docs.microsoft.com/azure/data-factory/connector-db2#linked-service-properties

- **Öneri**: "packageCollection" özelliğindeki "nullıd" ayarlamayı deneyin


## <a name="delimited-text-format"></a>Sınırlandırılmış metin biçimi

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Hata kodu: Delimitedtextcolumnsüs Otallownull

- **İleti**: `The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Neden**: etkinlikte ' firstRowAsHeader ' ayarlandığında, ilk satır sütun adı olarak kullanılır. Bu hata, ilk satırın boş değer içerdiği anlamına gelir. Örneğin: ' ColumnA, ColumnB '.

- **Öneri**: ilk satırı denetleyin ve boş değer varsa değeri düzeltir.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Hata kodu: Delimitedtextmorecolumnsıısdefined tanımlandı

- **İleti**: `Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %expectedColumnCount;.`

- **Neden**: sorunlu satırın sütun sayısı, ilk satırın sütun sayısından daha büyük. Bu durum, veri sorunu ya da yanlış sütun sınırlayıcısı/quote char ayarlarından kaynaklanıyor olabilir.

- **Öneri**: hata iletisinde satır sayısını alın, satırın sütununu kontrol edin ve verileri onarın.

- **Neden**: beklenen sütun sayısı hata iletisinde "1" ise yanlış sıkıştırma veya Biçim ayarları belirttiniz olabilir. Bu nedenle, ADF Dosyanızı yanlış bir şekilde ayrıştırdı.

- **Öneri**: kaynak dosya (lar) ile eşleştiğinden emin olmak için biçim ayarlarını kontrol edin.

- **Neden**: kaynağınız bir klasörysa, belirtilen klasörde bulunan dosyaların farklı bir şeması olması mümkündür.

- **Öneri**: belirtilen klasörde bulunan dosyaların aynı şemaya sahip olduğundan emin olun.


## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Hata kodu: DynamicsCreateServiceClientError

- **İleti**: `This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Neden**: Bu, Dynamics Server tarafında geçici bir sorundur.

- **Öneri**: işlem hattını yeniden çalıştırın. Başarısız olursa paralelliği azaltmayı deneyin. Hala başarısız olduysa, lütfen Dynamics destek birimine başvurun.


### <a name="columns-are-missing-when-previewingimporting-schema"></a>Şema önizleniyor/içeri aktarılırken sütunlar eksik

- **Belirtiler**: şemayı içeri aktarırken veya verileri önizlerken bazı sütunlardan bazıları eksik olur. Hata iletisi: `The valid structure information (column name and type) are required for Dynamics source.`

- **Neden**: ADF, ilk 10 kayıtta hiçbir değere sahip olmayan sütunları gösteremediğinden bu sorun temel olarak tasarıma göre yapılır. Eklediğiniz sütunların doğru biçimde olduğundan emin olun. 

- **Öneri**: eşleme sekmesine sütunları el ile ekleyin.


### <a name="error-code--dynamicsmissingtargetformultitargetlookupfield"></a>Hata kodu: DynamicsMissingTargetForMultiTargetLookupField

- **İleti**: `Cannot find the target column for multi-target lookup field: '%fieldName;'.`

- **Neden**: hedef sütun kaynakta veya sütun eşlemesinde yok.

- **Öneri**: 1. Kaynağın hedef sütununu içerdiğinden emin olun. 2. Sütun eşlemesinde Target sütununu ekleyin. Havuz sütununun "{fieldName}" düzeninde olduğundan emin olun @EntityReference .


### <a name="error-code--dynamicsinvalidtargetformultitargetlookupfield"></a>Hata kodu: DynamicsInvalidTargetForMultiTargetLookupField

- **İleti**: `The provided target: '%targetName;' is not a valid target of field: '%fieldName;'. Valid targets are: '%validTargetNames;"`

- **Neden**: bir çok hedef arama alanının hedef varlığı olarak yanlış bir varlık adı belirtildi.

- **Öneri**: çoklu hedef arama alanı için geçerli bir varlık adı belirtin.


### <a name="error-code--dynamicsinvalidtypeformultitargetlookupfield"></a>Hata kodu: DynamicsInvalidTypeForMultiTargetLookupField

- **İleti**: `The provided target type is not a valid string. Field: '%fieldName;'.`

- **Neden**: Target sütunundaki değer bir dize değil

- **Öneri**: çoklu hedef arama hedefi sütununda geçerli bir dize belirtin.


### <a name="error-code--dynamicsfailedtorequetserver"></a>Hata kodu: Dynamicsfailedtobir Etserver

- **İleti**: `The dynamics server or the network is experiencing issues. Check network connectivity or check dynamics server log for more details.`

- **Neden**: Dynamics sunucusu kararlı değil veya erişilemez durumda veya ağda sorun yaşanıyor.

- **Öneri**: daha fazla ayrıntı için ağ bağlantısını denetleyin veya Dynamics Server günlüğünü kontrol edin. Daha fazla yardım için Dynamics desteği ile iletişime geçin.


## <a name="excel-format"></a>Excel biçimi

### <a name="timeout-or-slow-performance-when-parsing-large-excel-file"></a>Büyük Excel dosyası ayrıştırılırken zaman aşımı veya yavaş performans

- **Belirtiler**:

    - Excel veri kümesi oluşturduğunuzda ve şemayı bağlantı/depolama alanından içeri aktardığınızda, verileri Önizle, Listele veya Yenile, Excel dosyası boyutta büyükse zaman aşımı hatası ile karşılaşabilirsiniz.

    - Büyük Excel dosyasından (>= 100 MB) verileri başka veri deposuna kopyalamak için kopyalama etkinliğini kullandığınızda, yavaş performans veya OOM sorunu yaşayabilirsiniz.

- **Neden**: 

    - Excel veri kümesindeki şemayı içeri aktarma, verileri önizleme ve çalışma sayfalarını listeleme gibi işlemler için zaman aşımı 100 s ve statiktir. Büyük Excel dosyası için, bu işlemler zaman aşımı değeri içinde bitmeyebilir.

    - ADF kopyalama etkinliği tüm Excel dosyasını belleğe okur ve verileri okumak için belirtilen çalışma sayfasını ve hücreleri bulur. Bu davranış, temel alınan ADF tarafından kullanılan SDK 'nın kullanımından kaynaklanır.

- **Çözüm**: 

    - Şemayı içeri aktarmak için, özgün dosyanın bir alt kümesi olan daha küçük bir örnek dosya oluşturabilir ve "şemayı bağlantı/depodan içeri aktar" yerine "örnek dosyadan şemayı içeri aktar" seçeneğini belirleyebilirsiniz.

    - Çalışma sayfası listesinde, çalışma sayfası açılan menüsünde "Düzenle" düğmesine tıklayabilir ve bunun yerine sayfa adını/dizinini girebilirsiniz.

    - Büyük Excel dosyasını (>100 MB) başka bir depoya kopyalamak için, spor akışı okuma ve daha iyi bir şekilde gerçekleştirdiğiniz veri akışı Excel kaynağını kullanabilirsiniz.
    

## <a name="ftp"></a>FTP

### <a name="error-code--ftpfailedtoconnecttoftpserver"></a>Hata kodu: FtpFailedToConnectToFtpServer

- **İleti**: `Failed to connect to FTP server. Please make sure the provided server informantion is correct, and try again.`

- **Neden**: bir FTP sunucusuna bağlanmak için SFTP bağlı hizmeti 'ni kullanma gıbı, FTP sunucusu için yanlış bağlı hizmet türü kullanılıyor olabilir.

- **Öneri**: hedef sunucunun bağlantı noktasını denetleyin. Varsayılan olarak FTP bağlantı noktası 21 ' i kullanır.


## <a name="http"></a>Http

### <a name="error-code--httpfilefailedtoread"></a>Hata kodu: HttpFileFailedToRead

- **İleti**: `Failed to read data from http server. Check the error from http server：%message;`

- **Neden**: Bu hata http sunucusuyla iletişim Azure Data Factory, ancak http isteği işlemi başarısız olduğunda meydana gelir.

- **Öneri**: hata iletisinde http durum kodu \ iletisini denetleyin ve uzak sunucu sorununu onarın.


## <a name="oracle"></a>Oracle

### <a name="error-code-argumentoutofrangeexception"></a>Hata kodu: ArgumentOutOfRangeException

- **İleti**: `Hour, Minute, and Second parameters describe an un-representable DateTime.`

- **Neden**: ADF 'de, tarih saat değerleri 0001-01-01 00:00:00 ile 9999-12-31 23:59:59 arasında desteklenir. Ancak, Oracle, ADF 'de hataya neden olan BC yüzyıl veya min/sn>59) daha geniş aralıktaki DateTime değerlerini destekler.

- **Öneri**: 

    `select dump(<column name>)`Oracle 'daki DEĞERIN ADF 'nin aralığında olup olmadığını denetlemek için öğesini çalıştırın. 

    Sonuç olarak bayt sırasını biliyorsanız lütfen kontrol edin https://stackoverflow.com/questions/13568193/how-are-dates-stored-in-oracle .


## <a name="orc-format"></a>Orc biçimi

### <a name="error-code--orcjavainvocationexception"></a>Hata kodu: Orcjavaınvocationexception

- **İleti**: `An error occurred when invoking java, message: %javaException;.`

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ', ' Java yığın alanı ' ve ' doublecapacity ' içerdiğinde, genellikle tümleştirme çalışma zamanının eski sürümünde bir bellek yönetimi sorunudur.

- **Öneri**: şirket içinde barındırılan Integration Runtime kullanıyorsanız, en son sürüme yükseltmeyi önerin.

- **Neden**: hata iletisi ' Java. lang. OutOfMemory ' içerdiğinde, tümleştirme çalışma zamanı dosyaları işlemek için yeterli kaynağa sahip değil.

- **Öneri**: Tümleştirme çalışma zamanında eşzamanlı çalıştırmaları sınırlayın. Şirket içinde barındırılan Integration Runtime için, 8 GB 'a eşit veya daha büyük belleği olan güçlü bir makineye kadar ölçeklendirin.

- **Neden**: hata Iletisi ' NullPointerReference ' içerdiğinde, geçici bir hata olabilir.

- **Öneri**: yeniden dene. Sorun devam ederse lütfen desteğe başvurun.

- **Neden**: hata Iletisi ' bufferoverflowexception ' içerdiğinde, geçici bir hata olabilir.

- **Öneri**: yeniden dene. Sorun devam ederse lütfen desteğe başvurun.

- **Neden**: hata iletisi "Java. lang. ClassCastException:org. Apache. Hadoop. Hive. serde2. IO. Hivecharyazılabilir" org. Apache. Hadoop. IO. Text "öğesine atanamaz, bu da Java çalışma zamanı içinde tür dönüştürme sorunudur. Genellikle, kaynak verilerin neden Java çalışma zamanı 'nda iyi işlenemeyebilir.

- **Öneri**: Bu veri sorunudur. Orc biçim verilerinde char/varchar yerine dize kullanmayı deneyin.

### <a name="error-code--orcdatetimeexceedlimit"></a>Hata kodu: OrcDateTimeExceedLimit

- **İleti**: `The Ticks value '%ticks;' for the datetime column must be between valid datetime ticks range -621355968000000000 and 2534022144000000000.`

- **Neden**: DateTime değeri ' 0001-01-01 00:00:00 ' Ise, Jülyen takvimi Ile Gregoryen takvimi arasındaki fark neden olabilir. https://en.wikipedia.org/wiki/Proleptic_Gregorian_calendar#Difference_between_Julian_and_proleptic_Gregorian_calendar_dates.

- **Öneri**: ticks değerini denetleyin ve ' 0001-01-01 00:00:00 ' Tarih saat değerini kullanmaktan kaçının.


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

    - Havuz sütun adında boşluk olup olmadığını iki kez denetleyin.

    - Boşluk içeren ilk satırın sütun adı olarak kullanıldığını iki kez işaretleyin.

    - OriginalType türünün desteklenip desteklenmediğini iki kez denetleyin. Bu özel simgelerden kaçınmaya çalışın `,;{}()\n\t=` . 


## <a name="rest"></a>REST

### <a name="error-code--restsinkcallfailed"></a>Hata kodu: RestSinkCallFailed

- **İleti**: `Rest Endpoint responded with Failure from server. Check the error from server:%message;`

- **Neden**: Azure Data Factory http protokolü üzerinden REST uç noktasıyla iletişim kuramadığında bu hata oluşur ve istek işlemi başarısız olur.

- **Öneri**: hata iletisinde http durum kodu \ iletisini denetleyin ve uzak sunucu sorununu onarın.

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

#### <a name="error-code--sftpoperationfail"></a>Hata kodu: SftpOperationFail

- **İleti**: `Failed to '%operation;'. Check detailed error from SFTP.`

- **Neden**: SFTP işlemi isabet sorunu.

- **Öneri**: SFTP 'den ayrıntılı hata olup olmadığını denetleyin.


### <a name="error-code--sftprenameoperationfail"></a>Hata kodu: SftpRenameOperationFail

- **İleti**: `Failed to rename the temp file. Your SFTP server doesn't support renaming temp file, please set "useTempFileRename" as false in copy sink to disable uploading to temp file.`

- **Neden**: SFTP sunucunuz geçici dosyayı yeniden adlandırmayı desteklemiyor.

- **Öneri**: Temp dosyasına karşıya yüklemeyi devre dışı bırakmak için kopya havuzunda "useTempFileRename" değerini false olarak ayarlayın.


### <a name="error-code--sftpinvalidsftpcredential"></a>Hata kodu: Sftpınvalidsftpcredential

- **İleti**: `Invalid Sftp credential provided for '%type;' authentication type.`

- **Neden**: özel anahtar içeriği Akv/SDK 'dan getirildi ancak doğru kodlanmamış.

- **Öneri**:  

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

- **Neden**: yanlış anahtar içerik biçimi seçildi

- **Öneri**:  

    PKCS # 8 biçimlendirme SSH özel anahtarı ("-----ŞIFRELENMIŞ özel anahtar-----" ile başlar) Şu anda ADF 'de SFTP sunucusuna erişmek için desteklenmiyor. 

    Anahtarı geleneksel SSH anahtar biçimine dönüştürmek için aşağıdaki komutları çalıştırın ("-----BEGIN RSA özel anahtarı-----" ile başlayın):

    ```
    openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file
    chmod 600 traditional_format_key_file
    ssh-keygen -f traditional_format_key_file -p
    ```

- **Neden**: geçersiz kimlik bilgisi veya özel anahtar içeriği

- **Öneri**: anahtar dosyanızın veya parolanızın doğru olup olmadığını görmek Için WinSCP gibi araçlarla çift kontrol edin.

### <a name="sftp-copy-activity-failed"></a>SFTP kopyalama etkinliği başarısız oldu

- **Belirtiler**: hata kodu: Usererrorınvalidcolumnmappingcolumnnotfound. Hata iletisi: `Column &apos;AccMngr&apos; specified in column mapping cannot be found in source data.`

- **Neden**: kaynak, "AccMngr" adlı bir sütun içermiyor.

- **Çözüm**: Bu tür "AccMngr" sütununun olup olmadığını doğrulamak için veri kümesinin hedef veri kümesi sütununu eşleyerek nasıl yapılandırıldığını iki kez denetleyin.


### <a name="error-code--sftpfailedtoconnecttosftpserver"></a>Hata kodu: SftpFailedToConnectToSftpServer

- **İleti**: `Failed to connect to Sftp server '%server;'.`

- **Neden**: hata Iletisinde ' yuva okuma işlemi 30000 milisaniyelik sonra zaman aşımına uğradı ' içeriyorsa, olası bir neden, SFTP sunucusu için yanlış bağlı hizmet türünün bir SFTP sunucusuna bağlanmak Için FTP bağlantılı hizmetini kullanma gibi, yanlış bağlantılı hizmet türünde kullanılması olabilir.

- **Öneri**: hedef sunucunun bağlantı noktasını denetleyin. Varsayılan olarak, SFTP bağlantı noktası 22 ' i kullanır.

- **Neden**: hata Iletisinde ' sunucu yanıtı SSH protokol kimliği içermez ' hatası varsa, olası bir neden, SFTP sunucusunun bağlantıyı kısıtlamasının olması olabilir. ADF, SFTP sunucusundan paralel olarak indirilecek birden çok bağlantı oluşturacak ve bazen SFTP sunucusu azaltmasına ulaşacaktır. Pratikte, farklı sunucu, vuruş azaltma sırasında farklı bir hata döndürür.

- **Öneri**:  

    SFTP veri kümesinin en fazla eş zamanlı bağlantısını 1 olarak belirtin ve kopyayı yeniden çalıştırın. Başarılı olursa, azaltma işleminin nedeni olduğundan emin olabilirsiniz.

    Düşük aktarım hızını yükseltmek istiyorsanız, lütfen eş zamanlı bağlantı sayısı sınırını artırmak için SFTP yöneticisiyle iletişime geçin veya aşağıdaki IP 'yi izin verilenler listesine ekleyin:

    - Yönetilen IR kullanıyorsanız lütfen [Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)ekleyin.
      Ya da SFTP sunucusu izin verilenler listesine büyük IP aralıklarının listesini eklemek istemiyorsanız şirket içinde barındırılan IR 'yi de yükleyebilirsiniz.

    - Şirket içinde barındırılan IR kullanıyorsanız, lütfen Shar 'ın yüklü olduğu makine IP 'sini izin verilenler listesine ekleyin.


## <a name="sharepoint-online-list"></a>SharePoint Online Listesi

### <a name="error-code--sharepointonlineauthfailed"></a>Hata kodu: SharePointOnlineAuthFailed

- **İleti**: `The access token generated failed, status code: %code;, error message: %message;.`

- **Neden**: hızmet sorumlusu kimliği ve anahtarı doğru şekilde ayarlanmayabilir.

- **Öneri**: kayıtlı uygulamanızı (HIZMET sorumlusu kimliği) ve anahtarın doğru şekilde ayarlanmış olup olmadığını denetleyin.


## <a name="xml-format"></a>XML biçimi

### <a name="error-code--xmlsinknotsupported"></a>Hata kodu: Xmlsinde Tsupported

- **İleti**: `Write data in xml format is not supported yet, please choose a different format!`

- **Neden**: kopyalama etkinliğinizdeki havuz veri kümesi olarak bir XML veri kümesi kullanıldı

- **Öneri**: bir veri kümesini kopya havuzu olarak farklı biçimde kullanın.


### <a name="error-code--xmlattributecolumnnameconflict"></a>Hata kodu: XmlAttributeColumnNameConflict

- **İleti**: `Column names %attrNames;' for attributes of element '%element;' conflict with that for corresponding child elements, and the attribute prefix used is '%prefix;'.`

- **Neden**: çakışmaya neden olan bir öznitelik öneki kullanıldı.

- **Öneri**: "attributeprefix" özelliğinin farklı bir değerini ayarlayın.


### <a name="error-code--xmlvaluecolumnnameconflict"></a>Hata kodu: XmlValueColumnNameConflict

- **İleti**: `Column name for the value of element '%element;' is '%columnName;' and it conflicts with the child element having the same name.`

- **Neden**: öğe değerinin sütun adı olarak alt öğe adlarından biri kullanılır.

- **Öneri**: "valueColumn" özelliğinin farklı bir değerini ayarlayın.


### <a name="error-code--xmlinvalid"></a>Hata kodu: XmlInvalid

- **İleti**: `Input XML file '%file;' is invalid with parsing error '%error;'.`

- **Neden**: giriş XML dosyası düzgün biçimlendirilmemiş.

- **Öneri**: iyi biçimlendirilmiş olması için XML dosyasını düzeltin.


## <a name="general-copy-activity-error"></a>Genel kopyalama etkinliği hatası

### <a name="error-code--jrenotfound"></a>Hata kodu: JreNotFound

- **İleti**: `Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Neden**: şirket içinde barındırılan tümleştirme çalışma zamanı Java çalışma zamanı bulamıyor. Java çalışma zamanı, belirli bir kaynağı okumak için gereklidir.

- **Öneri**: Tümleştirme çalışma zamanı ortamınızı, belge başvurusu: https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>Hata kodu: Yavaya Cardpathsinyatsupported

- **İleti**: `Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Neden**: havuz veri kümesi joker karakteri desteklemiyor.

- **Öneri**: havuz veri kümesini denetleyin ve Joker değer olmadan yolu düzeltir.


### <a name="fips-issue"></a>FIPS sorunu

- **Belirtiler**: kopyalama ETKINLIĞI, FIPS özellikli, kendiliğinden konak Integration Runtime makinesinde hata iletisiyle başarısız olur `This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.` . Bu durum, Azure blob, SFTP vb. gibi bağlayıcılarla veri kopyalanırken meydana gelebilir.

- **Neden**: FIPS (Federal bilgi işleme standartları), kullanılmasına izin verilen belirli bir şifreleme algoritmaları kümesini tanımlar. Makinede FIPS modu etkin olduğunda, kopyalama etkinliğinin bağımlı olduğu bazı şifreleme sınıfları bazı senaryolarda engellenir.

- **Çözüm**: [Bu MAKALEDEN](https://techcommunity.microsoft.com/t5/microsoft-security-baselines/why-we-8217-re-not-recommending-8220-fips-mode-8221-anymore/ba-p/701037)Windows 'da FIPS modunun geçerli durumu hakkında bilgi alabilir ve şirket IÇINDE barındırılan Integration Runtime makinesinde FIPS 'yi devre dışı bırakabileceğinizi değerlendirin.

    Öte yandan, Azure Data Factory FIPS 'yi atlamak ve etkinliğin başarılı olmasını sağlamak istiyorsanız aşağıdaki adımları izleyebilirsiniz:

    1. Şirket içinde barındırılan Integration Runtime yüklendiği klasörü açın, genellikle altında `C:\Program Files\Microsoft Integration Runtime\<IR version>\Shared` .

    2. "diawp.exe.config" öğesini açın, `<enforceFIPSPolicy enabled="false"/>` `<runtime>` aşağıdaki gibi bölümüne ekleyin.

        ![FIPS’yi devre dışı bırakma](./media/connector-troubleshoot-guide/disable-fips-policy.png)

    3. Şirket içinde barındırılan Integration Runtime makinesini yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
