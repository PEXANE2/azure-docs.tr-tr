---
title: Örnek utterer ekleme-lu
titleSuffix: Azure Cognitive Services
description: Örnek söyleyler, Kullanıcı sorularının veya komutlarının metin örnekleridir. Language Understanding (LUSıS) öğretmek için bir amaca yönelik örnek ekleme yapmanız gerekir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: ed6321963422cf17c858f43593092f8ffa4a1119
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467737"
---
# <a name="add-an-entity-to-example-utterances"></a>Örneğe bir varlık ekleyin 

Örnek söyleyler, Kullanıcı sorularının veya komutlarının metin örnekleridir. Language Understanding (LUSıS) öğretmek [için bir amaca](luis-concept-intent.md)yönelik [örnek](luis-concept-utterance.md) ekleme yapmanız gerekir.


Genellikle, öncelikle amacına bir örnek ekleyin ve ardından **amaçlar** sayfasında varlıklar oluşturup Etiketler oluşturun. Önce varlık oluşturmayı tercih ediyorsanız, bkz. [varlık ekleme](luis-how-to-add-entities.md).

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="marking-entities-in-example-utterances"></a>Varlıkları örnek olarak işaretleme

Bir varlık için işaretlemek üzere örnekteki metinde metin seçtiğinizde bir yerinde açılır menü görünür. Bir varlık oluşturmak ya da seçmek için bu menüyü kullanın. 

Önceden oluşturulmuş varlıklar ve normal ifade varlıkları gibi belirli varlık türleri, otomatik olarak etiketlendiklerinden, örnek utde etiketlenemez. 

## <a name="add-a-simple-entity"></a>Basit bir varlık ekleyin

Aşağıdaki yordamda, **amaçlar** sayfasında aşağıdaki şekilde bir özel varlık oluşturur ve etiketlerseniz:

```text
Are there any SQL server jobs?
```

1. Bir basit varlık olarak etiketlemek için söylenişi 'teki `SQL server` seçin. Görüntülenen varlık açılan kutusunda, var olan bir varlığı seçebilir veya yeni bir varlık ekleyebilirsiniz. Yeni bir varlık eklemek için, adını metin kutusuna `Job` yazın ve ardından **yeni varlık oluştur**' u seçin.

    ![Varlık adı girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/create-simple-entity.png)

    > [!NOTE]
    > Varlıklar olarak etiketlemek için sözcükler seçerken:
    > * Tek bir sözcük için yalnızca seçmeniz yeterlidir. 
    > * İki veya daha fazla sözcük kümesi için ilk sözcüğü ve sonra son sözcüğü seçin.

1. **Ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunda, varlık adını doğrulayın ve **basit** varlık türünü seçin ve **bitti**' yi seçin.

    Bir [ifade listesi](luis-concept-feature.md) genellikle basit bir varlığın sinyalini artırmak için kullanılır.

## <a name="add-a-list-entity"></a>Liste varlığı ekleme

Liste varlıkları, sisteminizdeki ilgili sözcüklerle bir tam metin eşleşmesi kümesini temsil eder. 

Bir şirketin Bölüm listesi için normalleştirilmiş değerlere sahip olabilirsiniz: `Accounting` ve `Human Resources`. Her normalleştirilmiş adın eşanlamlıları vardır. Bir departman için, bu eş anlamlılar herhangi bir departman kısalt, sayı veya Slang içerebilir. Varlığı oluştururken tüm değerleri bilmeniz gerekmez. Eş anlamlıları olan gerçek Kullanıcı araslarını inceledikten sonra daha fazla ekleyebilirsiniz.

1. **Amaçlar** sayfasında bir örnek olarak, yeni listede istediğiniz sözcük veya tümceciği seçin. Varlık açılan kutusu göründüğünde, üstteki metin kutusuna yeni liste varlığının adını girin ve ardından **yeni varlık oluştur**' u seçin.   

1. **Ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunda, varlığı yazın ve **liste** olarak seçin. Bu liste öğesinin eşanlamlılarını ekleyin ve **bitti**' yi seçin. 

    ![Liste varlığı eşanlamlılarını girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    Diğer söyleyleri etiketleyerek veya sol Gezinti alanındaki **varlıklardan** varlığı düzenleyerek daha fazla liste öğesi veya daha fazla öğe eşanlamlı ekleyebilirsiniz. Varlıkları [düzenlemek](luis-how-to-add-entities.md#add-list-entities) , ilgili eşanlamlı eşanlamlıya veya bir listeyi içeri aktarmaya ek öğe girme seçeneklerini sağlar. 

## <a name="add-a-composite-entity"></a>Bileşik varlık ekleme

Bileşik varlıklar, bir üst varlık oluşturmak için mevcut **varlıklardan** oluşturulur. 

`Does John Smith work in Seattle?`, bir bileşik söylenişi 'in, çalışan adı `John Smith`varlık bilgilerini ve bir bileşik varlıktaki konum `Seattle` döndürebilirler. Alt varlıkların uygulamada zaten mevcut olması ve Bileşik varlık oluşturulmadan önce örnekte işaretlenmiş olması gerekir.

1. Alt varlıkları bileşik bir varlığa kaydırmak için, bileşik varlık için utterde **ilk** etiketlenmiş varlığı (en soldaki) seçin. Bu seçimin seçeneklerini göstermek için bir açılır liste görüntülenir.

1. Açılan listeden **bileşik varlıkta Kaydır '** ı seçin. 

1. Bileşik varlığın son sözcüğünü seçin (sağ çok). Yeşil bir çizgi bileşik varlığı izler. Bu, bileşik bir varlık için görsel göstergedir ve Birleşik varlıktaki en sol alt varlıktaki tüm sözcüklerin altında yer almalıdır ve en alt varlığa doğru olmalıdır.

1. Açılan listede Bileşik varlık adını girin.

    Varlıkları doğru sardığınızda, bir yeşil çizgi, tüm ifadenin altındadır.

1. Birleşik varlık ayrıntılarını **oluşturmak istediğiniz varlık türü** üzerinde doğrulayın? açılır kutu ardından **bitti**' yi seçin.

    ![Varlık Ayrıntıları açılır penceresi ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. Bileşik varlık tek tek varlıklar için hem mavi vurgularla hem de tüm Bileşik varlık için yeşil bir alt çizgiyle görüntülenir. 

    ![Karma varlık ile amaç Ayrıntıları sayfasının ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-entitys-role-to-utterance"></a>Varlığın rolünü söylenişi 'e Ekle

Rol, bir varlığın, utterance bağlamı tarafından belirlenen bir adlandırılmış alt türüdür. Varlık olarak bir varlığı bir varlık olarak işaretleyebilir veya bir varlık içinde bir rol seçebilirsiniz. Herhangi bir varlıkta makine öğrenmiş (basit varlıklar ve bileşik varlıklar) özel varlıklar dahil olmak üzere, makine tarafından öğrenilen (önceden oluşturulmuş varlıklar, normal ifade varlıkları, liste varlıkları) gibi roller bulunabilir. 

Uygulamalı öğreticiden [varlık rolleriyle bir şekilde nasıl işaretleyeceğinizi](tutorial-entity-roles.md) öğrenin. 

## <a name="entity-status-predictions"></a>Varlık durumu tahminleri

Luu portalında yeni bir utteranance girdiğinizde, söylenişi varlık tahmin hatalarına sahip olabilir. Tahmin hatası, bir varlığın, halın varlığı nasıl tahmin etti ile karşılaştırıldığında etiketlendiği bir farktır. 

Bu fark, LUO portalında, utterance 'teki kırmızı bir alt çizgiyle görsel olarak temsil edilir. Kırmızı alt çizgi, varlık ayraçları veya köşeli ayraç dışında görünebilir. 

![Varlık durumu tahmin farkının ekran görüntüsü](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Deterance 'de kırmızı renkte altı çizili olan sözcükleri seçin. 

Bir tahmin tutarsızlığı varsa varlık kutusu, **varlık durumunu** kırmızı bir ünlem işaretiyle görüntüler. Etiketli ve tahmin edilen varlıklar arasındaki fark hakkında bilgi içeren varlık durumunu görmek için **varlık durumu** ' nu seçin, ardından sağdaki öğeyi seçin.

![Varlık durumu seçiminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/entity-prediction-error-correction.png)

Kırmızı çizgi aşağıdaki zamanların herhangi birinde görünebilir:

   * Bir söylenişi girildiğinde, ancak varlık etiketlenmadan önce
   * Varlık etiketi uygulandığında
   * Varlık etiketi kaldırıldığında
   * O metin için birden fazla varlık etiketi tahmin edildiğinde 

Aşağıdaki çözümler, varlık tahmin farkının çözümlenmesine yardımcı olur:

|Varlık|Görsel gösterge|Hızlı|Çözüm|
|--|--|--|--|
|Utterance girildi, varlık henüz etiketlendi.|kırmızı altı çizili|Tahmin doğru.|Varlığı tahmin edilen değerle etiketleyin.|
|Etiketsiz metin|kırmızı altı çizili|Yanlış tahmin|Bu yanlış varlığı kullanan geçerli söyleyenlerde tüm amaçlar genelinde gözden geçirilmesi gerekir. Geçerli söyleyde, bu metnin öngörülen varlık olduğu için bu metin,
|Doğru etiketli metin|mavi varlık vurgusu, kırmızı alt çizgi|Yanlış tahmin|Çeşitli yerlerde ve kullanımlardaki doğru etiketli varlıkla daha fazla bilgi sağlayın. Bu varlık, bu varlık veya benzer varlıkların aynı bağlamda göründüğünü öğretmeyi öğretmek için yeterli değil. Benzer varlık tek bir varlıkta birleştirilmelidir, bu yüzden LUIN karıştırılmamalıdır. Diğer bir çözüm ise sözcüklerin önemini artırmak için bir tümcecik listesi eklemektir. |
|Yanlış etiketlenmiş metin|mavi varlık vurgusu, kırmızı alt çizgi|Doğru tahmin| Çeşitli yerlerde ve kullanımlardaki doğru etiketli varlıkla daha fazla bilgi sağlayın. 

## <a name="other-actions"></a>Diğer eylemler

Seçili bir grup veya tek bir öğe olarak örnek oluşturma işlemleri gerçekleştirebilirsiniz. Seçili örnek grupları, listenin üzerindeki bağlamsal menüyü değiştirir. Tek öğeler, hem listenin üzerinde bağlam menüsünü hem de her bir utterlik satırının sonundaki bireysel bağlamsal üç noktayı kullanabilir. 

### <a name="remove-entity-labels-from-utterances"></a>Varlık etiketlerini, utterlerden kaldır

Makine tarafından öğrenilen varlık etiketlerini amaçlar sayfasında bir noktadan kaldırabilirsiniz. Varlık makine tarafından öğrenilmiyorsa, bir utterden kaldırılamaz. Makine tarafından öğrenilen bir varlığı, utterance 'ten kaldırmanız gerekiyorsa, varlığı uygulamanın tamamında silmeniz gerekir. 

Bir noktadan itibaren makine tarafından öğrenilen bir varlık etiketini kaldırmak için, söz konusu varlığı de seçin. Ardından görüntülenen varlık açılan kutusunda **etiketi kaldır** ' ı seçin.

### <a name="add-a-prebuilt-entity-label"></a>Önceden oluşturulmuş varlık etiketi ekleme

Önderlenmiş varlıkları LUSıS uygulamanıza eklediğinizde, bu varlıklarla birlikte, bu varlıklarla ilgili olarak etiket eklemeniz gerekmez. Önceden oluşturulmuş varlıklar ve bunların nasıl ekleneceği hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

### <a name="add-a-regular-expression-entity-label"></a>Normal ifade varlık etiketi ekle

LUSıS uygulamanıza normal ifade varlıkları eklerseniz, bu varlıklarla birlikte gelen ifadeleri etiketlemeye gerek kalmaz. Normal ifade varlıkları ve bunların nasıl ekleneceği hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md#add-regular-expression-entities-for-highly-structured-concepts).


### <a name="create-a-pattern-from-an-utterance"></a>Söylenişi 'ten bir desen oluşturma

Bkz. [amaç veya varlık sayfasındaki mevcut dıklardan desen ekleme](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).


### <a name="add-a-patternany-entity"></a>Bir model ekleyin. herhangi bir varlık

Desen eklerseniz, LUSıS uygulamanıza tüm varlıklar bu varlıklarla ilgili olarak etiket oluşturamazsınız. Bunlar yalnızca desenlerinde geçerlidir. Desenler hakkında daha fazla bilgi edinmek için bkz. [varlıklar ekleme](luis-how-to-add-entities.md#add-patternany-entities-to-capture-free-form-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Modellerle modeli değiştirdikten sonra uygulamanızı eğitme

Atlamalarını ekledikten, düzenledikten veya kaldırdıktan sonra, uç nokta sorgularını etkilemek için uygulamanızı yaptığınız değişiklikler için [eğitin](luis-how-to-train.md) ve [yayımlayın](luis-how-to-publish-app.md) . 

## <a name="next-steps"></a>Sonraki adımlar

Amaçlarınızı etiketleyerek etiketleyerek artık [bileşik bir varlık](luis-how-to-add-entities.md)oluşturabilirsiniz.
