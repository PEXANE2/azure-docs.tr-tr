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
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 9c5398ff7cb31698db3d4a798b6a082f9e74b99b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955128"
---
# <a name="collaborate-on-your-knowledge-base"></a>Bilgi bankasında işbirliği yapın

Soru-Cevap Oluşturma, bir Bilgi Bankası 'nda birden çok kişinin işbirliği yapmasına izin verir. Bu özellik Azure [rol tabanlı Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)sağlanır. 

Soru-Cevap Oluşturma hizmetinizi biriyle paylaşmak için aşağıdaki adımları gerçekleştirin:

1. Azure portal oturum açın ve Soru-Cevap Oluşturma kaynağına gidin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. **Access Control (IAM)** sekmesine gidin.

    ![Soru-Cevap Oluşturma ıAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma ıAM ekleme](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. **Sahibi** veya **katkıda** bulunan rolünü seçin. Rol tabanlı Access Control aracılığıyla salt okuma erişimi verilemez. Sahip ve katkıda bulunan rolü Soru-Cevap Oluşturma hizmetine okuma-yazma erişim hakkına sahiptir.

    ![Soru-Cevap Oluşturma ıAM rol Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Paylaşmak istediğiniz e-postayı girin ve Kaydet ' e basın.

    ![Soru-Cevap Oluşturma ıAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Artık Soru-Cevap Oluşturma hizmetinizi paylaştığınız kişi, [soru-cevap oluşturma portalında](https://qnamaker.ai) oturum açar ve bu hizmette tüm bilgi temellerini görebilirler.

Bir Soru-Cevap Oluşturma hizmetinde belirli bir Bilgi Bankası 'nı paylaşacağınızı unutmayın. Daha ayrıntılı erişim denetimi istiyorsanız bilgi tabanlarınızı farklı Soru-Cevap Oluşturma hizmetlerine dağıtmayı göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını test etme](./test-knowledge-base.md)
