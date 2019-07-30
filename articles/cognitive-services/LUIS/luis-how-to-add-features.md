---
title: Tümcecik listeleri-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu kategori ve desenleri algılama veya hedefleri ve varlıkların tahmin iyileştirebilir uygulama özelliklerini eklemek için Language Understanding (LUIS) kullanın
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 0ce1fb182320b04aebb45b0f848d8c8f6c965fd9
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638184"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Kullanım deyimi word listesinin boost sinyale listeler

LUIS uygulamanızı kendi doğruluğunu artırmak için özellikler ekleyebilirsiniz. Bu belirli kelimeleri ipuçları sağlayarak Yardım LUIS özellikleri ve ifadeleri bir uygulama etki alanı sözlüğü bir parçasıdır. 

A [tümcecik listesi](luis-concept-feature.md) aynı sınıfa ait benzer şekilde (örneğin, şehirler ya da ürün adlarını) işlenmesi gereken değerleri (sözcük ve tümcecikleri) bir grup içerir. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Bu liste, eşleşen sözcüklerin bir [liste varlığıyla](reference-entity-list.md) (tam metin eşleşmeleri) aynı şey değildir.

Bir ifade listesi uygulama etki alanının sözlüğü LUIS için ikinci bir sinyal sözcükleri ilgili olarak ekler.

## <a name="add-phrase-list"></a>İfade listesi ekleme

LUO, uygulama başına en fazla 10 ifade listesine izin verir. 

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **derleme**, ardından **tümcecik listeleri** uygulamanızın sol panelde. 

1. Üzerinde **tümcecik listeleri** sayfasında **oluştur yeni ifade listesi**. 
 
1. **Tümcecik listesi Ekle** iletişim kutusunda, tümcecik listesinin adı `Cities` olarak yazın. İçinde **değer** tümcecik listesi değerlerini yazın. Bir saat veya virgülle ayrılmış değerler kümesi tek bir değer yazın ve sonra basın **Enter**.

    ![İfade listesi şehirler Ekle](./media/luis-add-features/add-phrase-list-cities.png)

1. LUIS, tümcecik listenize eklemek için ilgili değerleri önerebilirsiniz. Tıklayın **önerilir** bir grup için added value(s) anlamsal olarak ilişkili önerilen değerleri almak için. Önerilen değerlerden herhangi birini tıklatın veya tıklatın **Ekle** bunları eklemek için tüm.

    ![Tümcecik listesi önerilen değerler-tümünü ekle](./media/luis-add-features/related-values.png)

1. Tıklayın **birbirinin yerine bu değerleri** birbirlerinin yerine kullanılabilir seçenekleri eklendi ifade listesi değerler.

    ![Tümcecik listesi önerilen değerler-değiştirilebilir kutu seç](./media/luis-add-features/interchangeable.png)

1. **Bitti**’ye tıklayın. "Şehir" tümceciği listeye eklenen **tümcecik listeleri** sayfası.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Silme veya üzerinde bağlamsal araç tümcecik listeden devre dışı bırakmak **tümcecik listeleri** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

Sonra ekleme, düzenleme, silme veya bir ifade listesi devre dışı bırakma [eğitme ve uygulamayı test etme](luis-interactive-test.md) performansı artıran yeniden görmek için.
