---
title: Desenler doğruluk katıyor - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) uygulamalarında tahmin doğruluğunu artırmak için desen şablonları ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 21afb12bf2464218119ebf52ebd980745e3d731d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76311725"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak için desen ekleme
Bir LUIS uygulaması uç nokta sözcük lerini aldıktan sonra, sözcük sırası ve sözcük seçiminde bir desen ortaya çıkan sözcük lerin tahmin doğruluğunu artırmak için bir [desen](luis-concept-patterns.md) kullanın. Desenler, varlıkların, [varlık](luis-concept-entity-types.md) [rollerinin](luis-concept-roles.md)ve isteğe bağlı metnin konumunu belirtmek için belirli [sözdizimini](luis-concept-patterns.md#pattern-syntax) kullanır.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

> [!CAUTION]
> Desenler yalnızca makinede öğrenilen varlık ebeveynlerini içerir, alt bileşenleri değil.

## <a name="adding-example-utterances-as-pattern"></a>Örnek söyleyiyi desen olarak ekleme

Bir varlık için desen eklemek istiyorsanız, _en kolay_ yol, Niyet ayrıntıları sayfasından deseni oluşturmaktır. Bu, sözdiziminizin örnek söyleyişle eşleşmesini sağlar.

1. Önizleme [LUIS portalında,](https://preview.luis.ai) **Uygulamayı Uygulamalarım** sayfasından seçin.
1. **Niyetler** listesi sayfasında, şablon sözcük oluşturmak istediğiniz örnek söyleşinin niyet adını seçin.
1. Niyet ayrıntıları sayfasında, şablon sözcük olarak kullanmak istediğiniz örnek söyleyiş için satırı seçin ve ardından bağlam araç çubuğundan **desen olarak ekle'yi** seçin.

    > [!div class="mx-imgBorder"]
    > ![Amaç ayrıntıları sayfasında şablon deseni olarak örnek sözcük seçimiekran görüntüsü.](./media/luis-how-to-model-intent-pattern/add-example-utterances-as-pattern-template-utterance-from-intent-detail-page.png)

1. Açılan kutuda, **Desenleri Onayla** sayfasında **Bitti'yi** seçin. Varlıkların alt bileşenlerini, kısıtlamalarını veya tanımlayıcılarını tanımlamanız gerekmez. Yalnızca makinede öğrenilen varlığı listele etmeniz gerekir.

    > [!div class="mx-imgBorder"]
    > ![Amaç ayrıntıları sayfasında şablon deseni olarak örnek söyleyiyi doğrulayan ekran görüntüsü.](./media/luis-how-to-model-intent-pattern/confirm-patterns-from-example-utterance-intent-detail-page.png)

1. `[]` (Kare) köşeli ayraçlarla metni isteğe bağlı olarak seçmek gibi şablonu düzenlemeniz gerekiyorsa, bu ediyi **Desenler** sayfasından yapmanız gerekir.

1. Navigasyon çubuğunda, uygulamayı yeni desenle eğitmek için **Train'i** seçin.

## <a name="add-template-utterance-using-correct-syntax"></a>Doğru sözdizimini kullanarak şablon oluşturma oluşturma

1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın ve **uygulama performansını artırın**altında sol paneldeki **Desenler'i** seçin.

    > [!div class="mx-imgBorder"]
    > ![Desenler Listesiekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

1. Desen için doğru niyeti seçin.

1. Şablon metin kutusunda, şablon sözcüğünü yazın ve Enter'u seçin. Varlık adını girmek istediğinizde, doğru desen varlık sözdizimini kullanın. Varlık sözdizimini `{`' ile başlatın. Varlıkların listesi görüntüler. Doğru varlığı seçin.

    > [!div class="mx-imgBorder"]
    > ![Desen için varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir [rol](luis-concept-roles.md)içeriyorsa, tek bir `:`iki nokta üst üste `{Location:Origin}`rolü gösterir, varlık adından sonra, . Varlıklar için roller listesi bir listede görüntülenir. Rolü seçin ve sonra Enter'u seçin.

    > [!div class="mx-imgBorder"]
    > ![Rolü olan varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlığı seçtikten sonra, deseni girmeyi bitirin ve sonra Enter'u seçin. Desenleri girmeyi bitirdiğinizde, uygulamanızı [eğitin.](luis-how-to-train.md)

    > [!div class="mx-imgBorder"]
    > ![Her iki varlık türüyle girilen desenin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Modelinizi desenlerle değiştirdikten sonra uygulamanızı eğitin
Bir desen ekledikten, düzenledikten, kaldırdıktan veya yeniden atadıktan sonra, bitiş noktası sorgularını etkilemek için uygulamanızı [eğitin](luis-how-to-train.md) ve [yayımlayın.](luis-how-to-publish-app.md)

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

## <a name="use-contextual-toolbar"></a>Bağlamsal araç çubuğu kullanma

Desenler listesinin üzerindeki bağlamsal araç çubuğu şunları yapmanızı sağlar:

* Desenleri ara
* Bir deseni düzenle
* Bireysel deseni farklı amaclara yeniden atama
* Farklı amaciçin çeşitli desenleri yeniden atama
* Sil-a-tek desen
* Çeşitli desenleri silme
* Varlığa göre filtre desen listesi
* Filtre-desen-liste-niyete göre
* Varlık veya niyet filtresini kaldırma
* Niyet veya varlık sayfasında varolan söyleyişden desen ekleme

## <a name="next-steps"></a>Sonraki adımlar

* Bir desen le nasıl [örüntü oluşturup](luis-tutorial-pattern.md) öğreticiyle herhangi bir desen ve rol yapmayı öğrenin.
* Uygulamanızı nasıl [eğittini](luis-how-to-train.md) öğrenin.
