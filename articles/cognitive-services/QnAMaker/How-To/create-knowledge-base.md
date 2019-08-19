---
title: Bilgi Bankası oluşturma-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: CHIT-chat ile Bilgi Bankası oluşturma eklemek için Soru-Cevap Oluşturma API'si Service Portal 'ı kullanın. Bu, uygulamanızı etkileyici hale getirir. Botunuzun CHIT-chat için bir başlangıç noktası olarak, en yüksek bir üst kısımdaki sohbet sohbeti ekleyin ve bunları sıfırdan yazmanın zaman ve maliyet tasarrufu yapın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b2cacc8cc6d7c22a93f46007e8150a4e55dc7650
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967726"
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

    - Hizmetinize vermek bir **adı.** Yinelenen adları ve özel karakterler desteklenir.
    - URL için ayıklanan istediğiniz verileri ekleyin. Desteklenen kaynakları türleri hakkında daha fazla bilgi [burada](../Concepts/data-sources-supported.md).
    - Ayıklanan istediğiniz veri dosyalarını karşıya yükleyin. Bkz: [fiyatlandırma bilgileri](https://aka.ms/qnamaker-pricing) kaç belgeleri görmek için ekleyebilirsiniz.
    - Bankalarıyla el ile eklemek istiyorsanız, atlayabilirsiniz **4. adım** önceki görüntüde gösterildiği.

1. Ekleme **Chit sohbet** , KB. Her birini seçerek, bot için CHIT-chat desteği eklemeyi seçin. 

    ![KB 'ye sohbet ekleyin](../media/qnamaker-how-to-create-kb/create-kb-chit-chat.png)

1. Seçin **, KB oluşturma**.

    ![KB oluşturma](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Ayıklanacak veri birkaç dakika sürer.

    ![Ayıklama](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Bilgi bankanızı başarıyla oluşturulduğunda yönlendirilirsiniz **Bilgi Bankası** sayfası.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası ile işiniz bittiğinde Soru-Cevap Oluşturma portalında kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) .

> [!div class="nextstepaction"]
> [Sohbet chit kişisel Ekle](./chit-chat-knowledge-base.md)
