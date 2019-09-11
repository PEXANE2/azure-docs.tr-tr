---
title: Arama dizini üzerinde sorgu genişletmesi için eş anlamlılar-Azure Search
description: Azure Search dizindeki bir arama sorgusunun kapsamını genişletmek için bir eş anlamlı eşleme oluşturun. Kapsam, bir listede sağladığınız eşdeğer terimleri içerecek şekilde belirlenir.
author: brjohnstmsft
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
manager: nitinme
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d9ddb5af42c538558a69ce68e7ea90161c947b12
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186449"
---
# <a name="synonyms-in-azure-search"></a>Azure Search eş anlamlılar

Arama altyapılarındaki eş anlamlılar, kullanıcının terimi gerçekten sağlaması gerekmeden bir sorgunun kapsamını örtülü olarak genişletmekte olan eşdeğer terimleri ilişkilendirir. Örneğin, "köpek" teriminin ve "Canine" ve "pupkopyala" teriminin eşanlamlısı, "köpek", "Canine" veya "pupkopyala" gibi tüm belgeler, sorgunun kapsamı içinde yer alacak.

Azure Search, eş anlamlı genişletmesi sorgu zamanında yapılır. Mevcut işlemlere kesinti olmadan, bir hizmete eş anlamlı eşlemeler ekleyebilirsiniz. Dizini yeniden derlemek zorunda kalmadan, bir alan tanımına bir **eş anlamlı Eşeşlemeler** özelliği ekleyebilirsiniz.

## <a name="create-synonyms"></a>Eş anlamlı oluştur

Eş anlamlıları oluşturmak için portal desteği yoktur, ancak REST API veya .NET SDK 'sını kullanabilirsiniz. REST ile çalışmaya başlamak için, bu API 'YI kullanarak isteklerin [Postman](search-get-started-postman.md) ve formüllerine göre kullanılmasını öneririz: [Eş anlamlı haritalar oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map). Geliştiriciler C# için, [kullanarak C#Azure Arama 'da eş anlamlılar ekleme ](search-synonyms-tutorial-sdk.md)ile çalışmaya başlayın.

İsteğe bağlı olarak, hizmet tarafı şifreleme için [müşteri tarafından yönetilen anahtarlar](search-security-manage-encryption-keys.md) kullanıyorsanız, bu korumayı eş anlamlı Haritalarınızın içeriğine uygulayabilirsiniz.

## <a name="use-synonyms"></a>Eş anlamlıları kullanma

Azure Search, eş anlamlı destek, sizin tanımladığınız ve hizmetinize yüklediğiniz eş anlamlı Maps tabanlıdır. Bu haritalar bağımsız bir kaynak (dizinler veya veri kaynakları gibi) oluşturur ve arama hizmetinizdeki herhangi bir dizinde aranabilir bir alan tarafından kullanılabilir.

Eş anlamlı haritalar ve dizinler bağımsız olarak korunur. Bir eş anlamlı eşleme tanımladıktan ve bu dosyayı hizmetinize yükledikten sonra, alan tanımında eş anlamlılar özelliğini, eş anlamlılar adlı yeni bir özellik ekleyerek etkinleştirebilirsiniz. Bir eş anlamlı eşleme oluşturmak, güncelleştirmek ve silmek her zaman bir tam belge işlemidir. Bu, eş anlamlı eşleme parçalarını artımlı olarak oluşturamaz, güncelleştiremez veya silemezsiniz. Tek bir girdinin güncelleştirilmesi, yeniden yükleme gerektirir.

Eş anlamlıları arama uygulamanıza eklemek iki adımlı bir işlemdir:

1.  Aşağıdaki API 'Ler aracılığıyla arama hizmetinize bir eş anlamlı eşleme ekleyin.  

2.  Dizin tanımında eş anlamlı eşlemeyi kullanmak için aranabilir bir alan yapılandırın.

### <a name="synonymmaps-resource-apis"></a>Eş eşlemler kaynak API 'Leri

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>POST veya PUT kullanarak hizmetiniz altına bir eş anlamlı harita ekleyin veya güncelleştirin.

Eş anlamlı eşlemeler POST veya PUT aracılığıyla hizmete yüklenir. Her kural yeni satır karakteriyle (' \n ') sınırlandırmalıdır. Tüm diğer SKU 'Larda ücretsiz bir hizmette ve 10.000 kurallarında, eş anlamlı eşleme başına en fazla 5.000 kural tanımlayabilirsiniz. Her kural en fazla 20 genişleme sahip olabilir.

Eş anlamlı eşlemeler aşağıda açıklanan Apache Solr biçiminde olmalıdır. Farklı bir biçimde mevcut bir eş anlamlı sözlüğü varsa ve bunu doğrudan kullanmak istiyorsanız [UserVoice](https://feedback.azure.com/forums/263029-azure-search)'ta bize bildirin.

Aşağıdaki örnekte olduğu gibi, HTTP POST kullanarak yeni bir eş anlamlı eşleme oluşturabilirsiniz:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternatif olarak, koy ' u kullanabilir ve URI üzerinde eş anlamlı eşleme adını belirtebilirsiniz. Eş anlamlı eşleme yoksa, oluşturulur.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

    {
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr eş anlamlı biçimi

Solr biçimi eşdeğer ve açık eş anlamlı eşlemelerini destekler. Eşleme kuralları, bu belgede açıklanan Apache Solr 'nin açık kaynaklı eş anlamlı filtre belirtimine uyar: [Eş anlamlı filtre](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Eşdeğer eş anlamlılar için örnek bir kural aşağıda verilmiştir.
```
USA, United States, United States of America
```

Yukarıdaki kuralla birlikte "USA" arama sorgusu "ABD" veya "Birleşik Devletler" ya da "Birleşik Devletler" olarak genişletilir.

Açık eşleme bir ok "= >" ile gösterilir. Belirtildiğinde, "= >" öğesinin sol tarafıyla eşleşen bir arama sorgusunun terim sırası, sağ taraftaki alternatiflerle birlikte değişir. Aşağıdaki kural verildiğinde, "Washington", "yıkama" sorguları üzerinde arama yapın. ya da "WA" tümü "WA" olarak yeniden yazılır. Açık eşleme yalnızca belirtilen yönde geçerlidir ve bu durumda "WA" sorgusunu "Washington" olarak yeniden yazmaz.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Hizmetiniz altındaki eş anlamlı haritaları listeleyin.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Hizmetiniz altında bir eş anlamlı eşleme alın.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Hizmetiniz altındaki bir eş anlamlı eşlemeyi silin.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Dizin tanımında eş anlamlı eşlemeyi kullanmak için aranabilir bir alan yapılandırın.

Yeni bir alan özelliği olan eşanlamlı bir alan için kullanılacak bir eş anlamlı eşleme belirtmek için, **eş** anlamlılar özelliği kullanılabilir. Eş anlamlı eşlemeler hizmet düzeyi kaynaklardır ve hizmet altındaki bir dizinin herhangi bir alanı tarafından başvurulabilir.

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

' Edm. String ' veya ' Collection (EDM. String) ' türündeki aranabilir alanlar için **eş anlamlı** alanları belirtilebilir.

> [!NOTE]
> Alan başına yalnızca bir eş anlamlı eşlemeye sahip olabilirsiniz. Birden çok eş anlamlı eşleme kullanmak istiyorsanız lütfen [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde bize bildirin.

## <a name="impact-of-synonyms-on-other-search-features"></a>Diğer arama özelliklerinde eş anlamlıların etkisi

Eş anlamlılar özelliği, veya işleciyle birlikte eşanlamlı olan özgün sorguyu yeniden yazar. Bu nedenle, isabet vurgulama ve Puanlama profillerinin özgün terim ve eş anlamlıları eşdeğer olarak kabul eder.

Eş anlamlı özelliği arama sorguları için geçerlidir ve filtreler veya modeller için geçerli değildir. Benzer şekilde, öneriler yalnızca özgün terime dayalıdır; eş anlamlı eşleşmeler yanıtta görünmez.

Joker karakter arama terimleri için eş anlamlı genişletmeleri uygulanmaz; ön ek, belirsiz ve Regex terimleri genişletilmedi.

Eş anlamlı genişletme ve joker karakter, Regex veya benzer aramalar uygulayan tek bir sorgu yapmanız gerekiyorsa, veya söz dizimini kullanarak sorguları birleştirebilirsiniz. Örneğin, basit sorgu söz dizimi için eş anlamlıları joker karakterlerle birleştirmek için, terimi `<query> | <query>*`olur.

## <a name="tips-for-building-a-synonym-map"></a>Eş anlamlı harita oluşturmaya yönelik ipuçları

- Kısa, iyi tasarlanmış bir eş anlamlı eş eşlem, olası eşleşmelerin kapsamlı bir listesinden daha etkilidir. Aşırı büyük veya karmaşık sözlüklerde sorgu çok sayıda eş anlamlıya genişlediğinde sorgu gecikmesini ayrıştırmak ve etkilemek daha uzun sürer. Tahmin yerine bir [arama trafik analizi raporu](search-traffic-analytics.md)aracılığıyla gerçek koşulları edinebilirsiniz.

- Hem bir ön hem de doğrulama alıştırmada, bu raporu etkinleştirin ve bu raporu kullanarak, bir eş anlamlı eşleşmeden hangi koşulların avantajına sahip olduğunu kesin olarak belirleyip, daha sonra eş anlamlı eşlemenin daha iyi bir sonuç üreteceğini doğrulama olarak kullanmaya devam edin. Önceden tanımlanmış raporda, "en yaygın arama sorguları" ve "sıfır-sonuç arama sorguları" kutucukları size gerekli bilgileri verecektir.

- Arama uygulamanız için birden fazla eş anlamlı harita oluşturabilirsiniz (örneğin, uygulamanız çok dilli müşteri temelini destekliyorsa, dile göre). Şu anda bir alan yalnızca birini kullanabilir. Bir alanın eş eşlemler özelliğini dilediğiniz zaman güncelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Geliştirme (üretim dışı) ortamında var olan bir dizininiz varsa, eş anlamlılar eklemenin, arama deneyimini nasıl değiştirdiğinden, Puanlama profilleri, isabet vurgulama ve öneriler de dahil olmak üzere küçük bir sözlükten deneyin.

- [Arama trafiği analizlerini etkinleştirin](search-traffic-analytics.md) ve en çok kullanılan koşulları öğrenmek için önceden tanımlı Power BI raporunu kullanın ve hangi koşulların sıfır belge döndürtiklerini öğrenin. Bu öngörülere sahip olan sözlüğü, dizininizdeki belgelere çözümlenmesi gereken, üretken olmayan sorgulara yönelik Eşanlamlı sözcükleri içerecek şekilde değiştirin.
