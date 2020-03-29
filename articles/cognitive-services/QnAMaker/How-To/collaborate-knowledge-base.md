---
title: Bilgi tabanında işbirliği - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker birden fazla kişinin bir bilgi tabanında işbirliği yapmasına izin verir. Bu özellik Azure Rol Tabanlı Erişim Denetimi ile sağlanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/03/2020
ms.author: diberry
ms.openlocfilehash: b5adc7ebacde056a141ca3b361b9eb2ea7900a39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75660758"
---
# <a name="collaborate-on-your-knowledge-base"></a>Bilgi tabanınızda işbirliği yapın

QnA Maker, aynı QnA Maker kaynağındaki tüm bilgi üslerinde birden çok kişinin işbirliği yapmasına olanak tanır. Bu özellik Azure [Rol Tabanlı Erişim Denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)ile sağlanır.

QnA Maker hizmetinizi biriyle paylaşmak için aşağıdaki adımları gerçekleştirin:

1. Azure portalında oturum açın ve QnA Maker kaynağınıza gidin.

    ![QnA Maker kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

1. **Erişim Denetimi (IAM)** sekmesine gidin.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

1. **Ekle'yi**seçin.

    ![QnA Maker IAM eklemek](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

1. **Sahibi** veya **Katılımcı** rolünü seçin. Role Tabanlı Erişim Denetimi aracılığıyla salt okunur erişim izni veremezsiniz. Sahibi ve Katılımcı rolleri, QnA Maker hizmetine okuma yazma erişim izinlerine sahiptir.

    ![QnA Maker IAM rol eklemek](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

1. Kullanıcının e-posta adresini girin ve **Kaydet'e**basın.

    ![QnA Maker IAM e-posta eklemek](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

QnA Maker hizmetinizi paylaştığınız kişi, [QnA Maker portalına](https://qnamaker.ai) giriş yaptığında bu hizmetteki tüm bilgi tabanlarını görebilir.

QnA Maker hizmetinde belirli bir bilgi tabanını paylaşamazsınız. Daha ayrıntılı erişim denetimi istiyorsanız, bilgi temellerinizi farklı QnA Maker hizmetleri arasında dağıtmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını test etme](./test-knowledge-base.md)
