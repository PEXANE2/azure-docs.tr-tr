---
title: Özellikler-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu kategorilerin ve desenlerin ve varlıklarının kullanımını ve tahminini iyileştirebilecek uygulama özellikleri eklemek için Language Understanding (LUO) kullanın
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: diberry
ms.openlocfilehash: 12445ec5b14f4c274e471bf1b061a3b221664d20
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592313"
---
# <a name="use-features-to-boost-signal-of-word-list"></a>Sözcük listesinin sinyalini artırmak için özellikleri kullanın

Doğruluğunu artırmak için LUSıS uygulamanıza özellikler ekleyebilirsiniz. Özellikler, belirli sözcüklerin ve deyimlerin bir uygulama etki alanı sözlüğü 'nün parçası olduğunu gösteren ipuçları sunarak LUYA yardımcı olur.

Bir özelliğin ne zaman ve neden kullanılacağını anlamak için [kavramları](luis-concept-feature.md) gözden geçirin.

## <a name="add-phrase-list-as-a-feature"></a>Özellik olarak tümcecik listesi ekleme

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Oluştur**' u ve ardından uygulamanızın sol panelinden **Özellikler** ' i seçin.

1. **Özellikler** sayfasında **+ Oluştur**' u seçin.

1. **Yeni tümcecik listesi özelliği oluştur** iletişim kutusunda, gibi bir ad girin `Cities` . **Değer** kutusuna, gibi şehirlerin örneklerini girin `Seattle` . Tek seferde bir değer veya virgülle ayrılmış bir değerler kümesi yazabilir ve ardından **ENTER**tuşuna basabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Özellik (tümcecik listesi) şehirleri ekleme ekran görüntüsü](./media/luis-add-features/add-phrase-list-cities.png)

    LUO için yeterli değer girdikten sonra öneriler görünür. Önerilen değerlerin **Tümünü ekleyebilir** veya tek tek terimleri seçebilirsiniz.

1. Deyimlerin birbirlerinin yerine kullanılabilir olması halinde **Bu değerleri koru değiştirilebilir** .

1. Tümcecik listesi, **genel** ayarı olan tüm uygulamaya veya belirli bir modele (amaç veya varlık) uygulanabilir. Tümcecik listesini bir amaç veya varlıktan bir _özellik_ olarak oluşturursanız, geçiş işlemi genel için ayarlanmadı. Bu durumda, iki durumlu özelliğin anlamı, uygulamanın _genel değil_ , bu modelde yalnızca yerel olduğu bir modeldir.

1. **Done** (Bitti) öğesini seçin. Yeni özellik **ml özellikleri** sayfasına eklenir.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Bir tümcecik listesini, **ml özellikleri** sayfasındaki bağlamsal araç çubuğundan silebilir veya devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir özelliği ekledikten, düzenledikten, sildikten veya devre dışı bırakadıktan sonra, performansın gelişip artmediğini görmek için [uygulamayı yeniden eğitin ve test](luis-interactive-test.md) edin.
