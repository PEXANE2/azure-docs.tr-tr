---
title: Bilgi Bankası 'nda işbirliği-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma, bir Bilgi Bankası 'nda birden çok kişinin işbirliği yapmasına izin verir. Bu özellik Azure rol tabanlı erişim denetimi (Azure RBAC) ile birlikte sağlanır.
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 0d59ca83e6067c083e4bfebdb7afaaf6669dda8a
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87926857"
---
# <a name="collaboration-with-authors-and-editors"></a>Yazarlar ve düzenleyicilerle işbirliği

İşbirliği, işbirliğinin rolüne bağlı olarak ortak çalışan erişimini kısıtlamanız için Soru-Cevap Oluşturma kaynak düzeyinde sağlanır. Soru-Cevap Oluşturma ortak çalışan kimlik doğrulama [kavramları](../Concepts/role-based-access-control.md)hakkında daha fazla bilgi edinin.

## <a name="add-azure-role-based-access-control-azure-rbac-to-your-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağınız için Azure rol tabanlı erişim denetimi (Azure RBAC) ekleme

Soru-Cevap Oluşturma, birden fazla kişinin aynı Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanlarında işbirliği yapmasına izin verir. Bu özellik [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../../active-directory/role-based-access-control-configure.md)ile birlikte sağlanır.

## <a name="access-at-the-qna-maker-resource-level"></a>Soru-Cevap Oluşturma kaynak düzeyinde erişim

Soru-Cevap Oluşturma hizmetinde belirli bir Bilgi Bankası 'nı paylaşamazsınız. Daha ayrıntılı erişim denetimi istiyorsanız bilgi tabanlarınızı farklı Soru-Cevap Oluşturma kaynaklarına dağıtmayı ve ardından her kaynağa roller eklemeyi düşünün.

## <a name="add-role-to-resource"></a>Kaynağa rol Ekle

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağına bir kullanıcı hesabı ekleme

Aşağıdaki adımlarda ortak çalışan rolü kullanılır, ancak bu adımlar kullanılarak [Roller](../reference-role-based-access-control.md) eklenebilir

1. [Azure](https://portal.azure.com/) portalında oturum açın ve soru-cevap oluşturma kaynağına gidin.

    ![Soru-Cevap Oluşturma kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. **Access Control (IAM)** sekmesine gidin.

    ![Soru-Cevap Oluşturma ıAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma ıAM ekleme](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Aşağıdaki listeden bir rol seçin:

    |Rol|
    |--|
    |Sahip|
    |Katılımcı|
    |Bilişsel hizmetler Soru-Cevap Oluşturma okuyucu|
    |Bilişsel hizmetler Soru-Cevap Oluşturma Düzenleyicisi|
    |Bilişsel hizmetler kullanıcısı|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="Soru-Cevap Oluşturma ıAM rol Ekle.":::

1. Kullanıcının e-posta adresini girin ve **Kaydet**' e basın.

    ![Soru-Cevap Oluşturma ıAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Bilgi temellerini görüntüleyin Soru-Cevap Oluşturma

Soru-Cevap Oluşturma hizmetinizi paylaştığınız kişi [soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açtıklarında, bu hizmet içindeki tüm bilgi temellerini kendi rollerine göre görebilirler.

Bir Bilgi Bankası seçerken, bu Soru-Cevap Oluşturma kaynaktaki geçerli rolleri Bilgi Bankası adının yanında görünür.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Web sayfasının sol üst köşesinde bulunan Bilgi Bankası adı ' nın yanında, parantez içinde rol adı olan düzenleme modunda Bilgi Bankası 'nın ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını test etme](./test-knowledge-base.md)

İşbirliği hakkında daha fazla bilgi edinin:
* [Azure](../../../active-directory/role-based-access-control-configure.md) rol tabanlı erişim denetimi
* Rol tabanlı erişim denetimi [kavramlarını](../Concepts/role-based-access-control.md) soru-cevap oluşturma
