---
title: İsteğe bağlı SQL için depolama hesabı erişimini kontrol edin (önizleme)
description: SQL isteğe bağlı (önizleme) Azure Depolama'ya nasıl eriştiğini ve Azure Synapse Analytics'te isteğe bağlı SQL için depolama erişimini nasıl denetlediğinizi açıklar.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0d2683091898e9c84457b3b538776f0e6b0469d4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424037"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL için depolama hesabı erişimini kontrol edin (önizleme)

İsteğe bağlı bir SQL (önizleme) sorgusu dosyaları doğrudan Azure Depolama'dan okur. Depolama hesabı SQL isteğe bağlı kaynağın dışında bir nesne olduğundan, uygun kimlik bilgileri gereklidir. Bir kullanıcı gerekli kimlik bilgilerini kullanmak için verilen geçerli izinlere ihtiyaç duyar. Bu makalede, kullanabileceğiniz kimlik bilgileri türleri ve SQL ve Azure AD kullanıcıları için kimlik bilgisi aramasının nasıl yürürlüğe konan olduğu açıklanmaktadır.

## <a name="supported-storage-authorization-types"></a>Desteklenen depolama yetkilendirme türleri

İsteğe bağlı bir SQL kaynağına giriş yapmış bir kullanıcının Azure Depolama'daki dosyalara erişme ve sorgu yetkisi ne kadar yetkilendirilmiş olması gerekir. Üç yetkilendirme türü desteklenir:

- [Paylaşılan erişim imzası](#shared-access-signature)
- [Yönetilen Kimlik](#managed-identity)
- [Kullanıcı Kimliği](#user-identity)

> [!NOTE]
> [Azure AD geçişi,](#force-azure-ad-pass-through) bir çalışma alanı oluşturduğunuzda varsayılan davranıştır. Bunu kullanırsanız, AD oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez. Bu [davranışı devre dışı atabilirsiniz.](#disable-forcing-azure-ad-pass-through)

Aşağıdaki tabloda desteklenen veya yakında desteklenecek farklı yetkilendirme türlerini bulacaksınız.

| Yetki türü                    | *SQL kullanıcısı*    | *Azure AD kullanıcısı*     |
| ------------------------------------- | ------------- | -----------    |
| [Sas](#shared-access-signature)       | Destekleniyor     | Destekleniyor      |
| [Yönetilen Kimlik](#managed-identity) | Desteklenmiyor | Desteklenmiyor  |
| [Kullanıcı Kimliği](#user-identity)       | Desteklenmiyor | Destekleniyor      |

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası

**Paylaşılan erişim imzası (SAS),** depolama hesabındaki kaynaklara temsilci erişimi sağlar. SAS ile müşteri, müşterilerin hesap anahtarlarını paylaşmadan bir depolama hesabındaki kaynaklara erişim izni verebilir. SAS, geçerlilik aralığı, verilen izinler, kabul edilebilir IP adresi aralığı ve kabul edilebilir protokol (https/http) dahil olmak üzere SAS'si olan istemcilere sağladığınız erişim türü üzerinde ayrıntılı denetim sağlar.

SAS ve **bağlantı dizesi oluşturma > İzinleri Yapılandırmaİze > Açık erişim imzası -> Paylaşılan erişim imzasına -Azure portalı -> Depolama Hesabı'nda** gezinerek bir SAS belirteci alabilirsiniz.

> [!IMPORTANT]
> Bir SAS belirteci oluşturulduğunda, belirteci başında bir soru işareti ('?') içerir. İsteğe bağlı SQL'deki belirteci kullanmak için, kimlik bilgisi oluştururken soru işaretini ('?') kaldırmanız gerekir. Örneğin:
>
> SAS jeton: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSSMANd0ef9BrIPBNJ3VYEIq78%3D

### <a name="user-identity"></a>Kullanıcı Kimliği

**Kullanıcı Kimliği**("geçiş" olarak da bilinir), sql'e isteğe bağlı olarak oturum açan Azure AD kullanıcısının kimliğinin veri erişimini yetkilendirmek için kullanıldığı bir yetkilendirme türüdür. Verilere erişmeden önce Azure Depolama yöneticisinin Azure AD kullanıcısına izin vermesi gerekir. Yukarıdaki tabloda belirtildiği gibi, SQL kullanıcı türü için desteklenmez.

> [!NOTE]
> [Azure AD geçişini](#force-azure-ad-pass-through) kullanıyorsanız, AD oturum açmaları kullanılarak erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez.

> [!IMPORTANT]
> Verilere erişmek için kimliğinizi kullanmak için Depolama Blob Veri Sahibi/Katılımcı/Okuyucu rolüne sahip olmanız gerekir.
> Bir Depolama Hesabısahibi olsanız bile, yine de kendinizi Depolama Blob Veri rollerinden birine eklemeniz gerekir.
>
> Azure Veri Gölü Deposu Gen2'de erişim denetimi hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2 makalesinde Erişim denetimini](../../storage/blobs/data-lake-storage-access-control.md) inceleyin.
>

### <a name="managed-identity"></a>Yönetilen Kimlik

**Yönetilen Kimlik,** MSI olarak da bilinir. Azure Active Directory'nin (Azure AD) bir özelliğidir ve SQL isteğe bağlı olarak Azure hizmetleri sağlar. Ayrıca, Azure AD'de otomatik olarak yönetilen bir kimlik dağıtır. Bu kimlik, Azure Depolama'da veri erişimi isteğini yetkilendirmek için kullanılabilir.

Verilere erişmeden önce Azure Depolama yöneticisinin verilere erişmek için Yönetilen Kimlik'e izin vermesi gerekir. Yönetilen Kimlik için izin verilmesi, diğer Azure AD kullanıcısına izin vermekle aynı şekilde yapılır.

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Azure Depolama'da bulunan bir dosyayı sorgulamak için, SQL isteğe bağlı bitiş noktanızın kimlik doğrulama bilgilerini içeren sunucu düzeyinde bir Kimlik Kimliğine ihtiyacı vardır. [CREATE CREDİlAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)çalıştırılarak bir kimlik bilgisi eklenir. Bir Kimlik AD bağımsız değişkeni sağlamanız gerekir. Yolun bir bölümüyle veya Depolama'daki verilere giden yolun tamamıyla eşleşmelidir (aşağıya bakın).

> [!NOTE]
> FOR CRYPTOGRAPHIC Provider argümanı desteklenmez.

Desteklenen tüm yetkilendirme türleri için kimlik bilgileri bir hesaba, kapsayıcıya, herhangi bir dizine (kök süzgecinden) veya tek bir dosyayı işaret edebilir.

Kimlik ADInin tam yolu kapsayıcıya, klasöre veya dosyaya aşağıdaki biçimde eşleştirmesi gerekir:`<prefix>://<storage_account_path>/<storage_path>`

| Dış Veri Kaynağı       | Ön ek | Depolama hesabı yolu                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Depolama         | https  | <storage_account>.blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <storage_account>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <storage_account>.dfs.core.windows.net              |

 '<storage_path>' depolama alanınızda okumak istediğiniz klasörü veya dosyayı gösteren bir yoldur.

> [!NOTE]
> [Azure AD geçişini zorlayan](#force-azure-ad-pass-through)özel Bir Kimlik AD'ı `UserIdentity` vardır. Lütfen sorguları yürücerken [kimlik bilgisi araması](#credential-lookup) üzerindeki etkisi hakkında bilgi edinin.

İsteğe bağlı olarak, bir kullanıcının bir kimlik bilgisi oluşturmasına veya bırakmasına izin vermek için, yönetici bir kullanıcıya HERHANGI bir Kimlik BELGESINI VERİTE/REDDEDEBILIR:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

### <a name="supported-storages-and-authorization-types"></a>Desteklenen depolama ve yetkilendirme türleri

Aşağıdaki yetkilendirme ve Azure Depolama türleri birleşimlerini kullanabilirsiniz:

|                     | Blob Depolama   | ADLS Gen1        | ADLS Gen2     |
| ------------------- | ------------   | --------------   | -----------   |
| *Sas*               | Destekleniyor      | Desteklenmiyor   | Destekleniyor     |
| *Yönetilen Kimlik* | Desteklenmiyor  | Desteklenmiyor    | Desteklenmiyor |
| *Kullanıcı Kimliği*    | Destekleniyor      | Destekleniyor        | Destekleniyor     |

### <a name="examples"></a>Örnekler

[Yetkilendirme türüne](#supported-storage-authorization-types)bağlı olarak, aşağıdaki T-SQL sözdizimini kullanarak kimlik bilgileri oluşturabilirsiniz.

**Paylaşılan Erişim İmzası ve Blob Depolama**

Gerçek *depolama hesap adınız* ile> <mystoragehesap adı ve gerçek konteyner adı ile> *mystorageaccountcontainername* <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Kullanıcı Kimliği ve Azure Veri Gölü Depolama Gen1**

Gerçek *depolama hesap adınız* ile> <mystoragehesap adı ve gerçek konteyner adı ile> *mystorageaccountcontainername* <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.azuredatalakestore.net/webhdfs/v1/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

**Kullanıcı Kimliği ve Azure Veri Gölü Depolama Gen2**

Gerçek *depolama hesap adınız* ile> <mystoragehesap adı ve gerçek konteyner adı ile> *mystorageaccountcontainername* <:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.dfs.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

## <a name="force-azure-ad-pass-through"></a>Zorla Azure AD geçişi

Azure AD geçişini zorlamak, `UserIdentity`Azure Sinaps çalışma alanı sağlama sırasında otomatik olarak oluşturulan özel bir Kimlik AD'ı tarafından elde edilen varsayılan bir davranıştır. Diğer kimlik bilgilerinin varlığına rağmen oluşacak her Azure AD girişinin her sorgusu için bir Azure AD geçişi kullanımını zorlar.

> [!NOTE]
> Azure AD geçişi varsayılan bir davranıştır. AD oturum açmaları tarafından erişilen her depolama hesabı için kimlik bilgileri oluşturmanız gerekmez.

[Her sorgu için Azure AD geçişini zorlamayı devre dışı bıraktıysanız](#disable-forcing-azure-ad-pass-through)ve yeniden etkinleştirmek istiyorsanız, çalıştırın:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Belirli bir kullanıcı için Azure AD geçişini zorlamak `UserIdentity` için, belirli bir kullanıcıya kimlik bilgisi için REFERANS izni verebilirsiniz. Aşağıdaki örnek, bir Azure AD geçişini user_name zorlamanızı sağlar:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

SQL on-demand'in kullanılacak kimlik bilgilerini nasıl bulduğu hakkında daha fazla bilgi için [bkz.](#credential-lookup)

## <a name="disable-forcing-azure-ad-pass-through"></a>Azure AD geçişini zorlamayı devre dışı

[Her sorgu için Azure AD geçişini zorlamayı](#force-azure-ad-pass-through)devre dışı kullanabilirsiniz. Devre dışı bırakmak için `Userdentity` kimlik bilgisi nin aşağıdakileri kullanarak bırakın:

```sql
DROP CREDENTIAL [UserIdentity];
```

Yeniden etkinleştirmek istiyorsanız, [Azure AD geçiş gücü](#force-azure-ad-pass-through) bölümüne bakın.

Belirli bir kullanıcı için Azure AD geçişini zorlamayı devre dışı katlamak `UserIdentity` için, belirli bir kullanıcının kimlik bilgisi için REFERANS iznini reddedebilirsiniz. Aşağıdaki örnek, bir user_name için zorunlu Azure AD geçişini devre dışı kalmaktadır:

```sql
DENY REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

SQL on-demand'in kullanılacak kimlik bilgilerini nasıl bulduğu hakkında daha fazla bilgi için [kimlik bilgisi aramasına](#credential-lookup)bakın.

## <a name="grant-permissions-to-use-credential"></a>Kimlik bilgisi kullanma izinleri verme

Kimlik bilgilerini kullanmak için, bir kullanıcının belirli bir kimlik bilgisi için REFERENCES iznine sahip olması gerekir. bir specific_user için bir storage_credential REFERANSLAR izni vermek için, yürütmek:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Sorunsuz bir Azure AD geçiş deneyimi sağlamak için, varsayılan olarak tüm kullanıcılar `UserIdentity` kimlik belgesini kullanma hakkına sahip olacaktır. Bu, Azure Synapse çalışma alanı sağlama ile ilgili aşağıdaki ifadenin otomatik olarak yürütülmesiyle elde edilir:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="credential-lookup"></a>Kimlik bilgisi araması

Sorguları yetkilendirirken, kimlik bilgisi araması bir depolama hesabına erişmek için kullanılır ve aşağıdaki kurallara dayanır:

- Kullanıcı Azure AD girişi olarak oturum açtı

  - Bir UserIdentity kimlik bilgisi varsa ve kullanıcının üzerinde başvuru izinleri varsa, Azure AD geçişi kullanılacaktır, aksi takdirde [yol ile kimlik bilgilerini ara](#lookup-credential-by-path)

- Kullanıcı SQL girişi olarak oturum açtı

  - [Yola göre arama kimlik bilgisi](#lookup-credential-by-path) kullanma

### <a name="lookup-credential-by-path"></a>Yola göre kimlik bilgisi arama

Azure AD geçişini zorlamak devre dışı bırakılırsa, kimlik bilgisi araması depolama yoluna (önce derinlik) ve bu kimlik bilgisi üzerinde BIR REFERANS izninin varlığına dayanır. Aynı dosyaya erişmek için kullanılabilecek birden çok kimlik bilgileri olduğunda, isteğe bağlı SQL en özel dosyayı kullanır.  

Aşağıda aşağıdaki dosya yolu üzerinde bir sorgu örneği: *account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX.ext*

Kimlik bilgisi araması şu sırayla tamamlanır:

```
account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX
account.dfs.core.windows.net/filesystem/folder1/.../folderN
account.dfs.core.windows.net/filesystem/folder1
account.dfs.core.windows.net/filesystem
account.dfs.core.windows.net
```

Bir kullanıcının kimlik bilgisi sayısı 5'te REFERANS izni yoksa, SQL on-demand kullanıcının REFERANSlar izni ni bulana kadar kullanıcının REFERANSlar iznini bulana kadar bir düzey daha yüksek bir kimlik bilgisi üzerinde REFERANS izni olup olmadığını denetler. Böyle bir izin bulunmazsa, bir hata iletisi döndürülür.

### <a name="credential-and-path-level"></a>Kimlik bilgisi ve yol düzeyi

İstediğiniz yol şekline bağlı olarak, sorguları çalıştırmak için aşağıdaki gereksinimler yerine getirilir:

- Sorgu birden çok dosyayı hedefliyorsa (joker kartlı veya joker olmayan klasörler), kullanıcının en azından kök dizin düzeyinde (kapsayıcı düzeyi) bir kimlik bilgisine erişimi olması gerekir. Listeleme dosyaları kök dizininden göreli olduğundan bu erişim düzeyi gereklidir (Azure Depolama sınırlamaları)
- Sorgu tek bir dosyayı hedefliyorsa, SQL isteğe bağlı olarak dosyaya doğrudan eriştiklerinden, yani klasörleri listelemeden herhangi bir düzeyde ki bir kimlik bilgisine erişmesi gerekir.

|                  | *Hesap* | *Kök dizini* | *Diğer dizinler* | *Dosya*        |
| ---------------- | --------- | ---------------- | --------------------- | ------------- |
| *Tek dosya*    | Destekleniyor | Destekleniyor        | Destekleniyor             | Destekleniyor     |
| *Birden çok dosya* | Destekleniyor | Destekleniyor        | Desteklenmiyor         | Desteklenmiyor |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda listelenen makaleler, farklı klasör türlerini, dosya türlerini nasıl sorguladığınızı ve görünümler oluşturmanızı ve kullanmanıza yardımcı olur:

- [Tek CSV dosyasorgulama](query-single-csv-file.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Belirli dosyaları sorgula](query-specific-files.md)
- [Parke dosyalarını sorgula](query-parquet-files.md)
- [Görünümoluşturma ve kullanma](create-use-views.md)
- [JSON dosyalarını sorgula](query-json-files.md)
- [Sorgu Parke iç içe türleri](query-parquet-nested-types.md)
