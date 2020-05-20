---
title: Amaç Ekle-LUSıS
titleSuffix: Azure Cognitive Services
description: Aynı amaçları içeren soruların veya komutların gruplarını belirlemek için LUSıS uygulamanıza amaçlar ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: f282bb0a80226afeb314aac4e8669634327f81d2
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654025"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Kullanıcı amaçlarını belirleme amacını ekleyin

Aynı amaca sahip olan soruların veya komutların gruplarını belirlemek için LUSıS uygulamanıza [amaçlar](luis-concept-intent.md) ekleyin.

LUU portalında, amaçlar üst gezinti çubuğunun **Build** bölümünden ve ardından sol bölmenin amaçlarından **yönetilir.**

## <a name="add-an-intent-to-your-app"></a>Uygulamanıza bir amaç ekleyin

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Amaçlar** sayfasında **+ Oluştur**' u seçin.
1. **Yeni amaç oluştur** iletişim kutusunda, amaç adını girin, örneğin `ModifyOrder` ve **bitti**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Amaç Ekle](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Amaç, yayımlanan tahmin uç noktasında yapılan noktaları tahmin etmek için [örnek](luis-concept-utterance.md) söylerinizi gerektirir.

## <a name="add-an-example-utterance"></a>Örnek bir mekan ekleyin

Örnek söyleyler, Kullanıcı sorularının veya komutlarının metin örnekleridir. Bu amacı tahmin etmek için Language Understanding (LUSıS) öğretmek için bir amaca yönelik örnek ekleme yapmanız gerekir. Amacınızı anlamak için, en fazla 15 ila 30 örnek araslar aralığında LUYA gerekir. Toplu olarak örnek eklemeyin. Her bir söylemeyle, zaten amaç içinde olan örneklerden farklı olduğu için dikkatle seçilmelidir.

1. Amaç ayrıntıları sayfasında, kullanıcılarınız için, amaç adının altındaki metin kutusunda olduğu gibi ilgili bir konterlik girin `Deliver a large cheese pizza` ve ardından ENTER tuşuna basın.

    > [!div class="mx-imgBorder"]
    > ![Ara vurgulanan, amaç Ayrıntıları sayfasının ekran görüntüsü](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png)

    LUO, tüm aralıkları küçük harfe dönüştürür ve kısa çizgiler gibi [belirteçlerin](luis-language-support.md#tokenization) etrafına boşluk ekler.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Amaç tahmin hataları

Amaç için eğitilen uygulamayla tahmin edildiğinde, bir amaç tahmin hatası belirlenir.

1. Söylenişi tahmin hatalarını bulmak ve bunları onarmak için yanlış ve belirsiz ' ın **filtre** seçeneklerini kullanın.

    > [!div class="mx-imgBorder"]
    > ![Söylenişi tahmin hatalarını bulmak ve bunları onarmak için filtre seçeneğini kullanın.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

1. Amaç ayrıntıları sayfasında puan değerini görüntülemek için, **Görünüm** seçenekleri menüsünden **ayrıntı hedefini göster** ' i seçin.

    Filtreler ve görünüm uygulandığında ve hatalar ile ilgili örnek bir sorun oluşursa, örnek bir liste, söz konusu konuları ve sorunları gösterir.

Her satır, bu iki puandaki fark olan en yakın Rival 'in puanı olan utterance için geçerli eğitimin tahmin Puanını gösterir.

### <a name="fixing-intents"></a>Amaçlar düzeltiliyor

Amaç tahmin hatalarını nasıl düzelteceğinizi öğrenmek için [Özet panosunu](luis-how-to-use-dashboard.md)kullanın. Özet Panosu, etkin sürümün son eğitimi için analiz sağlar ve modelinizi gidermeye yönelik en önemli önerileri sunar.

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Bağlam araç çubuğu diğer eylemleri sağlar:

* Örnek, düzenleme veya silme
* Farklı bir amaca örnek olarak yeniden atama
* Filtreler ve görünümler: yalnızca filtrelenmiş varlıkları içeren detersliği göster veya isteğe bağlı ayrıntıları görüntüle
* Örnek söyleylerini arayın

## <a name="train-your-app-after-changing-model-with-intents"></a>Bir modeli amaçlar ile değiştirdikten sonra uygulamanızı eğitme

Hedefleri ekledikten, düzenledikten veya kaldırdıktan sonra, değişikliklerinizin uç nokta sorgularına uygulanması için uygulamanızı [eğitin](luis-how-to-train.md) ve [yayımlayın](luis-how-to-publish-app.md) . Her tek değişiklikten sonra eğmeyin. Bir değişiklik grubundan sonra eğitme.

## <a name="next-steps"></a>Sonraki adımlar

Varlıklarla [örnek](luis-how-to-add-example-utterances.md) ekleme hakkında daha fazla bilgi edinin.
