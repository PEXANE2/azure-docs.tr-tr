---
title: Niyet ekle - LUIS
titleSuffix: Azure Cognitive Services
description: Aynı niyetlere sahip soru veya komut gruplarını belirlemek için LUIS uygulamanıza niyetler ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 66a3350dee60772ce706af8995179dcd8c485b64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73904302"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Kullanıcının söyleyiş niyetini belirlemek için niyet ekleme

Aynı amacına sahip soru veya komut gruplarını belirlemek için LUIS uygulamanıza [niyet](luis-concept-intent.md) ler ekleyin. 

Niyetler üst gezinti çubuğunun **Yapı** bölümünden, ardından sol panelin **Niyetlerinden**yönetilir. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-intent"></a>Amaç ekleme

1. LUIS [önizleme portalında,](https://preview.luis.ai)amaçları görüntülemek için **Yapı'yı** seçin. 
1. **Niyetler** sayfasında + **Oluştur'u**seçin.
1. Yeni **niyet** oluştur iletişim kutusuna, örneğin `ModifyOrder`niyet adını girin ve **Bitti'yi**seçin.

    > [!div class="mx-imgBorder"]
    > ![Niyet Ekle](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Niyetin örnek söze ihtiyacı var.

## <a name="add-an-example-utterance"></a>Örnek bir söz ekle

Örnek söz, kullanıcı sorularının veya komutlarının metin örnekleridir. Dil Anlama (LUIS) öğretmek için ne zaman bu niyeti tahmin etmek için, bir niyet için örnek söyleyiş eklemeniz gerekir. LUIS'in niyeti anlamaya başlamak için 15-30 örnek söz arasında olması gerekir. Örnek sözlerini toplu olarak eklemeyin. Her söyleyiş, niyetteki örneklerden nasıl farklı olduğu konusunda dikkatle seçilmelidir. 

1. Niyet ayrıntıları sayfasına, kullanıcılarınızın amacının altındaki metin kutusunda `Deliver a large cheese pizza` olduğu gibi, kullanıcılarınızdan beklediğiniz alakalı bir söyleyiş girin ve ardından Enter tuşuna basın.
 
    > [!div class="mx-imgBorder"]
    > ![Kasıtlı ayrıntıları sayfasının ekran görüntüsü, söyleyiş vurgulanmış](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS tüm söyleyişleri küçük harfe dönüştürür ve tire gibi [belirteçlerin](luis-language-support.md#tokenization) etrafına boşluklar ekler.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Niyet tahmin hataları 

Bir niyetle yapılan bir örnek, örnek söyleyicinin şu anda bulunduğu amaç ile eğitim sırasında belirlenen niyet arasında bir niyet tahmini hatasına sahip olabilir. 

Sözcük tahmini hatalarını bulmak ve düzeltmek için, **Ayrıntılı görünümün** **Görünüm** seçeneğiyle birlikte Yanlış ve Belirsiz **Filtre** seçeneklerini kullanın. 

![Sözcük tahmini hatalarını bulmak ve düzeltmek için Filtre seçeneğini kullanın.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Filtreler ve görünüm uygulandığında ve hatalarla ilgili örnek sözcükler olduğunda, örnek söyleyiş listesi, sözcük ve sorunları gösterir.

> [!div class="mx-imgBorder"]
> ![! [Filtreler ve görünüm uygulandığında ve hatalarla ilgili örnek söyleyişler olduğunda, örnek söyleyiş listesi söyleyiyi ve sorunları gösterir.] (./media/luis-how-to-add-intents/find-errors-in-utterances.png)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Her satır, bu iki skorarasındaki fark olan en yakın rakibin skoru olan örnek söyleyiş için geçerli eğitimin tahmin puanını gösterir. 

### <a name="fixing-intents"></a>Amaçları düzeltme

Niyet tahmini hatalarını nasıl düzelteceğimi öğrenmek için [Özet Panosu'nu](luis-how-to-use-dashboard.md)kullanın. Özet panosu, etkin sürümün son eğitimi için analiz sağlar ve modelinizi düzeltmek için en iyi önerileri sunar.  

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Bağlam araç çubuğu diğer eylemleri sağlar:

* Örnek söyleyiyi düzenle veya silme
* Örnek söyleyiyi farklı bir amaciçin yeniden atama
* Filtreler ve görünümler: yalnızca filtreuygulanmış varlıklar içeren veya isteğe bağlı ayrıntıları görüntüleyen dilerleri gösterin
* Örnek söyleyişlerle arama yapın

## <a name="train-your-app-after-changing-model-with-intents"></a>Modeli niyetle değiştirdikten sonra uygulamanızı eğitin

Son nokta sorgularına değişikliklerinizin uygulanabilmesi için uygulamanızı ekleme, değiştirme veya kaldırma, [uygulamanızı eğitin](luis-how-to-train.md) ve [yayımlayın.](luis-how-to-publish-app.md) Her değişiklikten sonra antrenman yapmayın. Bir grup değişiklikten sonra eğitin. 

## <a name="next-steps"></a>Sonraki adımlar

Varlıklarla [örnek söz ekleme](luis-how-to-add-example-utterances.md) hakkında daha fazla bilgi edinin. 
