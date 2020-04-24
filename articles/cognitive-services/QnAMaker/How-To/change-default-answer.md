---
title: Varsayılan yanıtı al-Soru-Cevap Oluşturma
description: Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.
ms.topic: how-to
ms.date: 04/22/2020
ms.openlocfilehash: db5a79ec612a73066ac37365a1815841fafb3862
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82097107"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı için varsayılan yanıtı değiştirme

Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.

## <a name="change-default-answer-in-the-azure-portal"></a>Azure portal varsayılan yanıtı değiştirme

1. [Azure Portal](https://portal.azure.com) gidin ve oluşturduğunuz soru-cevap oluşturma hizmeti temsil eden kaynak grubuna gidin.

2. **App Service**açmak için tıklayın.

    ![Azure portal, Soru-Cevap Oluşturma için App Service 'e erişin](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. **Uygulama ayarları** ' na tıklayın ve **Defaultanswer** alanını istenen varsayılan yanıta göre düzenleyin. **Kaydet**’e tıklayın.

    ![Uygulama ayarları ' nı seçin ve ardından Soru-Cevap Oluşturma için DefaultAnswer öğesini düzenleyin](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App Service 'i yeniden başlatın

    ![DefaultAnswer 'ı değiştirdikten sonra, Soru-Cevap Oluşturma appservice 'i yeniden başlatın](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Bilgi bankası oluşturma](../How-to/manage-knowledge-bases.md)