---
title: Azure Data Factory bağlayıcılarıyla ilgili sorunları giderme
description: Azure Data Factory 'deki bağlayıcı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/26/2019
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 218031830a7516dfd539e1c0b9b665807822f38d
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533159"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Azure Data Factory bağlayıcılarıyla ilgili sorunları giderme

Bu makalede Azure Data Factory içindeki bağlayıcılar için genel sorun giderme yöntemleri incelenmektedir.

## <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

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

## <a name="sftp"></a>SFTP

### <a name="error-message-invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>Hata iletisi: ' SshPublicKey ' kimlik doğrulama türü için geçersiz SFTP kimlik bilgisi belirtildi

- **Belirtiler**: `SshPublicKey` kimlik doğrulaması kullanıyorsunuz ve şu hatayı alırsınız:

    ```
    Invalid Sftp credential provided for 'SshPublicKey' authentication type
    ```

- **Neden**: 3 olası neden vardır:

    1. SFTP bağlantılı hizmetini yazmak için ADF yazma Kullanıcı arabirimini kullanıyorsanız, bu hata, kullanmak üzere seçtiğiniz özel anahtarın yanlış biçimde olduğunu gösterir. Bir PKCS # 8 SSH özel anahtarı biçimi kullanabilirsiniz, ancak ADF 'nin yalnızca geleneksel SSH anahtar biçimini desteklediğini unutmayın. Daha belirgin olarak, PKCS # 8 biçimi ve geleneksel anahtar biçimi arasındaki fark PKCS # 8 anahtar içerikleri, " *-----ŞIFRELENMIŞ özel anahtarla-----* " ile başlar, ancak geleneksel anahtar biçimi " *-----BEGIN RSA özel anahtar-----* " ile başlar.
    2. Özel anahtar içeriğini depolamak için Azure Key Vault kullanırsanız veya SFTP bağlantılı hizmetini yazmak için programmatical yöntemini kullanırsanız, bu hata özel anahtar içeriğinin yanlış olduğu anlamına gelir, büyük olasılıkla Base64 kodlamalı değildir.
    3. Geçersiz kimlik bilgisi veya özel anahtar içeriği.

- **Çözüm**: 

    - Nedeni #1 için, anahtarı geleneksel anahtar biçimine dönüştürmek için aşağıdaki komutları çalıştırın, sonra bunu ADF yazma Kullanıcı arabiriminde kullanın.

        ```
        # Decrypt the pkcs8 key and convert the format to traditional key format
        openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file

        chmod 600 traditional_format_key_file

        # Re-encrypte the key file using passphrase
        ssh-keygen -f traditional_format_key_file -p
        ```

    - Nedeni #2, bu tür bir dize oluşturmak Için, müşteri aşağıdaki 2 farklı şekilde kullanılabilir:
    - Üçüncü taraf Base64 dönüştürme aracını kullanma: tüm özel anahtar içeriğini [Base64 kodlama ve kod çözme](https://www.base64encode.org/)gibi araçlara yapıştırın, Base64 biçimli bir dizeye kodlayın, ardından bu dizeyi anahtar kasasına yapıştırın veya SFTP bağlantılı hizmetini programlı bir şekilde yazmak için bu değeri kullanın.
    - Kod C# kullanma:

        ```c#
        byte[] keyContentBytes = File.ReadAllBytes(privateKeyPath);
        string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
        ```

    - Nedeni #3 için, anahtar dosyasının veya parolanın, SFTP sunucusuna düzgün şekilde erişmek üzere kullanıp kullanamayacağınızı doğrulamak üzere diğer araçları kullanarak doğru olup olmadığını iki kez denetleyin.
  

## <a name="azure-sql-data-warehouse--azure-sql-database--sql-server"></a>Azure SQL veri ambarı \ Azure SQL veritabanı \ SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Hata kodu: SqlFailedToConnect

- **İleti**: `Cannot connect to SQL database: '%server;', Database: '%database;', User: '%user;'. Please check the linked service configuration is correct, and make sure the SQL database firewall allows the integration runtime to access.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: lütfen daha fazla ayrıntı için bu başvuru BELGESI içindeki SQL hata kodu ile arama yapın: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Daha fazla yardıma ihtiyacınız varsa lütfen Azure SQL desteği 'ne başvurun.

- **Neden**: hata ILETISI "IP adresi olan istemci '... ' içeriyorsa) sunucusuna erişim izni verilmez "ve Azure SQL veritabanı 'na bağlanmaya çalışıyorsunuz, genellikle Azure SQL veritabanı güvenlik duvarı sorunu nedeniyle oluşur.

- **Öneri**: Azure SQL Server güvenlik duvarı yapılandırması 'nda, "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine izin ver" seçeneğini etkinleştirin. Başvuru belgesi: https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure.


### <a name="error-code--sqloperationfailed"></a>Hata kodu: SqlOperationFailed

- **İleti**: `A database operation failed. Please search error to get more details.`

- **Neden**: hata Iletisinde "SqlException" varsa, SQL veritabanı belirli bir işlemi başarısız olduğunu belirten bir hata oluşturur.

- **Öneri**: lütfen daha fazla ayrıntı için bu başvuru BELGESI içindeki SQL hata kodu ile arama yapın: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Daha fazla yardıma ihtiyacınız varsa lütfen Azure SQL desteği 'ne başvurun.

- **Neden**: hata Iletisi "PdwManagedToNativeInteropException" içeriyorsa, genellikle kaynak ve havuz sütun boyutları arasında uyuşmazlık olur.

- **Öneri**: lütfen hem kaynak hem de havuz sütunlarının boyutunu denetleyin. Daha fazla yardıma ihtiyacınız varsa lütfen Azure SQL desteği 'ne başvurun.

- **Neden**: hata Iletisi "InvalidOperationException" içeriyorsa, genellikle geçersiz giriş verileri oluşur.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlunauthorizedaccess"></a>Hata kodu: SqlUnauthorizedAccess

- **İleti**: `Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Neden**: kimlik bilgisi yanlış veya oturum açma hesabı SQL veritabanına erişemiyor.

- **Öneri**: lütfen oturum açma hesabının SQL veritabanına erişmek için yeterli izni olduğundan emin olun.


### <a name="error-code--sqlopenconnectiontimeout"></a>Hata kodu: SqlOpenConnectionTimeout

- **İleti**: `Open connection to database timeout after '%timeoutValue;' seconds.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: lütfen bağlı hizmet bağlantı dizesini daha büyük bağlantı zaman aşımı değeri ile güncelleştirmeyi yeniden deneyin.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Hata kodu: SqlAutoCreateTableTypeMapFailed

- **İleti**: `Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(colunm name:'%colunmName;') in auto-create table.`

- **Neden**: otomatik oluşturma tablosu kaynak gereksinimini karşılamaz.

- **Öneri**: lütfen ' eşlemeler ' içindeki sütun türünü güncelleştirin veya hedef sunucuda havuz tablosunu el ile oluşturun.


### <a name="error-code--sqldatatypenotsupported"></a>Hata kodu: SqlDataTypeNotSupported

- **İleti**: `A database operation failed. Please check the SQL errors.`

- **Neden**: sorun SQL kaynağında olursa ve hata SqlDateTime overflow ile ilgiliyse, veri değeri Logic Type aralığı üzerinden (1/1/1753 12:00:00 ÖÖ-12/31/9999 11:59:59 PM) oluşur.

- **Öneri**: lütfen türü kaynak SQL sorgusunda dizeye atayın veya kopyalama etkinliği sütun eşlemesinde sütun türünü ' String ' olarak değiştirin.

- **Neden**: sorun SQL havuzunda gerçekleşirse ve hata SqlDateTime overflow ile ilgiliyse, veri değeri havuz tablosu 'nda izin verilen aralığın üzerinde olur.

- **Öneri**: lütfen havuz tablosunda karşılık gelen sütun türünü ' datetime2 ' türüne güncelleştirin.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Hata kodu: SqlInvalidDbStoredProcedure

- **İleti**: `The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Neden**: belirtilen saklı yordam geçerli değil. Bu, saklı yordamın herhangi bir veri döndürmemesinin nedeni olabilir.

- **Öneri**: lütfen SQL araçları 'nın saklı yordamını doğrulayın. Saklı yordamın verileri döndüre, emin olun.


### <a name="error-code--sqlinvaliddbquerystring"></a>Hata kodu: SqlInvalidDbQueryString

- **İleti**: `The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Neden**: belirtilen SQL sorgusu geçerli değil. Sorgunun herhangi bir veri döndürmediğinden kaynaklanabilir

- **Öneri**: lütfen SQL araçları Ile SQL sorgusunu doğrulayın. Sorgunun verileri döndüre, emin olun.


### <a name="error-code--sqlinvalidcolumnname"></a>Hata kodu: SqlInvalidColumnName

- **İleti**: `Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Neden**: sütun bulunamıyor. Olası yapılandırma yanlış.

- **Öneri**: lütfen sorgudaki sütunu, veri kümesindeki ' Structure ' öğesini ve etkinlik içindeki ' eşlemeler ' öğesini doğrulayın.


### <a name="error-code--sqlbatchwritetimeout"></a>Hata kodu: SqlBatchWriteTimeout

- **İleti**: `Timeout in SQL write opertaion.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: sorun yeniden üretme varsa lütfen Azure SQL desteği 'ne başvurun.


### <a name="error-code--sqlbatchwriterollbackfailed"></a>Hata kodu: SqlBatchWriteRollbackFailed

- **İleti**: `Timeout in SQL write operation and rollback also fail.`

- **Neden**: SQL veritabanı geçici hatası olabilir.

- **Öneri**: lütfen bağlı hizmet bağlantı dizesini daha büyük bağlantı zaman aşımı değeri ile güncelleştirmeyi yeniden deneyin.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Hata kodu: Sqlbulkcopyınvalidcolumnlength

- **İleti**: `SQL Bulk Copy failed due to received an invalid column length from the bcp client.`

- **Neden**: bcp istemcisinden geçersiz bir sütun uzunluğu aldığından SQL toplu kopyalama başarısız oldu.

- **Öneri**: sorunla ilgili hangi satırın olduğunu belirlemek için kopyalama etkinliğinde hata toleransı özelliğini etkinleştirin. Bu, daha fazla araştırma için sorunlu satırları depolamaya yeniden yönlendirebilir. Başvuru belgesi: https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlconnectionisclosed"></a>Hata kodu: Sqlconnectionısclosed

- **İleti**: `The connection is closed by SQL database.`

- **Neden**: yüksek eşzamanlı çalıştırma ve sunucu bağlantısı sona erdiğinde SQL bağlantısı SQL veritabanı tarafından kapalıdır.

- **Öneri**: uzak sunucu SQL bağlantısını kapatır. Lütfen yeniden deneyin. Sorun yeniden üretme varsa lütfen Azure SQL desteği 'ne başvurun.

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
            

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="error-code--azurebloboperationfailed"></a>Hata kodu: AzureBlobOperationFailed

- **İleti**: `Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Neden**: BLOB depolama işlemi isabet sorunu.

- **Öneri**: Lütfen ayrıntılar bölümündeki hatayı kontrol edin. Lütfen blob yardım belgesi: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codesbakın. Yardım gerekirse depolama ekibine başvurun.



## <a name="azure-data-lake-gen2"></a>Azure Data Lake Gen2

### <a name="error-code--adlsgen2operationfailed"></a>Hata kodu: AdlsGen2OperationFailed

- **İleti**: `ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Neden**: ADLS 2. işlem başarısız olduğunu gösteren hatayı oluşturur.

- **Öneri**: lütfen ADLS 2. tarafından oluşturulan ayrıntılı hata iletisini denetleyin. Geçici bir hata nedeniyle, lütfen yeniden deneyin. Daha fazla yardıma ihtiyacınız varsa lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.

- **Neden**: hata Iletisi ' yasak ' içerdiğinde, kullandığınız hizmet sorumlusu veya yönetilen kimlik ADLS 2. erişmek için yeterli izne sahip olmayabilir.

- **Öneri**: lütfen yardım belgesine başvurun: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication.

- **Neden**: hata Iletisi ' ınternalservererror ' içerdiğinde hata ADLS 2. tarafından döndürülür.

- **Öneri**: geçici bir hatadan kaynaklanıyor olabilir, lütfen yeniden deneyin. Sorun devam ederse lütfen Azure depolama desteği 'ne başvurun ve istek KIMLIĞINI hata iletisinde belirtin.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
            
