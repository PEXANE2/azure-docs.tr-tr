---
title: İngilizce olmayan Bilgi Bankası-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma birçok dilde Bilgi Bankası içeriğini destekler. Ancak, her bir Soru-Cevap Oluşturma Hizmeti tek bir dil için ayrılmalıdır. Belirli bir Soru-Cevap Oluşturma hizmetini hedeflemek için oluşturulan ilk bilgi tabanı, bu hizmetin dilini ayarlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 26792246267ced6d9fff50fe4fea11cc8d280d6a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966682"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Soru-Cevap Oluşturma için Bilgi Bankası içeriğinin dil desteği
Soru-Cevap Oluşturma birçok dilde Bilgi Bankası içeriğini destekler. Ancak, her bir Soru-Cevap Oluşturma Hizmeti tek bir dil için ayrılmalıdır. Belirli bir Soru-Cevap Oluşturma hizmetini hedeflemek için oluşturulan ilk bilgi tabanı, bu hizmetin dilini ayarlar. Desteklenen dillerin listesi için [buraya](../Overview/languages-supported.md) bakın.

Dil, ayıklanan veri kaynaklarının içeriğinden otomatik olarak tanınır. Yeni bir Soru-Cevap Oluşturma Hizmeti ve bu hizmette yeni bir Bilgi Bankası oluşturduktan sonra, dilin doğru şekilde ayarlandığını doğrulayabilirsiniz.

1. [Azure Portal](https://portal.azure.com/)' a gidin.

2. **Kaynak grupları** ' nı seçin ve soru-cevap oluşturma hizmetinin dağıtıldığı kaynak grubuna gidin ve **Azure Search** kaynağını seçin.

    ![Azure Search kaynak seçin](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. **Testkb** dizinini seçin. Bu Azure Search Dizin her zaman ilk oluşturulur ve bu hizmette bulunan tüm bilgi temellerine ait kaydedilen içeriği içerir. 

    ![Sınama KB 'sini seçin](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Testkb ayrıntılarının gösterildiği **alanlar** bölümünü seçin.

    ![Alanları Seçin](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Dil ayrıntılarını görmek için **çözümleyici** kutusunu işaretleyin.

    ![Çözümleyici 'yi seçin](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. Çözümleyicisinin belirli bir dile ayarlandığını bulmalısınız. Bu dil, Bilgi Bankası oluşturma adımı sırasında otomatik olarak algılandı. Bu dil, kaynak oluşturulduktan sonra değiştirilemez.

    ![Seçili çözümleyici](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure bot hizmeti ile bir QnA bot oluşturma](../Tutorials/create-qna-bot.md)
