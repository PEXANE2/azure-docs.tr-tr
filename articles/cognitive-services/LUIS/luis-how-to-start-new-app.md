---
title: Yeni bir uygulama oluşturun-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUSıS) Web sayfasında uygulamalarınızı oluşturun ve yönetin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: 304e76a2bf8bae3e6ee2120b892ef97d89c3decc
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144396"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>LUSıS portalında yeni bir LUO uygulaması oluşturma
Bir LUSıS uygulaması oluşturmanın birkaç yolu vardır. LUIS portalında veya LUıS yazma [API 'leri](developer-reference-resource.md)aracılığıyla bir LUIS uygulaması oluşturabilirsiniz.

## <a name="using-the-luis-portal"></a>LUO portalını kullanma

Portalda çeşitli yollarla yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulamayla başlayın ve amaçlar, utbotslar ve varlıklar oluşturun.
* Boş bir uygulama ile başlayın ve [önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md)ekleyin.
* Bir veya daha fazla `.lu` `.json` Amaç, utde ve varlık içeren bir dosyadan bir Luo uygulamasını içeri aktarın.

## <a name="using-the-authoring-apis"></a>Yazma API 'Lerini kullanma
Yazma API 'Leriyle birkaç yolla yeni bir uygulama oluşturabilirsiniz:

* [Uygulama ekleyin](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2f) -boş bir uygulamayla başlayın ve amaçlar, utbotslar ve varlıklar oluşturun.
* [Önceden oluşturulmuş uygulama ekleme](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/59104e515aca2f0b48c76be5) -amaçlar, utterslar ve varlıklar dahil önceden oluşturulmuş bir etki alanıyla başlayın.


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>


[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-new-app-in-luis"></a>LUSıS 'de yeni uygulama oluşturma

1. **Uygulamalarım** sayfasında **aboneliğinizi**seçin ve **kaynağı** yazıp **+ Oluştur**' a tıklayın. 

> [!div class="mx-imgBorder"]
> ![LUIS uygulamalarının listesi](./media/create-app-in-portal.png)

1. İletişim kutusuna uygulamanızın adını (gibi) girin `Pizza Tutorial` .

    ![Yeni uygulama oluştur iletişim kutusu](./media/create-pizza-tutorial-app-in-portal.png)

1. Uygulamanızın kültür ' i seçin ve **bitti**' yi seçin. Açıklama ve tahmin kaynağı bu noktada isteğe bağlıdır. Daha sonra portalın **Yönet** bölümünde istediğiniz zaman ayarlayabilirsiniz.

    > [!NOTE]
    > Uygulama oluşturduktan sonra kültür değiştirilemez.

    Uygulama oluşturulduktan sonra, Lua **portalı,** `None` sizin için önceden oluşturulmuş amaç listesini gösterir. Artık boş bir uygulamanız var.

    > [!div class="mx-imgBorder"]
    > ![Hiçbir örnek olmadan oluşturulan amaç listesi.](media/pizza-tutorial-new-app-empty-intent-list.png)

## <a name="other-actions-available-on-my-apps-page"></a>Uygulamalarım sayfasında kullanılabilir diğer eylemler

Bağlam araç çubuğu diğer eylemleri sağlar:

* Uygulamayı yeniden adlandır
* Veya kullanarak dosyadan içeri aktar `.lu``.json`
* Uygulamayı `.lu` ( [LUI](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)için), `.json` , veya `.zip` ( [lusıs kapsayıcısı](luis-container-howto.md)için) olarak dışa aktar
* Uç nokta utslerini gözden geçirmek için kapsayıcı uç nokta günlüklerini içeri aktarma
* Çevrimdışı analiz için uç nokta günlüklerini, bir olarak dışarı aktar `.csv`
* Uygulamayı Sil

## <a name="next-steps"></a>Sonraki adımlar

Uygulama tasarımınızda amaç algılama, [yeni amaçlar oluşturma](luis-how-to-add-intents.md)ve örnek ekleme dahil olmak üzere. Uygulama tasarımınız yalnızca veri ayıklama ise, None amacına örnek bir değer ekleyin, daha sonra [varlıklar oluşturun](luis-how-to-add-example-utterances.md)ve bu varlıklarla örnek utbotları etiketleyin.
