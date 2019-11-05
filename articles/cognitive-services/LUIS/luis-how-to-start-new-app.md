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
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 227efcdbcb7d8e776dd77b38c5d1dedd54d71b6b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500299"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>LUSıS portalında yeni bir LUO uygulaması oluşturma
Bir LUSıS uygulaması oluşturmanın birkaç yolu vardır. [LUIS portalında veya](https://www.luis.ai) LUIS yazma [API 'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)aracılığıyla bir LUIS uygulaması oluşturabilirsiniz.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="using-the-luis-portal"></a>LUO portalını kullanma

LUO portalında çeşitli yollarla yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulamayla başlayın ve amaçlar, utbotslar ve varlıklar oluşturun.
* Boş bir uygulama ile başlayın ve [önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md)ekleyin.
* Zaten amaçlar, utbotslar ve varlıklar içeren bir JSON dosyasından bir LUO uygulamasını içeri aktarın.

## <a name="using-the-authoring-apis"></a>Yazma API 'Lerini kullanma
Yazma API 'Leriyle birkaç yolla yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulamayla [başlayın](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) ve amaçlar, utbotslar ve varlıklar oluşturun.
* Önceden oluşturulmuş bir etki alanıyla [başlayın](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) .  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-new-app-in-luis"></a>LUSıS 'de yeni uygulama oluşturma

1. **Uygulamalarım** sayfasında **+ Oluştur**' u seçin.

    ![LUIS uygulamalarının listesi](./media/luis-create-new-app/apps-list.png)


2. İletişim kutusunda, uygulamanızı "TravelAgent" olarak adlandırın.

    ![Yeni uygulama oluştur iletişim kutusu](./media/luis-create-new-app/create-app.png)

3. Uygulama külbilgilerinizi seçin (TravelAgent uygulaması için Ingilizce ' yi seçin) ve **bitti**' yi seçin. 

    > [!NOTE]
    > Uygulama oluşturduktan sonra kültür değiştirilemez. 

## <a name="import-an-app-from-file"></a>Bir uygulamayı dosyadan içeri aktar

1. **Uygulamalarım** sayfasında **Yeni uygulama al**' ı seçin.
1. Açılan iletişim kutusunda geçerli bir uygulama JSON dosyası seçin ve **bitti**' yi seçin.

### <a name="import-errors"></a>İçeri aktarma hataları

Olası hatalar şunlardır: 

* Bu ada sahip bir uygulama zaten var. Bunu onarmak için, uygulamayı yeniden içeri aktarın ve **Isteğe bağlı adı** yeni bir adla ayarlayın. 

## <a name="export-app-for-backup"></a>Yedekleme için uygulamayı dışarı aktarma

1. **Uygulamalarım** sayfasında, **dışarı aktar**' ı seçin.
1. **JSON olarak dışarı aktar**' ı seçin. Tarayıcınız uygulamanın etkin sürümünü indirir.
1. Modeli arşivlemek için bu dosyayı yedekleme sisteminize ekleyin.

## <a name="export-app-for-containers"></a>Kapsayıcılar için uygulamayı dışarı aktarma

1. **Uygulamalarım** sayfasında, **dışarı aktar**' ı seçin.
1. **Kapsayıcı olarak dışarı aktar** ' ı seçin ve ardından dışarı aktarmak istediğiniz yayımlanmış yuvayı (üretim veya aşama) seçin.
1. Bu dosyayı [lusıs kapsayıcınızda](luis-container-howto.md)kullanın. 

    Yalnızca bir eğitilen modeli HALSıS kapsayıcısı ile kullanılmak üzere dışarı aktarmaya ilgileniyorsanız, **sürümler** sayfasına gidin ve oradan dışarı aktarın. 

## <a name="delete-app"></a>Uygulamayı Sil

1. **Uygulamalarım** sayfasında, uygulama satırının sonundaki üç noktayı (...) seçin.
1. Menüden **Sil** ' i seçin.
1. Onay penceresinde **Tamam ' ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamadaki ilk göreviniz [Amaç eklemektir](luis-how-to-add-intents.md).
