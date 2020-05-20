---
title: Desenler doğruluk Ekle-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUSıS) uygulamalarında tahmin doğruluğunu artırmak için model şablonları ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: f1bad4a8f178358ba33d1a14815b8630b447615a
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680415"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak için desenler ekleme
Bir Lua uygulaması uç nokta dıklılığını aldıktan sonra, sözcük sırası ve Word seçimindeki bir düzeni açığa çıkarmaya yönelik tahmin doğruluğunu artırmak için bir [desen](luis-concept-patterns.md) kullanın. Desenler, konumunu belirtmek için özel [sözdizimini](luis-concept-patterns.md#pattern-syntax) kullanır: [varlıklar](luis-concept-entity-types.md), varlık [rolleri](luis-concept-roles.md)ve isteğe bağlı metin.

> [!CAUTION]
> Desenler, alt varlıklar değil yalnızca makine öğrenimi varlık üst öğelerini içerir.

## <a name="add-template-utterance-using-correct-syntax"></a>Doğru sözdizimini kullanarak şablon söylenişi ekleyin

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Sol panelde, **uygulama performansını iyileştirme**altında **desenler** ' i seçin.

1. Model için doğru amacı seçin.

1. Şablon metin kutusuna, şablonu yazın ve ENTER ' u seçin. Varlık adını girmek istediğinizde, doğru model varlığı sözdizimini kullanın. Varlık söz dizimini ile başlatın `{` . Varlıkların listesi görüntülenir. Doğru varlığı seçin.

    > [!div class="mx-imgBorder"]
    > ![Düzenin varlık ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir [rol](luis-concept-roles.md)içeriyorsa, rolü tek bir iki nokta üst üste ile, örneğin, `:` varlık adından sonra belirtin `{Location:Origin}` . Varlıkların rollerinin listesi bir listede görüntülenir. Rolü seçin ve ardından ENTER ' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Role sahip varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlığı seçtikten sonra, kalıbı girmeyi tamamladıktan sonra ENTER ' u seçin. Desenleri girmeyi bitirdiğinizde uygulamanızı [eğitme](luis-how-to-train.md) .

    > [!div class="mx-imgBorder"]
    > ![Her iki varlık türü ile girilen düzenin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="create-a-patternany-entity"></a>Bir model oluşturun. herhangi bir varlık

[Desen. tüm](luis-concept-entity-types.md) varlıklar yalnızca [desenlere](luis-how-to-model-intent-pattern.md)göre geçerlidir, amaç örnekleri değildir. Bu tür bir varlık, LUSıS 'in değişen uzunluk ve sözcük seçimine ait varlıkların sonunu bulmasına yardımcı olur. Bu varlık bir düzende kullanıldığı için, Lua, varlığın sonunun söylenişi şablonunda olduğunu bilir.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **+ Oluştur**' u seçin.

1. **Varlık türü seç** iletişim kutusunda, **ad** kutusuna varlık adını girin ve ardından model ' i seçin **.** tür olarak **yazın** ve **Oluştur**' u seçin.

    Bu varlığı kullanarak [bir desen oluşturduktan](luis-how-to-model-intent-pattern.md) sonra, varlık Birleşik makine öğrenimi ve metin eşleştirme algoritması ile ayıklanır.

## <a name="adding-example-utterances-as-pattern"></a>Desen olarak örnek ekleme

Bir varlık için bir model eklemek istiyorsanız, _en kolay_ yol, amaç ayrıntıları sayfasından deseninin oluşturulması olur. Bu, sözdiziminizin örnek uttaslı ile eşleşmesini sağlar.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Amaç** listesi sayfasında, bir şablon oluşturmak istediğiniz örnek olarak kullanılacak amaç adını seçin.
1. Amaç ayrıntıları sayfasında, şablon olarak kullanmak istediğiniz örnek için satırı seçin ve bağlam araç çubuğundan **+ as desen ekle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Amaç ayrıntıları sayfasında şablon stili olarak örnek oluşturma ekranının ekran görüntüsü.](./media/luis-how-to-model-intent-pattern/add-example-utterances-as-pattern-template-utterance-from-intent-detail-page.png)

    Söylenişi 'ten bir desen oluşturmak için bir varlık içermelidir.

1. Açılır kutuda, **düzenleri Onayla** sayfasında **bitti** ' yi seçin. Varlıkların alt varlıklarını veya özelliklerini tanımlamanız gerekmez. Yalnızca makine öğrenme varlığını listeetmeniz gerekir.

    > [!div class="mx-imgBorder"]
    > ![Amaç ayrıntıları sayfasında şablon stili olarak onaylama örnek ekran görüntüsü.](./media/luis-how-to-model-intent-pattern/confirm-patterns-from-example-utterance-intent-detail-page.png)

1. Şablonu düzenlemeniz gerekiyorsa (örneğin, metni isteğe bağlı olarak seçme gibi `[]` ), (köşeli ayraç), bu düzenleme **desenler** sayfasından yapmanız gerekir.

1. Gezinti çubuğunda, uygulamayı yeni bir Düzenle eğiteiçin **eğitme** ' yi seçin.

## <a name="train-your-app-after-changing-model-with-patterns"></a>Modeli desenlerle değiştirdikten sonra uygulamanızı eğitme
Bir düzen ekledikten, düzenledikten, kaldırdıktan veya yeniden atadıktan sonra, uç nokta sorgularını etkilemek için uygulamanızı yaptığınız değişiklikler için [eğitin](luis-how-to-train.md) ve [yayımlayın](luis-how-to-publish-app.md) .

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullan

Desenler listesinin üzerindeki bağlamsal araç çubuğu şunları yapmanıza olanak sağlar:

* Desenler ara
* Bir düzen düzenleme
* Tek bir kalıbı farklı amaca yeniden atama
* Çeşitli desenleri farklı amaca yeniden atama
* Tek-stili Sil
* Birkaç deseni Sil
* Varlık listesini varlığa göre filtrele
* Filtre-ölçütü-ölçütü-Listele
* Varlık veya amaç filtresini kaldır
* Amaç veya varlık sayfasında var olan dıklardan desen ekleme

## <a name="next-steps"></a>Sonraki adımlar

* Bir modelle bir model [oluşturma](luis-tutorial-pattern.md) hakkında bilgi edinin.
* Uygulamanızı [eğitme](luis-how-to-train.md) hakkında bilgi edinin.
