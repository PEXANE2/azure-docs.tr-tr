---
title: 'Öğretici: desenler-LUSıS'
description: Bu öğreticide daha az örnek Aralık sağlarken amacı ve varlık tahminini artırmak için desenleri kullanın. Desen, varlıkları ve yoksayılabilir metni tanımlamak için sözdizimi içeren bir şablon söylenişi örneği olarak sağlanır.
ms.topic: tutorial
ms.date: 07/06/2020
ms.openlocfilehash: 3ca8bb15d19b0fa0dd6b33d35a380c0b1b07abe0
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "86039509"
---
# <a name="tutorial-add-common-pattern-template-utterance-formats-to-improve-predictions"></a>Öğretici: tahminleri geliştirmek için ortak desen şablonu söylenişi biçimleri ekleme

Bu öğreticide, daha az örnek elde etmenizi sağlayan amaç ve varlık tahminini artırmak için desenleri kullanın. Bu desen, varlıkları ve yoksayılabilir metni tanımlamak için sözdizimi içeren bir amaca atanan bir şablon utterine atanır.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Desen oluşturma
> * Desen tahmin geliştirmelerini onaylama
> * Metni yok sayılabilir olarak işaretleme ve desen içine yerleştirme
> * Test panelini kullanarak desen erişimini doğrulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="utterances-in-intent-and-pattern"></a>Amaç ve desen için söylenme

LUSıS uygulamasında depolanan iki tür utterde vardır:

* Amaç ile örnek söylenme
* Desen içindeki şablon utsliği

Şablon utinlerini bir desen olarak eklemek, bir amaca göre daha az örnek elde etmenizi sağlar.

Desen, metin eşleştirmesi ile makine öğrenmesinin bir birleşimi olarak uygulanır.  Desendeki şablon ifadesi amaçtaki örnek ifadeyle birlikte LUIS hizmetinin amaca uygun konuşmaları anlamasını kolaylaştırır.

## <a name="import-example-app-and-clone-to-new-version"></a>Örnek uygulamayı ve kopyayı yeni sürüme aktar

Aşağıdaki adımları kullanın:

1.  [Uygulama json dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/tutorial-fix-unsure-predictions.json?raw=true)indirip kaydedin.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. JSON 'ı yeni bir uygulamaya bir [halin portalına](https://www.luis.ai)aktarın. **Uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktar**' ı seçin. Önceki adımda indirdiğiniz dosyayı seçin, uygulamayı adlandırın `Patterns tutorial` .

## <a name="create-new-intents-and-their-utterances"></a>Yeni amaçları ve konuşmalarını oluşturma

İki amaç, söylenişi metnine göre yöneticiyi veya yöneticinin doğrudan raporlarını bulur. Zorluk, iki amaç farklı şeyler _anlamına gelmektedir_ ancak sözcüklerin çoğu aynı olur. Yalnızca sözcük sırası farklıdır. Amacın doğru bir şekilde tahmin edilmesi için birçok örnek içermelidir.

1. Gezinti çubuğundan **Oluştur** ' u seçin.

1. Yeni bir amaç oluşturmak için **amaçlar** sayfasında **+ Oluştur** ' u seçin.

1. Açılan iletişim kutusuna `OrgChart-Manager` girip **Done** (Bitti) öğesini seçin.

    ![Yeni ileti oluşturma açılır penceresi](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

1. Amaca örnek konuşmalar ekleyin. Bu uttaslar _tam olarak_ benzer değildir ancak ayıklanabilen bir desen vardır.

    |Örnek konuşmalar|
    |--|
    |`Who is John W. Smith the subordinate of?`|
    |`Who does John W. Smith report to?`|
    |`Who is John W. Smith's manager?`|
    |`Who does Jill Jones directly report to?`|
    |`Who is Jill Jones supervisor?`|

1. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin.

1. Yeni bir amaç oluşturmak için **+ Oluştur** ' u seçin. Açılan iletişim kutusuna `OrgChart-Reports` girip **Done** (Bitti) öğesini seçin.

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

### <a name="train-the-app-before-testing-or-publishing"></a>Test veya yayımlamadan önce uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

### <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

### <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ ile değiştirin: `Who is the boss of Jill Jones?` .

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.326605469
                },
                "OrgChart-Reports": {
                    "score": 0.127583548
                },
                "EmployeeFeedback": {
                    "score": 0.0299124215
                },
                "MoveEmployee": {
                    "score": 0.01159851
                },
                "GetJobInformation": {
                    "score": 0.0104600191
                },
                "ApplyForJob": {
                    "score": 0.007508645
                },
                "Utilities.StartOver": {
                    "score": 0.00359402061
                },
                "Utilities.Stop": {
                    "score": 0.00336530479
                },
                "FindForm": {
                    "score": 0.002653719
                },
                "Utilities.Cancel": {
                    "score": 0.00263288687
                },
                "None": {
                    "score": 0.00238638581
                },
                "Utilities.Help": {
                    "score": 0.00226386427
                },
                "Utilities.Confirm": {
                    "score": 0.00211663754
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "EmployeeListEntity": [
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
                    "EmployeeListEntity": [
                        {
                            "type": "EmployeeListEntity",
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

Doğru en iyi amaç tahmin edildi, `OrgChart-Manager` ancak puan %70 üzerinde değil ve en yüksek bir sonraki amaç üzerinde yeterince fazla değil. Doğru amacın puan yüzdesini bir sonraki en yüksek puandan bir miktar daha yüksek ve uzak hale getirmek için desenleri kullanın.

Bu ikinci tarayıcı penceresini açık bırakın. Öğreticide daha sonra bir kez kullanacaksınız.

## <a name="template-utterances"></a>Konuşma şablonları
Insan kaynakları konu etki alanının doğası gereği, kuruluşlardaki çalışan ilişkilerini sormanın bazı yaygın yolları vardır. Örneğin:

|İfadeler|
|--|
|`Who does Jill Jones report to?`|
|`Who reports to Jill Jones?`|

Bu söyleyler, _pek çok_ söylenişi örneği sağlamadan her birinin bağlamsal benzersizliğini belirlemede çok yakın. Bir amaç için bir desen ekleyerek, çok sayıda daha fazla söylenişi örneği sağlamaya gerek kalmadan bir amaç için yaygın olarak kullanılan bir desenler öğreniyor.

Bu amaç için bazı konuşma şablonu örnekleri şunlardır:

|Konuşma şablonu örnekleri|söz dizimini anlamı|
|--|--|
|`Who does {EmployeeListEntity} report to[?]`|değiştirilebilir `{EmployeeListEntity}`<br>Yoksay `[?]`|
|`Who reports to {EmployeeListEntity}[?]`|değiştirilebilir `{EmployeeListEntity}`<br>Yoksay `[?]`|

`{EmployeeListEntity}` söz dizimi, varlığın konuşma şablonu içindeki konumunu ve hangi varlık olduğunu belirtir. İsteğe bağlı sözdizimi, `[?]` , kelime veya [noktalama](luis-reference-application-settings.md#punctuation-normalization) işareti isteğe bağlıdır. LUIS konuşmayı eşleştirir ve parantez içindeki isteğe bağlı metni yoksayar.

Sözdizimi bir normal ifade gibi göründüğünden, normal bir ifade değildir. Yalnızca küme ayracı `{}` ve köşeli ayraç `[]` söz dizimi desteklenir. İki düzeye kadar iç içe yerleştirme yapılabilir.

Bir düzenin bir söylenişi ile eşleşmesi için, _ilk_ olarak, bir desen içindeki varlıkların, şablondaki varlıklarla eşleşmesi gerekir. Bu, varlıkların varlıklar içeren desenler başarılı olmadan önce, büyük bir tahmine sahip bir şekilde daha fazla örneğe sahip olması anlamına gelir. Ancak şablon varlıkların değil yalnızca amaçların tahmin edilmesine yardımcı olur.

**Desenler daha az örnek konuşma sağlamanızı mümkün kılsa da varlıkların algılanmaması durumunda desen eşleşmez.**

### <a name="add-the-patterns-for-the-orgchart-manager-intent"></a>Kuruluş Yöneticisi amacı için desenler ekleme

1. Üst menüden **Build** (Derle) öğesini seçin.

1. Sol gezinti bölmesinin **Improve app performance** (Uygulama performansını geliştir) bölümünde **Patterns** (Desenler) öğesini seçin.

1. **OrgChart-Manager** amacını seçip aşağıdaki konuşma şablonlarını girin:

    |Konuşma şablonları|
    |:--|
    |`Who is {EmployeeListEntity} the subordinate of[?]`|
    |`Who does {EmployeeListEntity} report to[?]`|
    |`Who is {EmployeeListEntity}['s] manager[?]`|
    |`Who does {EmployeeListEntity} directly report to[?]`|
    |`Who is {EmployeeListEntity}['s] supervisor[?]`|
    |`Who is the boss of {EmployeeListEntity}[?]`|

    Bu şablon, küme ayracı gösterimine sahip **Employeelistentity** varlığını içerir.

1. Desenler sayfasında, **kuruluş şeması-raporlar** hedefini seçin, sonra aşağıdaki şablon utslerini girin:

    |Konuşma şablonları|
    |:--|
    |`Who are {EmployeeListEntity}['s] subordinates[?]`|
    |`Who reports to {EmployeeListEntity}[?]`|
    |`Who does {EmployeeListEntity} manage[?]`|
    |`Who are {EmployeeListEntity} direct reports[?]`|
    |`Who does {EmployeeListEntity} supervise[?]`|
    |`Who does {EmployeeListEntity} boss[?]`|

### <a name="query-endpoint-when-patterns-are-used"></a>Desenler kullanıldığında uç noktayı sorgulama

Artık desenler uygulamaya eklendiğine göre, uygulamayı tahmin çalışma zamanı uç noktasında eğitin, yayımlar ve sorgular.

1. **Eğit**'i seçin. Eğitim tamamlandıktan sonra **Yayımla** ' yı seçin ve **Üretim** yuvasını seçin ve **bitti**' yi seçin.

1. Yayımlama işlemi tamamlandıktan sonra, tarayıcı sekmelerini uç nokta URL 'SI sekmesine geri geçirin.

1. Adres çubuğunda URL 'nin sonuna gidin ve sorgunuzun hala `Who is the boss of Jill Jones?` Yeni bir tahmin için URL 'yi göndereceğini doğrulayın.

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.999999046
                },
                "OrgChart-Reports": {
                    "score": 3.237443E-05
                },
                "EmployeeFeedback": {
                    "score": 4.364242E-06
                },
                "GetJobInformation": {
                    "score": 1.616159E-06
                },
                "MoveEmployee": {
                    "score": 7.575752E-07
                },
                "ApplyForJob": {
                    "score": 5.234157E-07
                },
                "None": {
                    "score": 3.3E-09
                },
                "Utilities.StartOver": {
                    "score": 1.26E-09
                },
                "FindForm": {
                    "score": 1.13636367E-09
                },
                "Utilities.Cancel": {
                    "score": 1.13636367E-09
                },
                "Utilities.Confirm": {
                    "score": 1.13636367E-09
                },
                "Utilities.Help": {
                    "score": 1.13636367E-09
                },
                "Utilities.Stop": {
                    "score": 1.13636367E-09
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "EmployeeListEntity": [
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
                    "EmployeeListEntity": [
                        {
                            "type": "EmployeeListEntity",
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

Amaç tahmini artık önemli ölçüde daha önemlidir ve bir sonraki en yüksek amaç puanı çok düşüktür. Bu iki amaç, eğitim sırasında-flop ' i çevirmez.

### <a name="working-with-optional-text-and-prebuilt-entities"></a>İsteğe bağlı metin ve önceden oluşturulmuş varlıklarla çalışma

Bu öğreticinin önceki bölümlerinde kullanılan desen konuşma şablonları s harfinin iyelik olarak kullanılması `'s` ve soru işaretinin kullanılması `?` gibi birkaç isteğe bağlı metin örneğine sahiptir. Söylenişi metninde geçerli ve gelecek tarihlere izin vermeniz gerektiğini varsayalım.

Örnek konuşmalar şunlardır:

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

Bu örneklerin her birinde LUIS'in doğru tahmin yapabilmesi için gerekli olan bir fiil çekimi (`was`, `is`, `will be`) ve tarih (`March 3`, `now` ve `in a month`) bulunmaktadır. Tablodaki son iki örneğin ve hariç neredeyse aynı metni kullandığına dikkat edin `in` `on` .

Bu isteğe bağlı bilgilere izin veren örnek şablon örnekleri:

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|:--|:--|
|OrgChart-Manager|`who was {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who is {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|


Söz diziminde isteğe bağlı köşeli parantez `[]` kullanılması isteğe bağlı metnin konuşma şablonuna eklenmesini kolaylaştırır, ikinci düzeye kadar iç içe yerleştirilebilir `[[]]` ve varlık ya da metin içerebilir.


**Soru: her bir şablon için `w` ilk harf olan tüm harfler, küçük harflerle neden kullanılır? İsteğe bağlı olarak büyük veya küçük harf olmaması gerekir mi?** İstemci uygulaması tarafından sorgu uç noktasına gönderilen konuşma küçük harfe dönüştürülür. Konuşma şablonu ve uç nokta konuşmasında büyük harf veya küçük harf kullanılabilir. Karşılaştırma her zaman küçük harfe dönüştürme sonrasında gerçekleştirilir.

**Soru: March 3 (Mart 3) hem sayı `3` hem de tarih `March 3` olarak tahmin ediliyorsa konuşma şablonunun sayı bölümü neden önceden oluşturulmuş durumda değil?** Konuşma şablonu tahmini `March 3` olarak doğrudan veya `in a month` çıkarımıyla bağlamsal olarak kullanmaktadır. Tarih, sayı içerebilir ancak her sayı tarih olmayabilir. Her zaman tahmin JSON sonuçlarında döndürülmesini istediğiniz türü en iyi temsil eden varlığını kullanın.

**Soru: `Who will {EmployeeListEntity}['s] manager be on March 3?` gibi zayıf ifadeler nasıl işlenir?** `will` ve `be` ifadelerinin ayrılması gereken bunun gibi dilbilgisi açısından farklı fiil çekimlerinin yeni bir konuşma şablonu halinde ayrılması gerekir. Var olan konuşma şablonu bununla eşleşmez. Konuşmanın amacı değişmiş olmasına rağmen konuşmadaki kelime yerleşimleri değişmemiştir. Bu değişiklik LUIS tahminini etkiler. Bu söyleyleri birleştirmek için [ve veya](#use-the-or-operator-and-groups) fiil-tenslerini gruplandırabilirsiniz.

> [!CAUTION]
> **Unutmayın: Önce varlıklar bulunur, ardından desen eşleştirilir.**

### <a name="add-new-pattern-template-utterances"></a>Yeni konuşma şablonu deseni ekleme

1. **Build** (Derle) menüsünün **Patterns** (Desenler) bölümünde birden fazla yeni konuşma şablonu deseni ekleyebilirsiniz. Intent (Amaç) açılan menüsünden **OrgChart-Manager** öğesini seçin ve aşağıdaki konuşma şablonlarını girin:

    |Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
    |--|--|
    |OrgChart-Manager|`who was {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|

2. Uygulamayı eğitebilmeniz için gezinti çubuğunda **eğitme** ' yi seçin.

3. Eğitim tamamlandıktan sonra, test panelini açmak için panelin en üstündeki **Test** ' i seçin.

4. Desenin eşleştirildiğini ve amaç puanının oldukça yüksek olduğunu doğrulamak için birkaç test konuşması girin.

    İlk konuşmayı girdikten sonra tüm tahmin sonuçlarını görebilmek için sonucun altındaki **Inspect** (İncele) öğesini seçin. Her bir söylemeye, **Kuruluş Yöneticisi** amacına sahip olmalıdır ve ve varlıklarının değerlerini ayıklamalıdır `EmployeeListEntity` `datetimeV2` .

    |İfade|
    |--|
    |`Who will be Jill Jones manager`|
    |`who will be jill jones's manager`|
    |`Who will be Jill Jones's manager?`|
    |`who will be Jill jones manager on March 3`|
    |`Who will be Jill Jones manager next Month`|
    |`Who will be Jill Jones manager in a month?`|

Bu konuşmaların tümü varlık bulduğundan aynı desenle eşleşir ve yüksek tahmin puanına sahiptir. Birçok farklı çeşitliliğe uyum sağlayacak birkaç desen eklediniz. Şablonda şablon utinine sahip olmak için herhangi bir örnek ekleme gerekmedi.

Belirtilen desenlerin kullanımı:
* Daha yüksek tahmin puanları
* Amaç ile aynı örnek,
* Yalnızca birkaç iyi oluşturulmuş şablon ile birlikte desen

### <a name="use-the-or-operator-and-groups"></a>OR işlecini ve gruplarını kullanın

Önceki şablon dıklıları çok yakın. **group** `()` **OR** `|` Şablon Uttaslarını azaltmak için grubunu ve veya sözdizimini kullanın.

Aşağıdaki iki desen, Grup `()` ve veya söz dizimini kullanarak tek bir düzende birleştirilebilir `|` .

|Amaç|İsteğe bağlı metin ve önceden oluşturulmuş varlıklara sahip örnek konuşmalar|
|--|--|
|OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|

Yeni şablon söylenişi şu şekilde olacaktır:

`who ( was | is | will be ) {EmployeeListEntity}['s] manager [([in]|[on]){datetimeV2}?]`.

Bu, gerekli **group** fiil zaman hali ve isteğe bağlı `in` ve aralarında `on` bir **veya** dikey çizgi etrafında bir grup kullanır.

> [!NOTE]
> _Or_ sembolünü `|` (pipe) kullanırken, kanal sembolünü örnek şablonda ve sonra gelen bir boşluk ile ayırdığınızdan emin olun.

1. **Desenler** sayfasında, **kuruluş şeması-yönetici** filtresini seçin. Öğesini arayarak listeyi daraltın `manager` .

1. Şablonun tek bir sürümünü saklayın (bir sonraki adımda düzenlemek için) ve diğer çeşitlemeleri silin.

1. Konuşma şablonunu şu şekilde değiştirin: 

    `who ( was | is | will be ) {EmployeeListEntity}['s] manager [([in]|[on]){datetimeV2}?]`

2. Uygulamayı eğitebilmeniz için gezinti çubuğunda **eğitme** ' yi seçin.

3. Eğitim tamamlandıktan sonra, test panelini açmak için panelin en üstündeki **Test** ' i seçin.

    Utterance 'in sürümlerini test etmek için test bölmesini kullanın:

    |Test bölmesine girilecek söylenme|
    |--|
    |`Who is Jill Jones manager this month`|
    |`Who is Jill Jones manager on July 5th`|
    |`Who was Jill Jones manager last month`|
    |`Who was Jill Jones manager on July 5th`|
    |`Who will be Jill Jones manager in a month`|
    |`Who will be Jill Jones manager on July 5th`|

Daha fazla desen söz dizimi kullanarak, uygulamanızda saklanması gereken şablon Puanlarınızın sayısını, hala yüksek bir tahmin puanı elde ederek azaltabilirsiniz.

### <a name="use-the-utterance-beginning-and-ending-anchors"></a>Söylenişi başlangıç ve bitiş bağlayıcılarını kullanma

Desen sözdizimi, bir giriş işaretinin başlangıç ve bitiş söylenişi bağlantı sözdizimini sağlar `^` . Başlangıç ve bitiş söylenişi bağlayıcıları, tek özel ve büyük olasılıkla sabit değerli bir şekilde hedeflemek veya hedef amaçlar için ayrı olarak kullanılmak üzere birlikte kullanılabilir.

## <a name="using-patternany-entity"></a>Model kullanma. herhangi bir varlık

[!INCLUDE [Pattern.any entity - concepts](./includes/pattern-any-entity.md)]

### <a name="add-example-utterances-with-patternany"></a>Desen ile örnek ekleme

1. Üst gezinti bölmesinden **Build** (Derle) öğesini, sol gezinti bölmesinden de **Intents** (Amaçlar) öğesini seçin.

1. Amaç listesinden **FindForm** öğesini seçin.

1. Örnek bir örnek ekleyin. Bir model olarak tahmin edilecek metin. herhangi biri **kalın metinte**. Form adı, çevresindeki diğer sözcüklerden daha kolay bir şekilde belirlenir. . Any, varlığın sınırları işaretlenerek yardımcı olur.

    |Örnek konuşma|Form adı|
    |--|--|
    |Where is the form **What to do when a fire breaks out in the Lab** and who needs to sign it after I read it? (Laboratuvarda yangın çıktığında yapılacak işlemler formu nerede ve ben okuduktan sonra kimin imzalaması gerekiyor?)|Bir yangın laboratuvarda kesilse ne yapmalı
    |Where is **Request relocation from employee new to the company** on the server? (Yeni şirket çalışanı taşınma talep formu sunucuda nerede?)|Şirketten yeni çalışana yeniden konumlandırma isteği|
    |Who authored "**Health and wellness requests on the main campus**" and what is the most current version? ("Ana kampüsteki sağlık ve sağlıklı yaşam istekleri"ni kim yazdı ve en güncel sürümü nedir?)|Ana kampüs üzerinde sistem durumu ve welllik istekleri|
    |I'm looking for the form named "**Office move request including physical assets**". ("Fiziksel eşyalar dahil olmak üzere ofis taşıma isteği" adlı formu arıyorum.) |Fiziksel varlıklar dahil Office taşıma isteği|

    Form adlarındaki değişik kullanımlar nedeniyle Pattern.any varlığı olmadan LUIS'in form başlığının sonunu belirlemesi zor olacaktır.

### <a name="create-a-patternany-entity"></a>Pattern.any varlığı oluşturma
Pattern.any varlığı farklı uzunluklardaki varlıkları ayıklar. Yalnızca bir düzende çalışarak, model varlığın başlangıcını ve sonunu sözdizimiyle işaret ettiğinden.

1. Sol gezinti panelinden **Entities** (Varlıklar) öğesini seçin.

1. **+ Oluştur**' u seçin, adı girin `FormName` ve **model. herhangi bir** tür olarak seçin. **Oluştur**’u seçin.

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

### <a name="using-an-explicit-list"></a>Açık liste kullanma

Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın.

## <a name="what-did-this-tutorial-accomplish"></a>Bu öğretici ne başardı?

Bu öğreticide, LUTO 'ın daha fazla örnek eklemek zorunda kalmadan önemli ölçüde daha yüksek bir puana sahip olma amacını tahmin etmeye yönelik desenler eklenmiştir. Varlıkları ve yok sayılabilir metinleri işaretlemek LUIS'in deseni daha fazla konuşmaya uygulamasını mümkün hale getirdi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [Desen ile rol kullanmayı öğrenin](luis-tutorial-pattern.md)
