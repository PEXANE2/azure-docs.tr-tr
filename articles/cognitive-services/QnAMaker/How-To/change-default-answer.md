---
title: Varsayılan yanıtı al-Soru-Cevap Oluşturma
description: Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: fae5c38fd64435a3fae56862bad04e000916e88b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843284"
---
# <a name="set-default-answer-for-a-knowledge-base"></a>Bilgi Bankası için varsayılan yanıtı ayarlama

Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.

## <a name="change-default-answer"></a>Varsayılan yanıt değiştirme

1. Git [Azure portalında](https://portal.azure.com) ve oluşturduğunuz soru-cevap Oluşturucu hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklayın **App Service**.

    ![Azure portalında App service için soru-cevap Oluşturucu erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayarak **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Save (Kaydet)** düğmesine tıklayın.

    ![Uygulama ayarlarını seçin ve ardından DefaultAnswer için soru-cevap Oluşturucu düzenleyin](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App service'ı yeniden başlatın

    ![Soru-cevap Oluşturucu appservice DefaultAnswer değiştirdikten sonra yeniden başlatın](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Soru-Cevap Oluşturma ve LUSıS ile bir bot oluşturma](../tutorials/integrate-qnamaker-luis.md)