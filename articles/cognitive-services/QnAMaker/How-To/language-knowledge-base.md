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
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 63eb13dd131fcc1c424c02fdac10f531cc9f0282
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876621"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Soru-Cevap Oluşturma için Bilgi Bankası içeriğinin dil desteği

Soru-Cevap Oluşturma birçok dilde Bilgi Bankası içeriğini destekler. Ancak, her bir Soru-Cevap Oluşturma Hizmeti tek bir dil için ayrılmalıdır. Belirli bir Soru-Cevap Oluşturma hizmetini hedeflemek için oluşturulan ilk bilgi tabanı, bu hizmetin dilini ayarlar. Desteklenen dillerin listesi için [buraya](../Overview/languages-supported.md) bakın.

Dil, ayıklanan veri kaynaklarının içeriğinden otomatik olarak tanınır. Yeni bir Soru-Cevap Oluşturma Hizmeti ve bu hizmette yeni bir Bilgi Bankası oluşturduktan sonra, dilin doğru şekilde ayarlandığını doğrulayabilirsiniz.

1. [Azure portalına](https://portal.azure.com/) gidin.

1. **Kaynak grupları** ' nı seçin ve soru-cevap oluşturma hizmetinin dağıtıldığı kaynak grubuna gidin ve **Azure Search** kaynağını seçin.

    ![Azure Search kaynak seçin](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. **Testkb** dizinini seçin. Bu Azure Search Dizin her zaman ilk oluşturulur ve bu hizmette bulunan tüm bilgi temellerine ait kaydedilen içeriği içerir. 

    ![Sınama KB 'sini seçin](../media/qnamaker-how-to-language-kb/select-testkb.png)

1. _Testkb_ ayrıntılarının gösterildiği **alanlar** bölümünü seçin.

    ![Alanları Seçin](../media/qnamaker-how-to-language-kb/selectfields.png)

1. Dil ayrıntılarını görmek için **çözümleyici** kutusunu işaretleyin.

    ![Çözümleyici 'yi seçin](../media/qnamaker-how-to-language-kb/select-analyzer.png)

1. _Çözümleyici_ 'nin belirli bir dile ayarlandığını bulmalısınız. Bu dil, içeri aktarılan dosyalar ve URL 'lerden bilgi tabanı oluşturma adımı sırasında otomatik olarak algılandı. Bu dil, kaynak oluşturulduktan sonra değiştirilemez.

    ![Seçili çözümleyici](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure bot hizmeti ile bir QnA bot oluşturma](../Tutorials/create-qna-bot.md)
