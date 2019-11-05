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
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: cbd8ad73ff4a03790dd6b22d5ce33acf09a2b125
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491354"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak için desenler ekleme
Bir Lua uygulaması uç nokta dıklılığını aldıktan sonra, sözcük sırası ve Word seçimindeki bir düzeni açığa çıkarmaya yönelik tahmin doğruluğunu artırmak için bir [desen](luis-concept-patterns.md) kullanın. Desenler, konumunu belirtmek için özel [sözdizimini](luis-concept-patterns.md#pattern-syntax) kullanır: [varlıklar](luis-concept-entity-types.md), varlık [rolleri](luis-concept-roles.md)ve isteğe bağlı metin.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-template-utterance-to-create-pattern"></a>Desen oluşturmak için Şablon Ekle
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın ve ardından sol paneldeki **uygulama performansını iyileştirme**altında **desenler** ' i seçin.

    ![Desenler listesinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Model için doğru amacı seçin. 

    ![Amaç seçin](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Şablon metin kutusuna, şablonu yazın ve ENTER ' u seçin. Varlık adını girmek istediğinizde, doğru model varlığı sözdizimini kullanın. `{`varlık sözdizimini başlatın. Varlıkların listesi görüntülenir. Doğru varlığı seçin ve ardından ENTER ' ı seçin. 

    ![Düzenin varlık ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir [rol](luis-concept-roles.md)içeriyorsa, `{Location:Origin}`gibi varlık adından sonra `:`tek bir iki nokta üst üste sahip rolü belirtin. Varlıkların rollerinin listesi bir listede görüntülenir. Rolü seçin ve ardından ENTER ' u seçin. 

    ![Role sahip varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlığı seçtikten sonra, kalıbı girmeyi tamamladıktan sonra ENTER ' u seçin. Desenleri girmeyi bitirdiğinizde uygulamanızı [eğitme](luis-how-to-train.md) .

    ![Her iki varlık türü ile girilen düzenin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

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
