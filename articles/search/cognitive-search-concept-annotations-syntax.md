---
title: Beceri kümelerinde referans giriş ve çıktıları
titleSuffix: Azure Cognitive Search
description: Ek açıklama sözdizimini ve Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık alanında bir skillset'in giriş ve çıktılarında nasıl başvurulmayı açıklar.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: e27f61239c0631fb248217777a311b13ee48a3f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113860"
---
# <a name="how-to-reference-annotations-in-an-azure-cognitive-search-skillset"></a>Azure Bilişsel Arama becerilerinde ek açıklamalara başvuru nasil

Bu makalede, çeşitli senaryoları göstermek için örnekler kullanarak beceri tanımlarında ek açıklamalara nasıl başvurulmayı öğrenirsiniz. Bir belgenin içeriği bir dizi beceri den geçtikçe, ek açıklamalarla zenginleşir. Ek açıklamalar daha fazla aşağı zenginleştirme için giriş ler olarak kullanılabilir veya bir diziniçinde bir çıkış alanına eşlenebilir. 
 
Bu makaledeki örnekler, belge çözme aşamasının bir parçası olarak [Azure Blob dizin oluşturiları](search-howto-indexing-azure-blob-storage.md) tarafından otomatik olarak oluşturulan *içerik* alanına dayanmaktadır. Blob kapsayıcısından gelen belgelere atıfta bulunulurken, `"/document/content"` *içerik* alanının *belgenin*bir parçası olduğu bir biçim kullanın. 

## <a name="background-concepts"></a>Arka plan kavramları

Sözdizimini gözden geçirmeden önce, bu makalede daha sonra verilen örnekleri daha iyi anlamak için birkaç önemli kavramı yeniden gözden geçirelim.

| Sözleşme Dönemi | Açıklama |
|------|-------------|
| Zenginleştirilmiş Belge | Zenginleştirilmiş belge, bir belgeyle ilgili tüm ek açıklamaları tutmak için ardışık hatlar tarafından oluşturulan ve kullanılan bir iç yapıdır. Zenginleştirilmiş bir belgeyi ek açıklamalar ağacı olarak düşünün. Genellikle, önceki bir ek açıklama oluşturulan bir ek açıklama onun alt olur.<p/>Zenginleştirilmiş belgeler sadece skillset yürütme süresi için var. İçerik arama dizinine eşlendikten sonra, zenginleştirilmiş belgeye artık gerek kalmaz. Zenginleştirilmiş belgelerle doğrudan etkileşimde bulunmayan sanız da, bir beceri oluştururken belgelerin zihinsel bir modeline sahip olmak yararlıdır. |
| Zenginleştirme Bağlamı | Zenginleştirmenin gerçekleştiği bağlam, hangi öğenin zenginleştirilmiş olduğu açısından. Varsayılan olarak, zenginleştirme bağlamı `"/document"` tek tek belgelere yönelik düzeydedir. Bir beceri çalıştığında, bu becerinin çıktıları [tanımlanan bağlamın özellikleri](#example-2)haline gelir.|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Örnek 1: Basit ek açıklama başvurusu

Azure Blob depolama alanında, varlık tanıma yı kullanarak ayıklamak istediğiniz kişilerin adlarına başvurular içeren çeşitli dosyalarınız olduğunu varsayalım. Aşağıdaki beceri tanımında, `"/document/content"` belgenin tamamının metinsel gösterimi ve "kişiler" kişi olarak tanımlanan varlıklar için tam adların çıkarılmasıdır.

Varsayılan bağlam olduğundan, `"/document"`kişi listesi artık . `"/document/people"` Bu özel `"/document/people"` durumda, şimdi bir dizin içinde bir alana eşlenen veya aynı beceri başka bir beceri kullanılan bir ek açıklama vardır.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```

<a name="example-2"></a>

## <a name="example-2-reference-an-array-within-a-document"></a>Örnek 2: Belge içindeki bir diziye başvuru

Bu örnek, aynı belge üzerinde bir zenginleştirme adımı birden çok kez çağırmak için nasıl gösteren, önceki bir oluşturur. Önceki örnekte, tek bir belgeden 10 kişi adiçeren bir dizi dize oluşturduğunu varsayalım. Makul bir sonraki adım, soyadını tam bir isimden çıkaran ikinci bir zenginleştirme olabilir. 10 ad olduğundan, bu adımın bu belgede her kişi için bir kez olmak üzere 10 kez çağrılmasını istersiniz. 

Doğru yineleme sayısını çağırmak için, bağlamı `"/document/people/*"`, yıldız işaretinin`"*"`( ) zenginleştirilmiş belgedeki tüm düğümleri soyundan gelenler olarak temsil ettiği olarak `"/document/people"`ayarlayın. Bu beceri beceri dizisinde yalnızca bir kez tanımlansa da, tüm üyeler işlenene kadar belgeiçindeki her üye için çağrılır.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the last name from a full name",
    "uri": "http://names.azurewebsites.net/api/GetLastName",
    "context" : "/document/people/*",
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "fullname",
        "source": "/document/people/*"
      }
    ],
    "outputs": [
      {
        "name": "lastname",
        "targetName": "last"
      }
    ]
  }
```

Ek açıklamalar diziler veya dizeleri koleksiyonları olduğunda, dizi yerine belirli üyeleri bir bütün olarak hedeflemek isteyebilirsiniz. Yukarıdaki örnek, bağlam tarafından temsil `"last"` edilen her düğüm altında çağrılan bir ek açıklama oluşturur. Bu ek açıklamalar ailesine başvurmak istiyorsanız, sözdizimini `"/document/people/*/last"`kullanabilirsiniz. Belirli bir ek açıklamaya başvurmak istiyorsanız, belgede tanımlanan ilk `"/document/people/1/last`kişinin soyadına başvurmak için açık bir dizin kullanabilirsiniz: " Bu sözdizimi dizilerinde "0 dizilimi" olduğuna dikkat edin.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Örnek 3: Bir dizi içindeki başvuru üyeleri

Bazen belirli bir türdeki tüm ek açıklamaları belirli bir beceriye aktarmak için gruplandırmanız gerekir. Örnek 2'de ayıklanan tüm soyadlardan en yaygın soyadını tanımlayan varsayımsal bir özel beceri düşünün. Özel beceriye sadece son adları sağlamak için `"/document"` bağlamı ve `"/document/people/*/lastname"`girişi olarak belirtin.

Kardinallik belgenin `"/document/people/*/lastname"` daha büyük olduğuna dikkat edin. Bu belge için yalnızca bir belge düğümü varken 10 soyad düğümü olabilir. Bu durumda, sistem belgedeki tüm öğeleri `"/document/people/*/lastname"` içeren bir dizi otomatik olarak oluşturur.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the most common string from an array of strings",
    "uri": "http://names.azurewebsites.net/api/MostCommonString",
    "context" : "/document",
    "inputs": [
      {
        "name": "strings",
        "source": "/document/people/*/lastname"
      }
    ],
    "outputs": [
      {
        "name": "mostcommon",
        "targetName": "common-lastname"
      }
    ]
  }
```



## <a name="see-also"></a>Ayrıca bkz.
+ [Özel bir beceriyi zenginleştirme hattına nasıl entegre edinilir?](cognitive-search-custom-skill-interface.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanları bir dizine nasıl eşlersin?](cognitive-search-output-field-mapping.md)
