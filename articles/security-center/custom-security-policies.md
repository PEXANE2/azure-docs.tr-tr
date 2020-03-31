---
title: Azure Güvenlik Merkezi'nde özel güvenlik ilkeleri oluşturma | Microsoft Dokümanlar
description: Azure Güvenlik Merkezi tarafından izlenen Azure özel ilke tanımları.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: memildin
ms.openlocfilehash: c709890ae6c57a001c6a0e9df4e973bd3bd24602
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258269"
---
# <a name="using-custom-security-policies"></a>Özel güvenlik ilkelerini kullanma

Azure Güvenlik Merkezi, sistemlerinizin ve ortamınızın güvenliğini sağlamaya yardımcı olmak için güvenlik önerileri oluşturur. Bu öneriler, tüm müşterilere sağlanan genel, varsayılan güvenlik ilkesine dahil edilen endüstrinin en iyi uygulamalarını temel alır. Ayrıca Güvenlik Merkezi'nin endüstri ve mevzuat standartları hakkındaki bilgisinden de gelebilirler.

Bu özellik sayesinde, kendi *özel* girişimlerinizi ekleyebilirsiniz. Ortamınız oluşturduğunuz ilkelere uymuyorsa öneriler alırsınız. Oluşturduğunuz özel girişimler, [öğreticide](security-center-compliance-dashboard.md)açıklanan mevzuata uygunluk panosunda yerleşik girişimlerin yanında görünür.

Azure İlkesi belgelerinde [burada](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#definition-location) tartışıldığı gibi, özel girişiminiz için bir konum belirttiğinizde, bir yönetim grubu veya abonelik olmalıdır. 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>Aboneliğinize özel bir girişim eklemek için 

1. Güvenlik Merkezi'nin kenar çubuğundan **Güvenlik ilkesi** sayfasını açın.

1. Özel bir girişim eklemek istediğiniz bir abonelik veya Yönetim Grubu seçin.

    [![Özel politikanızı oluşturacağınız bir abonelik seçme](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Güvenlik Merkezi'nde değerlendirilmeleri ve görüntülenmeleri için abonelik düzeyinde (veya daha yüksek) özel standartlar eklemeniz gerekir. 
    >
    > Özel bir standart eklediğinizde, bu kapsama bir *girişim* atar. Bu nedenle, bu atama için gereken en geniş kapsamı seçmenizi öneririz.

1. Güvenlik ilkesi sayfasında, özel girişimleriniz **altında, özel bir girişim ekle'yi**tıklatın.

    [![**Özel bir girişim ekle** seçeneğini tıklatın**](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    Aşağıdaki sayfa görüntülenir:

    ![İlke oluşturma veya ekleme](media/custom-security-policies/create-or-add-custom-policy.png)

1. Özel girişimler ekle sayfasında, kuruluşunuzda zaten oluşturulmuş özel ilkelerin listesini gözden geçirin. Aboneliğinize atamak istediğiniz birini görürseniz **Ekle'yi**tıklatın. Listede ihtiyaçlarınızı karşılayan bir girişim yoksa, bu adımı atlayın.

1. Yeni bir özel girişim oluşturmak için:

    1. **Yeni Oluştur'u**tıklatın.
    1. Tanımın konumunu ve adını girin.
    1. Eklemek için ilkeleri seçin ve **Ekle'yi**tıklatın.
    1. İstenilen parametreleri girin.
    1. **Kaydet**'e tıklayın.
    1. Özel girişimler ekle sayfasında, yenile'yi tıklatın ve yeni girişiminiz kullanılabilir olarak gösterilir.
    1. **Ekle'yi** tıklatın ve aboneliğinize atayın.

    > [!NOTE]
    > Yeni girişimler oluşturmak için abonelik sahibi kimlik bilgileri gerekir. Azure rolleri hakkında daha fazla bilgi için Azure [Güvenlik Merkezi'ndeki İzinler'e](security-center-permissions.md)bakın.

    Yeni girişiminiz yürürlüğe girer ve etkisini aşağıdaki iki şekilde görebilirsiniz:

    * Güvenlik Merkezi kenar çubuğundan, Politika & Uyumluluk **kapsamında, Mevzuata uygunluk'u**seçin. Uyumluluk panosu, yerleşik girişimlerin yanı sıra yeni özel girişiminizi göstermek için açılır.
    
    * Ortamınız tanımladığınız ilkeleri izlemezse öneriler almaya başlarsınız.

1. İlkiniz için ortaya çıkan önerileri görmek için, öneriler sayfasını açmak için kenar çubuğundan **Öneriler'i** tıklatın. Öneriler bir "Özel" etiketiyle görünür ve yaklaşık bir saat içinde kullanılabilir.

    [![Özel öneriler](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel güvenlik ilkeleri oluşturmayı öğrendiniz. 

Diğer ilgili materyaller için aşağıdaki makalelere bakın: 

- [Güvenlik ilkelerine genel bakış](tutorial-security-policy.md)
- [Yerleşik güvenlik ilkelerinin listesi](security-center-policy-definitions.md)