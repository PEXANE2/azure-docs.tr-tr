---
title: Tanımlayıcılar - LUIS
titleSuffix: Azure Cognitive Services
description: Kategorilerve desenlerin amaç ve varlıklarının algılanmasını veya tahmin ini geliştirebilecek uygulama özellikleri eklemek için Dil Anlayışını (LUIS) kullanın
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: diberry
ms.openlocfilehash: 2b5046bb61dcafbba0b0540935e08777fbd747a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74123128"
---
# <a name="use-descriptors-to-boost-signal-of-word-list"></a>Sözcük listesinin sinyalini artırmak için tanımlayıcıları kullanma

Doğruluğunu artırmak için LUIS uygulamanıza özellikler ekleyebilirsiniz. Özellikler, belirli sözcük ve tümceciklerin bir uygulama etki alanı sözlüğünün parçası olduğuna dair ipuçları sağlayarak LUIS'e yardımcı olur. 

Tanımlayıcı [descriptor](luis-concept-feature.md) (tümcecik listesi), aynı sınıfa ait olan ve benzer şekilde ele alınması gereken (örneğin, şehirlerin veya ürünlerin adları) bir değer grubunu (sözcük veya tümcecikler) içerir. LUIS'in bunlardan biri hakkında öğrendikleri otomatik olarak diğerlerine de uygulanır. Bu liste, eşleşen sözcüklerin [liste varlığı](reference-entity-list.md) (tam metin eşleşmeleri) ile aynı şey değildir.

Bir tanımlayıcı, bu kelimeler hakkında LUIS'e ikinci bir sinyal olarak uygulama etki alanının kelime dağarcığına ekler.

Tanımlayıcının ne zaman ve neden kullanılacağını anlamak için [özellik kavramlarını](luis-concept-feature.md) gözden geçirin. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-descriptor"></a>Tanımlayıcı ekleme

1. **Uygulamalarım** sayfasında adını tıklayarak uygulamanızı açın ve ardından **Oluştur'u**tıklatın ve ardından uygulamanızın sol panelinde **Tanımlayıcılar'ı** tıklatın. 

1. **Tanımlayıcılar** sayfasında + Tanımlayıcı **Ekle'yi**tıklatın. 
 
1. Yeni **ifade listesi açıklayıcı** iletişim kutusunda, tanımlayıcı için gibi `Cities` bir ad girin. **Değer** kutusunda, tanımlayıcıların değerlerini yazın, örneğin. `Seattle` Aynı anda bir değer yazabilir veya virgülle ayrılmış bir değer kümesi yazabilir ve sonra **Enter**tuşuna basabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Tanımlayıcı Şehirler ekle](./media/luis-add-features/add-phrase-list-cities.png)

    LUIS için yeterli değer girdikten sonra öneriler görüntülenir. + Önerilen **değerlerin tümünü ekleyebilir** veya tek tek terimler seçebilirsiniz.

1. Eklenen tanımlayıcı değerler birbirinin yerine kullanılabilecek alternatiflerse, **bu değerler değiştirilebilir** denetlenir.

1. **Done** (Bitti) öğesini seçin. Yeni tanımlayıcı **Tanımlayıcılar** sayfasına eklenir.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> **Tanımlayıcılar** sayfasındaki bağlamsal araç çubuğundan bir tanımlayıcıyı silebilir veya devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir tanımlayıcıyı ekledikten, düzenlemeden, siletdikten veya devre dışı bıraktıktan sonra, performansın iyileşip iyileşmediğini görmek için uygulamayı yeniden [eğitin ve test](luis-interactive-test.md) edin.
