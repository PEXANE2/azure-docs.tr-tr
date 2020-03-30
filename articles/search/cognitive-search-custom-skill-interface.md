---
title: Özel beceriler için arayüz tanımı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık biriminde web api özel becerisi için özel veri ayıklama arabirimi.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 78f5f6eda28bed164668445b5671dad92f8dedd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77500261"
---
# <a name="how-to-add-a-custom-skill-to-an-azure-cognitive-search-enrichment-pipeline"></a>Azure Bilişsel Arama zenginleştirme hattına özel bir beceri ekleme

Azure Bilişsel Arama'daki [bir zenginleştirme boru hattı,](cognitive-search-concept-intro.md) [yerleşik bilişsel becerilerin](cognitive-search-predefined-skills.md) yanı sıra, kişisel olarak oluşturduğunuz ve boru hattına eklediğiniz [özel becerilerden](cognitive-search-custom-skill-web-api.md) de bir leştirilebilir. Bu makalede, bir Arabirimi ortaya çıkaran ve bir AI zenginleştirme ardışık birimine dahil edilmesine izin veren özel bir becerinin nasıl oluşturuleceğini öğrenin. 

Özel bir beceri oluşturmak, içeriğinize özgü dönüşümler eklemeniz için bir yol sağlar. Özel bir beceri, ihtiyacınız olan zenginleştirme adımLarını uygulayarak bağımsız olarak yürütülür. Örneğin, alana özel özel varlıklar tanımlayabilir, iş ve mali sözleşmeleri ve belgeleri farklılaştırmak için özel sınıflandırma modelleri oluşturabilir veya ilgili içerik için ses dosyalarına daha derinlere ulaşmak için konuşma tanıma becerisi ekleyebilirsiniz. Adım adım bir örnek için bkz: [Örnek: AI zenginleştirme için özel bir beceri oluşturma.](cognitive-search-create-custom-skill-example.md)

 Ne olursa olsun özel yetenek gerektirir, zenginleştirme boru hattının geri kalanına özel bir beceri bağlamak için basit ve net bir arayüz vardır. Bir [skillset'e](cognitive-search-defining-skillset.md) dahil edilmenin tek şartı, girdileri kabul etme ve çıktıları bir bütün olarak beceriler içinde tüketilebilen şekillerde yayılabilme yeteneğidir. Bu makalenin odak noktası, zenginleştirme ardışık alanının gerektirdiği giriş ve çıktı biçimleridir.

## <a name="web-api-custom-skill-interface"></a>Web API özel beceri arabirimi

30 saniyelik bir pencere içinde yanıt döndürmezlerse, varsayılan zaman ekine göre özel WebAPI beceri bitiş noktaları. Dizin oluşturma ardışık gelir ve dizin oluşturma, bu pencerede bir yanıt alınmazsa bir zaman aralaması hatası üretir.  Zaman aşım parametresini ayarlayarak zaman aşımını 230 saniyeye kadar yapılandırmak mümkündür:

```json
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill has a 230 second timeout",
        "uri": "https://[your custom skill uri goes here]",
        "timeout": "PT230S",
```

URI'nin güvenli olduğundan emin olun (HTTPS).

Şu anda, özel bir beceri ile etkileşim için tek mekanizma bir Web API arabirimi geçer. Web API gereksinimleri bu bölümde açıklanan gereksinimleri karşılamalıdır.

### <a name="1--web-api-input-format"></a>1. Web API Giriş Biçimi

Web API'si işlenecek bir dizi kaydı kabul etmelidir. Her kayıt, Web API'nize sağlanan giriş olan bir "özellik torbası" içermelidir. 

Sözleşme metninde belirtilen ilk tarihi tanımlayan basit bir zenginleştirici oluşturmak istediğinizi varsayalım. Bu örnekte, beceri sözleşme metni olarak tek bir giriş *sözleşmesiMetni* kabul eder. Beceri de sözleşme tarihi olan tek bir çıktı vardır. Zenginleştiriciyi daha ilginç hale getirmek için, bu *sözleşmeTarihini* çok parçalı karmaşık bir tür şeklinde döndürün.

Web API'niz bir dizi giriş kaydı almaya hazır olmalıdır. *Değerler* dizisinin her üyesi belirli bir kaydın girdisini temsil eder. Her kaydın aşağıdaki öğelere sahip olması gerekir:

+ Belirli bir kayıt için benzersiz tanımlayıcı olan bir *kayıt Kimliği* üyesi. Zenginleştiriciniz sonuçları döndürdüğünde, arayanın kayıt sonuçlarını girişleriyle eşleştirebilmesi için bu *kayıt Id'i* sağlaması gerekir.

+ Aslında her kayıt için giriş alanları bir çanta olan bir *veri* üyesi.

Yukarıdaki örnekte daha somut olması için, Web API'nizaşağıdaki gibi görünen istekleri beklemelidir:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
Gerçekte, hizmetiniz burada gösterilen üç kayıt yerine yüzlerce veya binlerce kayıtla çağrılabilir.

### <a name="2-web-api-output-format"></a>2. Web API Çıkış Biçimi

Çıktının biçimi, bir *kayıt Kimliği*ve bir özellik çantası içeren bir kayıt kümesidir 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Bu özel örnekte yalnızca bir çıktı vardır, ancak birden fazla özellik çıktı olabilir. 

### <a name="errors-and-warning"></a>Hatalar ve Uyarı

Önceki örnekte gösterildiği gibi, her kayıt için hata ve uyarı iletileri döndürebilirsiniz.

## <a name="consuming-custom-skills-from-skillset"></a>Skillset'ten özel beceriler tüketme

Bir Web API zenginleştirici oluşturduğunuzda, HTTP üstbilgilerini ve parametrelerini isteğin bir parçası olarak tanımlayabilirsiniz. Aşağıdaki parçacık, istek parametrelerinin ve *isteğe bağlı* HTTP üstbilgilerinin skillset tanımının bir parçası olarak nasıl tanımlanabileceğini gösterir. HTTP üstbilgiler bir gereklilik değildir, ancak becerinize ek yapılandırma özellikleri eklemenize ve bunları beceri tanımından ayarlamanıza olanak sağlar.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel bir beceriyi bir beceriye entegre etmek için gereken arabirim gereksinimleri ele alınmıştır. Özel beceriler ve beceri kompozisyonu hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları tıklayın.

+ [Güç Becerileri: özel becerilerin deposu](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Örnek: AI zenginleştirme için özel bir beceri oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanların haritası nasıl eşlenir?](cognitive-search-output-field-mapping.md)
