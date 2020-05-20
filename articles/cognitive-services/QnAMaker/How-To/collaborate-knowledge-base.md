---
title: Bilgi Bankası 'nda işbirliği-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma, bir Bilgi Bankası 'nda birden çok kişinin işbirliği yapmasına izin verir. Bu özellik Azure rol tabanlı Access Control sağlanır.
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 1a2908b4b65017f427682627ce5d83b186956a58
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650793"
---
# <a name="collaboration-with-authors-and-editors"></a>Yazarlar ve düzenleyicilerle işbirliği

İşbirliği, işbirliğinin rolüne bağlı olarak ortak çalışan erişimini kısıtlamanız için Soru-Cevap Oluşturma kaynak düzeyinde sağlanır. Soru-Cevap Oluşturma ortak çalışan kimlik doğrulama [kavramları](../Concepts/role-based-access-control.md)hakkında daha fazla bilgi edinin.

## <a name="add-role-based-access-rbac-to-your-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağına rol tabanlı erişim (RBAC) ekleme

Soru-Cevap Oluşturma, birden fazla kişinin aynı Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanlarında işbirliği yapmasına izin verir. Bu özellik Azure [rol tabanlı Access Control](../../../active-directory/role-based-access-control-configure.md)sağlanır.

## <a name="access-at-the-qna-maker-resource-level"></a>Soru-Cevap Oluşturma kaynak düzeyinde erişim

Soru-Cevap Oluşturma hizmetinde belirli bir Bilgi Bankası 'nı paylaşamazsınız. Daha ayrıntılı erişim denetimi istiyorsanız bilgi tabanlarınızı farklı Soru-Cevap Oluşturma kaynaklarına dağıtmayı ve ardından her kaynağa roller eklemeyi düşünün.

## <a name="add-role-to-resource"></a>Kaynağa rol Ekle

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağına bir kullanıcı hesabı ekleme

Aşağıdaki adımlarda ortak çalışan rolü kullanılır, ancak bu adımlar kullanılarak [Roller](../reference-role-based-access-control.md) eklenebilir

1. [Azure](https://portal.azure.com/) portalında oturum açın ve soru-cevap oluşturma kaynağına gidin.

    ![Soru-Cevap Oluşturma kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

1. **Access Control (IAM)** sekmesine gidin.

    ![Soru-Cevap Oluşturma ıAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma ıAM ekleme](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

1. Aşağıdaki listeden bir rol seçin:

    |Rol|
    |--|
    |Sahip|
    |Katılımcı|
    |Soru-Cevap Oluşturma okuyucu|
    |Soru-Cevap Oluşturma Düzenleyicisi|
    |Bilişsel hizmetler kullanıcısı|

    ![Soru-Cevap Oluşturma ıAM rol Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

1. Kullanıcının e-posta adresini girin ve **Kaydet**' e basın.

    ![Soru-Cevap Oluşturma ıAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Soru-Cevap Oluşturma hizmetinizi paylaştığınız kişi [soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açtıklarında, bu hizmet içindeki tüm bilgi temellerini kendi rollerine göre görebilirler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını test etme](./test-knowledge-base.md)

İşbirliği hakkında daha fazla bilgi edinin:
* [Azure](../../../active-directory/role-based-access-control-configure.md) rol tabanlı erişim denetimi
* Rol tabanlı erişim denetimi [kavramlarını](../Concepts/role-based-access-control.md) soru-cevap oluşturma
