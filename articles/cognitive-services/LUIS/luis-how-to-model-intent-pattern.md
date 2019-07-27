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
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: f0e801afde240d8b2d6e0798e6894b2e83f9c5cb
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560440"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak için desenler ekleme
Bir Lua uygulaması uç nokta dıklılığını aldıktan sonra, sözcük sırası ve Word seçimindeki bir düzeni açığa çıkarmaya yönelik tahmin doğruluğunu artırmak için bir [desen](luis-concept-patterns.md) kullanın. Desenler, konumunu belirtmek için özel [sözdizimini](luis-concept-patterns.md#pattern-syntax) kullanır: [varlıklar](luis-concept-entity-types.md), varlık [rolleri](luis-concept-roles.md)ve isteğe bağlı metin.

## <a name="add-template-utterance-to-create-pattern"></a>Desen oluşturmak için şablon utterance Ekle
1. Adını seçerek uygulamanızı açın **uygulamalarım** sayfasında ve ardından **desenleri** sol bölmede altında **uygulama performansını**.

    ![Desen listesinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Desen için doğru hedefini seçin. 

    ![Hedefi seçin](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Şablon metin şablonu utterance yazın ve Enter'ı seçin. Varlık adı girmek istediğiniz zaman doğru deseni varlık sözdizimini kullanın. Varlık sözdizimi ile başlayan `{`. Varlıklar görüntüler listesi. Doğru varlığı seçin ve ardından Enter'ı seçin. 

    ![Varlık deseni için ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir [rol](luis-concept-roles.md)içeriyorsa, rolü tek bir iki nokta üst üste `:`ile, örneğin, varlık `{Location:Origin}`adından sonra belirtin. Rolleri varlıkların listesini bir liste görüntüler. Rolü seçin ve ardından Enter'ı seçin. 

    ![Rolü içeren varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlık seçtikten sonra deseni girdikten ve ardından Enter'ı seçin. Girme desenleri, işiniz bittiğinde [eğitme](luis-how-to-train.md) uygulamanızı.

    ![Girilen deseninin her iki türdeki varlık ile ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Model desenleri ile değiştirdikten sonra uygulamanızı eğitin
Sonra ekleme, düzenleme, kaldırmak veya bir desen yeniden atama [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

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
* Bir desen Düzenle
* Farklı bir amaç için ayrı ayrı desen yeniden atama
* Farklı bir hedefi için çeşitli desenlerden yeniden atama
* Tek-stili Sil
* Çeşitli desenlerden Sil
* Varlık tarafından filtre deseni listesi
* Filtre-ölçütü-ölçütü-Listele
* Varlık veya hedefi Filtreyi Kaldır
* Desen hedefi veya varlık sayfasında mevcut utterance ekleyin

## <a name="next-steps"></a>Sonraki adımlar

* Bir modelle bir model [oluşturma](luis-tutorial-pattern.md) hakkında bilgi edinin.
* Bilgi nasıl [eğitme](luis-how-to-train.md) uygulamanızı.
