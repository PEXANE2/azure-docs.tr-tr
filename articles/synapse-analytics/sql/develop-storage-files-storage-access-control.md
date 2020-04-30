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
ms.openlocfilehash: 0d2683091898e9c84457b3b538776f0e6b0469d4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424037"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te isteğe bağlı SQL (Önizleme) için depolama hesabı erişimini denetleme

Bir SQL isteğe bağlı (Önizleme) sorgusu, dosyaları doğrudan Azure Storage 'dan okur. Depolama hesabı, isteğe bağlı SQL kaynağına ait bir nesne olduğundan, uygun kimlik bilgileri gereklidir. Kullanıcı, önkoşul kimlik bilgisini kullanmak için uygun izinlere sahip olmalıdır. Bu makalede, kullanabileceğiniz kimlik bilgileri türleri ve SQL ve Azure AD kullanıcıları için kimlik bilgileri aramasının nasıl yapılacağı açıklanır.

## <a name="supported-storage-authorization-types"></a>Desteklenen depolama yetkilendirme türleri

İsteğe bağlı bir SQL kaynağında oturum açan bir kullanıcının Azure Storage 'daki dosyalara erişme ve bunları sorgulama yetkisi olması gerekir. Üç yetkilendirme türü desteklenir:

- [Paylaşılan erişim imzası](#shared-access-signature)
- [Yönetilen Kimlik](#managed-identity)
- [Kullanıcı kimliği](#user-identity)

> [!NOTE]
> [Azure AD geçiş](#force-azure-ad-pass-through) , bir çalışma alanı oluşturduğunuzda varsayılan davranıştır. Kullanıyorsanız, AD oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez. [Bu davranışı devre dışı](#disable-forcing-azure-ad-pass-through)bırakabilirsiniz.

Aşağıdaki tabloda, desteklenen ya da yakında desteklenecek farklı yetkilendirme türlerini bulacaksınız.

| Yetki türü                    | *SQL kullanıcısı*    | *Azure AD kullanıcısı*     |
| ------------------------------------- | ------------- | -----------    |
| ['LARıNıN](#shared-access-signature)       | Destekleniyor     | Destekleniyor      |
| [Yönetilen Kimlik](#managed-identity) | Desteklenmiyor | Desteklenmiyor  |
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

> [!NOTE]
> [Azure AD geçişi](#force-azure-ad-pass-through) KULLANıYORSANıZ, ad oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez.

> [!IMPORTANT]
> Verilerinize erişmek için kimliğinizi kullanabilmeniz için bir Depolama Blobu veri sahibi/katkıda bulunan/okuyucu rolüne sahip olmanız gerekir.
> Bir depolama hesabının sahibi olsanız bile, yine de Depolama Blobu veri rollerinden birine eklemeniz gerekir.
>
> Azure Data Lake Store Gen2 ' de erişim denetimi hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. makalesinde erişim denetimini](../../storage/blobs/data-lake-storage-access-control.md) gözden geçirin.
>

### <a name="managed-identity"></a>Yönetilen Kimlik

**Yönetilen kimlik** de MSI olarak bilinir. İsteğe bağlı SQL için Azure hizmetleri sağlayan Azure Active Directory (Azure AD) özelliğidir. Ayrıca, Azure AD 'de otomatik olarak yönetilen bir kimlik dağıtır. Bu kimlik, Azure Storage 'da veri erişimi isteğine yetki vermek için kullanılabilir.

Verilere erişmeden önce Azure depolama yöneticisinin verilere erişim için yönetilen kimliğe izinler vermesi gerekir. Yönetilen kimliğe izin verilmesi, diğer Azure AD kullanıcıları için izin verme ile aynı şekilde yapılır.

## <a name="create-credentials"></a>Kimlik bilgileri oluştur

Azure depolama 'da bulunan bir dosyayı sorgulamak için, SQL isteğe bağlı uç noktanağınızın kimlik doğrulama bilgilerini içeren sunucu düzeyinde bir KIMLIK BILGISI olması gerekir. Kimlik bilgisi [Oluştur](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)çalıştırılarak bir kimlik bilgisi eklenir. KIMLIK BILGISI adı bağımsız değişkeni sağlamanız gerekir. Bu, yolun bir bölümüyle veya depolama alanındaki verilerin tamamına uymalıdır (aşağıya bakın).

> [!NOTE]
> ŞIFRELEME sağlayıcısı IÇIN bağımsız değişken desteklenmiyor.

Desteklenen tüm yetkilendirme türlerinde, kimlik bilgileri bir hesabı, kapsayıcıyı, herhangi bir dizini (kök olmayan) veya tek bir dosyayı işaret edebilir.

KIMLIK BILGISI adı, kapsayıcı, klasör veya dosyanın tam yolu ile eşleşmelidir ve aşağıdaki biçimde olmalıdır:`<prefix>://<storage_account_path>/<storage_path>`

| Dış veri kaynağı       | Ön ek | Depolama hesabı yolu                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Depolama         | https  | <storage_account>. blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <storage_account>. azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <storage_account>. dfs.core.windows.net              |

 ' <storage_path> ', depolama alanınızı, okumak istediğiniz klasörü veya dosyayı işaret eden bir yoldur.

> [!NOTE]
> [Azure AD geçişini zorlayan](#force-azure-ad-pass-through)özel `UserIdentity` kimlik bilgisi adı vardır. Sorgular yürütülürken [kimlik bilgileri aramasında](#credential-lookup) bulunan etkisi hakkında bilgi edinin.

İsteğe bağlı olarak, bir kullanıcının kimlik bilgisini oluşturmasına veya çalıştırmasına izin vermek için yönetici, bir kullanıcıya HERHANGI bir KIMLIK BILGISI izni VEREBILIR/REDDEDEBILIR:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

### <a name="supported-storages-and-authorization-types"></a>Desteklenen depolama alanları ve yetkilendirme türleri

Aşağıdaki yetkilendirme ve Azure Depolama türleri birleşimlerini kullanabilirsiniz:

|                     | Blob Depolama   | ADLS 1.        | ADLS 2.     |
| ------------------- | ------------   | --------------   | -----------   |
| *'LARıNıN*               | Destekleniyor      | Desteklenmiyor   | Destekleniyor     |
| *Yönetilen kimlik* | Desteklenmiyor  | Desteklenmiyor    | Desteklenmiyor |
| *Kullanıcı kimliği*    | Destekleniyor      | Destekleniyor        | Destekleniyor     |

### <a name="examples"></a>Örnekler

[Yetkilendirme türüne](#supported-storage-authorization-types)bağlı olarak, aşağıdaki T-SQL söz dizimini kullanarak kimlik bilgileri oluşturabilirsiniz.

**Paylaşılan erişim Imzası ve BLOB depolama**

Gerçek depolama hesabı adınızla birlikte Exchange <*mystorageaccountname*> ve *mystorageaccountcontainername*> gerçek kapsayıcı adıyla <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Kullanıcı kimliği ve Azure Data Lake Storage 1.**

Gerçek depolama hesabı adınızla birlikte Exchange <*mystorageaccountname*> ve *mystorageaccountcontainername*> gerçek kapsayıcı adıyla <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.azuredatalakestore.net/webhdfs/v1/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

**Kullanıcı kimliği ve Azure Data Lake Storage 2.**

Gerçek depolama hesabı adınızla birlikte Exchange <*mystorageaccountname*> ve *mystorageaccountcontainername*> gerçek kapsayıcı adıyla <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.dfs.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

## <a name="force-azure-ad-pass-through"></a>Azure AD geçişine zorla

Azure AD geçişini zorlamak, Azure SYNAPSE çalışma alanı sağlama sırasında otomatik olarak oluşturulan özel bir KIMLIK BILGISI `UserIdentity`adı tarafından elde edilen varsayılan bir davranıştır. Her Azure AD oturum açmanın her bir sorgusu için bir Azure AD geçişinin kullanımını zorlar ve bu da diğer kimlik bilgilerinin varlığına rağmen gerçekleşir.

> [!NOTE]
> Azure AD geçişi varsayılan bir davranıştır. AD oturum açmaları tarafından erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez.

[Her sorgu Için Azure AD geçişine zorlamanızı devre dışı](#disable-forcing-azure-ad-pass-through)bırakırsanız ve yeniden etkinleştirmek istiyorsanız şunu yürütün:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Belirli bir kullanıcı için bir Azure AD geçişinin zorlanmasına olanak tanımak için, söz konusu kullanıcının kimlik bilgileri `UserIdentity` için başvuru izni verebilirsiniz. Aşağıdaki örnek, user_name için bir Azure AD geçişine zorlamanıza izin verebilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

SQL istek üzerine kullanılacak kimlik bilgilerini bulma hakkında daha fazla bilgi için bkz. [kimlik bilgisi arama](#credential-lookup).

## <a name="disable-forcing-azure-ad-pass-through"></a>Azure AD geçişine zorlayarak devre dışı bırak

[Her sorgu Için Azure AD geçişine zorlayarak](#force-azure-ad-pass-through)devre dışı bırakabilirsiniz. Devre dışı bırakmak için, şunu `Userdentity` kullanarak kimlik bilgisini bırakın:

```sql
DROP CREDENTIAL [UserIdentity];
```

Yeniden etkinleştirmek istiyorsanız, [Azure AD geçiş 'yi zorlama](#force-azure-ad-pass-through) bölümüne bakın.

Belirli bir kullanıcı için Azure AD geçişine izin vermeyi devre dışı bırakmak için, belirli bir kullanıcının kimlik bilgilerinde `UserIdentity` başvuru iznini reddedebilirsiniz. Aşağıdaki örnek, bir user_name için Azure AD geçişine zorlayarak devre dışı bırakır:

```sql
DENY REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

SQL isteğe bağlı olarak kullanılacak kimlik bilgilerini bulma hakkında daha fazla bilgi için bkz. [kimlik bilgisi arama](#credential-lookup).

## <a name="grant-permissions-to-use-credential"></a>Kimlik bilgilerini kullanmak için izinleri verme

Kimlik bilgisini kullanmak için, kullanıcının belirli bir kimlik bilgisi üzerinde BAŞVURULARı olmalıdır. Bir specific_user storage_credential bir BAŞVURUYA izin vermek için şunu yürütün:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Sorunsuz bir Azure AD geçiş deneyimi sağlamak için, tüm kullanıcıların varsayılan olarak `UserIdentity` kimlik bilgisini kullanma hakkı olur. Bu, Azure SYNAPSE çalışma alanı sağlama sırasında aşağıdaki deyimin otomatik olarak yürütülmesi ile elde edilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="credential-lookup"></a>Kimlik bilgisi arama

Sorgular yetkilendirirken, bir depolama hesabına erişmek için kimlik bilgisi arama kullanılır ve aşağıdaki kurallara dayanır:

- Kullanıcı Azure AD oturum açma olarak oturum açtı

  - Bir UserIdentity kimlik bilgisi varsa ve kullanıcının bu sunucuda başvuru izinleri varsa, Azure AD geçişi kullanılacaktır, aksi takdirde [yola göre arama kimlik bilgileri](#lookup-credential-by-path)

- Kullanıcı SQL oturumu olarak oturum açtı

  - [Yola göre arama kimlik bilgilerini](#lookup-credential-by-path) kullan

### <a name="lookup-credential-by-path"></a>Yola göre arama kimlik bilgileri

Azure AD geçişini zorlamak devre dışı bırakıldıysa, kimlik bilgisi arama depolama yolu (ilk derinlik) ve bu kimlik bilgisi için bir başvurular izninin varlığına göre yapılır. Aynı dosyaya erişmek için kullanılabilecek birden çok kimlik bilgisi olduğunda isteğe bağlı SQL, en belirli olanı kullanır.  

Aşağıdaki dosya yolu üzerinde bir sorgu örneği aşağıda verilmiştir: *account.DFS.Core.Windows.net/FileSystem/folder1/.../folderN/fileX.ext*

Kimlik bilgisi arama şu sırada tamamlanır:

```
account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX
account.dfs.core.windows.net/filesystem/folder1/.../folderN
account.dfs.core.windows.net/filesystem/folder1
account.dfs.core.windows.net/filesystem
account.dfs.core.windows.net
```

Bir kullanıcının kimlik numarası 5 ' te başvuru yoksa, SQL isteğe bağlı olarak, kullanıcının başvurduğu kimlik bilgilerini bulana kadar kullanıcının bir düzey kimlik bilgisi üzerinde izin başvurusu olduğunu denetler. Böyle bir izin bulunmazsa bir hata mesajı döndürülür.

### <a name="credential-and-path-level"></a>Kimlik bilgisi ve yol düzeyi

İstediğiniz yol şekline bağlı olarak, sorguları çalıştırmak için aşağıdaki gereksinimler vardır:

- Sorgu birden çok dosyayı (joker karakter içeren veya olmayan klasörler) hedefliyorsanız, kullanıcının en azından kök dizin düzeyinde (kapsayıcı düzeyi) kimlik bilgilerine erişimi olması gerekir. Listeleme dosyaları kök dizinden (Azure Depolama sınırlamaları) göreli olduğundan bu erişim düzeyi gereklidir
- Sorgu tek bir dosyayı hedefliyorsanız, bir kullanıcının isteğe bağlı olarak, dosyaları listelemeden doğrudan, diğer bir deyişle, bir kimlik bilgisine erişimi olması gerekir.

|                  | *Hesap* | *Kök dizin* | *Diğer tüm dizin* | *Dosya*        |
| ---------------- | --------- | ---------------- | --------------------- | ------------- |
| *Tek dosya*    | Destekleniyor | Destekleniyor        | Destekleniyor             | Destekleniyor     |
| *Birden çok dosya* | Destekleniyor | Destekleniyor        | Desteklenmiyor         | Desteklenmiyor |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda listelenen makaleler farklı klasör türlerini, dosya türlerini sorgulamayı ve görünümleri oluşturma ve kullanma hakkında bilgi edinmenize yardımcı olur:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](query-specific-files.md)
- [Parquet dosyalarını sorgulama](query-parquet-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
- [JSON dosyalarını sorgulama](query-json-files.md)
- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)
