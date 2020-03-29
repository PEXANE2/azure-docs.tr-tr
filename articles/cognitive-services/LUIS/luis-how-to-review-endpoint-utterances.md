---
title: Kullanıcı söyleyişlerini inceleyin - LUIS
titleSuffix: Azure Cognitive Services
description: Okuma dünyası söyleygisleri için niyet seçmek ve varlıkları işaretlemek için aktif öğrenme tarafından yakalanan sözleri gözden geçirin; değişiklikleri kabul edin, eğitin ve yayınlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: diberry
ms.openlocfilehash: 95b7c7446a47fafd26d00b0da4d880786340fcd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219857"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>Uç nokta söyleyerek LUIS uygulaması nasıl geliştirilir?

Doğru tahminler için uç nokta söyleyişlerini gözden geçirme işlemine [Etkin öğrenme](luis-concept-review-endpoint-utterances.md)denir. Etkin öğrenme uç nokta sorgularını yakalar ve kullanıcının emin olmadığı uç nokta söyleyişlerini seçer. Bu okuma dünyası söyleyişlerinin amacını seçmek ve varlıkları işaretlemek için bu sözleri gözden geçiriyorsunuz. Bu değişiklikleri örnek sözlerinizi kabul edin, ardından eğitin ve yayınlayın. LUIS daha sonra daha doğru bir şekilde söyleyiş tanımlar.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="enable-active-learning"></a>Etkin öğrenmeyi etkinleştirme

Etkin öğrenmeyi etkinleştirmek için kullanıcı sorgularını günlüğe kaydetmeniz gerekir. Bu querystring parametresi ve değeri `log=true` ile [bitiş noktası sorgusu](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) çağırarak gerçekleştirilir.

Doğru bitiş noktası sorgusunu oluşturmak için LUIS portalını kullanın.

1. Önizleme [LUIS portalında](https://preview.luis.ai/)uygulamanızı uygulamalar listesinden seçin.
1. **Yönet** bölümüne gidin ve ardından **Azure kaynaklarını**seçin.
1. Atanan tahmin kaynağı için **sorgu parametrelerini değiştir'i**seçin.

    > [!div class="mx-imgBorder"]
    > ![Etkin öğrenme için gerekli olan günlükleri kaydetmek için LUIS portalını kullanın.](./media/luis-tutorial-review-endpoint-utterances/azure-portal-change-query-url-settings.png)

1. Geçiş **Kaydet günlükleri** sonra **Bitti'yi**seçerek kaydedin.

    > [!div class="mx-imgBorder"]
    > ![Etkin öğrenme için gerekli olan günlükleri kaydetmek için LUIS portalını kullanın.](./media/luis-tutorial-review-endpoint-utterances/luis-portal-manage-azure-resource-save-logs.png)

     Bu eylem, sorgu string `log=true` parametresini ekleyerek örnek URL'yi değiştirir. Çalışma zamanı bitiş noktasına öngörü sorguları yaparken değiştirilen örnek sorgu URL'sini kopyalayın ve kullanın.

## <a name="correct-intent-predictions-to-align-utterances"></a>Söyleyiyi hizalamak için doğru niyet tahminleri

Her söyleyyenin **Hizalanmış niyet** sütununda görüntülenen önerilen bir niyeti vardır.

> [!div class="mx-imgBorder"]
> [![LUIS'in emin olmadığı son nokta söyleyişlerini gözden geçirin](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

Bu amacı kabul ederseniz, onay işaretini seçin. Öneriye katılmıyorsanız, hizalanmış niyet açılır listesinden doğru niyeti seçin ve ardından hizalanan niyetin sağındaki onay işaretini seçin. Onay işaretini seçtikten sonra, söyleyiş niyete taşınır ve **Gözden Geçir Bitiş Noktası Sözleri** listesinden kaldırılır.

> [!TIP]
> **İnceleme Endpoint Utterances** listesindeki tüm örnek tüm sözlülüklerden varlık tahminlerini gözden geçirmek ve düzeltmek için Niyet ayrıntıları sayfasına gitmek önemlidir.

## <a name="delete-utterance"></a>Söyleyiyi silme

Her söyleyiş gözden geçirme listesinden silinebilir. Silindikten sonra, listede bir daha görünmez. Bu, kullanıcı bitiş noktasından aynı söyleyiyi girse bile geçerlidir.

Söyleyiyi silmeniz gerektiğinden emin değilseniz, söyleyiyi Yok amacına taşıyın veya `miscellaneous` bu amaçla yeni bir niyet oluşturun ve söyleyiyi bu amaca taşıyın.

## <a name="disable-active-learning"></a>Aktif öğrenmeyi devre dışı

Etkin öğrenmeyi devre dışı kalım için kullanıcı sorgularını günlüğe kaydetmeyin. Bu, varsayılan değer yanlış `log=false` olduğundan, sorgustring parametresi ve değeri ile bitiş noktası [sorgusu](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) ayarlayarak veya querystring değerini kullanmayarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sözcükleri etiketledikten sonra performansın nasıl iyileştiğini test etmek için, üst panelde **Test'i** seçerek test konsoluna erişebilirsiniz. Test konsolunu kullanarak uygulamanızı nasıl sınayacağınıza ilişkin talimatlar için [Train'e bakın ve uygulamanızı test edin.](luis-interactive-test.md)
