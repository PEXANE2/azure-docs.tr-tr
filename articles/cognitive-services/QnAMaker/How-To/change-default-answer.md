---
title: Varsayılan yanıtı al-Soru-Cevap Oluşturma
description: Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.
ms.topic: how-to
ms.date: 07/02/2020
ms.openlocfilehash: 005442938167c1bf7927603c44d6f870795cbeee
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85979988"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı için varsayılan yanıtı değiştirme

Bir bilgi tabanı için varsayılan yanıt, bir yanıt bulunamadığında döndürülür. [Azure bot hizmeti](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs#calling-qna-maker-from-your-bot)gibi bir istemci uygulaması kullanıyorsanız, puan eşiğini karşılayan bir yanıt olmadığını belirten ayrı bir varsayılan yanıta da sahip olabilir.

## <a name="set-default-answer-when-you-create-knowledge-base"></a>Bilgi Bankası oluştururken varsayılan yanıtı ayarla

Yeni bir Bilgi Bankası oluşturduğunuzda varsayılan yanıt metni ayarlardan biridir. Oluşturma işlemi sırasında ayarlamayı tercih ederseniz, daha sonra aşağıdaki yordamı kullanarak değiştirebilirsiniz.

## <a name="change-default-answer-in-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında varsayılan yanıtı değiştirme

Bilgi Bankası varsayılan yanıtı, Soru-Cevap Oluşturma hizmetinden yanıt döndürülmediğinde döndürülür.

1. [Soru-cevap oluşturma portalında](https://www.qnamaker.ai/) oturum açın ve listeden bilgi bankasını seçin.
1. Gezinti çubuğundan **Ayarlar** ' ı seçin.
1. **Bilgi bankasını Yönet** bölümünde **Varsayılan Yanıt metninin** değerini değiştirin.

    :::image type="content" source="../media/qnamaker-concepts-confidencescore/change-default-answer.png" alt-text="Varsayılan yanıt metin kutusu vurgulanmış şekilde Soru-Cevap Oluşturma Portal, Ayarlar sayfasının ekran görüntüsü.":::

1. Değişikliği kaydetmek için **Kaydet ve eğitme '** yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Bilgi bankası oluşturma](../How-to/manage-knowledge-bases.md)