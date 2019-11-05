---
title: Varlık Ekle-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 54c9d79c62052daeee76de5dffb1099dc7d75180
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467724"
---
# <a name="create-entities-without-utterances"></a>Söyleymeksizin varlık oluşturma

Varlık, ayıklanarak ayıklanmasını istediğiniz bir sözcük veya tümceciği temsil eder. Bir varlık, benzer nesneler (yer, şeyler, kişiler, olaylar veya kavramlar) koleksiyonu dahil bir sınıfı temsil eder. Varlıklar, amaca uygun bilgileri betimler ve bazen uygulamanızın görevini gerçekleştirmesi için önemlidir. Bir amaca veya bir amaca göre (önce veya sonra) bir amaca göre ekleme yaptığınızda varlıklar oluşturabilirsiniz.

Bir varlık sayfasında varlıklar **listesi** aracılığıyla lusıs **uygulamanızda varlık ekleyebilir** , düzenleyebilir veya silebilirsiniz. LUSıS iki ana varlık türü sunar: [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md)ve kendi [özel varlıklarınız](luis-concept-entity-types.md#types-of-entities).

Makine tarafından öğrenilen bir varlık oluşturulduktan sonra, söz konusu varlığı içindeki tüm amaçların her örneğinde işaretlemeniz gerekir.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

<a name="add-prebuilt-entity"></a>

## <a name="add-a-prebuilt-entity-to-your-app"></a>Uygulamanıza önceden oluşturulmuş bir varlık ekleyin

Bir uygulamaya eklenen ortak önceden oluşturulmuş varlıklar *Number* ve *datetimeV2*. 

1. Uygulamanızda, **derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin.
 
1. **Varlıklar** sayfasında, **önceden oluşturulmuş varlıklar Ekle**' yi seçin.

1. **Önceden oluşturulmuş varlıklar Ekle** iletişim kutusunda, **Number** ve **datetimeV2** önceden oluşturulmuş varlıkları seçin. Ardından **Bitti**'yi seçin.

    ![Önceden oluşturulmuş varlık Ekle iletişim kutusunun ekran görüntüsü](./media/add-entities/list-of-prebuilt-entities.png)

<a name="add-simple-entities"></a>

## <a name="add-simple-entities-for-single-concepts"></a>Tek kavramlar için basit varlıklar ekleyin

Basit bir varlık, tek bir kavram açıklar. *İnsan kaynakları* veya *işlemler*gibi şirket departmanı adlarını çıkaran bir varlık oluşturmak için aşağıdaki yordamı kullanın.   

1. Uygulamanızda, **derleme** bölümünü seçin, sonra sol panelde **varlıklar** ' ı seçin ve ardından **yeni varlık oluştur**' u seçin.

1. Açılır iletişim kutusunda **varlık adı** kutusuna `Location` yazın, **varlık türü** listesinden **basit** ' i seçin ve **bitti**' yi seçin.

    Bu varlık oluşturulduktan sonra, varlığı içeren örnek, tüm amaçlar bölümüne gidin. Örnekteki metni seçin ve metni varlık olarak işaretleyin. 

    Bir [ifade listesi](luis-concept-feature.md) genellikle basit bir varlığın sinyalini artırmak için kullanılır.

<a name="add-regular-expression-entities"></a>

## <a name="add-regular-expression-entities-for-highly-structured-concepts"></a>Yüksek düzeyde yapılandırılmış kavramlar için normal ifade varlıkları ekleyin

Normal ifade varlığı, sağladığınız bir normal ifadeye göre, verileri almak için kullanılır. 

1. Uygulamanızda, sol gezinti bölmesinde bulunan **varlıklar** ' ı seçin ve ardından **yeni varlık oluştur**' u seçin.

1. Açılır iletişim kutusunda **varlık adı** kutusuna `Human resources form name` girin, **varlık türü** listesinden **normal ifade** ' i seçin, `hrf-[0-9]{6}`normal ifade girin ve **bitti**' yi seçin. 

    Bu normal ifade, bir Insan kaynakları formu için bir form numarasını göstermek üzere `hrf-`, sonra 6 basamaklı harflerden oluşmalıdır.

<a name="add-composite-entities"></a>

## <a name="add-composite-entities-to-group-into-a-parent-child-relationship"></a>Gruba bir üst-alt ilişkisine bileşik varlıklar ekleme

Bileşik bir varlık oluşturarak, farklı türlerin varlıkları arasında ilişkiler tanımlayabilirsiniz. Aşağıdaki örnekte, varlık bir normal ifade ve önceden oluşturulmuş bir ad varlığı içerir.  

Söylenişi `Send hrf-123456 to John Smith`, metin `hrf-123456` bir insan kaynakları [normal ifadesiyle](#add-regular-expression-entities) eşleştirilir ve `John Smith` önceden oluşturulmuş varlık adı ile ayıklanır. Her varlık daha büyük, üst varlığın bir parçasıdır. 

1. Uygulamanızda, **derleme** bölümünün sol gezinti bölmesindeki **varlıklar** ' ı seçin ve ardından **önceden oluşturulmuş varlık Ekle**' yi seçin.

1. Önceden oluşturulmuş varlık **kişilik adını**ekleyin. Yönergeler için bkz. [önceden oluşturulmuş varlıklar ekleme](#add-prebuilt-entity). 

1. Sol gezinti bölmesinde **varlıklar** ' ı seçin ve ardından **yeni varlık oluştur**' u seçin.

1. Açılır iletişim kutusunda **varlık adı** kutusuna `SendHrForm` girin ve ardından **varlık türü** listesinden **bileşik** ' i seçin.

1. Yeni bir alt öğe eklemek için **alt öğe Ekle** ' yi seçin.

1. **Alt #1**, listeden varlık **numarasını** seçin.

1. **Alt #2**, listeden varlık **insan kaynakları form adı** ' nı seçin. 

1. **Done** (Bitti) öğesini seçin.

<a name="add-pattern-any-entities"></a>

## <a name="add-patternany-entities-to-capture-free-form-entities"></a>Ücretsiz form varlıklarını yakalamak için herhangi bir varlık ekleyin.

[. Tüm](luis-concept-entity-types.md) varlıklar yalnızca [desenlerinde](luis-how-to-model-intent-pattern.md)geçerlidir, amaçlar için geçerli değildir. Bu tür bir varlık, LUSıS 'in değişen uzunluk ve sözcük seçimine ait varlıkların sonunu bulmasına yardımcı olur. Bu varlık bir düzende kullanıldığı için, Lua, varlığın sonunun söylenişi şablonunda olduğunu bilir.

Bir uygulamanın `FindHumanResourcesForm` amacı varsa, ayıklanan form başlığı amaç tahminini etkileyebilir. Hangi sözcüklerin form başlığında olduğunu netleştirmek için, bir düzen içindeki bir düzen kullanın. LUSıS tahmini, utterance ile başlar. İlk olarak, söylenişi varlıklar için denetlenir ve eşleştirilir, varlıklar bulunduğunda bu desen denetlenir ve eşleştirilir. 

Söylenişi `Where is Request relocation from employee new to the company on the server?`, başlığın bittiği ve en geri kalanının başladığı yerde bağlamsal olarak açık olmadığından, form başlığı karmaşık hale gelir. Başlıklar, tek bir sözcük, noktalama içeren karmaşık deyimler ve sözcüklerin sabit listesi olmayan sözcükler dahil olmak üzere herhangi bir sözcük sırası olabilir. Bir model, tam ve tam varlığın ayıklanabileceği bir varlık oluşturmanıza olanak sağlar. Başlık bulduktan sonra, bu düzenin amacı olduğundan `FindHumanResourcesForm` amacı tahmin edilir.

1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **yeni varlık oluştur**' u seçin.

1. **Varlık Ekle** Iletişim kutusunda **varlık adı** kutusuna `HumanResourcesFormTitle` girin ve **model. herhangi bir** **varlık türü**olarak öğesini seçin.

    Deseni kullanmak için, **desen sayfasında,** **uygulama performansını iyileştirme** bölümünde, `Where is **{HumanResourcesFormTitle}** on the server?`gibi doğru küme ayracı sözdizimi ile bir desen ekleyin.

    Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](luis-concept-patterns.md#explicit-lists) kullanın. 

<a name="add-a-role-to-pattern-based-entity"></a>

## <a name="add-a-role-to-distinguish-different-contexts"></a>Farklı bağlamları ayırt etmek için rol ekleme

Rol, bağlamı temel alan adlandırılmış bir alt türdür. Önceden oluşturulmuş ve makine tarafından öğrenilen varlıklar dahil tüm varlıklarda kullanılabilir. 

Rol için sözdizimi, varlık adının ardından iki nokta üst üste, ardından rol adından **`{Entityname:Rolename}`** . Örneğin, `Move {personName} from {Location:Origin} to {Location:Destination}`.

1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin.

1. **Create new entity** (Yeni varlık oluştur) öğesini seçin. `Location`adını girin. **Basit** türü seçin ve **bitti**' yi seçin. 

1. Sol paneldeki **varlıklar** ' ı seçin ve ardından önceki adımda oluşturulan yeni varlık **konumunu** seçin.

1. **Rol adı** metin kutusuna, `Origin` rolün adını girin ve girin. İkinci bir rol adı `Destination`ekleyin. 

    ![Konum varlığına kaynak rolü ekleme ekran görüntüsü](./media/add-entities/roles-enter-role-name-text.png)

<a name="add-list-entities"></a>

## <a name="add-list-entities-for-exact-matches"></a>Tam eşleşmeler için liste varlıkları ekleme

Liste varlıkları sabit ve kapalı bir ilgili sözcük kümesini temsil eder. 

Insan kaynakları uygulaması için, departmanların tüm eş anlamlılarıyla birlikte tüm bölümlerin listesine sahip olabilirsiniz. Varlığı oluştururken tüm değerleri bilmeniz gerekmez. Eş anlamlıları olan gerçek Kullanıcı araslarını inceledikten sonra daha fazla ekleyebilirsiniz.

1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **yeni varlık oluştur**' u seçin.

1. **Varlık Ekle** Iletişim kutusunda **varlık adı** kutusuna `Department` yazın ve **varlık türü**olarak **Listele** ' yi seçin. **Done** (Bitti) öğesini seçin.
  
1. Varlık listesi sayfası, normalleştirilmiş adlar eklemenize olanak tanır. **Değerler** metin kutusunda, liste için bir departman adı girin, örneğin `HumanResources`. ardından klavyede ENTER tuşuna basın. 

1. Normalleştirilmiş değerin sağında, her öğeden sonra klavyede ENTER tuşuna basarak eşanlamlı yazın.

1. Liste için daha fazla normalleştirilmiş öğe isterseniz, [anlam sözlüğündeki](luis-glossary.md#semantic-dictionary)seçenekleri görmek için **öner** ' i seçin.

    ![Seçenekleri görmek için önerilen özelliği seçme ekran görüntüsü](./media/add-entities/hr-list-2.png)


1. Normalleştirilmiş bir değer olarak eklemek için önerilen listeden bir öğe seçin veya tüm öğeleri eklemek için **Tümünü Ekle** ' yi seçin. 
    Aşağıdaki JSON biçimini kullanarak, mevcut bir liste varlığına değerleri içeri aktarabilirsiniz:

    ```JSON
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

<a name="change-entity-type"></a>

## <a name="do-not-change-entity-type"></a>Varlık türünü değiştirme

LUSıS, varlığı oluşturmak için nelerin ekleneceğini veya kaldırılacağını bilmediğinden, varlığın türünü değiştirmenize izin vermez. Türü değiştirmek için, biraz farklı bir ada sahip doğru türde yeni bir varlık oluşturmak daha iyidir. Varlık oluşturulduktan sonra, her seferde, etiketli eski varlık adını kaldırın ve yeni varlık adını ekleyin. Tüm dıklılık bir kez daha alındıktan sonra eski varlığı silin. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-pattern-from-an-example-utterance"></a>Örnekle bir desen oluşturun

Bkz. [amaç veya varlık sayfasındaki mevcut dıklardan desen ekleme](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Modelleme varlıkları ile değiştirdikten sonra uygulamanızı eğitme

Varlıkları ekledikten, düzenledikten veya kaldırdıktan sonra, uç nokta sorgularını etkilemek için uygulamanızı yaptığınız değişiklikler için [eğitin](luis-how-to-train.md) ve [yayımlayın](luis-how-to-publish-app.md) . 

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş varlıklar hakkında daha fazla bilgi için bkz. [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text) projesi. 

Varlığın JSON Endpoint sorgu yanıtında nasıl göründüğü hakkında bilgi için bkz. [veri ayıklama](luis-concept-data-extraction.md)

Artık hedefleri, utterlerini ve varlıkları eklediğine göre, temel bir LUSıS uygulamanız vardır. Uygulamanızı [eğitme](luis-how-to-train.md), [Test](luis-interactive-test.md)etme ve [Yayımlama](luis-how-to-publish-app.md) hakkında bilgi edinin.
 
