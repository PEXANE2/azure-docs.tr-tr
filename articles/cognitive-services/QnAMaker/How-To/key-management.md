---
title: Kaynak ve anahtar yönetimi - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi iki tür anahtarlar, Abonelik anahtarları ve uç nokta anahtarlar ilgilenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b0b82f37b76a6bca6d84a05bc48b7ae9986fb76b
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967678"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Soru-cevap Oluşturucu anahtarlarını yönetme

Soru-cevap Oluşturucu hizmetinizi anahtarları, iki tür ile ilgilenen **Abonelik anahtarları** ve **uç nokta anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonelik anahtarları**: Bu anahtarlar [soru-cevap oluşturma Management Service API 'lerine](https://go.microsoft.com/fwlink/?linkid=2092179)erişmek için kullanılır. Bu API 'Ler, bilgi tabanınızı düzenleme gerçekleştirmenize olanak tanır.  

2. **Uç nokta anahtarları**: Bu anahtarlar, bir Kullanıcı sorusuna yanıt almak için Bilgi Bankası uç noktasına erişmek üzere kullanılır. Bu uç noktayı genellikle sohbet bot 'unuza veya Soru-Cevap Oluşturma hizmetini kullanan istemci uygulaması koduna kullanacaksınız.
 
## <a name="subscription-keys"></a>Abonelik anahtarları
Görüntüleyebilir ve soru-cevap Oluşturucu kaynağın oluşturulduğu Azure portalından abonelik anahtarlarınızı sıfırlayın. 
1. Azure Portalı'nda soru-cevap Oluşturucu kaynağa gidin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Git **anahtarları**.

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Uç nokta anahtarları

Uç nokta anahtarları alanından yönetilebilir [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın, profilinize gidin ve ardından **hizmet ayarları**' na tıklayın.

    ![Uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Görüntüleyin ya da anahtarlarınızı sıfırlayın.

    ![uç nokta Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Anahtarlarınızı tehlikeye girmiş düşünüyorsanız yenileyin. Bu, istemci uygulamanızda veya bot kodunuzda ilgili değişiklikleri gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Farklı bir dilde Bilgi Bankası oluşturma](./language-knowledge-base.md)
