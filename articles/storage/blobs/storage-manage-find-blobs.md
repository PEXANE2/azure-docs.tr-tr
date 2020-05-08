---
title: Blob dizini (Önizleme) ile Azure Blob depolama üzerinde verileri yönetme ve bulma
description: Blob nesnelerini bulma, yönetme ve sorgulama için blob dizini etiketlerini nasıl kullanacağınızı öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: f1a4d9af8a1b1095527078dd790e80ef45a5ee9a
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722901"
---
# <a name="manage-and-find-data-on-azure-blob-storage-with-blob-index-preview"></a>Blob dizini (Önizleme) ile Azure Blob depolama üzerinde verileri yönetme ve bulma

Veri kümeleri daha büyük ve daha büyük olduğundan, veri Sea içindeki belirli bir nesnenin bulunması zor ve sinir bozucu olabilir. Blob dizini, tek bir kapsayıcı içinde veya depolama hesabınızdaki tüm kapsayıcılar üzerinde nesneleri kategorilere ayırmanıza ve bulmanıza olanak sağlayan anahtar-değer dizin etiketi özniteliklerini kullanarak veri yönetimi ve bulma özellikleri sağlar. Daha sonra, verilerin gereksinimleri değiştikçe nesneler, geçerli kapsayıcı kuruluşlarıyla yerinde kaldığında, dizin etiketleri güncelleştirilerek dinamik olarak yeniden kategorilere ayrılır. Blob dizininin kullanılmasıyla, BLOB verilerinizi ve ilişkili dizin özniteliklerini aynı hizmette birleştirerek geliştirme basitleşebilir; Yerel özellikleri kullanarak etkili ve ölçeklenebilir uygulamalar oluşturmanıza olanak tanır. 

Blob dizini şunları yapmanızı sağlar:

- Veri yönetimi için anahtar-değer dizin etiketlerini kullanarak bloblarınızı dinamik olarak kategorilere ayırın
- Tek bir kapsayıcı veya tüm depolama hesabı genelinde belirli etiketli Blobları hızlıca bulun
- Dizin etiketlerinin değerlendirmesini temel alan blob API 'Leri için koşullu davranışları belirtin
- [Yaşam döngüsü yönetimi](storage-lifecycle-management-concepts.md) gibi BLOB platformu özelliklerinin Gelişmiş denetimleri için Dizin etiketlerini kullanın

Depolama hesabınızda yazılmış ve birçok farklı uygulama tarafından erişilen milyonlarca blob 'un bulunduğu senaryoyu göz önünde bulundurun. Tek bir projeden ilgili tüm verileri bulmak istiyorsunuz, ancak veriler farklı blob adlandırma kurallarıyla birden çok kapsayıcıya yayılabilecek olduğundan kapsamda ne olduğundan emin değilsiniz. Ancak, uygulamalarınızın tüm verileri ilgili proje ve açıklama tanımlama özelliklerine göre karşıya yükleyeceğimizi bilirsiniz. Milyonlarca blob 'u aramak ve adları ve özellikleri karşılaştırmak yerine yalnızca bulma ölçütünüz olarak kullanabilirsiniz `Project = Contoso` . Blob dizini, tüm depolama hesabınızdaki tüm kapsayıcıları filtreleyecek ve yalnızca 50 Blobları kümesini hızlı bir şekilde bulup döndürecek `Project = Contoso`. 

Blob dizininin nasıl kullanılacağına ilişkin örneklere başlamak için bkz. [verileri yönetmek ve bulmak Için blob dizinini](storage-blob-index-how-to.md)kullanma.

## <a name="blob-index-tags-and-data-management"></a>Blob dizin etiketleri ve Veri Yönetimi

Kapsayıcı ve BLOB adı önekleri, depolanan verilerinize ait tek boyutlu kategorileştirsahiptir. Blob dizini artık, tüm [BLOB veri türleri (blok, ekleme veya sayfa)](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) için uygulanan öznitelik etiketleriyle çok boyutlu kategorilere izin veriyor. Bu çok boyutlu Kategori yerel olarak dizinlenir ve verilerinizi hızlı bir şekilde sorgulayabilir ve bulabilirsiniz.

Depolama hesabınızda aşağıdaki beş blobu göz önünde bulundurun:
>
> kapsayıcı1/Transaction. csv  
> container2/Campaign. docx  
> Fotoğraflar/bannerphoto. png  
> Arşivler/tamamlandı/2019review. PDF  
> Günlükler/2020/01/01/LogFile. txt  
>

Bu Bloblar Şu anda kapsayıcı/sanal klasör/blob adı öneki kullanılarak ayrılmıştır. Blob diziniyle, bu beş `Project = Contoso` blobda bir dizin etiketi özniteliğini, geçerli ön ek kuruluşlarını koruyarak birlikte kategorilere ayırmak için ayarlayabilirsiniz. Bu, depolama platformunun çok boyutlu dizinini kullanarak verileri filtreleme ve bulma özelliğini açığa çıkararak verileri taşıma gereksinimini ortadan kaldırır.

## <a name="setting-blob-index-tags"></a>Blob dizini etiketlerini ayarlama

Blob dizin etiketleri, depolama hesabınızda yeni veya mevcut nesnelere uygulanabilen anahtar-değer öznitelikleridir. PutBlob, PutBlockList veya CopyBlob işlemlerini ve isteğe bağlı x-MS-Tags üst bilgisini kullanarak karşıya yükleme işlemi sırasında dizin etiketlerini belirtebilirsiniz. Depolama hesabınızda zaten bloblarınız varsa, istek gövdesinde blob dizini etiketi özniteliklerini belirten biçimli bir XML belgesi ile SetBlobTags çağırabilirsiniz. 

Ayarlanacak aşağıdaki örnek etiketleri göz önünde bulundurun

Veri işlemenin ne zaman tamamlandığını anlatmak için bloba tek bir etiket uygulayabilirsiniz.
>
> "processedDate" = ' 2020-01-01 '
>
Verileri daha açıklayıcı bir şekilde yapmak için bloba birden çok etiket uygulayabilirsiniz.
>
> "Proje" = ' contoso '  
> "Sınıflandırılmış" = ' true '  
> "Durum" = ' Işlenmemiş '  
> "Öncelik" = ' 01 ' 
>

Var olan dizin etiketi özniteliklerini değiştirmek için, önce var olan etiket özniteliklerini almalısınız, Tag özniteliklerini değiştirmeli ve öğesini SetBlobTags işlemiyle değiştirmelisiniz. Blob 'dan tüm dizin etiketlerini kaldırmak için, etiket öznitelikleri belirtilmemiş SetBlobTags işlemini çağırın. Blob dizin etiketleri blob veri içeriklerine bir alt kaynak olduğundan, SetBlobTags temeldeki içerikleri değiştirmez ve Blobun son değiştirilme zamanını değiştirmez.

Aşağıdaki sınırlar blob dizin etiketleri için geçerlidir:
- Her blob en fazla 10 blob Dizin etiketine sahip olabilir
- Etiket anahtarları 1 ila 128 karakter arasında olmalıdır
- Etiket değerleri 0 ila 256 karakter arasında olmalıdır
- Etiket anahtarları ve değerleri büyük/küçük harfe duyarlıdır
- Etiket anahtarları ve değerleri yalnızca dize veri türlerini destekler; tüm sayılar veya özel karakterler dizeler olarak kaydedilir
- Etiket anahtarları ve değerleri aşağıdaki adlandırma kurallarına uymalıdır:
  - Alfa sayısal karakterler: a-z, A-Z, 0-9
  - Özel karakterler: boşluk, artı, eksi, nokta, iki nokta, eşittir, alt çizgi, eğik çizgi

## <a name="getting-and-listing-blob-index-tags"></a>Blob dizini etiketlerini alma ve listeleme

Blob dizin etiketleri, blob verileriyle birlikte alt kaynak olarak depolanır ve temel alınan blob veri içeriğinden bağımsız olarak alınabilir. Bir kez ayarlandıktan sonra, tek bir Blobun blob dizin etiketleri, GetBlobTags işlemiyle hemen alınabilir ve incelenebilir. WITH parametresi ile `include:tags` listbloblar işlemi, uygulanan blob Dizin etiketleriyle birlikte bir kapsayıcı içindeki tüm Blobları da döndürür. 

En az 1 blob Dizin etiketine sahip tüm Bloblar için x-MS-Tag-Count, blob üzerinde var olan blob dizini etiketlerinin sayısını belirten Listbloblar, GetBlob ve GetBlobProperties işlemlerinde döndürülür.

## <a name="finding-data-using-blob-index-tags"></a>Blob dizini etiketlerini kullanarak veri bulma

Bloblarda blob dizini etiketleriyle ayarlanan dizin oluşturma altyapısı, bu anahtar/değer özniteliklerini çok boyutlu bir dizin olarak gösterir. Dizin etiketlerinizin blob üzerinde var olmasına karşın hemen alınabilirken, blob dizini yenilenen Dizin etiketi öznitelikleriyle güncellenmeden önce bu işlem biraz zaman alabilir. Blob dizini güncelleştirildikten sonra, BLOB depolama tarafından sunulan yerel sorgu ve bulma olanaklarından faydalanabilirsiniz.

FindBlobsByTags işlemi, dizin etiketleri belirli bir blob dizin sorgu ifadesiyle eşleşen bir filtre uygulanmış blob dönüş kümesi almanızı sağlar. Blob dizini, Depolama hesabınızdaki tüm kapsayıcılarda filtrelemeyi destekler veya filtreleme kapsamını yalnızca tek bir kapsayıcıya atayabilirsiniz. Tüm blob dizini etiketi anahtarları ve değerleri dizeler olduğundan, desteklenen ilişkisel işleçler Dizin etiketi değerlerinde bir lexicographic sıralaması kullanır.

Blob dizini filtrelemesi için aşağıdaki ölçütler geçerlidir:
-   Etiket anahtarları çift tırnak içine alınmalıdır (")
-   Etiket değerleri ve kapsayıcı adları tek tırnak (') içine alınmalıdır
-   @ Karakterine yalnızca belirli bir kapsayıcı adı üzerinde filtreleme için izin verilir (yani, @container = ' ContainerName ')
- Filtreler, dizeler üzerinde lexıgrafgraphic sıralaması ile uygulanır
-   Aynı anahtar üzerinde aynı taraflı aralıklı işlemler geçersiz (yani "Rank" > ' 10 ' ve "Rank" >= ' 15 ')
- Bir filtre ifadesi oluşturmak için REST kullanılırken, karakterlerin URI kodlamalı olması gerekir

Aşağıdaki tabloda FindBlobsByTags için geçerli tüm işleçler gösterilmektedir:

|  İşleç  |  Açıklama  | Örnek |
|------------|---------------|---------|
|     =      |     Eşittir     | "Status" = ' sürüyor ' | 
|     >      |  Büyüktür |  "Date" > ' 2018-06-18 ' |
|     >=     |  Büyük veya eşittir | "Öncelik" >= ' 5 ' | 
|     <      |  Küçüktür    | "Age" < ' 32 ' |
|     <=     |  Küçük veya eşittir  | "Şirket" <= ' contoso ' |
|    AND     |  Mantıksal and  | "Rank" >= ' 010 ' ve "Rank" < ' 100 ' |
| @container |  Belirli bir kapsayıcının kapsamı   | @container= ' videof, ' ve "Status" = ' bitti ' |

## <a name="conditional-blob-operations-with-blob-index-tags"></a>Blob dizini etiketleriyle koşullu blob işlemleri
REST sürümleri 2019-10-10 ve üzeri sürümlerde, çoğu [BLOB hizmeti API 'leri](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs) artık, belirtilen blob dizini koşulu karşılandığında işlemin başarılı olabilmesi için bir koşullu üst bilgi olan x-MS-if-Tag ' i desteklemektedir. Koşul karşılanmazsa, alacaksınız `error 412: The condition specified using HTTP conditional header(s) is not met`.

X-MS-If-Tags üstbilgisi, var olan diğer HTTP koşullu üstbilgileri (IF-Match, If-None-Match, vb.) ile birleştirilebilir.  Bir istekte birden fazla koşullu başlık sağlanmışsa, işlemin başarılı olması için hepsi true olarak değerlendirilmelidir.  Tüm koşullu üstbilgiler mantıksal ve ile etkili bir şekilde birleştirilir. 

Aşağıdaki tabloda koşullu işlemlere yönelik tüm geçerli işleçler gösterilmektedir:

|  İşleç  |  Açıklama  | Örnek |
|------------|---------------|---------|
|     =      |     Eşittir     | "Status" = ' sürüyor ' |
|     <>     |   Eşit değildir   | "Durum"  <>  ' bitti '  | 
|     >      |  Büyüktür |  "Date" > ' 2018-06-18 ' |
|     >=     |  Büyük veya eşittir | "Öncelik" >= ' 5 ' | 
|     <      |  Küçüktür    | "Age" < ' 32 ' |
|     <=     |  Küçük veya eşittir  | "Şirket" <= ' contoso ' |
|    AND     |  Mantıksal and  | "Rank" >= ' 010 ' ve "Rank" < ' 100 ' |
|     OR     |  Mantıksal or   | "Status" = ' bitti ' veya "Priority" >= ' 05 ' |

> [!NOTE]
> Blob işlemi için koşullu x-MS-if-Tags üst bilgisinde izin verilen ancak FindBlobsByTags işleminde mevcut olmayan iki ek işleç vardır, buna eşittir ve mantıksal veya.

## <a name="platform-integrations-with-blob-index-tags"></a>Blob dizini etiketleriyle platform tümleştirmeleri

Blob dizininin etiketleri yalnızca BLOB verilerinizi sınıflandırmanız, yönetmeniz ve aramanız, ayrıca [yaşam döngüsü yönetimi](storage-lifecycle-management-concepts.md)gibi diğer blob hizmeti özellikleriyle tümleştirmeler sağlamanıza yardımcı olur. 

### <a name="lifecycle-management"></a>Yaşam döngüsü yönetimi

Yeni blobIndexMatch ' i yaşam döngüsü yönetiminde kural filtresi olarak kullanarak, verileri daha soğuk katmanlara taşıyabilir veya bloblarınıza uygulanan Dizin etiketlerine göre verileri silebilirsiniz. Bu, kurallarınızın daha ayrıntılı olmasını sağlar ve yalnızca belirtilen Etiketler ölçütleriyle eşleşiyorsa verileri taşıyabilir veya silebilirsiniz.

Etiketli verilere yönelik eylemler uygulamak için yaşam döngüsü kuralında bağımsız bir filtre kümesi olarak bir blob dizini eşleşmesi ayarlayabilirsiniz. Ya da, daha belirli veri kümeleriyle eşleşecek şekilde hem önek eşleşmesi hem de bir blob dizini eşleşmesi birleştirebilirsiniz. Bir yaşam döngüsü kuralına birden çok filtre uygulamak, eylemin yalnızca tüm filtre ölçütleri eşleşiyorsa uygulanması gibi mantıksal bir işlemdir. 

Aşağıdaki örnek yaşam döngüsü yönetim kuralı, ' videofler ' kapsayıcısının ve katman Bloblarındaki blok bloblarının yalnızca veri blobu Dizin etiketi ölçütleriyle eşleşiyorsa depolama alanını arşivlemek için geçerlidir ```"Status" = 'Processed' AND "Source" == 'RAW'```.

# <a name="portal"></a>[Portal](#tab/azure-portal)
![Azure portal 'de yaşam döngüsü yönetimi için blob dizin eşleştirme kuralı örneği](media/storage-blob-index-concepts/blob-index-lifecycle-management-example.png)
   
# <a name="json"></a>[JSON](#tab/json)
```json
{
    "rules": [
        {
            "enabled": true,
            "name": "ArchiveProcessedSourceVideos",
            "type": "Lifecycle",
            "definition": {
                "actions": {
                    "baseBlob": {
                        "tierToArchive": {
                            "daysAfterModificationGreaterThan": 0
                        }
                    }
                },
                "filters": {
                    "blobIndexMatch": [
                        {
                            "name": "Status",
                            "op": "==",
                            "value": "Processed"
                        },
                        {
                            "name": "Source",
                            "op": "==",
                            "value": "RAW"
                        }
                    ],
                    "blobTypes": [
                        "blockBlob"
                    ],
                    "prefixMatch": [
                        "videofiles/"
                    ]
                }
            }
        }
    ]
}
```
---

## <a name="permissions-and-authorization"></a>İzinler ve yetkilendirme

Aşağıdaki yaklaşımlardan birini kullanarak blob dizinine erişim yetkisi verebilirsiniz:

- Rol tabanlı erişim denetimi 'ni (RBAC) kullanarak Azure Active Directory (Azure AD) güvenlik sorumlusu için izin verin. Microsoft, üstün güvenlik ve kullanım kolaylığı için Azure AD 'nin kullanılmasını önerir. Azure AD 'yi blob işlemleriyle kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad.md).
- Blob dizinine erişim yetkisi vermek için paylaşılan erişim imzasını (SAS) kullanarak. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md).
- Paylaşılan anahtarla işlemleri yetkilendirmek için hesap erişim anahtarlarını kullanarak. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](/rest/api/storageservices/authorize-with-shared-key).

Blob dizin etiketleri, blob verilerine bir alt kaynaktır. Blob 'ları okuma veya yazma için izinleri olan bir kullanıcının veya bir SAS belirtecinin blob Dizin etiketlerine erişimi olmayabilir. 

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control 
[AAD kimliği](../common/storage-auth-aad.md) kullanan çağıranlar, blob dizini etiketlerinde çalışmak için aşağıdaki izinlere izin verebilir. 

|   Blob işlemleri   |  RBAC eylemi   |
|---------------------|----------------|
| Blob 'Ları etiketlere göre bul  | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/filtre |
| Blob etiketlerini ayarla         | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/Etiketler/yazma | 
| Blob etiketlerini al         | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/Etiketler/okuma |

Etiketlerin üzerinde çalışması için, temeldeki blob verilerinden ayrı ek izinler gerekir. Depolama Blobu veri katılımcısı rolüne bu izinlerin üçü de verilir. Depolama Blobu veri okuyucusuna, etiketlere göre yalnızca blob bulma ve BLOB etiketleri Al izinleri verilecektir.

### <a name="sas-permissions"></a>SAS Izinleri 
[Paylaşılan erişim imzası (SAS)](../common/storage-sas-overview.md) kullanan çağıranlara, blob etiketlerinde çalışmak için kapsamlı izinler verilebilir.
#### <a name="blob-sas"></a>Blob SAS
Blob dizini etiketlerine erişime izin vermek için bir blob hizmeti SAS içinde aşağıdaki izinler verilebilir. Blob okuma ve yazma izinleri tek başına Dizin etiketlerinin okunmasına veya yazılmasına izin vermek için yeterli değildir.

|  İzin  |  URI simgesi  | İzin verilen işlemler |
|--------------|--------------|--------------------|
|  Dizin etiketleri  |      t         | Blob Dizin etiketlerini al ve ayarla |

#### <a name="container-sas"></a>Kapsayıcı SAS
Blob etiketlerinde filtrelemeye izin vermek için kapsayıcı hizmeti SAS ' da aşağıdaki izinler verilebilir.  Blob listesi izni, Blobların Dizin etiketlerine göre filtrelemeye izin vermek için yeterli değil.

|  İzin  |  URI simgesi  | İzin verilen işlemler |
|--------------|--------------|--------------------|
| Dizin etiketleri     |      f       | Blob dizini etiketleriyle blob bulma | 

## <a name="choosing-between-metadata-and-blob-index-tags"></a>Meta veriler ve BLOB dizin etiketleri arasında seçim yapma 
Blob dizini etiketleri ve meta veriler, bir blob kaynağıyla birlikte rastgele Kullanıcı tanımlı anahtar/değer özelliklerini depolamanıza olanak sağlar. Her ikisi de, blob içeriğini döndürmeden veya değiştirmeden doğrudan alınabilir ve ayarlanabilir. Hem Metadata hem de dizin etiketlerini kullanmak mümkündür.

Ancak yalnızca blob dizini etiketleri otomatik olarak dizinlenir ve yerel blob hizmeti tarafından sorgulanabilir hale getirilir. [Azure Search](../../search/search-blob-ai-integration.md)gibi ayrı bir hizmet kullanmadığınız sürece meta veriler yerel olarak dizinlenemez ve sorgulanamaz. Blob dizini etiketlerinin Ayrıca, temel alınan blob verilerinden ayrı okuma/filtreleme ve yazma için ek izinleri vardır. Meta veriler blob ile aynı izinleri kullanır ve GetBlob veya GetBlobProperties işlemlerinde HTTP üstbilgileri olarak döndürülür. Blob dizin etiketleri, bir [Microsoft tarafından yönetilen anahtar](../common/storage-service-encryption.md) kullanılarak şifrelenir, ancak meta veriler, blob verileri için belirtilen şifreleme anahtarı kullanılarak, Rest 'te şifrelenir. 

Aşağıdaki tabloda meta veriler ve BLOB dizin etiketleri arasındaki farklılıklar özetlenmektedir:

|              |   Meta Veriler   |   Blob dizin etiketleri  |
|--------------|--------------|--------------------|
| **Sınırlar**         | Sayısal sınır yok; 8 KB toplam; büyük/küçük harf duyarsız | blob Max başına 10 etiket; etiket başına 768 bayt; büyük/küçük harfe duyarlı |
| **Güncelleştirmeler**      | Arşiv katmanında izin verilmiyor; SetBlobMetadata, mevcut tüm meta verileri değiştirir; SetBlobMetadata, Blobun son değiştirilme zamanını değiştirir | Tüm erişim katmanları için izin verilir; SetBlobTags tüm mevcut etiketlerin yerini alır; SetBlobTags Blobun son değiştirilme zamanını değiştirmez |
| **Depolama**        | Blob verileriyle birlikte depolanır |  Blob verilerine alt kaynak | 
| **Dizin oluşturma & sorgulama** | Yerel olarak yok; Azure Search gibi ayrı bir hizmet kullanılmalıdır | Evet, yerel dizin oluşturma ve BLOB depolamada yerleşik olarak sorgulama özellikleri |
| **Şifreleme** | Blob verileri için kullanılan aynı şifreleme anahtarıyla Rest 'de şifrelendi |  Microsoft tarafından yönetilen bir şifreleme anahtarıyla Rest 'te şifrelendi |
| **Fiyatlandırma**   | Meta verilerin boyutu bir blob için depolama maliyetlerine dahildir |    Dizin etiketi başına sabit maliyet | 
| **Üst bilgi yanıtı** | GetBlob ve GetBlobProperties içinde üst bilgi olarak döndürülen meta veriler | GetBlob veya GetBlobProperties; içinde TagCount döndürüldü Yalnızca GetBlobTags ve Listbloblarda döndürülen Etiketler |
| **İzinler**  |    Blob verilerine yönelik okuma veya yazma izinleri meta verilere genişletiliyor |    Etiketleri okumak/filtrelemek ya da yazmak için ek izinler gerekir |

## <a name="pricing"></a>Fiyatlandırma
Blob dizini fiyatlandırması Şu anda genel önizleme aşamasındadır ve genel kullanıma yönelik değişikliğe tabidir. Müşteriler, aylık olarak bir depolama hesabındaki blob dizini etiketlerinin toplam sayısı üzerinden ücretlendirilir. Dizin oluşturma altyapısının maliyeti yoktur. SetBlobTags, GetBlobTags ve FindBlobsByTags istekleri ilgili işlem türlerine göre ücretlendirilir. [Daha fazla bilgi için bkz. Blok Blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="regional-availability-and-storage-account-support"></a>Bölgesel kullanılabilirlik ve depolama hesabı desteği

Blob dizini şu anda yalnızca Genel Amaçlı v2 (GPv2) hesapları ile kullanılabilir. Azure portal, var olan bir Genel Amaçlı (GPv1) hesabını bir GPv2 hesabına yükseltebilirsiniz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

Genel önizlemede, blob dizini şu anda yalnızca aşağıdaki seçim bölgelerinde kullanılabilir:
- Orta Fransa
- Güney Fransa

Başlamak için bkz. [verileri yönetmek ve bulmak Için blob dizinini](storage-blob-index-how-to.md)kullanma.

> [!IMPORTANT]
> Bu makalenin koşullar bölümüne bakın. Önizlemeye kaydolmak için bu makalenin aboneliğinizi kaydetme bölümüne bakın. Depolama hesaplarınızda blob dizinini kullanabilmeniz için aboneliğinizi kaydetmeniz gerekir.

### <a name="register-your-subscription-preview"></a>Aboneliğinizi kaydetme (Önizleme)
Blob dizini yalnızca genel önizlemede olduğundan, özelliği kullanabilmeniz için aboneliğinizi kaydetmeniz gerekir. Bir istek göndermek için aşağıdaki PowerShell veya CLı komutlarını çalıştırın.

#### <a name="register-by-using-powershell"></a>PowerShell kullanarak kaydetme
```powershell
Register-AzProviderFeature -FeatureName BlobIndex -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

#### <a name="register-by-using-azure-cli"></a>Azure CLı kullanarak kaydolun
```azurecli
az feature register --namespace Microsoft.Storage --name BlobIndex
az provider register --namespace 'Microsoft.Storage'
```

## <a name="conditions-and-known-issues-preview"></a>Koşullar ve bilinen sorunlar (Önizleme)
Bu bölümde, blob dizininin geçerli genel önizlemesinin bilinen sorunları ve koşulları açıklanmaktadır. Çoğu önizlemede olduğu gibi, bu özellik, davranışlar değişene kadar, bu özellik, üretim iş yükleri için kullanılmamalıdır.

-   Önizleme için, önizleme bölgelerinde depolama hesabınız için blob dizinini kullanabilmeniz için önce aboneliğinizi kaydetmeniz gerekir.
-   Şu anda önizleme aşamasında yalnızca GPv2 hesapları desteklenmektedir. Blob, blok Blobstorage ve HNS etkin DataLake Gen2 hesapları Şu anda blob diziniyle desteklenmiyor.
-   Sayfa bloblarını Dizin etiketleriyle karşıya yüklemek şu anda etiketleri kalıcı tutmaz. Sayfa Blobu karşıya yükledikten sonra etiketleri ayarlamanız gerekir.
-   Filtreleme tek bir kapsayıcının @container kapsamına eklendiğinde, yalnızca filtre ifadesindeki tüm dizin etiketleri eşitlik denetimleri ise (anahtar = değer) geçirilebilir. 
-   VE koşulunun bulunduğu Aralık işlecini kullanırken, yalnızca aynı dizin etiketi anahtar adını (Age > ' 013 ' ve Age < ' 100 ') belirtebilirsiniz.
-   Sürüm oluşturma ve blob dizini şu anda desteklenmiyor. Blob dizin etiketleri sürümler için korunur ancak şu anda blob dizin altyapısına geçirilmiyor.
-   Hesap yük devretmesi Şu anda desteklenmiyor. Blob dizini yük devretmeden sonra düzgün şekilde güncelleştirilemez.
-   Yaşam döngüsü yönetimi şu anda yalnızca blob dizini eşleşmesi olan eşitlik denetimlerini destekliyor.
-   CopyBlob, blob dizini etiketlerini kaynak blobundan yeni hedef bloba kopyalamaz. Kopyalama işlemi sırasında hedef bloba uygulanmasını istediğiniz etiketleri belirtebilirsiniz. 
-   Anlık görüntü oluşturulurken Etiketler kalıcıdır; ancak anlık görüntünün yükseltme işlemi şu anda desteklenmiyor ve boş bir etiket kümesine neden olabilir.

## <a name="faq"></a>SSS

### <a name="can-blob-index-help-me-filter-and-query-content-inside-my-blobs"></a>Blob dizini, bloblarım içindeki içeriği filtrelemenize ve sorgularıma yardımcı olur mu? 
Hayır, blob dizin etiketleri Aradığınız Blobları bulmanıza yardımcı olabilir. Bloblarınızda arama yapmanız gerekiyorsa sorgu hızlandırma veya Azure Search kullanın.

### <a name="are-blob-index-tags-and-azure-resource-manager-tags-related"></a>Blob dizin etiketleri ve ilişkili Etiketler Azure Resource Manager?
Hayır, Azure Resource Manager Etiketler, abonelikler, kaynak grupları ve depolama hesapları gibi denetim düzlemi kaynaklarını düzenlemenize yardımcı olur. Blob dizin etiketleri, bir depolama hesabındaki Bloblar gibi veri düzlemi kaynaklarında nesne yönetimi ve bulma sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Blob dizini nasıl kullanacağınızı gösteren bir örneğe bakın. Bkz. [verileri yönetmek ve bulmak Için blob dizinini](storage-blob-index-how-to.md) kullanma

