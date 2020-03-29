---
title: 'Özellikler: Eylem ve bağlam - Personalizer'
titleSuffix: Azure Cognitive Services
description: Personalizer, daha iyi sıralama önerileri yapmak için özellikleri, eylemler ve bağlam hakkında bilgiler kullanır. Özellikler çok genel veya bir öğeye özgü olabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 408501232891a7971d03c89acc647d9ed19609b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77026158"
---
# <a name="features-are-information-about-actions-and-context"></a>Özellikler eylemler ve bağlam hakkında bilgidir

Personalizer hizmeti, uygulamanızın belirli bir bağlamda kullanıcılara ne göstermesi gerektiğini öğrenerek çalışır.

Personalizer en iyi **eylemi**seçmek için **mevcut bağlam** hakkında bilgi **özellikleri**kullanır. Özellikler, daha yüksek ödüller elde etmek için kişiselleştirmeye yardımcı olabileceğini düşündüğünüz tüm bilgileri temsil eder. Özellikler çok genel veya bir öğeye özgü olabilir. 

Örneğin, şu konuda bir **özelliğiniz** olabilir:

* Bir _user persona_ `Sports_Shopper`. gibi kullanıcı kişiliği Bu tek bir kullanıcı kimliği olmamalıdır. 
* Bir _content_ video , `Documentary`a veya a `Movie` `TV Series`, veya bir perakende öğe mağazada kullanılabilir olup olmadığı gibi içerik.
* Haftanın hangi günü olduğu gibi _geçerli_ zaman dilimi.

Personalizer, eylemler ve bağlam için gönderebileceğiniz özellikleri reçete etmez, sınırlamaz veya düzeltmez:

* Bazı eylemler için bazı özellikler gönderebilirsiniz, diğerleri için değil, yoksa. Örneğin, TV dizilerinde filmlerin sahip olmadığı öznitelikler olabilir.
* Bazı özelliklere sahip olabilirsiniz, yalnızca birkaç kez kullanılabilir. Örneğin, bir mobil uygulama bir web sayfasından daha fazla bilgi sağlayabilir. 
* Zaman içinde, bağlam ve eylemlerle ilgili özellikler ekleyebilir ve kaldırabilirsiniz. Personalizer mevcut bilgilerden öğrenmeye devam ediyor.
* Bağlam için en az bir özellik olmalıdır. Personalizer boş bir bağlamı desteklemez. Yalnızca her seferinde sabit bir bağlam gönderirseniz, Personalizer yalnızca eylemlerdeki özelliklerle ilgili sıralamalar için eylemi seçer.
* Kategorik özellikler için olası değerleri tanımlamanız gerekmez ve sayısal değerler için aralıkları önceden tanımlamanız gerekmez.

## <a name="supported-feature-types"></a>Desteklenen özellik türleri

Personalizer dize, sayısal ve boolean türlerinin özelliklerini destekler.

### <a name="how-choice-of-feature-type-affects-machine-learning-in-personalizer"></a>Özellik türü seçimi Kiyatifte Makine Öğrenimini Nasıl Etkiler?

* **Strings**: String türleri için, anahtar ve değerin her birleşimi Personalizer makine öğrenme modelinde yeni ağırlıklar oluşturur. 
* **Sayısal**: Sayı kişiselleştirme sonucunu orantılı olarak etkilemesi gerektiğinde sayısal değerler kullanmalısınız. Bu çok senaryo bağımlı. Basitleştirilmiş bir örnekte, örneğin bir perakende deneyimini kişiselleştirirken, NumberOfPetsOwned 2 veya 3 evcil hayvanı olan kişilerin kişiselleştirme sonucunu 1 evcil hayvana sahip olmak kadar iki kez veya üç kez etkilemesini isteyebileceğin sayısal bir özellik olabilir. Sayısal birimlere dayanan ancak anlamın doğrusal olmadığı (Yaş, Sıcaklık veya Kişi Yüksekliği gibi) özellikler en iyi dizeleri olarak kodlanır ve özellik kalitesi genellikle aralıklar kullanılarak geliştirilebilir. Örneğin, Yaş "Age":"0-5", "Age":"6-10" vb. olarak kodlanabilir.
* **Boolean** değerleri sanki hiç gönderilmemiş gibi "yanlış" hareket değeri ile gönderildi.

Var olmayan özellikler istekten çıkarılmalıdır. Modeli eğitirken varolan ve "null" değeriyle işlenecektir, çünkü null değeri olan özellikler göndermekten kaçının.

## <a name="categorize-features-with-namespaces"></a>Özellikleri ad boşluklarıyla kategorilere ayırın

Personalizer, ad boşluklarına düzenlenmiş özellikleri alır. Uygulamanızda ad alanlarının kullanIlip kullanılmaması gerektiğini ve ne olmaları gerektiğini belirlersiniz. Ad alanları, benzer bir konu yla ilgili özellikleri veya belirli bir kaynaktan gelen özellikleri gruplandırmak için kullanılır.

Uygulamalar tarafından kullanılan özellik ad alanlarına örnekler aşağıda verilmiştir:

* User_Profile_from_CRM
* Zaman
* Mobile_Device_Info
* http_user_agent
* Video Çözünürlüğü
* UserDeviceInfo
* Hava durumu
* Product_Recommendation_Ratings
* current_time
* NewsArticle_TextAnalytics

Geçerli JSON anahtarları olduğu sürece, özellik ad boşluklarını kendi kurallarınızı izleyerek adlandırabilirsiniz. Ad alanları, özellikleri farklı kümeler halinde düzenlemek ve benzer adlara sahip özellikleri ayrıştırmak için kullanılır. Ad alanlarını özellik adlarına eklenen bir 'önek' olarak düşünebilirsiniz. Ad alanları iç içe geçemez.


Aşağıdaki JSON'da `user` `state`ve `device` özellik ad boşluklarıdır. 

> [!Note]
> Şu anda, UTF-8 tabanlı özellik ad alanları için adlar kullanmanızı ve farklı harflerle başlamanızı şiddetle öneririz. Örneğin, `user`, `state`, `device` ve `u` `s`ile `d`başlamak , , ve . Şu anda aynı ilk karakterlere sahip ad boşluklarına sahip olmak, makine öğrenimi için kullanılan dizinlerde çakışmalara neden olabilir.

JSON nesneleri iç içe json nesneleri ve basit özellik / değerleri içerebilir. Bir dizi yalnızca dizi öğeleri sayıysa eklenebilir. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "profileType":"AnonymousUser",
                "latlong": [47.6, -122.1]
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true
            }
        }
    ]
}
```

### <a name="restrictions-in-character-sets-for-namespaces"></a>Ad boşlukları için karakter kümelerinde kısıtlamalar

Ad alanını adlandırmak için kullandığınız dize bazı kısıtlamalar izlemelidir: 
* Tek şifreli olamaz.
* Ad alanı adları için 256 < kodları olan yazdırılabilir simgelerden bazılarını kullanabilirsiniz. 
* 32 (yazdırılamaz), 32 (boşluk), 58 (üst üste), 124 (boru) ve 126-140 < kodlu semboller kullanamazsınız.
* Bir alt "_" ile başlamamalıdır veya özellik yoksayılır.

## <a name="how-to-make-feature-sets-more-effective-for-personalizer"></a>Özellik kümelerini Personalizer için nasıl daha etkili hale getirilir?

İyi bir özellik seti, Personalizer'ın en yüksek ödülü sağlayacak eylemi nasıl tahmin edeceğini öğrenmesine yardımcı olur. 

Bu önerileri izleyen Personalizer Rank API'ye özellikler göndermeyi düşünün:

* Kişiselleştirmeyi yönlendirecek kadar özellik vardır. İçeriğin ne kadar kesin hedeflenirse, o kadar çok özelliğe ihtiyaç duyulur.

* Çeşitli *yoğunluklarda*yeterli özellikleri vardır. Birçok öğe birkaç kova halinde gruplandırılırsa bir özellik *yoğundur.* Örneğin, binlerce video "Uzun" (5 dk'dan uzun) ve "Kısa" (5 dk'nın altında) olarak sınıflandırılabilir. Bu *çok yoğun* bir özelliktir. Diğer taraftan, aynı binlerce öğenin "Başlık" adlı bir özelliği olabilir ve bu özellik bir öğeden diğerine neredeyse hiç aynı değere sahip olmaz. Bu çok yoğun olmayan veya *seyrek* bir özelliktir.  

Yüksek yoğunluklu özelliklere sahip olmak, Personalizer'ın bir öğeden diğerine öğrenmeyi tahmin etmesini sağlar. Ama sadece birkaç özellik vardır ve çok yoğun, Personalizer tam seçim için sadece birkaç kova ile içerik hedef çalışacağız.

### <a name="improve-feature-sets"></a>Özellik kümelerini geliştirin 

Çevrimdışı Değerlendirme yaparak kullanıcı davranışını çözümle. Bu, hangi özelliklerin olumlu ödüllere daha az katkıda bulunanlara karşı büyük ölçüde katkıda olduğunu görmek için geçmiş verilere bakmanızı sağlar. Hangi özelliklerin yardımcı olduğunu görebilirsiniz ve sonuçları daha da iyileştirmek için Personalizer'a gönderebileceğiniz daha iyi özellikler bulmak size ve uygulamanıza kalmış olacaktır.

Aşağıdaki bölümler, Personalizer'a gönderilen özellikleri geliştirmek için yaygın uygulamalardır.

#### <a name="make-features-more-dense"></a>Özellikleri daha yoğun hale getirin

Özellik kümelerinizi daha büyük ve daha az yoğun hale getirmek için düzenleyerek özellik kümelerinizi geliştirmek mümkündür.

Örneğin, ikinci ye kadar bir zaman damgası çok seyrek bir özelliktir. Zamanları "sabah", "öğlen", "öğleden sonra" vb. olarak sınıflandırarak daha yoğun (etkili) hale getirilebilir.

Konum bilgileri de genellikle daha geniş sınıflandırmalar oluşturmadan yararlanır. Örneğin, Lat: 47.67402° N, Uzun: 122.12154° W gibi enlem-boylam koordinatı çok hassastır ve modeli farklı boyutlar olarak enlem ve boylam öğrenmeye zorlar. Konum bilgilerine göre kişiselleştirmeye çalıştığınızda, konum bilgilerini daha büyük sektörlerde gruplandırmaya yardımcı olur. Bunu yapmanın kolay bir yolu, Lat-Long sayıları için uygun bir yuvarlama hassasiyeti seçmek ve enlem ve boylamları tek bir dize halinde yaparak "alanlar" olarak birleştirmektir. Örneğin, 47.67402° N, Uzun: 122.12154° W'yi yaklaşık birkaç kilometre genişliğindeki bölgelerde temsil etmenin iyi bir yolu "location":"34.3 , 12.1" olacaktır.


#### <a name="expand-feature-sets-with-extrapolated-information"></a>Ekstrapolated bilgilerle özellik kümelerini genişletme

Ayrıca, zaten sahip olduğunuz bilgilerden türetilebilen keşfedilmemiş öznitelikleri düşünerek daha fazla özellik elde edebilirsiniz. Örneğin, hayali bir film listesi kişiselleştirme, bir hafta sonu vs hafta içi kullanıcılardan farklı davranışlar ortaya çıkarmak mümkün mü? Zaman bir "hafta sonu" veya "hafta içi" özniteliği için genişletilebilir. Ulusal kültür tatilleri belli film türlerine dikkat çekiyor mu? Örneğin, bir "Cadılar Bayramı" özniteliği, ilgili olduğu yerlerde yararlıdır. Yağmurlu havabirçok kişi için bir film seçimi üzerinde önemli bir etkisi olması mümkün mü? Zaman ve yer ile, bir hava servisi bu bilgileri sağlayabilir ve ekstra bir özellik olarak ekleyebilirsiniz. 

#### <a name="expand-feature-sets-with-artificial-intelligence-and-cognitive-services"></a>Yapay zeka ve bilişsel hizmetlerle özellik kümelerini genişletin

Yapay Zeka ve kullanıma hazır Bilişsel Hizmetler Personalizer için çok güçlü bir ek olabilir. 

Yapay zeka hizmetlerini kullanarak öğelerinizi önceden işleyerek, kişiselleştirmeyle ilgili olabilecek bilgileri otomatik olarak ayıklayabilirsiniz.

Örnek:

* Sahne öğelerini, metni, duyarlılığı ve diğer birçok öznitelikleri ayıklamak için [Video Indexer](https://azure.microsoft.com/services/media-services/video-indexer/) aracılığıyla bir film dosyası çalıştırabilirsiniz. Bu öznitelikler, özgün madde meta verilerinin sahip olmadığı özellikleri yansıtmak için daha yoğun hale getirilebilir. 
* Görüntüler nesne algılama yoluyla çalıştırılabilir, duygular, vb ile yüzler
* Metindeki bilgiler, varlıkları, duyguları ayıklayarak, Bing bilgi grafiğine sahip varlıkları genişleterek vb. artırılabilir.

Diğer birkaç [Azure Bilişsel Hizmeti'ni](https://www.microsoft.com/cognitive-services)de kullanabilirsiniz,

* [Varlık Bağlama](../entitylinking/home.md)
* [Metin Analizi](../text-analytics/overview.md)
* [Duygu Tanıma](../emotion/home.md)
* [BilgisayarLı Vizyon](../computer-vision/home.md)

## <a name="actions-represent-a-list-of-options"></a>Eylemler seçenekler listesini temsil ediyor

Her eylem:

* _Olay_ kimliği var. Zaten bir etkinlik kimliğiniz varsa, bunu göndermeniz gerekir. Etkinlik kimliğiniz yoksa, bir tane göndermeyin, Personalizer sizin için bir tane oluşturur ve Rank isteğine yanıt olarak iade eder. Kimlik, kullanıcıyla değil, Rank olayıyla ilişkilidir. Bir kimlik oluşturursanız, guid en iyi şekilde çalışır. 
* Özelliklerin bir listesi vardır.
* Özellikler listesi büyük (yüzlerce) olabilir, ancak ödül almaya katkıda bulunmayan özellikleri kaldırmak için özellik etkinliğini değerlendirmenizi öneririz. 
* **Eylemlerdeki** özellikler, Personalizer tarafından kullanılan **bağlamdaki** özelliklerle herhangi bir korelasyon alabiliyor olabilir veya olmayabilir.
* Eylemleriçin özellikler bazı eylemlerde bulunabilir, diğerlerinde olmayabilir. 
* Belirli bir eylem kimliğiiçin özellikler bir gün kullanılabilir, ancak daha sonra kullanılamaz hale gelir. 

Personalizer'ın makine öğrenimi algoritmaları kararlı özellik kümeleri olduğunda daha iyi performans gösterir, ancak özellik kümesi zaman içinde değişirse Rank çağrıları başarısız olmaz.

Sıralama eylemleri 50'den fazla eylem göndermeyin. Bunlar her seferinde aynı 50 eylem olabilir veya değişebilir. Örneğin, bir e-ticaret uygulaması için 10.000 öğeden oluşan bir ürün kataloğunuz varsa, bir müşterinin sevebileceği en iyi 40 öğeyi belirlemek için bir öneri veya filtreleme motoru kullanabilir ve en çok ödül oluşturacak olanı bulmak için Kisizer'i kullanabilirsiniz (örneğin , kullanıcı sepete ekler) geçerli bağlam için.


### <a name="examples-of-actions"></a>Eylem örnekleri

Rank API'sine gönderdiğiniz eylemler, kişiselleştirmeye çalıştığınız şeye bağlıdır.

İşte bazı örnekler:

|Amaç|Eylem|
|--|--|
|Bir haber sitesinde hangi makalenin vurgulandığıkişiselleştirin.|Her eylem potansiyel bir haber makalesidir.|
|Bir web sitesindeki reklam yerleşimini optimize edin.|Her eylem, reklamlar için bir düzen oluşturmak için bir düzen veya kural olacaktır (örneğin, üstte, sağda, küçük resimler, büyük resimler).|
|Önerilen öğelerin kişiselleştirilmiş sıralamasını bir alışveriş web sitesinde görüntüleyin.|Her eylem belirli bir üründür.|
|Belirli bir fotoğrafa uygulamak için filtreler gibi kullanıcı arabirimi öğeleri önerin.|Her eylem farklı bir filtre olabilir.|
|Kullanıcı niyetini netleştirmek veya bir eylem önermek için bir sohbet botu yanıtını seçin.|Her eylem, yanıtı nasıl yorumlanacağına yönelik bir seçenektir.|
|Arama sonuçları listesinin en üstünde ne göstereceğinizi seçme|Her eylem, en iyi birkaç arama sonuçlarından biridir.|


### <a name="examples-of-features-for-actions"></a>Eylemler için özellik örnekleri

Aşağıda eylemler için iyi özellikler örnekleri verilmiştir. Bu her uygulama çok bağlıdır.

* Eylemlerin özellikleri ile özellikleri. Örneğin, bir film ya da bir dizi mi?
* Kullanıcıların geçmişte bu eylemle nasıl etkileşimde bulunanlarla nasıl etkileşimde bulunanlarla ilgili özellikler. Örneğin, bu film çoğunlukla demografik A veya B insanlar tarafından görülür, genellikle en fazla bir kez oynanır.
* Kullanıcının eylemleri nasıl *gördüğüyle* ilgili özellikler. Örneğin, küçük resimde gösterilen filmin posterinde yüzler, arabalar veya manzaralar yer alıyor mu?

### <a name="load-actions-from-the-client-application"></a>İstemci uygulamasından yükleme eylemleri

Eylemlerden elde edilen özellikler genellikle içerik yönetim sistemlerinden, kataloglardan ve tavsiye sistemlerinden gelebilir. Uygulamanız, sahip olduğunuz ilgili veritabanlarından ve sistemlerden eylemlerle ilgili bilgileri yüklemekten sorumludur. Eylemlerinizin değişmesi veya her seferinde yüklenmesi performans üzerinde gereksiz bir etki yaratmazsa, bu bilgileri önbelleğe almak için uygulamanıza mantık ekleyebilirsiniz.

### <a name="prevent-actions-from-being-ranked"></a>Eylemlerin derecelendirilen olmasını önleme

Bazı durumlarda, kullanıcılara görüntülemek istemediğiniz eylemler vardır. Bir eylemin en üst sırada yer alan bir eylem olmasını önlemenin en iyi yolu, eylemi ilk etapta Rank API'sine dahil etmemektir.

Bazı durumlarda, bir Rank API çağrısının ortaya çıkan bir _eylemi_ bir kullanıcıya gösterilecekse, iş mantığınızda daha sonra belirlenebilir. Bu gibi durumlarda _Etkin Olmayan Olaylar'ı_kullanmalısınız.

## <a name="json-format-for-actions"></a>Eylemler için JSON biçimi

Rank'ı ararken, aralarından seçim yapmak için birden çok eylem gönderirsiniz:

JSON nesneleri iç içe json nesneleri ve basit özellik / değerleri içerebilir. Bir dizi yalnızca dizi öğeleri sayıysa eklenebilir. 

```json
{
    "actions": [
    {
      "id": "pasta",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "medium",
          "grams": [400,800]
        },
        {
          "nutritionLevel": 5,
          "cuisine": "italian"
        }
      ]
    },
    {
      "id": "ice cream",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [150, 300, 450]
        },
        {
          "nutritionalLevel": 2
        }
      ]
    },
    {
      "id": "juice",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [300, 600, 900]
        },
        {
          "nutritionLevel": 5
        },
        {
          "drink": true
        }
      ]
    },
    {
      "id": "salad",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "low",
          "grams": [300, 600]
        },
        {
          "nutritionLevel": 8
        }
      ]
    }
  ]
}
```

## <a name="examples-of-context-information"></a>Bağlam bilgisi örnekleri

_Bağlam_ bilgileri her uygulama ve kullanım örneğine bağlıdır, ancak genellikle aşağıdakiler gibi bilgiler içerebilir:

* Kullanıcınız hakkında demografik ve profil bilgileri.
* Kullanıcı aracısı gibi HTTP üstbilgilerinden çıkarılan veya IP adreslerine dayalı ters coğrafi aramalar gibi HTTP bilgilerinden türetilen bilgiler.
* Haftanın günü, hafta sonu olsun olmasın, sabah veya öğleden sonra, tatil sezonu olsun olmasın, vb. gibi geçerli saat hakkında bilgi
* Konum, hareket veya pil düzeyi gibi mobil uygulamalardan elde edilen bilgiler.
* Kullanıcıların davranışlarının tarihsel toplamları - bu kullanıcının en çok izlediği film türleri gibi.

Uygulamanız, bağlam hakkındaki bilgileri ilgili veritabanlarından, sensörlerden ve sistemlerinizden yüklemekten sorumludur. Bağlam bilgileriniz değişmezse, sıralama API'sine göndermeden önce bu bilgileri önbelleğe almak için uygulamanıza mantık ekleyebilirsiniz.

## <a name="json-format-for-context"></a>Bağlam için JSON biçimi 

Bağlam, Rank API'sine gönderilen bir JSON nesnesi olarak ifade edilir:

JSON nesneleri iç içe json nesneleri ve basit özellik / değerleri içerebilir. Bir dizi yalnızca dizi öğeleri sayıysa eklenebilir. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "name":"Doug"
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true,
                "screensize": [1680,1050]
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 
