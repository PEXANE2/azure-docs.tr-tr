---
title: Tümcecik listeleri-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu kategorilerin ve desenlerin ve varlıklarının kullanımını ve tahminini iyileştirebilecek uygulama özellikleri eklemek için Language Understanding (LUO) kullanın
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: diberry
ms.openlocfilehash: 0e3e4226eaaa0505eea96d8b3aca820f2327349e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467622"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Sözcük listesinin sinyalini artırmak için tümcecik listelerini kullanma

Doğruluğunu artırmak için LUSıS uygulamanıza özellikler ekleyebilirsiniz. Özellikler, belirli sözcüklerin ve deyimlerin bir uygulama etki alanı sözlüğü 'nün parçası olduğunu gösteren ipuçları sunarak LUYA yardımcı olur. 

[Tümcecik listesi](luis-concept-feature.md) , aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirlerin veya ürünlerin adları) bir grup değer (sözcükler veya ifadeler) içerir. Bunlardan biri hakkında ne tür bir saldırgan, diğerlerine de otomatik olarak uygulanır. Bu liste, eşleşen sözcüklerin bir [liste varlığıyla](reference-entity-list.md) (tam metin eşleşmeleri) aynı şey değildir.

Bir tümcecik listesi, bu sözcüklerin yaklaşık olarak LUO 'ya ikinci bir sinyal olarak uygulama etki alanının sözlüğüne ekler.

Bir tümcecik listesinin ne zaman ve neden kullanılacağını anlamak için [özellik kavramlarını](luis-concept-feature.md) gözden geçirin. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-phrase-list"></a>Tümcecik listesi ekle

LUO, uygulama başına en fazla 10 ifade listesine izin verir. 

1. **Uygulamalarım** sayfasında adına tıklayarak uygulamanızı açın ve ardından **Oluştur**' a tıklayın ve ardından uygulamanızın sol panelinde **tümcecik listeleri** ' ne tıklayın. 

1. **Tümcecik listeleri** sayfasında, **Yeni tümcecik listesi oluştur**' a tıklayın. 
 
1. **Tümcecik listesi Ekle** iletişim kutusunda, tümcecik listesinin adı olarak `Cities` yazın. **Değer** kutusuna, tümcecik listesinin değerlerini yazın. Tek seferde bir değer veya virgülle ayrılmış bir değerler kümesi yazabilir ve ardından **ENTER**tuşuna basabilirsiniz.

    ![Tümcecik listesi şehirleri ekleme](./media/luis-add-features/add-phrase-list-cities.png)

1. LUSıS, tümcecik listenize eklemek için ilgili değerler önerebilir. Eklenen değer (ler) ile ilgili anlam içeren önerilen değerler grubunu almak için **öner** ' e tıklayın. Önerilen değerlerden herhangi birini tıklatabilir veya tümünü eklemek için **Tümünü Ekle** ' ye tıklayabilirsiniz.

    ![Tümcecik listesi önerilen değerler-tümünü ekle](./media/luis-add-features/related-values.png)

1. Eklenen tümcecik listesi değerleri birbirinin yerine kullanılabilen alternatifler ise, **Bu değerlere** tıklayın.

    ![Tümcecik listesi önerilen değerler-değiştirilebilir kutu seç](./media/luis-add-features/interchangeable.png)

1. **Bitti**’ye tıklayın. "Şehirler" ifade listesi, **tümcecik listeleri** sayfasına eklenir.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Tümcecik **listeleri** sayfasındaki bağlamsal araç çubuğundan bir tümcecik listesini silebilir veya devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir tümcecik listesini ekledikten, düzenledikten, sildikten veya devre dışı bırakadıktan sonra, performansın gelişip artmediğini görmek için [uygulamayı yeniden eğitin ve test](luis-interactive-test.md) edin.
