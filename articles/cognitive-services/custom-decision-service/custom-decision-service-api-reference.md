---
title: API başvurusu-Özel Karar Alma Hizmeti
titlesuffix: Azure Cognitive Services
description: Özel Karar Alma Hizmeti için kapsamlı bir API Kılavuzu.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 4f263e3b57103174f0084ab3d25430d8c47359fd
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707305"
---
# <a name="api"></a>API

Azure Özel Karar Alma Hizmeti her karar için çağrılan iki API sağlar: işlemlerin derecelendirmesini ve yeniden bir [API](#reward-api) 'nin çıktısını almak için [Derecelendirme API 'si](#ranking-api) . Ayrıca, Azure Özel Karar Alma Hizmeti yönelik eylemleri belirtmek için bir [eylem kümesi API 'si](#action-set-api-customer-provided) sağlarsınız. Bu makalede bu üç API yer almaktadır. Özel Karar Alma Hizmeti, makalelerin derecelendirmesinin ne zaman iyileştiri, göstermek için aşağıda tipik bir senaryo kullanılır.

## <a name="ranking-api"></a>Derecelendirme API 'SI

Derecelendirme API 'SI, gecikmeyi iyileştirmek ve [aynı kaynak ilkeyi](https://en.wikipedia.org/wiki/Same-origin_policy)atlamak için standart bir [JSONP](https://en.wikipedia.org/wiki/JSONP)stili iletişim modelini kullanır. İkincisi, JavaScript 'in sayfa kaynağının dışından veri getirmeyi yasaklar.

Bu kod parçacığını ön sayfanızın HTML kafasını (kişiselleştirilmiş makalelerin bir listesi görüntülenir) ekleyin:

```html
// define the "callback function" to render UI
<script> function callback(data) { … } </script>
// "data" provides the ranking of URLs, see example below.

// call to Ranking API
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>?<parameters>" async></script>
// action set id is the name of the corresponding RSS/Atom feed.

// same call with multiple action sets:
// <script src="https://ds.microsoft.com/api/v2/<appId>/rank/<A1>/<A2>/.../<An>?<parameters>" async></script>
```

> [!IMPORTANT]
> Geri çağırma işlevinin, derecelendirme API çağrısından önce tanımlanması gerekir.

> [!TIP]
> Gecikme süresini artırmak için, derecelendirme API 'SI, içinde `https://ds.microsoft.com/api/v2/<appId>/rank/*`olduğu gıbı https yerine http üzerinden kullanıma sunulur.
> Ancak, ön sayfa HTTPS üzerinden sunulduysa bir HTTPS uç noktasının kullanılması gerekir.

Parametreler kullanılmazsa, derecelendirme API 'sindeki HTTP yanıtı, JSONP biçimli bir dizedir:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

Daha sonra tarayıcı `callback()` işlevine bir çağrı olarak bu dizeyi yürütür.

Önceki örnekteki geri çağırma işlevinin parametresi aşağıdaki şemaya sahiptir:

- `ranking`görüntülenecek URL 'lerin sıralamasını sağlar.
- `eventId`, bu sıralamaya karşılık gelen tıklama ile eşleştirmek için Özel Karar Alma Hizmeti tarafından dahili olarak kullanılır.
- `appId`geri çağırma işlevinin aynı Web sayfasında çalışan Özel Karar Alma Hizmeti birden çok uygulama arasında ayrım yapmasına izin verir.
- `actionSets`Son başarılı yenilemenin UTC zaman damgasıyla birlikte, derecelendirme API çağrısında kullanılan her bir eylem kümesini listeler. Özel Karar Alma Hizmeti, eylem kümesi akışlarını düzenli aralıklarla yeniler. Örneğin, bazı eylem kümeleri güncel değilse, geri çağırma işlevinin varsayılan derecelendirmesini geri dönebilmeleri gerekebilir.

> [!IMPORTANT]
> Belirtilen eylem kümeleri işlenir ve muhtemelen varsayılan makalelerin sıralamasını oluşturacak şekilde ayıklanır. Varsayılan derecelendirme daha sonra yeniden sıralanabilir ve HTTP yanıtında döndürülür. Varsayılan derecelendirme burada tanımlanmıştır:
>
> - Her bir eylem kümesi içinde makaleler, en son 15 makaleye (15 ' ten fazla olursa) ayıklanır.
> - Birden çok eylem kümesi belirtildiğinde, API çağrısındaki ile aynı sırada birleştirilir. Makalelerin özgün sıralaması her bir eylem kümesi içinde korunur. Yinelemeler, önceki kopyaların yararına kaldırılır.
> - İlk `n` makaleler, varsayılan `n=20` olarak, Birleşik makale listesinden tutulur.

### <a name="ranking-api-with-parameters"></a>Parametrelerle API ile derecelendirme

Derecelendirme API 'SI şu parametrelere izin veriyor:

- `details=1`ve `details=2` ' de `ranking`listelenen her makale hakkında ek ayrıntılar ekler.
- `limit=<n>`Varsayılan derecelendirmeden makalelerin sayısını belirtir. `n`ve `2` `2` (ya da sırasıyla kesilmiş`30`) arasında olmalıdır. `30`
- `dnt=1`Kullanıcı tanımlama bilgilerini devre dışı bırakır.

Parametreler, standart, sorgu dizesi sözdiziminde birleştirilebilir, örneğin `details=2&dnt=1`.

> [!IMPORTANT]
> Avrupa 'daki varsayılan ayar, müşteri tanımlama `dnt=1` bilgisi başlığını kabul edene kadar olmalıdır. Ayrıca, minors 'ı hedefleyen web siteleri için varsayılan ayar olmalıdır. Daha fazla bilgi için bkz. [kullanım koşulları](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

Öğesi, eylem kümesi API 'si `guid`tarafından sunulduysa her bir makalenin öğesini ekler. `details=1` HTTP yanıtı:

```json
callback({
   "ranking":[{"id":"url1","details":[{"guid":"123"}]},
              {"id":"url2","details":[{"guid":"456"}]},
              {"id":"url3","details":[{"guid":"789"}]}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

Öğesi `details=2` , özel karar alma hizmeti makalelerin SEO Metatags özelliği tarafından ayıklanabilme [kodundan](https://github.com/Microsoft/mwt-ds/tree/master/Crawl)daha fazla ayrıntı ekler:

- `title``<meta property="og:title" content="..." />` veya`<meta property="twitter:title" content="..." />` veya veya`<title>...</title>`
- `description``<meta property="og:description" ... />` veya`<meta property="twitter:description" content="..." />` veya veya`<meta property="description" content="..." />`
- `image`Kaynak`<meta property="og:image" content="..." />`
- `ds_id`Kaynak`<meta name=”microsoft:ds_id” content="..." />`

HTTP yanıtı:

```json
callback({
   "ranking":[{"id":"url1","details":<details>},
              {"id":"url2","details":<details>},
              {"id":"url3","details":<details>}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

`<details>` Öğesi:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Reward API 'SI

Özel Karar Alma Hizmeti, tıklamaları yalnızca üst yuvada kullanır. Her tıklama 1 ' in bir ödül olarak yorumlanır. Bir tıklama olmaması, 0 ' ın bir açıklaması olarak yorumlanır. Tıklama, [DERECELENDIRME API](#ranking-api) çağrısı tarafından oluşturulan olay kimliklerini kullanarak ilgili derecelendirmeden eşleştirilir. Gerekirse, olay kimlikleri oturum tanımlama bilgileri aracılığıyla geçirilebilir.

Üst yuvaya tıklamak için, bu kodu ön sayfanıza koyun:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Daha `data` önce açıklandığı gibi `callback()` işlevin bağımsız değişkeni aşağıda verilmiştir. Tıklama `data` işleme kodunda kullanılması için bazı dikkatli olunması gerekir. Bu [öğreticide](custom-decision-service-tutorial-news.md#use-the-apis)bir örnek gösterilmektedir.

Yalnızca test için, Reward API 'SI [kıvrımlı](https://en.wikipedia.org/wiki/CURL)aracılığıyla çağrılabilir:

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

Beklenen efekt, 200 (Tamam) HTTP yanıtı olarak kullanılır. Günlükte bu olay için 1 ' in (Portalda bir Azure depolama hesabı anahtarı sağlanmış ise) yeniden olduğunu görebilirsiniz.

## <a name="action-set-api-customer-provided"></a>Eylem kümesi API 'SI (müşteri tarafından sağlanmış)

Yüksek düzeyde, eylem kümesi API 'SI makalelerin bir listesini döndürür (Eylemler). Her makale URL 'SI ve (isteğe bağlı olarak) makale başlığı ve yayımlama tarihi ile belirtilir. Portalda birkaç eylem kümesi belirtebilirsiniz. Ayrı bir URL olarak her bir eylem kümesi için farklı bir eylem kümesi API 'SI kullanılmalıdır.

Her eylem kümesi API 'SI iki şekilde uygulanabilir: RSS akışı veya Atom akışı olarak. Bunlardan biri, standarda uymalıdır ve doğru bir XML döndürmelidir. RSS için aşağıda bir örnek verilmiştir:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <guid>guid (could be a your internal database id)</guid>
      <pubDate>date</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Her üst düzey `<item>` öğe bir eylemi açıklar:

- `<link>`zorunludur ve bir eylem KIMLIĞI olarak kullanılır.
- `<date>`, 15 öğeden küçük veya buna eşitse yok sayılır; Aksi takdirde, bu zorunludur.
  - 15 ' ten fazla öğe varsa, en son 15 tane kullanılır.
  - Bu, sırasıyla RSS veya Atom için standart biçimde olmalıdır:
    - RSS için [RFC 822](https://tools.ietf.org/html/rfc822) : Örneğin,`"Fri, 28 Apr 2017 18:02:06 GMT"`
    - Atom için [RFC 3339](https://tools.ietf.org/html/rfc3339) : Örneğin,`"2016-12-19T16:39:57-08:00"`
- `<title>`isteğe bağlıdır ve makaleyi tanımlayan özellikler oluşturmak için kullanılır.
- `<guid>`isteğe bağlıdır ve sistemden geri çağırma işlevine geçirilir ( `?details` parametre derecelendirme API çağrısında belirtilmişse).

İçindeki `<item>` diğer öğeler yok sayılır.

Atom akışı sürümü aynı XML söz dizimini ve kurallarını kullanır.

> [!TIP]
> Sisteminiz kendi makale kimliklerini kullanıyorsa, kullanarak `<guid>`geri çağırma işlevine geçirilebilir.
