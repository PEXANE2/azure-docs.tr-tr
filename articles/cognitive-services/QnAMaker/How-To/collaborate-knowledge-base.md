---
title: Bilgi Bankası 'nda işbirliği-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, bir Bilgi Bankası 'nda birden çok kişinin işbirliği yapmasına izin verir. Bu özellik Azure rol tabanlı Access Control sağlanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/03/2020
ms.author: diberry
ms.openlocfilehash: b5adc7ebacde056a141ca3b361b9eb2ea7900a39
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75660758"
---
# <a name="collaborate-on-your-knowledge-base"></a>Bilgi bankasında işbirliği yapın

Soru-Cevap Oluşturma, birden fazla kişinin aynı Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanlarında işbirliği yapmasına izin verir. Bu özellik Azure [rol tabanlı Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)sağlanır.

Soru-Cevap Oluşturma hizmetinizi biriyle paylaşmak için aşağıdaki adımları gerçekleştirin:

1. Azure portal oturum açın ve Soru-Cevap Oluşturma kaynağına gidin.

    ![Soru-Cevap Oluşturma kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

1. **Access Control (IAM)** sekmesine gidin.

    ![Soru-Cevap Oluşturma ıAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma ıAM ekleme](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

1. **Sahibi** veya **katkıda** bulunan rolünü seçin. Rol tabanlı Access Control aracılığıyla salt okuma erişimi verilemez. Sahip ve katkıda bulunan rollerinin Soru-Cevap Oluşturma hizmetine okuma yazma erişim izinleri vardır.

    ![Soru-Cevap Oluşturma ıAM rol Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

1. Kullanıcının e-posta adresini girin ve **Kaydet**' e basın.

    ![Soru-Cevap Oluşturma ıAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Soru-Cevap Oluşturma hizmetinizi ile paylaştığınız kişi, [soru-cevap oluşturma portalında](https://qnamaker.ai) oturum açar ve bu hizmette tüm bilgi temellerini görebilirler.

Bir Soru-Cevap Oluşturma hizmetinde belirli bir Bilgi Bankası 'nı paylaşacağınızı unutmayın. Daha ayrıntılı erişim denetimi istiyorsanız bilgi tabanlarınızı farklı Soru-Cevap Oluşturma hizmetlerine dağıtmayı göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını test etme](./test-knowledge-base.md)
