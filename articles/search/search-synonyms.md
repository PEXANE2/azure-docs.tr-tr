---
title: Arama dizini üzerinden sorgu genişletme için eşanlamlı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinindeki arama sorgusunun kapsamını genişletmek için eşanlamlı bir eşleme oluşturun. Kapsam, bir listede sağladığınız eşdeğer terimleri içerecek şekilde genişletilir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/28/2020
ms.openlocfilehash: aa573e84fa9fff83bd6a894f516ce5f67b3afa79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194351"
---
# <a name="synonyms-in-azure-cognitive-search"></a>Azure Bilişsel Aramada Eş Anlamlı

Arama motorlarında eşanlamlılar, kullanıcının terimi gerçekten sağlamasına gerek kalmadan, sorgunun kapsamını dolaylı olarak genişleten eşdeğer terimleri ilişkilendirir. Örneğin, "köpek" ve "köpek" ve "köpek" eşanlamlı dernekleri göz önüne alındığında, "köpek", "köpek" veya "köpek" içeren herhangi bir belge sorgu kapsamına girer.

Azure Bilişsel Arama'da eş anlamlı genişletme sorgu zamanında yapılır. Varolan işlemleri kesintiye uğratmadan bir hizmete eş anlamlı eşlem eşler ekleyebilirsiniz. Dizin yeniden gerek kalmadan bir alan tanımına **eşanlamlı** haritalar ekleyebilirsiniz.

## <a name="create-synonyms"></a>Eş anlamlı lar oluşturma

Eşanlamlı oluşturmak için portal desteği yoktur, ancak REST API veya .NET SDK'yı kullanabilirsiniz. REST ile başlamak için [Postacı'yı kullanmanızı](search-get-started-postman.md) ve bu API'yi kullanarak isteklerin formülasyonlarını kullanmanızı öneririz: [Eş Anlamlı Haritalar Oluştur.](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map) C# geliştiricileri [için, C# kullanarak Azure Bilişsel Arama'da Eş Anlamlı Ekle](search-synonyms-tutorial-sdk.md)ile yola çıkabilirsiniz.

İsteğe bağlı olarak, hizmet tarafında şifreleme-at-rest için [müşteri tarafından yönetilen anahtarları](search-security-manage-encryption-keys.md) kullanıyorsanız, bu korumayı eşanlamlı haritanızın içeriğine uygulayabilirsiniz.

## <a name="use-synonyms"></a>Eş anlamlı ları kullanma

Azure Bilişsel Arama'da eş anlamlı destek, tanımladığınız ve hizmetinize yüklediğiniz eşanlamlı haritalara dayanır. Bu haritalar bağımsız bir kaynak (dizinler veya veri kaynakları gibi) oluşturur ve arama hizmetinizdeki herhangi bir dizinde aranabilir alan tarafından kullanılabilir.

Eşanlamlı haritalar ve dizinler bağımsız olarak korunur. Eş anlamlı bir harita tanımladıktan ve hizmetinize yükledikten sonra, alan tanımına **eş anlamlı** Haritalar adı verilen yeni bir özellik ekleyerek alandaki eş anlamlı özelliği etkinleştirebilirsiniz. Eşanlamlı eşlemi oluşturma, güncelleştirme ve silme işlemi her zaman bir tam belge işlemidir, yani eşanlamlı haritanın bölümlerini artımlı olarak oluşturamazsınız, güncelleyemez veya silemezsiniz. Tek bir girişi bile güncelleştirmek yeniden yükleme gerektirir.

Eş anlamlıları arama uygulamanıza dahil etmek iki aşamalı bir işlemdir:

1.  Aşağıdaki API'ler aracılığıyla arama hizmetinize eş anlamlı bir eşlenim haritası ekleyin.  

2.  Dizin tanımındaki eşanlamlı eşleni eşlemi kullanmak için aranabilir bir alanı yapılandırın.

Arama uygulamanız için birden çok eşanlamlı eşanlamlı harita oluşturabilirsiniz (örneğin, uygulamanız çok dilli bir müşteri tabanını destekliyorsa dile göre). Şu anda, bir alan yalnızca bunlardan birini kullanabilirsiniz. Bir alanın eşanlamlısı Haritalar özelliğini istediğiniz zaman güncelleştirebilirsiniz.

### <a name="synonymmaps-resource-apis"></a>Eş anlamlı Haritalar Kaynak API'leri

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>POST veya PUT'u kullanarak hizmetinizin altında eş anlamlı bir harita ekleyin veya güncelleyin.

Eş anlamlı haritalar POST veya PUT aracılığıyla servise yüklenir. Her kural yeni satır karakteri ('\n') ile sınırlandırılmalıdır. Ücretsiz bir hizmette eş anlamlı harita başına en fazla 5.000 kural ve diğer tüm SUS'larda harita başına 20.000 kural tanımlayabilirsiniz. Her kural en fazla 20 genişletme olabilir.

Eş anlamlı haritalar aşağıda açıklanan Apache Solr biçiminde olmalıdır. Farklı bir biçimde varolan bir eş anlamlı sözlüğünüz varsa ve doğrudan kullanmak istiyorsanız, lütfen [UserVoice'ta](https://feedback.azure.com/forums/263029-azure-search)bize bildirin.

Aşağıdaki örnekte olduğu gibi HTTP POST'u kullanarak yeni bir eş anlamlı harita oluşturabilirsiniz:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternatif olarak, PUT'u kullanabilir ve URI'deki eş anlamlı harita adını belirtebilirsiniz. Eşanlamlı harita yoksa, oluşturulur.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

    {
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr eşanlamlı biçimi

Solr biçimi eşdeğer ve açık eşanlamlı eşlemeleri destekler. Eşleme kuralları, bu belgede açıklanan Apache Solr'un açık kaynak eşanlamlı filtresi belirtimine bağlıdır: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Aşağıda eşdeğer eşanlamlılar için bir örnek kuraldır.
```
USA, United States, United States of America
```

Yukarıdaki kuralla, bir arama sorgusu "ABD" veya "Amerika Birleşik Devletleri" veya "Amerika Birleşik Devletleri" genişletecektir.

Açık eşleme bir ok "=>" ile gösterilir. Belirtildiğinde, "=>"nin sol tarafına uyan bir arama sorgusunun terim sırası, sağ taraftaki alternatiflerle değiştirilir. Aşağıdaki kural göz önüne alındığında, arama sorguları "Washington", "Wash." veya "WA" tüm "WA" için yeniden yazılacaktır. Açık eşleme yalnızca belirtilen yönde geçerlidir ve bu durumda "WA" sorgusunu "Washington" olarak yeniden yazmaz.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Eş anlamlı haritaları hizmetinizde listeleyin.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Hizmetinizde eş anlamlı bir harita alın.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Hizmetinizin altındaki eşanlamlı lar haritasını silin.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Dizin tanımındaki eşanlamlı eşleni eşlemi kullanmak için aranabilir bir alanı yapılandırın.

Yeni bir alan özelliği **eşanlamlısı** Haritalar aranabilir bir alan için kullanılacak eşanlamlı bir eşlenim eşanlamlı belirtmek için kullanılabilir. Eşanlamlı haritalar hizmet düzeyi kaynaklarıdır ve hizmet altında bir dizinin herhangi bir alanı tarafından başvurulabilir.

    POST https://[servicename].search.windows.net/indexes?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**eş anlamlısı Haritalar,** 'Edm.String' veya 'Collection(Edm.String)' türündeki aranabilir alanlar için belirtilebilir.

> [!NOTE]
> Alan başına yalnızca bir eşanlamlı haritanız olabilir. Birden fazla eşanlamlı harita kullanmak istiyorsanız, lütfen [UserVoice'ta](https://feedback.azure.com/forums/263029-azure-search)bize bildirin.

## <a name="impact-of-synonyms-on-other-search-features"></a>Eş anlamlıların diğer arama özellikleri üzerindeki etkisi

Eşanlamlı özelliği orilet işleci ile eşanlamlı orijinal sorgu yeniden yazar. Bu nedenle, hit vurgulama ve puanlama profilleri özgün terimi ve eşanlamlıları eşdeğer olarak ele alar.

Eşanlamlı özelliği arama sorguları için geçerlidir ve filtreler veya fasonlar için geçerli değildir. Benzer şekilde, öneriler yalnızca özgün terime dayanır; eşanlamlı eşleşmeleri yanıtta görünmez.

Eşanlamlı genişletmeler joker karakter arama terimleri için geçerli değildir; önek, bulanık ve regex terimleri genişletilmez.

Eşanlamlı genişletme ve joker karakter, regex veya bulanık aramalar uygulayan tek bir sorgu yapmanız gerekiyorsa, sorguları OR sözdizimini kullanarak birleştirebilirsiniz. Örneğin, eşanlamlıları basit sorgu sözdizimi için joker karakterlerle `<query> | <query>*`birleştirmek için terim .

Geliştirme (üretim dışı) bir ortamda varolan bir dizininiz varsa, eş anlamlılar eklemenin skor profilleri, hit vurgulama ve öneriler üzerindeki etkisi de dahil olmak üzere arama deneyimini nasıl değiştirdiğini görmek için küçük bir sözlükle denemeler yapın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Eş anlamlı harita oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map)