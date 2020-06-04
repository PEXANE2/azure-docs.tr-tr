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
ms.topic: how-to
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 7d9f1e520ab5c96b9c5288383324b165a4cd74f0
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344450"
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

## <a name="global-phrase-list-applies-to-entire-app"></a>Genel ifade listesi tüm uygulama için geçerlidir

Bir tümcecik listesi, yardım almak için tasarlanan amaç veya varlığa uygulanmalıdır, ancak bir tümcecik listesinin tüm uygulamaya **genel** bir özellik olarak uygulanması gerektiği zamanlar olabilir.

ML özellikleri sayfasında, tümcecik listesini seçin ve ardından üst bağlamsal araç çubuğunda **genel yap** ' ı seçin.

## <a name="model-as-a-feature"></a>Özellik olarak model oluşturma

Bir varlık bir [amaç veya varlığa yönelik bir özellik](luis-concept-feature.md)olabilir.

Bir amaca özellik olarak bir varlık eklemek için amaçlar sayfasından amacı seçin ve bağlam araç çubuğunun üstünde **+ Özellik Ekle** ' yi seçin. Liste, özellik olarak uygulanabilecek tüm tümcecik listelerini ve varlıkları içerir.

Bir varlığı başka bir varlığa bir özellik olarak eklemek için, [varlık paletini](label-entity-example-utterance.md#adding-entity-as-a-feature-from-the-entity-palette) kullanarak amaç ayrıntısı sayfasında özelliğini ekleyebilir veya özelliği varlık ayrıntısı sayfasına [ekleyebilirsiniz](luis-how-to-add-entities.md#add-a-feature-to-a-machine-learned-entity) .

## <a name="next-steps"></a>Sonraki adımlar

Bir özelliği ekledikten, düzenledikten, sildikten veya devre dışı bırakadıktan sonra, performansın gelişip artmediğini görmek için [uygulamayı yeniden eğitin ve test](luis-interactive-test.md) edin.
