---
title: 'Öğretici: Desenler - LUIS'
description: Bu öğreticide daha az örnek açıklama sağlarken, amaç ve varlık tahminini artırmak için desenleri kullanın. Desen, varlıkları ve yoksayılabilir metni tanımlamak için sözdizimini içeren bir şablon sözcük örneği olarak sağlanır.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: 10f0ade45dedb3413887cc4b4dea89e857c1bde7
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80545982"
---
# <a name="tutorial-add-common-pattern-template-utterance-formats-to-improve-predictions"></a>Öğretici: Tahminleri geliştirmek için ortak desen şablonu oluşturma biçimleri ekleyin

Bu öğreticide, daha az örnek sözcük sağlamanıza olanak tanıyan niyet ve varlık tahminini artırmak için desenler kullanın. Desen, varlıkları ve yoksayılabilir metni tanımlamak için sözdizimini içeren bir amaca atanmış bir şablon sözcükbiçimidir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Desen oluşturma
> * Desen tahmin geliştirmelerini onaylama
> * Metni yok sayılabilir olarak işaretleme ve desen içine yerleştirme
> * Test panelini kullanarak desen erişimini doğrulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="utterances-in-intent-and-pattern"></a>Niyet ve desen desöyleymeler

LUIS uygulamasında depolanan iki tür söyleyiş vardır:

* Niyet'teki örnek söz
* Desendeki şablon söyleyinçleri

Şablon sözcüklerini desen olarak eklemek, bir amac için genel olarak daha az örnek sözcük sağlar.

Desen, metin eşleştirme ve makine öğreniminin bir leşimi olarak uygulanır.  Desendeki şablon, niyetteki örnek sözlerle birlikte, LUIS'e hangi söyleyişlerin niyete uyduğunu daha iyi anlamasını sağlar.

## <a name="import-example-app-and-clone-to-new-version"></a>Örnek uygulamayı ve klonunu yeni sürüme aktarma

Aşağıdaki adımları kullanın:

1.  Uygulamayı indirin ve kaydedin [JSON dosyasını.](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json?raw=true)

1. JSON'u [önizleme LUIS portalına](https://preview.luis.ai)yeni bir uygulamaya aktarın. **Uygulamalarım** sayfasında, **konuşma için + Yeni uygulamasını**seçin, ardından **JSON olarak Içe Aktar'ı**seçin. Önceki adımda indirdiğiniz dosyayı seçin.

1. **Sürümler** sekmesinde **Yönet** bölümünden etkin sürümü seçin ve ardından **Klon'u**seçin. Klonlanmış sürümü `patterns`adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="create-new-intents-and-their-utterances"></a>Yeni amaçları ve konuşmalarını oluşturma

İki niyet, yöneticinin veya yöneticinin doğrudan raporlarını, söyleyiş metnine göre bulur. Zorluk iki niyet farklı şeyler _demek_ ama kelimelerin çoğu aynıdır. Sadece sipariş kelimesi farklıdır. Niyetin doğru tahmin edilebilmesi için birçok örnek olması gerekir.

1. Gezinti çubuğundan **Oluştur'u** seçin.

1. **Niyetler** sayfasında, yeni bir amaç oluşturmak için **+ Oluştur'u** seçin.

1. Açılan iletişim kutusuna `OrgChart-Manager` girip **Done** (Bitti) öğesini seçin.

    ![Yeni ileti oluşturma açılır penceresi](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

1. Amaca örnek konuşmalar ekleyin. Bu söyleyişler _tam olarak_ aynı değildir ama ayıklanabilir bir desen var.

    |Örnek konuşmalar|
    |--|
    |`Who is John W. Smith the subordinate of?`|
    |`Who does John W. Smith report to?`|
    |`Who is John W. Smith's manager?`|
    |`Who does Jill Jones directly report to?`|
    |`Who is Jill Jones supervisor?`|

    Amacın konuşmalarında employee varlığı yerine keyPhrase varlığı etiketlenmişse endişelenmeyin. İkisi de uç noktanın Test bölmesinde doğru şekilde tahmin edilir.

1. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin.

1. Yeni bir amaç oluşturmak için **+ Oluştur'u** seçin. Açılan iletişim kutusuna `OrgChart-Reports` girip **Done** (Bitti) öğesini seçin.

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |`Who are John W. Smith's subordinates?`|
    |`Who reports to John W. Smith?`|
    |`Who does John W. Smith manage?`|
    |`Who are Jill Jones direct reports?`|
    |`Who does Jill Jones supervise?`|

### <a name="caution-about-example-utterance-quantity"></a>Örnek konuşma miktarıyla ilgili uyarı

[!INCLUDE [Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

### <a name="train-the-app-before-testing-or-publishing"></a>Uygulamayı test etmeden veya yayınlamadan önce eğitin

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

### <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak için yayımlama

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

### <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğundaki URL'nin sonuna gidin _YOUR_QUERY_HERE_ ve YOUR_QUERY_HERE `Who is the boss of Jill Jones?`değiştirin: .

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.313054234
                },
                "OrgChart-Reports": {
                    "score": 0.2462688
                },
                "EmployeeFeedback": {
                    "score": 0.0488328524
                },
                "GetJobInformation": {
                    "score": 0.0156933
                },
                "MoveEmployee": {
                    "score": 0.011265873
                },
                "Utilities.StartOver": {
                    "score": 0.003065792
                },
                "Utilities.Stop": {
                    "score": 0.00300148362
                },
                "Utilities.Cancel": {
                    "score": 0.00271081156
                },
                "None": {
                    "score": 0.00212835032
                },
                "ApplyForJob": {
                    "score": 0.0020669254
                },
                "Utilities.Confirm": {
                    "score": 0.00200891262
                },
                "FindForm": {
                    "score": 0.00194145238
                },
                "Utilities.Help": {
                    "score": 0.00182301877
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "Employee": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "Employee": [
                        {
                            "type": "Employee",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

En iyi iki niyetin puanları yakın ama en yüksek niyet önemli ölçüde yüksek değil (%60'ın üzerinde). ve bir sonraki niyet puanının üzerinde yeterince uzak değil.

LUIS eğitimi her seferinde tam olarak aynı olmadığından (biraz varyasyon vardır), bu en iyi iki puan bir sonraki eğitim döngüsünde tersine çevirebilir. Sonuç olarak yanlış amaç döndürülebilir.

Doğru amacın puan yüzdesini bir sonraki en yüksek puandan bir miktar daha yüksek ve uzak hale getirmek için desenleri kullanın.

Bu ikinci tarayıcı penceresini açık bırakın. Öğreticinin sonraki bölümlerinde kullanacaksınız.

## <a name="template-utterances"></a>Konuşma şablonları
İnsan Kaynakları konu etki alanının doğası nedeniyle, kuruluşlardaki çalışan ilişkileri hakkında birkaç ortak soru yolu vardır. Örnek:

|Konuşmalar|
|--|
|`Who does Jill Jones report to?`|
|`Who reports to Jill Jones?`|

Bu konuşmalar birbirine çok yakın olduğundan bağlam açısından farklarını belirlemek için birden fazla konuşma örneği sağlamanız gerekir. Amaç için bir desen eklediğinizde LUIS, bir amaç için sık kullanılan konuşma desenlerini çok sayıda konuşma örneği eklemeden öğrenir.

Bu amaç için bazı konuşma şablonu örnekleri şunlardır:

|Konuşma şablonu örnekleri|söz dizimini anlamı|
|--|--|
|`Who does {Employee} report to[?]`|Değiştirilebilir`{Employee}`<br>Yoksay`[?]`|
|`Who reports to {Employee}[?]`|Değiştirilebilir`{Employee}`<br>Yoksay`[?]`|

`{Employee}` söz dizimi, varlığın konuşma şablonu içindeki konumunu ve hangi varlık olduğunu belirtir. İsteğe bağlı `[?]`sözdizimi, işaretleri sözcüklerveya isteğe bağlı noktalama. LUIS konuşmayı eşleştirir ve parantez içindeki isteğe bağlı metni yoksayar.

Sözdizimi normal bir ifade gibi görünse de, normal bir ifade değildir. Yalnızca küme ayracı `{}` ve köşeli ayraç `[]` söz dizimi desteklenir. İki düzeye kadar iç içe yerleştirme yapılabilir.

Bir örüntün bir söyleyiş ile eşleşebilmesi için, _öncelikle_ sözcük içindeki varlıkların şablon daki varlıklarla eşleşmesi gerekir. Bu, varlıkların desenleri başarılı olmadan önce, varlıkların yüksek derecede tahmin içeren örnek liyatasahip olması anlamına gelir. Ancak şablon varlıkların değil yalnızca amaçların tahmin edilmesine yardımcı olur.

**Desenler daha az örnek konuşma sağlamanızı mümkün kılsa da varlıkların algılanmaması durumunda desen eşleşmez.**

### <a name="add-the-patterns-for-the-orgchart-manager-intent"></a>OrgChart-Manager amacıyla desenleri ekleyin

1. Üst menüden **Build** (Derle) öğesini seçin.

1. Sol gezinti bölmesinin **Improve app performance** (Uygulama performansını geliştir) bölümünde **Patterns** (Desenler) öğesini seçin.

1. **OrgChart-Manager** amacını seçip aşağıdaki konuşma şablonlarını girin:

    |Konuşma şablonları|
    |:--|
    |`Who is {Employee} the subordinate of[?]`|
    |`Who does {Employee} report to[?]`|
    |`Who is {Employee}['s] manager[?]`|
    |`Who does {Employee} directly report to[?]`|
    |`Who is {Employee}['s] supervisor[?]`|
    |`Who is the boss of {Employee}[?]`|

    Bu şablon söyleyinceleri kıvırcık köşeli ayraç gösterimi ile **Çalışan** varlık içerir.

1. Desenler sayfasında yken, **OrgChart-Reports** amacını seçin ve ardından aşağıdaki şablon sözcülerini girin:

    |Konuşma şablonları|
    |:--|
    |`Who are {Employee}['s] subordinates[?]`|
    |`Who reports to {Employee}[?]`|
    |`Who does {Employee} manage[?]`|
    |`Who are {Employee} direct reports[?]`|
    |`Who does {Employee} supervise[?]`|
    |`Who does {Employee} boss[?]`|

### <a name="query-endpoint-when-patterns-are-used"></a>Desenler kullanıldığında uç noktayı sorgulama

Artık desenler uygulamaya eklendiklerine göre, tahmin çalışma zamanı bitiş noktasında uygulamayı eğitin, yayınlayın ve sorgulayın.

1. **Tren'i**seçin. Eğitim tamamlandıktan sonra **Yayımla'yı** seçin ve **Üretim** yuvasını seçin ve **ardından Bitti'yi**seçin.

1. Yayımlama tamamlandıktan sonra tarayıcı sekmelerini bitiş noktası URL sekmesine geri değiştirin.

1. Adres çubuğundaki URL'nin sonuna gidin ve _YOUR_QUERY_HERE_ değiştirin:`Who is the boss of Jill Jones?`

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.999997854
                },
                "OrgChart-Reports": {
                    "score": 6.13748343E-05
                },
                "EmployeeFeedback": {
                    "score": 8.052567E-06
                },
                "GetJobInformation": {
                    "score": 1.18197136E-06
                },
                "MoveEmployee": {
                    "score": 7.65549657E-07
                },
                "None": {
                    "score": 3.975E-09
                },
                "Utilities.StartOver": {
                    "score": 1.53E-09
                },
                "Utilities.Confirm": {
                    "score": 1.38181822E-09
                },
                "Utilities.Help": {
                    "score": 1.38181822E-09
                },
                "Utilities.Stop": {
                    "score": 1.38181822E-09
                },
                "Utilities.Cancel": {
                    "score": 1.25833333E-09
                },
                "FindForm": {
                    "score": 1.15384613E-09
                },
                "ApplyForJob": {
                    "score": 5.26923061E-10
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "Employee": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "Employee": [
                        {
                            "type": "Employee",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

Niyet tahmini şimdi önemli ölçüde daha güvenli ve sonraki en yüksek niyet puanı önemli ölçüde daha düşüktür. Bu iki niyet antrenman yaparken terlik sürmez.

### <a name="working-with-optional-text-and-prebuilt-entities"></a>İsteğe bağlı metin ve önceden oluşturulmuş varlıklarla çalışma

Bu öğreticinin önceki bölümlerinde kullanılan desen konuşma şablonları s harfinin iyelik olarak kullanılması `'s` ve soru işaretinin kullanılması `?` gibi birkaç isteğe bağlı metin örneğine sahiptir. Söyleyiş metninde geçerli ve gelecekteki tarihlere izin vermeniz gerektiğini varsayalım.

Örnek konuşmalar şunlardır:

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

Bu örneklerin her birinde LUIS'in doğru tahmin yapabilmesi için gerekli olan bir fiil çekimi (`was`, `is`, `will be`) ve tarih (`March 3`, `now` ve `in a month`) bulunmaktadır. Tablodaki son iki örnek dışında hemen hemen aynı `in` `on`metni kullandığına dikkat edin.

Bu isteğe bağlı bilgilere izin veren örnek şablon belaları:

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|:--|:--|
|OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|


Söz diziminde isteğe bağlı köşeli parantez `[]` kullanılması isteğe bağlı metnin konuşma şablonuna eklenmesini kolaylaştırır, ikinci düzeye kadar iç içe yerleştirilebilir `[[]]` ve varlık ya da metin içerebilir.


**Soru: Neden tüm `w` harfler, her şablonda ilk harf, küçük harf? İsteğe bağlı olarak daha büyük veya küçük olmaları gerekmez mi?** İstemci uygulaması tarafından sorgu uç noktasına gönderilen konuşma küçük harfe dönüştürülür. Konuşma şablonu ve uç nokta konuşmasında büyük harf veya küçük harf kullanılabilir. Karşılaştırma her zaman küçük harfe dönüştürme sonrasında gerçekleştirilir.

**Soru: March 3 (Mart 3) hem sayı `3` hem de tarih `March 3` olarak tahmin ediliyorsa konuşma şablonunun sayı bölümü neden önceden oluşturulmuş durumda değil?** Konuşma şablonu tahmini `March 3` olarak doğrudan veya `in a month` çıkarımıyla bağlamsal olarak kullanmaktadır. Tarih, sayı içerebilir ancak her sayı tarih olmayabilir. Her zaman tahmin JSON sonuçlarında döndürülmesini istediğiniz türü en iyi temsil eden varlığını kullanın.

**Soru: `Who will {Employee}['s] manager be on March 3?` gibi zayıf ifadeler nasıl işlenir?** `will` ve `be` ifadelerinin ayrılması gereken bunun gibi dilbilgisi açısından farklı fiil çekimlerinin yeni bir konuşma şablonu halinde ayrılması gerekir. Var olan konuşma şablonu bununla eşleşmez. Konuşmanın amacı değişmiş olmasına rağmen konuşmadaki kelime yerleşimleri değişmemiştir. Bu değişiklik LUIS tahminini etkiler. Bu söyleyiyi birleştirmek için [grupve veya](#use-the-or-operator-and-groups) fiil-zamanları gruplayabilirsiniz.

**Unutmayın: Önce varlıklar bulunur, ardından desen eşleştirilir.**

### <a name="add-new-pattern-template-utterances"></a>Yeni konuşma şablonu deseni ekleme

1. **Build** (Derle) menüsünün **Patterns** (Desenler) bölümünde birden fazla yeni konuşma şablonu deseni ekleyebilirsiniz. Intent (Amaç) açılan menüsünden **OrgChart-Manager** öğesini seçin ve aşağıdaki konuşma şablonlarını girin:

    |Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
    |--|--|
    |OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Uygulamayı eğitmek için navigasyon çubuğundan **Tren'i** seçin.

3. Eğitim tamamlandıktan sonra, test panelini açmak için panelin üst kısmındatest'i **seçin.**

4. Desenin eşleştirildiğini ve amaç puanının oldukça yüksek olduğunu doğrulamak için birkaç test konuşması girin.

    İlk konuşmayı girdikten sonra tüm tahmin sonuçlarını görebilmek için sonucun altındaki **Inspect** (İncele) öğesini seçin. Her söyleyiş **OrgChart-Manager** amacına sahip olmalı ve Çalışan ve datetimeV2 varlıkları için değerleri ayıklamak gerekir.

    |İfade|
    |--|
    |`Who will be Jill Jones manager`|
    |`who will be jill jones's manager`|
    |`Who will be Jill Jones's manager?`|
    |`who will be Jill jones manager on March 3`|
    |`Who will be Jill Jones manager next Month`|
    |`Who will be Jill Jones manager in a month?`|

Bu konuşmaların tümü varlık bulduğundan aynı desenle eşleşir ve yüksek tahmin puanına sahiptir. Birçok sözcük çeşileçliği yapacak birkaç desen eklediniz. Şablon söyleyişin desende çalışması için herhangi bir örnek söyleyiş eklemenize gerek yoktu.

Sağlanan desenlerin bu kullanımı:
* Daha yüksek tahmin puanları
* Niyet aynı örnek deyişletitler ile
* Desen deseni sadece birkaç iyi inşa şablon uyrarak ile

### <a name="use-the-or-operator-and-groups"></a>OR işleci ve gruplarını kullanma

Önceki şablon ların bazıları çok yakındır. Şablon söyleyişlerini azaltmak için **grubu** `()` ve **OR** `|` sözdizimini kullanın.

Aşağıdaki 2 desen, grup `()` ve OR `|` sözdizimini kullanarak tek bir desende birleşebilir.

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|--|--|
|OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

Yeni şablon söyleyiş olacaktır:

`who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`.

Bu, gerekli fiil zaman ve isteğe `on` bağlı `in` ve aralarında bir **veya** boru ile etrafında bir **grup** kullanır.

1. **Desenler** sayfasında **OrgChart-Manager** filtresini seçin. Arayarak `manager`listeyi daralt.

1. Şablon söyleyinin bir sürümünü tutun (bir sonraki adımda düzenlemek için) ve diğer varyasyonları silin.

1. Konuşma şablonunu şu şekilde değiştirin: 

    `who ( was | is | will be ) {Employee}['s] manager [([in]|[on]){datetimeV2}?]`

2. Uygulamayı eğitmek için navigasyon çubuğundan **Tren'i** seçin.

3. Eğitim tamamlandıktan sonra, test panelini açmak için panelin üst kısmındatest'i **seçin.**

    Söyleyiş sürümlerini test etmek için Test bölmesini kullanın:

    |Test bölmesine girilen söylenmeler|
    |--|
    |`Who is Jill Jones manager this month`|
    |`Who is Jill Jones manager on July 5th`|
    |`Who was Jill Jones manager last month`|
    |`Who was Jill Jones manager on July 5th`|
    |`Who will be Jill Jones manager in a month`|
    |`Who will be Jill Jones manager on July 5th`|

Daha fazla desen sözdizimini kullanarak, uygulamanızda tutmanız gereken şablon sözcük lerinin sayısını azaltırken, yine de yüksek bir tahmin puanına sahip olursunuz.

### <a name="use-the-utterance-beginning-and-ending-anchors"></a>Sözcük başlangıç ve bitiş çapalarını kullanma

Desen sözdizimi, bir bakıcının başlangıç ve bitiş `^`söyleyiş bağlantı sözdizimini sağlar. Başlangıç ve bitiş söyleyiş çapa ları çok özel ve muhtemelen edebi söyleyiyi hedeflemek için birlikte kullanılabilir veya niyetleri hedeflemek için ayrı ayrı kullanılabilir.

## <a name="using-patternany-entity"></a>Pattern.any entity'i kullanma

pattern.any varlığı, ifade nedeniyle varlığın sonunun konuşmanın geri kalanından ayrılmasının zor olduğu durumlarda serbest biçimli verileri bulmanızı sağlar.

Bu İnsan Kaynakları uygulaması, çalışanların şirket formlarını bulmasına yardımcı olmaktadır.

|İfade|
|--|
|Where is **HRF-123456**? (HRF-123456 nerede?)|
|Who authored **HRF-123234**? (HRF-123234'ü kim yazdı?)|
|**HRF-456098** is published in French? (HRF-456098 Fransızca mı yayımlandı?)|

Ancak her formun hem bir biçimlendirilmiş adı (yukarıdaki tabloda kullanılan) hem de kolay adı (`Request relocation from employee new to the company 2018 version 5` gibi) vardır.

Formun kolay adı konuşmada şu şekilde olur:

|İfade|
|--|
|Where is **Request relocation from employee new to the company 2018 version 5**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 nerede?)|
|Who authored **"Request relocation from employee new to the company 2018 version 5"**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5'i kim yazdı?)|
|**Request relocation from employee new to the company 2018 version 5** is published in French? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 Fransızca mı yayımlandı?)|

Sözcüklerin uzunluklarının değişmesi, LUIS'in varlığın sonunu belirleme konusunda karışıklık yaşamasına neden olabilir. Pattern.any varlığını bir desen içinde kullanmak, form adının başını ve sonunu belirterek LUIS'in form adını doğru şekilde ayıklamasına yardımcı olmanızı sağlar.

|Konuşma şablonu örneği|
|--|
|Where is {FormName}[?] ({FormName} nerede[?])|
|Who authored {FormName}[?] ({FormName} kim yazdı[?])|
|{FormName} is published in French[?] ({FormName} Fransızca mı yayımlandı[?])|

### <a name="add-example-utterances-with-patternany"></a>Desen ile örnek söyleyinekler ekleyin.any

1. Üst gezinti bölmesinden **Build** (Derle) öğesini, sol gezinti bölmesinden de **Intents** (Amaçlar) öğesini seçin.

1. Amaç listesinden **FindForm** öğesini seçin.

1. Bazı örnek söyleyişler ekleyin. Desen olarak tahmin edilmesi gereken metin.any **kalın**metindedir. Form adını, sözcükteki diğer sözcüklerden belirlemek zordur. Desen.any varlığın sınırlarını işaretleyerek yardımcı olacaktır.

    |Örnek konuşma|Form adı|
    |--|--|
    |Where is the form **What to do when a fire breaks out in the Lab** and who needs to sign it after I read it? (Laboratuvarda yangın çıktığında yapılacak işlemler formu nerede ve ben okuduktan sonra kimin imzalaması gerekiyor?)|Laboratuvarda yangın çıktığında ne yapmalı?
    |Where is **Request relocation from employee new to the company** on the server? (Yeni şirket çalışanı taşınma talep formu sunucuda nerede?)|Yeni çalışandan şirkete taşınma isteği|
    |Who authored "**Health and wellness requests on the main campus**" and what is the most current version? ("Ana kampüsteki sağlık ve sağlıklı yaşam istekleri"ni kim yazdı ve en güncel sürümü nedir?)|Ana kampüste sağlık ve zindelik talepleri|
    |I'm looking for the form named "**Office move request including physical assets**". ("Fiziksel eşyalar dahil olmak üzere ofis taşıma isteği" adlı formu arıyorum.) |Fiziksel varlıklar da dahil olmak üzere ofis taşıma isteği|

    Form adlarındaki değişik kullanımlar nedeniyle Pattern.any varlığı olmadan LUIS'in form başlığının sonunu belirlemesi zor olacaktır.

### <a name="create-a-patternany-entity"></a>Pattern.any varlığı oluşturma
Pattern.any varlığı farklı uzunluklardaki varlıkları ayıklar. Desen sözdizimi ile varlığın başlangıcını ve sonunu işaretlediği için yalnızca bir desende çalışır.

1. Sol gezinti panelinden **Entities** (Varlıklar) öğesini seçin.

1. **+ Oluştur**' u `FormName`seçin, adı girin ve **Desen.any'yi** seçin. **Oluştur'u**seçin.

### <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any kullanan bir desen ekleme

1. Sol gezinti bölmesinden **Patterns** (Desenler) öğesini seçin.

1. **FindForm** amacını seçin.

1. Aşağıdaki yeni varlığı kullanan konuşma şablonlarını girin:

    |Konuşma şablonları|
    |--|
    |`Where is the form ["]{FormName}["] and who needs to sign it after I read it[?]`|
    |`Where is ["]{FormName}["] on the server[?]`|
    |`Who authored ["]{FormName}["] and what is the most current version[?]`|
    |`I'm looking for the form named ["]{FormName}["][.]`|

1. Uygulamayı eğitin.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Serbest biçimli metin ayıklama için yeni deseni test etme
1. Üst çubuktan **Test**'i seçerek test panelini açın.

1. Aşağıdaki konuşmayı girin:

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

1. Varlık ve amaç test sonuçlarını görmek için sonucun altındaki **Inspect** (Denetle) öğesini seçin.

    Önce `FormName` varlığı bulunmuş, ardından amacı belirten desen bulunmuştur. Test sonucunda varlıklar algılanmadıysa ve bu nedenle desen bulunmadıysa amaçla (desenle değil) ilgili daha fazla örnek konuşma eklemeniz gerekir.

1. Üst gezinti çubuğundan **Test** düğmesini seçerek test panelini kapatın.

### <a name="using-an-explicit-list"></a>Açık bir liste kullanma

Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın.

## <a name="what-did-this-tutorial-accomplish"></a>Bu öğretici neyi başardı?

Bu öğretici, LUIS'in daha fazla örnek söyleyiş eklemek zorunda kalmadan amacı önemli ölçüde daha yüksek bir puanla tahmin etmelerine yardımcı olacak desenler ekledi. Varlıkları ve yok sayılabilir metinleri işaretlemek LUIS'in deseni daha fazla konuşmaya uygulamasını mümkün hale getirdi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [Desen ile rol kullanmayı öğrenin](luis-tutorial-pattern.md)
