---
title: 'Hızlı Başlangıç: Bilgi Bankası oluşturma-Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: CHIT-chat ile Bilgi Bankası oluşturma eklemek için Soru-Cevap Oluşturma API'si Service Portal 'ı kullanın. Bu, uygulamanızı etkileyici hale getirir. Botunuzun CHIT-chat için bir başlangıç noktası olarak, en yüksek bir üst kısımdaki sohbet sohbeti ekleyin ve bunları sıfırdan yazmanın zaman ve maliyet tasarrufu yapın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 798bbb725d4764c5ec7a1d69770e9508af0fdf5a
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376207"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Hızlı Başlangıç: Soru-Cevap Oluşturma API'si Service Portal 'ı kullanarak Bilgi Bankası oluşturma

Soru-Cevap Oluşturma API'si hizmet portalı, bir Bilgi Bankası oluştururken mevcut veri kaynaklarınızı eklemenizi kolaylaştırır. Yeni bir soru-cevap Oluşturucu Bilgi Bankası aşağıdaki belge türlerinden oluşturabilirsiniz:

<!-- added for scanability -->
* SSS sayfaları
* Ürünleri kılavuzları
* Yapılandırılmış belgeleri

Bilgilerinizi kullanıcılarınız hakkında daha ilgi çekici hale getirmek için bir CHIT-chat kişilik ekleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="create-a-new-knowledge-base"></a>Yeni bilgi bankası oluşturma

1. İçine oturum [soru-cevap Oluşturucu portalı](https://qnamaker.ai) seçin ve Azure kimlik bilgileri ile **Bilgi Bankası oluşturma**.

1. Soru-cevap Oluşturucu hizmetini zaten oluşturmadıysanız seçin **soru-cevap hizmeti oluşturma**. 

1. Bir Azure kiracısı, Azure abonelik adı ve listelerden soru-cevap Oluşturucu hizmeti ile ilişkili Azure kaynağı adı seçin **2. adım** soru-cevap Oluşturucu Portalı'nda. Bilgi Bankası barındıracak Azure soru-cevap Oluşturucu hizmetini seçin.

    ![Soru-cevap hizmetini ayarlama](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Yeni Bilgi Bankası için bilgi bankanızı ve veri kaynakları adını girin.

    ![Kümesi veri kaynakları](../media/qnamaker-how-to-create-kb/set-data-sources.png)

1. Hizmetinize gibi `my first kb`bir **ad** verin. Yinelenen adları ve özel karakterler desteklenir.

1. Soru-cevap oluşturma sorun giderme sayfasını URL olarak ekleyin: `https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/troubleshooting`, ardından öğesini seçin. `+ Add URL` Desteklenen kaynakları türleri hakkında daha fazla bilgi [burada](../Concepts/data-sources-supported.md). Bu hızlı başlangıçta, ayıklanmak istediğiniz veriler için **dosyaları karşıya yüklemeyin** . Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing) kaç belgeleri görmek için ekleyebilirsiniz.

1. Bir  **_profesyonel_ ÇıT-** KB 'nize sohbet ekleyin. 

1. Seçin **, KB oluşturma**.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Verilerin ayıklanabilmesi birkaç dakika sürebilir.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Bilgi tabanınız başarıyla oluşturulduğunda, **Bilgi Bankası** sayfasına yönlendirilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası ile işiniz bittiğinde Soru-Cevap Oluşturma portalında kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](set-up-qnamaker-service-azure.md#share-existing-services-with-qna-maker) .

> [!div class="nextstepaction"]
> [Meta verilerle soru ekleme](../quickstarts/add-question-metadata-portal.md)
