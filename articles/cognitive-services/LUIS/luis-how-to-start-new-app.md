---
title: Yeni bir uygulama oluşturun-LUSıS
titleSuffix: Azure Cognitive Services
description: Oluşturun ve Language Understanding (LUIS) sayfasındaki yönetin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 7f9809fde088a03d4b20b5c739253f446c7a84b6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563614"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>LUSıS portalında yeni bir LUO uygulaması oluşturma
Çeşitli şekillerde LUIS uygulaması oluşturmak için vardır. Bir LUIS uygulaması oluşturabileceğiniz [LUIS](https://www.luis.ai) portal ya da yazma LUIS aracılığıyla [API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

## <a name="using-the-luis-portal"></a>LUIS portalı kullanma
Çeşitli şekillerde LUIS portalında yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulama ile başlayın ve hedefleri, konuşma ve varlıklar oluşturun.
* Boş bir uygulamayla başlayıp bir [önceden oluşturulmuş etki alanı](luis-how-to-use-prebuilt-domains.md).
* Bir LUIS uygulaması zaten hedefleri ve konuşma varlıklarını içeren bir JSON dosyasından içeri aktarın.

## <a name="using-the-authoring-apis"></a>Yazma API'leri kullanma
Çeşitli şekillerde yazma API'leri ile yeni bir uygulama oluşturabilirsiniz:

* [Başlangıç](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) boş bir uygulama ile ve hedefleri, konuşma ve varlıklar oluşturun.
* [Başlangıç](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) önceden oluşturulmuş bir etki alanına sahip.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>LUIS yeni uygulama oluştur

1. Üzerinde **uygulamalarım** sayfasında **yeni uygulama oluştur**.

    ![LUIS uygulamalarının listesi](./media/luis-create-new-app/apps-list.png)


2. İletişim kutusunda, "TravelAgent" uygulamanızı adlandırın.

    ![Yeni uygulama iletişim kutusu oluşturma](./media/luis-create-new-app/create-app.png)

3. Uygulama kültürü seçin (TravelAgent bir uygulama için İngilizce seçin) ve ardından **Bitti**. 

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
1. Bu dosyayı [lusıs](luis-container-howto.md)kapsayıcınızda kullanın. 

    Yalnızca bir eğitilen modeli HALSıS kapsayıcısı ile kullanılmak üzere dışarı aktarmaya ilgileniyorsanız, **sürümler** sayfasına gidin ve oradan dışarı aktarın. 

## <a name="delete-app"></a>Uygulamayı silme

1. **Uygulamalarım** sayfasında, uygulama satırının sonundaki üç noktayı (...) seçin.
1. Menüden **Sil** ' i seçin.
1. Onay penceresinde **Tamam ' ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamadaki ilk göreviniz [hedef ekleme](luis-how-to-add-intents.md).
