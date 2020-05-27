---
title: İsteğe bağlı SQL için depolama hesabı erişimini denetleme (Önizleme)
description: Azure 'da isteğe bağlı SQL (Önizleme) Azure depolama 'nın nasıl eriştiği ve Azure SYNAPSE Analytics 'te isteğe bağlı SQL için depolama erişimini nasıl denetleyebileceğinizi açıklar.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 7d9157993e8cdbb6f7976ee2d4ce67b9039e7b52
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835844"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için depolama hesabı erişimini denetleme (Önizleme)

İsteğe bağlı bir SQL sorgusu, dosyaları doğrudan Azure Storage 'dan okur. Azure Storage 'daki dosyalara erişim izinleri iki düzeyde denetlenir:
- **Depolama düzeyi** -kullanıcının temeldeki depolama dosyalarına erişim izni olmalıdır. Depolama yöneticiniz, Azure AD Principal 'ın dosyaları okumasına/yazmasına veya depolamaya erişmek için kullanılacak SAS anahtarı oluşturmasına izin verir.
- **SQL hizmet düzeyi** -kullanıcının `SELECT` [dış tablo](develop-tables-external-tables.md) veya yürütme izninden veri okuma izni olması `ADMINISTER BULK ADMIN` `OPENROWSET` ve ayrıca depolama erişimi için kullanılacak kimlik bilgilerini kullanma izni olması gerekir.

Bu makalede, kullanabileceğiniz kimlik bilgileri türleri ve SQL ve Azure AD kullanıcıları için kimlik bilgileri aramasının nasıl yapılacağı açıklanır.

## <a name="supported-storage-authorization-types"></a>Desteklenen depolama yetkilendirme türleri

İsteğe bağlı bir SQL kaynağında oturum açan bir kullanıcının, dosyalar herkese açık değilse Azure Storage 'daki dosyalara erişme ve bunları sorgulama yetkisi olmalıdır. Genel olmayan depolama- [Kullanıcı kimliği](?tabs=user-identity), [paylaşılan erişim Imzası](?tabs=shared-access-signature)ve [yönetilen kimliğe](?tabs=managed-identity)erişmek için üç yetkilendirme türü kullanabilirsiniz.

> [!NOTE]
> [Azure AD geçiş](#force-azure-ad-pass-through) , bir çalışma alanı oluşturduğunuzda varsayılan davranıştır. Kullanıyorsanız, Azure AD oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez. [Bu davranışı devre dışı](#disable-forcing-azure-ad-pass-through)bırakabilirsiniz.

### <a name="shared-access-signature"></a>[Paylaşılan erişim imzası](#tab/shared-access-signature)

**Paylaşılan erişim imzası (SAS)** , bir depolama hesabındaki kaynaklara temsilci erişimi sağlar. SAS ile müşteri, istemcilere, hesap anahtarlarını paylaşmadan bir depolama hesabındaki kaynaklara erişim izni verebilir. SAS, geçerlilik aralığı, verilen izinler, kabul edilebilir IP adresi aralığı ve kabul edilebilir protokol (https/http) dahil olmak üzere SAS 'si olan istemcilere verdiğiniz erişim türü üzerinde ayrıntılı denetim sağlar.

**Azure portal > depolama hesabı-> paylaşılan erişim imzası-> Izinleri yapılandırma-> SAS ve bağlantı dizesi oluşturma '** ya gıderek bir SAS belirteci alabilirsiniz.

> [!IMPORTANT]
> Bir SAS belirteci oluşturulduğunda, belirtecin başlangıcında bir soru işareti ('? ') içerir. Belirteci SQL isteğe bağlı olarak kullanmak için, bir kimlik bilgisi oluştururken soru işaretini ('? ') kaldırmanız gerekir. Örneğin:
>
> SAS belirteci:? ZF = 2018-03-28&SS = bfqt&SRT = SCO&SP = rwdlacup&se = 2019-04-18T20:42:12Z&St = 2019-04-18T12:42:12Z&spr = https&SIG = lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78% 3D

SAS belirtecini kullanarak erişimi etkinleştirmek için veritabanı kapsamlı veya sunucu kapsamlı kimlik bilgileri oluşturmanız gerekir.

### <a name="user-identity"></a>[Kullanıcı kimliği](#tab/user-identity)

"Doğrudan geçiş" olarak da bilinen **Kullanıcı kimliği**, veri erişimine yetki vermek için Isteğe bağlı SQL 'de oturum açan Azure AD kullanıcısının kimliğinin kullanıldığı bir yetkilendirme türüdür. Verilere erişmeden önce Azure depolama yöneticisinin Azure AD kullanıcısına izin vermesi gerekir. Yukarıdaki tabloda gösterildiği gibi, SQL kullanıcı türü için desteklenmez.

> [!IMPORTANT]
> Verilerinize erişmek için kimliğinizi kullanabilmeniz için bir Depolama Blobu veri sahibi/katkıda bulunan/okuyucu rolüne sahip olmanız gerekir.
> Bir depolama hesabının sahibi olsanız bile, yine de Depolama Blobu veri rollerinden birine eklemeniz gerekir.
>
> Azure Data Lake Store Gen2 ' de erişim denetimi hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. makalesinde erişim denetimini](../../storage/blobs/data-lake-storage-access-control.md) gözden geçirin.
>

Azure AD kullanıcılarının kimliklerini kullanarak depolamaya erişmesini sağlamak için Azure AD geçişli kimlik doğrulamasını açıkça etkinleştirmeniz gerekir.

#### <a name="force-azure-ad-pass-through"></a>Azure AD geçişine zorla

Azure AD geçişini zorlamak, `UserIdentity` Azure SYNAPSE çalışma alanı sağlama sırasında otomatik olarak oluşturulan özel bır KIMLIK BILGISI adı tarafından elde edilen varsayılan bir davranıştır. Her Azure AD oturum açmanın her bir sorgusu için bir Azure AD geçişinin kullanımını zorlar ve bu da diğer kimlik bilgilerinin varlığına rağmen gerçekleşir.

> [!NOTE]
> Azure AD geçişi varsayılan bir davranıştır. AD oturum açmaları tarafından erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez.

[Her sorgu Için Azure AD geçişine zorlamanızı devre dışı](#disable-forcing-azure-ad-pass-through)bırakırsanız ve yeniden etkinleştirmek istiyorsanız şunu yürütün:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Belirli bir kullanıcı için bir Azure AD geçişinin zorlanmasına olanak tanımak için, söz konusu kullanıcının kimlik bilgileri için başvuru izni verebilirsiniz `UserIdentity` . Aşağıdaki örnek, user_name için bir Azure AD geçişine zorlamanıza izin verebilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

#### <a name="disable-forcing-azure-ad-pass-through"></a>Azure AD geçişine zorlayarak devre dışı bırak

[Her sorgu Için Azure AD geçişine zorlayarak](#force-azure-ad-pass-through)devre dışı bırakabilirsiniz. Devre dışı bırakmak için, şunu `Userdentity` kullanarak kimlik bilgisini bırakın:

```sql
DROP CREDENTIAL [UserIdentity];
```

Yeniden etkinleştirmek istiyorsanız, [Azure AD geçiş 'yi zorlama](#force-azure-ad-pass-through) bölümüne bakın.

### <a name="managed-identity"></a>[Yönetilen Kimlik](#tab/managed-identity)

**Yönetilen kimlik** de MSI olarak bilinir. İsteğe bağlı SQL için Azure hizmetleri sağlayan Azure Active Directory (Azure AD) özelliğidir. Ayrıca, Azure AD 'de otomatik olarak yönetilen bir kimlik dağıtır. Bu kimlik, Azure Storage 'da veri erişimi isteğine yetki vermek için kullanılabilir.

Verilere erişmeden önce Azure depolama yöneticisinin verilere erişim için yönetilen kimliğe izinler vermesi gerekir. Yönetilen kimliğe izin verilmesi, diğer Azure AD kullanıcıları için izin verme ile aynı şekilde yapılır.

### <a name="anonymous-access"></a>[Anonim erişim](#tab/public-access)

[Anonim erişime izin](/azure/storage/blobs/storage-manage-access-to-resources)veren Azure depolama hesaplarına yerleştirilmiş genel kullanıma açık dosyalara erişebilirsiniz.

---

### <a name="supported-authorization-types-for-databases-users"></a>Veritabanları kullanıcıları için desteklenen yetkilendirme türleri

Aşağıdaki tabloda kullanılabilir yetkilendirme türlerini bulabilirsiniz:

| Yetki türü                    | *SQL kullanıcısı*    | *Azure AD kullanıcısı*     |
| ------------------------------------- | ------------- | -----------    |
| [Kullanıcı kimliği](?tabs=user-identity#supported-storage-authorization-types)       | Desteklenmiyor | Destekleniyor      |
| ['LARıNıN](?tabs=shared-access-signature#supported-storage-authorization-types)       | Destekleniyor     | Destekleniyor      |
| [Yönetilen Kimlik](?tabs=managed-identity#supported-storage-authorization-types) | Desteklenmiyor | Destekleniyor      |

### <a name="supported-storages-and-authorization-types"></a>Desteklenen depolama alanları ve yetkilendirme türleri

Aşağıdaki yetkilendirme ve Azure Depolama türleri birleşimlerini kullanabilirsiniz:

|                     | Blob Depolama   | ADLS 1.        | ADLS 2.     |
| ------------------- | ------------   | --------------   | -----------   |
| *'LARıNıN*               | Destekleniyor      | Desteklenmiyor   | Destekleniyor     |
| *Yönetilen kimlik* | Destekleniyor      | Destekleniyor        | Destekleniyor     |
| *Kullanıcı kimliği*    | Destekleniyor      | Destekleniyor        | Destekleniyor     |

## <a name="credentials"></a>Kimlik bilgileri

Azure depolama 'da bulunan bir dosyayı sorgulamak için, SQL isteğe bağlı uç noktanağınızın kimlik doğrulama bilgilerini içeren bir kimlik bilgisi olması gerekir. İki tür kimlik bilgisi kullanılır:
- Sunucu düzeyindeki KIMLIK BILGILERI, işlev kullanılarak yürütülen geçici sorgular için kullanılır `OPENROWSET` . Kimlik bilgisi adının depolama URL 'siyle eşleşmesi gerekir.
- VERITABANı KAPSAMLı KIMLIK BILGILERI dış tablolar için kullanılır. Dış tablo `DATA SOURCE` , depolamaya erişmek için kullanılması gereken kimlik bilgileri ile başvurur.

Bir kullanıcının kimlik bilgisini oluşturmasına veya çalıştırmasına izin vermek için yönetici, bir kullanıcıya HERHANGI bir KIMLIK BILGISI izni VEREBILIR/REDDEDEBILIR:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

Dış depolamaya erişen veritabanı kullanıcılarının kimlik bilgilerini kullanma izni olmalıdır.

### <a name="grant-permissions-to-use-credential"></a>Kimlik bilgilerini kullanmak için izinleri verme

Kimlik bilgisini kullanmak için, bir kullanıcının `REFERENCES` belirli bir kimlik bilgisi üzerinde izni olması gerekir. Bir `REFERENCES` specific_user için storage_credential izin vermek için şunu yürütün:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Sorunsuz bir Azure AD geçiş deneyimi sağlamak için, tüm kullanıcıların varsayılan olarak kimlik bilgisini kullanma hakkı olur `UserIdentity` . Bu, Azure SYNAPSE çalışma alanı sağlama sırasında aşağıdaki deyimin otomatik olarak yürütülmesi ile elde edilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="server-scoped-credential"></a>Sunucu kapsamlı kimlik bilgisi

Sunucu kapsamlı kimlik bilgileri, SQL oturum açma `OPENROWSET` işlevi `DATA_SOURCE` bir depolama hesabındaki dosyaları okumak zorunda kalmadan çalıştırıldığında kullanılır. Sunucu kapsamlı kimlik bilgisinin adı, Azure depolama 'nın URL 'siyle aynı **olmalıdır** . Kimlik bilgisi [Oluştur](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)çalıştırılarak bir kimlik bilgisi eklenir. KIMLIK BILGISI adı bağımsız değişkeni sağlamanız gerekir. Bu, yolun bir bölümüyle veya depolama alanındaki verilerin tamamına uymalıdır (aşağıya bakın).

> [!NOTE]
> ŞIFRELEME sağlayıcısı IÇIN bağımsız değişken desteklenmiyor.

Sunucu düzeyi KIMLIK BILGISI adı, depolama hesabının (ve isteğe bağlı olarak kapsayıcı) tam yoluyla aşağıdaki biçimde olmalıdır: `<prefix>://<storage_account_path>/<storage_path>` . Depolama hesabı yolları aşağıdaki tabloda açıklanmıştır:

| Dış veri kaynağı       | Ön ek | Depolama hesabı yolu                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Depolama         | https  | <storage_account>. blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <storage_account>. azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <storage_account>. dfs.core.windows.net              |

> [!NOTE]
> `UserIdentity` [Azure AD geçişine zorlayan](?tabs=user-identity#force-azure-ad-pass-through)özel sunucu düzeyi kimlik bilgileri vardır.

Sunucu kapsamlı kimlik bilgileri, aşağıdaki kimlik doğrulama türlerini kullanarak Azure depolama 'ya erişim sağlar:

### <a name="shared-access-signature"></a>[Paylaşılan erişim imzası](#tab/shared-access-signature)

Aşağıdaki betik, `OPENROWSET` SAS belirtecini kullanarak Azure Storage 'daki herhangi bir dosyaya erişmek için işlev tarafından kullanılabilecek bir sunucu düzeyi kimlik bilgisi oluşturur. Bu kimlik bilgisini, `OPENROWSET` kimlik bilgisi ADıNDAKI URL ile eşleşen Azure depolama 'DA SAS anahtarıyla korunan dosyaları okumak için işlevi yürüten SQL sorumlusunu etkinleştirmek üzere oluşturun.

Gerçek depolama hesabı adınızla birlikte Exchange <*mystorageaccountname*> ve *mystorageaccountcontainername*> gerçek kapsayıcı adıyla <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="user-identity"></a>[Kullanıcı kimliği](#tab/user-identity)

Aşağıdaki betik, kullanıcının Azure AD kimlik kullanarak taklit etmesine olanak tanıyan sunucu düzeyinde bir kimlik bilgisi oluşturur.

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

### <a name="managed-identity"></a>[Yönetilen Kimlik](#tab/managed-identity)

Aşağıdaki betik, `OPENROWSET` çalışma alanı yönetilen kimliği kullanarak Azure Storage 'daki herhangi bir dosyaya erişmek için işlev tarafından kullanılabilecek bir sunucu düzeyi kimlik bilgisi oluşturur.

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='Managed Identity'
```

### <a name="public-access"></a>[Genel erişim](#tab/public-access)

Aşağıdaki betik, `OPENROWSET` genel olarak kullanılabilir Azure Storage 'daki herhangi bir dosyaya erişmek için işlev tarafından kullanılabilecek bir sunucu düzeyi kimlik bilgisi oluşturur. Bu kimlik bilgisini, çalışan SQL sorumlusunu, `OPENROWSET` kimlik bilgisi ADıNDAKI URL ile eşleşen Azure depolama üzerinde herkese açık dosyaları okumak üzere çalıştıran SQL sorumlusu 'nı etkinleştirmek için oluşturun.

Gerçek depolama hesabı adınızla Exchange <*mystorageaccountname*> ve gerçek kapsayıcı adı ile *mystorageaccountcontainername*> <sahip olmanız gerekir:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = '';
GO
```
---

## <a name="database-scoped-credential"></a>Veritabanı kapsamlı kimlik bilgileri

Veritabanı kapsamlı kimlik bilgileri, herhangi bir asıl çağrı çalıştırıldığında `OPENROWSET` `DATA_SOURCE` veya ortak dosyalara erişlemeyen [dış tablodaki](develop-tables-external-tables.md) verileri seçtiğinde kullanılır. Veritabanı kapsamlı kimlik bilgisinin, depolama konumunu tanımlayan VERI KAYNAĞıNDA açık olarak kullanılacağı için depolama hesabı adıyla eşleşmesi gerekmez.

Veritabanı kapsamlı kimlik bilgileri, aşağıdaki kimlik doğrulama türlerini kullanarak Azure depolama 'ya erişim sağlar:

### <a name="shared-access-signature"></a>[Paylaşılan erişim imzası](#tab/shared-access-signature)

Aşağıdaki betik, kimlik bilgilerinde belirtilen SAS belirtecini kullanarak depolamadaki dosyalara erişmek için kullanılan bir kimlik bilgisi oluşturur.

```sql
CREATE DATABASE SCOPED CREDENTIAL [SasToken]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="azure-ad-identity"></a>[Azure AD kimliği](#tab/user-identity)

Aşağıdaki betik, [external table](develop-tables-external-tables.md) `OPENROWSET` kendi Azure AD kimliklerini kullanarak depolama dosyalarına erişmek için kimlik bilgileriyle veri kaynağı kullanan dış tablo ve işlevler tarafından kullanılan veritabanı kapsamlı bir kimlik bilgisi oluşturur.

```sql
CREATE DATABASE SCOPED CREDENTIAL [AzureAD]
WITH IDENTITY = 'User Identity';
GO
```

### <a name="managed-identity"></a>[Yönetilen Kimlik](#tab/managed-identity)

Aşağıdaki betik, geçerli Azure AD kullanıcısının yönetilen hizmet kimliği olarak taklit etmek için kullanılabilecek veritabanı kapsamlı bir kimlik bilgisi oluşturur. 

```sql
CREATE DATABASE SCOPED CREDENTIAL [SynapseIdentity]
WITH IDENTITY = 'Managed Identity';
GO
```

Veritabanı kapsamlı kimlik bilgisinin, depolama konumunu tanımlayan VERI KAYNAĞıNDA açık olarak kullanılacağı için depolama hesabı adıyla eşleşmesi gerekmez.

### <a name="public-access"></a>[Genel erişim](#tab/public-access)

Genel kullanıma açık dosyalara erişime izin vermek için veritabanı kapsamlı kimlik bilgileri gerekli değildir. Azure depolama 'da genel kullanıma açık dosyalara erişmek için [veritabanı kapsamlı kimlik bilgileri olmadan veri kaynağı](develop-tables-external-tables.md?tabs=sql-ondemand#example-for-create-external-data-source) oluşturun.

---

Bu depolama alanına erişmek için hangi kimlik doğrulama yönteminin kullanılacağını belirtmek için, dış veri kaynaklarında veritabanı kapsamlı kimlik bilgileri kullanılır:

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples',
          CREDENTIAL = <name of database scoped credential> 
)
```

## <a name="examples"></a>Örnekler

**Genel kullanıma açık veri kaynağına erişme**

Genel olarak kullanılabilir veri kaynağına erişen bir tablo oluşturmak için aşağıdaki betiği kullanın.

```sql
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO
CREATE EXTERNAL DATA SOURCE publicData
WITH (    LOCATION   = 'https://****.blob.core.windows.net/public-access' )
GO

CREATE EXTERNAL TABLE dbo.userPublicData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [publicData], FILE_FORMAT = [SynapseParquetFormat] )
```

Veritabanı kullanıcısı, veri kaynağına başvuran dış tablo veya [OPENROWSET](develop-openrowset.md) işlevini kullanarak veri kaynağındaki dosyaların içeriğini okuyabilir:

```sql
SELECT TOP 10 * FROM dbo.userPublicData;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

**Kimlik bilgisi kullanarak veri kaynağına erişme**

SAS belirtecini, Kullanıcı Azure AD kimliğini veya çalışma alanının yönetilen kimliğini kullanarak Azure depolama 'ya erişen bir dış tablo oluşturmak için aşağıdaki betiği değiştirin.

```sql
-- Create master key in databases with some password (one-off per database)
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Y*********0'
GO

-- Create databases scoped credential that use User Identity, Managed Identity, or SAS. User needs to create only database-scoped credentials that should be used to access data source:

CREATE DATABASE SCOPED CREDENTIAL MyIdentity WITH IDENTITY = 'User Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL WorkspaceIdentity WITH IDENTITY = 'Managed Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL SasCredential WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2019-10-1********ZVsTOL0ltEGhf54N8KhDCRfLRI%3D'

-- Create data source that one of the credentials above, external file format, and external tables that reference this data source and file format:

CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO

CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://*******.blob.core.windows.net/samples'
-- Uncomment one of these options depending on authentication method that you want to use to access data source:
--,CREDENTIAL = MyIdentity 
--,CREDENTIAL = WorkspaceIdentity 
--,CREDENTIAL = SasCredential 
)

CREATE EXTERNAL TABLE dbo.userData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FILE_FORMAT = [SynapseParquetFormat] )

```

Veritabanı kullanıcısı, veri kaynağına başvuran [dış tablo](develop-tables-external-tables.md) veya [OPENROWSET](develop-openrowset.md) işlevini kullanarak veri kaynağındaki dosyaların içeriğini okuyabilir:

```sql
SELECT TOP 10 * FROM dbo.userdata;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = [mysample], FORMAT=PARQUET) as rows;
GO
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda listelenen makaleler farklı klasör türlerini, dosya türlerini sorgulamayı ve görünümleri oluşturma ve kullanma hakkında bilgi edinmenize yardımcı olur:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](query-specific-files.md)
- [Parquet dosyalarını sorgulama](query-parquet-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
- [JSON dosyalarını sorgulama](query-json-files.md)
- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)
