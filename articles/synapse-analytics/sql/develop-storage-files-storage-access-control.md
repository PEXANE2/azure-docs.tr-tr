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
ms.openlocfilehash: 6ebf23720d1d323b66671c6770ab2121c9091920
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83197697"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için depolama hesabı erişimini denetleme (Önizleme)

İsteğe bağlı bir SQL sorgusu, dosyaları doğrudan Azure Storage 'dan okur. Azure Storage 'daki dosyalara erişim izinleri iki düzeyde denetlenir:
- **Depolama düzeyi** -kullanıcının temeldeki depolama dosyalarına erişim izni olmalıdır. Depolama yöneticiniz, Azure AD sorumlusunun dosyaları okumasına/yazmasına izin verebilir veya depolama erişimi için kullanılacak SAS anahtarı oluşturabilir.
- **SQL hizmet düzeyi** -Kullanıcı `ADMINISTER BULK ADMIN` yürütme iznine sahip olmalıdır `OPENROWSET` ve depolama alanına erişmek için kullanılacak kimlik bilgilerini kullanma izni de vardır.

Bu makalede, kullanabileceğiniz kimlik bilgileri türleri ve SQL ve Azure AD kullanıcıları için kimlik bilgileri aramasının nasıl yapılacağı açıklanır.

## <a name="supported-storage-authorization-types"></a>Desteklenen depolama yetkilendirme türleri

İsteğe bağlı bir SQL kaynağında oturum açan bir kullanıcının Azure Storage 'daki dosyalara erişme ve bunları sorgulama yetkisi olması gerekir. Üç yetkilendirme türü desteklenir:

- [Paylaşılan erişim imzası](#shared-access-signature)
- [Yönetilen Kimlik](#managed-identity)
- [Kullanıcı kimliği](#user-identity)

> [!NOTE]
> [Azure AD geçiş](#force-azure-ad-pass-through) , bir çalışma alanı oluşturduğunuzda varsayılan davranıştır. Kullanıyorsanız, Azure AD oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez. [Bu davranışı devre dışı](#disable-forcing-azure-ad-pass-through)bırakabilirsiniz.

Aşağıdaki tabloda kullanılabilir yetkilendirme türlerini bulabilirsiniz:

| Yetki türü                    | *SQL kullanıcısı*    | *Azure AD kullanıcısı*     |
| ------------------------------------- | ------------- | -----------    |
| ['LARıNıN](#shared-access-signature)       | Destekleniyor     | Destekleniyor      |
| [Yönetilen Kimlik](#managed-identity) | Desteklenmiyor | Destekleniyor      |
| [Kullanıcı kimliği](#user-identity)       | Desteklenmiyor | Destekleniyor      |

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası

**Paylaşılan erişim imzası (SAS)** , bir depolama hesabındaki kaynaklara temsilci erişimi sağlar. SAS ile müşteri, istemcilere, hesap anahtarlarını paylaşmadan bir depolama hesabındaki kaynaklara erişim izni verebilir. SAS, geçerlilik aralığı, verilen izinler, kabul edilebilir IP adresi aralığı ve kabul edilebilir protokol (https/http) dahil olmak üzere SAS 'si olan istemcilere verdiğiniz erişim türü üzerinde ayrıntılı denetim sağlar.

**Azure portal > depolama hesabı-> paylaşılan erişim imzası-> Izinleri yapılandırma-> SAS ve bağlantı dizesi oluşturma '** ya gıderek bir SAS belirteci alabilirsiniz.

> [!IMPORTANT]
> Bir SAS belirteci oluşturulduğunda, belirtecin başlangıcında bir soru işareti ('? ') içerir. Belirteci SQL isteğe bağlı olarak kullanmak için, bir kimlik bilgisi oluştururken soru işaretini ('? ') kaldırmanız gerekir. Örneğin:
>
> SAS belirteci:? ZF = 2018-03-28&SS = bfqt&SRT = SCO&SP = rwdlacup&se = 2019-04-18T20:42:12Z&St = 2019-04-18T12:42:12Z&spr = https&SIG = lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78% 3D

### <a name="user-identity"></a>Kullanıcı kimliği

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

### <a name="managed-identity"></a>Yönetilen Kimlik

**Yönetilen kimlik** de MSI olarak bilinir. İsteğe bağlı SQL için Azure hizmetleri sağlayan Azure Active Directory (Azure AD) özelliğidir. Ayrıca, Azure AD 'de otomatik olarak yönetilen bir kimlik dağıtır. Bu kimlik, Azure Storage 'da veri erişimi isteğine yetki vermek için kullanılabilir.

Verilere erişmeden önce Azure depolama yöneticisinin verilere erişim için yönetilen kimliğe izinler vermesi gerekir. Yönetilen kimliğe izin verilmesi, diğer Azure AD kullanıcıları için izin verme ile aynı şekilde yapılır.

## <a name="credentials"></a>Kimlik bilgileri

Azure depolama 'da bulunan bir dosyayı sorgulamak için, SQL isteğe bağlı uç noktanağınızın kimlik doğrulama bilgilerini içeren bir kimlik bilgisi olması gerekir. İki tür kimlik bilgisi kullanılır:
- Sunucu düzeyindeki KIMLIK BILGILERI, işlev kullanılarak yürütülen geçici sorgular için kullanılır `OPENROWSET` . Kimlik bilgisi adının depolama URL 'siyle eşleşmesi gerekir.
- VERITABANı KAPSAMLı KIMLIK BILGILERI dış tablolar için kullanılır. Dış tablo `DATA SOURCE` , depolamaya erişmek için kullanılması gereken kimlik bilgileri ile başvurur.

Kimlik bilgisi [Oluştur](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)çalıştırılarak bir kimlik bilgisi eklenir. KIMLIK BILGISI adı bağımsız değişkeni sağlamanız gerekir. Bu, yolun bir bölümüyle veya depolama alanındaki verilerin tamamına uymalıdır (aşağıya bakın).

> [!NOTE]
> ŞIFRELEME sağlayıcısı IÇIN bağımsız değişken desteklenmiyor.

Desteklenen tüm yetkilendirme türleri için kimlik bilgileri bir hesabı veya kapsayıcıyı işaret edebilir.

Sunucu düzeyi KIMLIK BILGISI adı, depolama hesabının (ve isteğe bağlı olarak kapsayıcı) tam yoluyla aşağıdaki biçimde eşleşmelidir:`<prefix>://<storage_account_path>/<storage_path>`

| Dış veri kaynağı       | Ön ek | Depolama hesabı yolu                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Depolama         | https  | <storage_account>. blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <storage_account>. azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <storage_account>. dfs.core.windows.net              |


> [!NOTE]
> `UserIdentity` [Azure AD geçişine zorlayan](#force-azure-ad-pass-through)özel sunucu düzeyi kimlik bilgileri vardır.

İsteğe bağlı olarak, bir kullanıcının kimlik bilgisini oluşturmasına veya çalıştırmasına izin vermek için yönetici, bir kullanıcıya HERHANGI bir KIMLIK BILGISI izni VEREBILIR/REDDEDEBILIR:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

### <a name="supported-storages-and-authorization-types"></a>Desteklenen depolama alanları ve yetkilendirme türleri

Aşağıdaki yetkilendirme ve Azure Depolama türleri birleşimlerini kullanabilirsiniz:

|                     | Blob Depolama   | ADLS 1.        | ADLS 2.     |
| ------------------- | ------------   | --------------   | -----------   |
| *'LARıNıN*               | Destekleniyor      | Desteklenmiyor   | Destekleniyor     |
| *Yönetilen kimlik* | Destekleniyor      | Destekleniyor        | Destekleniyor     |
| *Kullanıcı kimliği*    | Destekleniyor      | Destekleniyor        | Destekleniyor     |


### <a name="grant-permissions-to-use-credential"></a>Kimlik bilgilerini kullanmak için izinleri verme

Kimlik bilgisini kullanmak için, kullanıcının belirli bir kimlik bilgisi üzerinde BAŞVURULARı olmalıdır. Bir specific_user storage_credential bir BAŞVURUYA izin vermek için şunu yürütün:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Sorunsuz bir Azure AD geçiş deneyimi sağlamak için, tüm kullanıcıların varsayılan olarak kimlik bilgisini kullanma hakkı olur `UserIdentity` . Bu, Azure SYNAPSE çalışma alanı sağlama sırasında aşağıdaki deyimin otomatik olarak yürütülmesi ile elde edilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

### <a name="examples"></a>Örnekler

[Yetkilendirme türüne](#supported-storage-authorization-types)bağlı olarak, aşağıdaki T-SQL söz dizimini kullanarak kimlik bilgileri oluşturabilirsiniz.
- Sunucu kapsamlı kimlik bilgileri, SQL oturum açma `OPENROWSET` işlevi `DATA_SOURCE` bir depolama hesabındaki dosyaları okumak zorunda kalmadan çalıştırıldığında kullanılır. Sunucu kapsamlı kimlik bilgisinin adı, Azure depolama 'nın URL 'siyle aynı **olmalıdır** .
- Veritabanı kapsamlı kimlik bilgileri, herhangi bir asıl çağrı çalıştırıldığında `OPENROWSET` `DATA_SOURCE` veya ortak dosyalara erişlemeyen dış tablodaki verileri seçtiğinde kullanılır. Veritabanı kapsamlı kimlik bilgisinin, depolama konumunu tanımlayan VERI KAYNAĞıNDA açık olarak kullanılacağı için depolama hesabı adıyla eşleşmesi gerekmez.

**BLOB depolama için paylaşılan erişim Imzasıyla sunucu kapsamlı kimlik bilgileri**

Aşağıdaki betik, SAS belirtecini kullanarak Azure Storage 'daki herhangi bir dosyaya erişmek için kullanılabilen sunucu düzeyinde bir kimlik bilgisi oluşturur. Bu kimlik bilgisini, `OPENROWSET` kimlik bilgisi ADıNDAKI URL ile eşleşen Azure depolama 'DA SAS anahtarıyla korunan dosyaları okumak için işlevi yürüten SQL sorumlusunu etkinleştirmek üzere oluşturun.

Gerçek depolama hesabı adınızla birlikte Exchange <*mystorageaccountname*> ve *mystorageaccountcontainername*> gerçek kapsayıcı adıyla <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Ortak depolamaya erişime izin veren sunucu kapsamlı kimlik bilgileri**

Aşağıdaki betik, genel olarak kullanılabilir Azure Storage 'daki herhangi bir dosyaya erişmek için kullanılabilen sunucu düzeyi bir kimlik bilgisi oluşturur. Bu kimlik bilgisini, çalışan SQL sorumlusunu, `OPENROWSET` kimlik bilgisi ADıNDAKI URL ile eşleşen Azure depolama üzerinde herkese açık dosyaları okumak üzere çalıştıran SQL sorumlusu 'nı etkinleştirmek için oluşturun.

Gerçek depolama hesabı adınızla Exchange <*mystorageaccountname*> ve gerçek kapsayıcı adı ile *mystorageaccountcontainername*> <sahip olmanız gerekir:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = '';
GO
```

**SAS belirteci ile veritabanı kapsamlı kimlik bilgileri**

Aşağıdaki betik, kimlik bilgilerinde belirtilen SAS belirtecini kullanarak depolamadaki dosyalara erişmek için kullanılan bir kimlik bilgisi oluşturur.

```sql
CREATE DATABASE SCOPED CREDENTIAL [SasToken]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Azure AD kimliğiyle veritabanı kapsamlı kimlik bilgileri**

Aşağıdaki betik, `OPENROWSET` kendi Azure AD kimliklerini kullanarak depolama dosyalarına erişmek için kimlik bilgileriyle veri kaynağı kullanan dış tablolar ve işlevler tarafından kullanılan bir kimlik bilgisi oluşturur.

```sql
CREATE DATABASE SCOPED CREDENTIAL [AzureAD]
WITH IDENTITY = 'User Identity';
GO
```

**Yönetilen kimliğe sahip veritabanı kapsamlı kimlik bilgileri**

Aşağıdaki betik, geçerli Azure AD kullanıcısının yönetilen hizmet kimliği olarak taklit etmek için kullanılabilecek bir kimlik bilgisi oluşturur. 

```sql
CREATE DATABASE SCOPED CREDENTIAL [SynapseIdentity]
WITH IDENTITY = 'Managed Identity';
GO
```

Veritabanı kapsamlı kimlik bilgisinin, depolama konumunu tanımlayan VERI KAYNAĞıNDA açık olarak kullanılacağı için depolama hesabı adıyla eşleşmesi gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda listelenen makaleler farklı klasör türlerini, dosya türlerini sorgulamayı ve görünümleri oluşturma ve kullanma hakkında bilgi edinmenize yardımcı olur:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](query-specific-files.md)
- [Parquet dosyalarını sorgulama](query-parquet-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
- [JSON dosyalarını sorgulama](query-json-files.md)
- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)
