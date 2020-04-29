---
title: Tanımlayıcılar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu kategorilerin ve desenlerin ve varlıklarının kullanımını ve tahminini iyileştirebilecek uygulama özellikleri eklemek için Language Understanding (LUO) kullanın
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: diberry
ms.openlocfilehash: 7560fdcbfc77ea2655e8af641794478ead4c11c7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80631460"
---
# <a name="use-descriptors-to-boost-signal-of-word-list"></a>Sözcük listesinin sinyalini artırmak için tanımlayıcıları kullanın

Doğruluğunu artırmak için LUSıS uygulamanıza özellikler ekleyebilirsiniz. Özellikler, belirli sözcüklerin ve deyimlerin bir uygulama etki alanı sözlüğü 'nün parçası olduğunu gösteren ipuçları sunarak LUYA yardımcı olur.

Bir [tanımlayıcı](luis-concept-feature.md) (tümcecik listesi), aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirlerin veya ürünlerin adları) bir grup değer (sözcükler veya ifadeler) içerir. Bunlardan biri hakkında ne tür bir saldırgan, diğerlerine de otomatik olarak uygulanır. Bu liste, eşleşen sözcüklerin bir [liste varlığıyla](reference-entity-list.md) (tam metin eşleşmeleri) aynı şey değildir.

Bir tanımlayıcı, bu kelimeyle ilgili ikinci bir sinyal olarak uygulama etki alanının sözlüğüne ekler.

Bir tanımlayıcının ne zaman ve neden kullanılacağını anlamak için [özellik kavramlarını](luis-concept-feature.md) gözden geçirin.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-descriptor"></a>Tanımlayıcı Ekle

1. **Uygulamalarım** sayfasında adına tıklayarak uygulamanızı açın ve ardından **Oluştur**' a tıklayın ve ardından uygulamanızın sol panelinde **tanımlayıcılar** ' a tıklayın.

1. **Tanımlayıcılar** sayfasında **+ tanımlayıcı Ekle**' ye tıklayın.

1. **Yeni tümcecik listesi tanımlayıcısı oluştur** iletişim kutusunda, tanımlayıcı için gibi `Cities` bir ad girin. **Değer** kutusuna, tanımlayıcılarının değerlerini yazın (örneğin,) `Seattle`. Tek seferde bir değer veya virgülle ayrılmış bir değerler kümesi yazabilir ve ardından **ENTER**tuşuna basabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Tanımlayıcı şehir ekleme](./media/luis-add-features/add-phrase-list-cities.png)

    LUO için yeterli değer girdikten sonra öneriler görünür. Önerilen değerlerin **Tümünü ekleyebilir** veya tek tek terimleri seçebilirsiniz.

1. Bu değerleri, eklenen tanımlayıcı değerleri birbirinin yerine kullanılabilen alternatifler ise, **Bu değerleri, değiştirilebilir olarak** bırakın.

1. Tümcecik listesi, **genel** ayarı olan tüm uygulamaya veya belirli bir modele (amaç veya varlık) uygulanabilir. Tümcecik listesini bir amaç veya varlıktan _tanımlayıcı_ olarak oluşturursanız, geçiş işlemi genel değil olarak ayarlanır. Bu durumda, iki durumlu bir özelliğin, bu nedenle, uygulamanın _genel değil_ , yalnızca o modele yönelik bir özellik olduğu anlamına gelir.

1. **Done** (Bitti) öğesini seçin. Yeni tanımlayıcı, **tanımlayıcılar** sayfasına eklenir.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> **Tanımlayıcılar** sayfasında bağlamsal araç çubuğundan tanımlayıcıyı silebilir veya devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir tanımlayıcıyı ekledikten, düzenledikten, sildikten veya devre dışı bırakarak, performansın gelişip artmediğini görmek için [uygulamayı yeniden eğitin ve test](luis-interactive-test.md) edin.
