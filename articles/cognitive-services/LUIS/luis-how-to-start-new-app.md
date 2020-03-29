---
title: Yeni bir uygulama oluşturun - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) web sayfasında uygulamalarınızı oluşturun ve yönetin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.openlocfilehash: d3f8696388a33a8ea112aae438c6bbe9af520c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220836"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>LUIS portalında yeni bir LUIS uygulaması oluşturun
LUIS uygulaması oluşturmanın birkaç yolu vardır. LUIS portalında veya LUIS [yazarapları](developer-reference-resource.md)aracılığıyla bir LUIS uygulaması oluşturabilirsiniz.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="using-the-luis-portal"></a>LUIS portalını kullanma

Önizleme portalında çeşitli yollarla yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulamayla başlayın ve niyetler, söyleyişler ve varlıklar oluşturun.
* Boş bir uygulamayla başlayın ve önceden oluşturulmuş bir [etki alanı](luis-how-to-use-prebuilt-domains.md)ekleyin.
* Zaten niyet, sözcük ve varlıklar içeren bir `.lu` veya `.json` dosyadan bir LUIS uygulaması içeri aktarın.

## <a name="using-the-authoring-apis"></a>Yazma API'lerini kullanma
Yazma API'leri ile birkaç şekilde yeni bir uygulama oluşturabilirsiniz:

* [Uygulama ekleyin](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2f) - boş bir uygulamayla başlayın ve niyetler, söyleyişler ve varlıklar oluşturun.
* [Önceden oluşturulmuş uygulama ekleyin](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/59104e515aca2f0b48c76be5) - niyetler, söyleyekler ve varlıklar dahil olmak üzere önceden oluşturulmuş bir etki alanıyla başlayın.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-new-app-in-luis"></a>LUIS'te yeni uygulama oluşturma

1. **Uygulamalarım** sayfasında aboneliğinizi seçin ve kaynak yazma sonra **+ Oluştur**. Ücretsiz deneme anahtarı kullanıyorsanız, [bir yazma kaynağı oluşturmayı](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal)öğrenin.

    ![LUIS uygulamalarının listesi](./media/create-app-in-portal.png)


1. İletişim kutusuna, uygulamanızın adını girin, örneğin. `Pizza Tutorial`

    ![Yeni uygulama iletişim kutusu oluşturma](./media/create-pizza-tutorial-app-in-portal.png)

1. Uygulama kültürünüzü seçin ve ardından **Bitti'yi**seçin. Açıklama ve tahmin kaynağı bu noktada isteğe bağlıdır. Daha sonra portalın **Yönet** bölümünde istediğiniz zaman ayarlayabilirsiniz.

    > [!NOTE]
    > Uygulama oluşturduktan sonra kültür değiştirilemez. 

    Uygulama oluşturulduktan sonra, LUIS portalı **sizin** için `None` zaten oluşturulmuş olan niyetle Niyet listesini gösterir. Artık boş bir uygulamanız var. 
    
    > [!div class="mx-imgBorder"]
    > ![Hiçbir örnek söyleyiş ile oluşturulan Hiçbir niyet ile Niyet listesi.](media/pizza-tutorial-new-app-empty-intent-list.png)

## <a name="other-actions-available"></a>Kullanılabilir diğer eylemler

Bağlam araç çubuğu diğer eylemleri sağlar:

* Uygulamayı yeniden adlandır
* Kullanarak dosyadan `.lu` alma veya`.json`
* (LUDown `.lu` için), [LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown) `.json`veya `.zip` [(LUIS konteyneri](luis-container-howto.md)için) olarak uygulama dışa aktarma
* Uç nokta sözcüklerini gözden geçirmek için kapsayıcı uç nokta günlüklerini içe aktarma
* Çevrimdışı çözümleme için uç `.csv`nokta günlüklerini dışa aktarma
* Uygulamayı silme

## <a name="next-steps"></a>Sonraki adımlar

Uygulama tasarımınız kasıtlı algılama [içeriyorsa, yeni niyetler oluşturun](luis-how-to-add-intents.md)ve örnek söz ekler ekleyin. Uygulama tasarımınız yalnızca veri ayıklamaysa, None amacına örnek sözlerle ekleyin, ardından [varlıklar oluşturun](luis-how-to-add-example-utterances.md)ve örnek söyleyiyi bu varlıklarla etiketle. 
