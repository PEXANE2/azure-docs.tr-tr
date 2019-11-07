---
title: Kullanıcı utslerini gözden geçirme-LUSıS
titleSuffix: Azure Cognitive Services
description: Etkin öğrenimi tarafından yakalanan bir şekilde gözden geçirerek, okuma ve varlık kullanımı için varlıkları seçin; değişiklikleri kabul edin, eğitme ve yayımlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 4a77ac26076fc1b1e4e94ee24dafb28a0e88c858
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73669369"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal-for-active-learning"></a>Etkin öğrenme için LUA portalındaki uç nokta utslerini gözden geçirme

[Etkin öğrenme](luis-concept-review-endpoint-utterances.md) , uç nokta sorgularını yakalar ve kullanıcının gereken uç nokta utlerini seçer. Bu yazılanları inceleyerek, bu okuma dünyasının amacını seçin ve varlıkları işaretleyin. Bu değişiklikleri örnek söyleyenlerinde kabul edin ve yayımlayın. LUO daha sonra, bir daha doğru şekilde daha doğru şekilde tanımlanır.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="enable-active-learning"></a>Etkin öğrenmeyi etkinleştir

Etkin öğrenmeyi etkinleştirmek için Kullanıcı sorgularını günlüğe kaydedin. Bu, `log=true` QueryString parametresi ve değeri ile [Endpoint sorgusu](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) ayarlanarak gerçekleştirilir.

## <a name="disable-active-learning"></a>Etkin öğrenmeyi devre dışı bırak

Etkin öğrenmeyi devre dışı bırakmak için Kullanıcı sorgularını günlüğe eklemeyin. Bu, `log=false` QueryString parametresi ve değeri ile [Endpoint sorgusu](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) ayarlanarak gerçekleştirilir.

## <a name="filter-utterances"></a>Fisliği filtrele

1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı (örneğin, TravelAgent) açın, sonra üst çubukta **Oluştur** ' u seçin.

1. **Uygulama performansını iyileştirme**altında, **uç nokta Dıklılığını gözden geçir**' i seçin.

1. **Endpoint deterlerini gözden geçir** sayfasında, hedefe **göre filtre listesini veya varlığı** metin kutusunu seçin. Bu açılan liste, **amaçlar** altındaki tüm hedefleri ve **varlıklar**altındaki tüm varlıkları içerir.

    ![Utterslar filtresi](./media/label-suggested-utterances/filter.png)

1. Aşağı açılan listeden bir kategori (amaçlar veya varlıklar) seçin ve ardından gelen kuralı gözden geçirin.

    ![Amaç](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Etiket varlıkları
LUO, varlık belirteçleri (sözcükler), mavi renkle vurgulanmış varlık adlarıyla değiştirilir. Bir söylenişi 'in etiketsiz varlıkları varsa, bunları utterde olarak etiketleyin. 

1. Deterance 'teki kelimeyle seçin. 

1. Listeden bir varlık seçin.

    ![Etiket varlığı](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Tek bir söyme Hizala

Her söylük, **hizalanmış amaç** sütununda gösterilen önerilen bir amaca sahiptir. 

1. Bu öneriyi kabul ediyorsanız onay işaretini seçin.

    ![Hizalanmış hedefi tut](./media/label-suggested-utterances/align-intent-check.png)

1. Öneriyi kabul etmiyorsanız, hizalanmış amaç açılan listesinden doğru amacı seçin, sonra hizalanmış amaç ' ın sağındaki onay işaretine tıklayın. 

    ![Hedefi Hizala](./media/label-suggested-utterances/align-intent.png)

1. Onay işaretini seçtikten sonra, söylenişi listeden kaldırılır. 

## <a name="align-several-utterances"></a>Birkaç kuralı hizalayın

Çeşitli söyleyleri hizalamak için, dıklarıyla ilgili kutuyu işaretleyin, sonra **Seçili Ekle** düğmesini seçin. 

![Birkaç align](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Hizalanmış amacı doğrula

**Amaç** sayfasına gidip, amaç adını seçerek ve göz anlıları gözden geçirerek, söylenişi 'in doğru amaç ile hizalandığını doğrulayabilirsiniz. **Gözden geçirme uç noktası çeşidindeki** utterlik, listede yer noktasıdır.

## <a name="delete-utterance"></a>Söylenişi silme

Her bir söylik İnceleme listesinden silinebilir. Silindikten sonra listede bir daha görünmez. Bu, Kullanıcı uç noktadan aynı şekilde girdiğinde bile geçerlidir. 

Söylenişi 'i silmeniz gerekip gerekmediğini bilmiyorsanız, bunu hiçbiri amacına taşıyın veya "çeşitli" gibi yeni bir amaç oluşturun ve bu amaca göre taşıyın. 

## <a name="delete-several-utterances"></a>Birkaç kuralı silme

Birçok göz silmek için, her bir öğeyi seçin ve **Seçili Ekle** düğmesinin sağındaki çöp kutusu ' nu seçin.

![Birkaçını Sil](./media/label-suggested-utterances/delete-several.png)


## <a name="next-steps"></a>Sonraki adımlar

Önerilen söyleylerini etiketledikten sonra performansın nasıl artdığı test etmek için, üst panelde **Test** ' i seçerek test konsoluna erişebilirsiniz. Test konsolunu kullanarak uygulamanızı test etme hakkında yönergeler için bkz. [uygulamanızı eğitme ve test](luis-interactive-test.md)etme.
