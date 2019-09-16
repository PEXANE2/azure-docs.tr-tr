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
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5e50c814fef24aa799549d055ad6496f5bdf05e0
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961496"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Soru-Cevap Oluşturma için Bilgi Bankası içeriğinin dil desteği

Soru-Cevap Oluşturma birçok dilde Bilgi Bankası içeriğini destekler. Ancak, her bir Soru-Cevap Oluşturma Hizmeti tek bir dil için ayrılmalıdır. Belirli bir Soru-Cevap Oluşturma hizmetini hedeflemek için oluşturulan ilk bilgi tabanı, bu hizmetin dilini ayarlar. Desteklenen dillerin listesi için [buraya](../Overview/languages-supported.md) bakın.

Dil, ayıklanan veri kaynaklarının içeriğinden otomatik olarak tanınır. Yeni bir Soru-Cevap Oluşturma Hizmeti ve bu hizmette yeni bir Bilgi Bankası oluşturduktan sonra, dilin doğru şekilde ayarlandığını doğrulayabilirsiniz.

1. [Azure portalına](https://portal.azure.com/) gidin.

1. **Kaynak grupları** ' nı seçin ve soru-cevap oluşturma hizmetinin dağıtıldığı kaynak grubuna gidin ve **Azure Search** kaynağını seçin.

    ![Azure Search kaynak seçin](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. **Dizinler**' i seçin ve ardından **Testkb** dizini ' ni seçin. Bu, oluşturulan ilk Azure Search dizindir ve bu hizmet içindeki tüm bilgi temellerine ait kaydedilen içeriği içerir. 

1. Dizindeki alanları görmek için **alanları** seçin.

1. `questions` Ve`answer` alanlarının çözümleyici sütunu belirli bir dile ayarlanır. Bu dil, içeri aktarılan dosyalar ve URL 'lerden bilgi tabanı oluşturma adımı sırasında otomatik olarak algılandı. Bu dil, kaynak oluşturulduktan sonra değiştirilemez.

    ![Seçili çözümleyici](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure bot hizmeti ile bir QnA bot oluşturma](../Tutorials/create-qna-bot.md)
