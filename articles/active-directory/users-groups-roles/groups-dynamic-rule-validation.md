---
title: Dinamik grup üyeliği için kuralları doğrulama (Önizleme)-Azure AD | Microsoft Docs
description: Üyeleri, Azure Active Directory dinamik gruplar için üyelik kuralına göre test etme.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 04/10/2020
ms.author: curtand
ms.reviewer: yukarppa
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cbbe37938249ee02c7deb4081a96aa1be2d8b7cb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84728513"
---
# <a name="validate-a-dynamic-group-membership-rule-preview-in-azure-active-directory"></a>Azure Active Directory bir dinamik grup üyeliği kuralını (Önizleme) doğrulama

Azure Active Directory (Azure AD) artık dinamik grup kurallarını (genel önizlemede) doğrulamak için araçlar sağlar. Kuralların beklenen şekilde çalıştığını onaylamak için **kuralları doğrula** sekmesinde, dinamik kuralınızı örnek grup üyelerine doğrulayabilirsiniz. Yöneticiler, dinamik grup kuralları oluştururken veya güncelleştirirken, bir kullanıcının veya cihazın grubun üyesi olup olmayacağını bilmesini ister. Bu, Kullanıcı veya cihazın kural ölçütlerini karşılayıp karşılamadığını değerlendirmeye ve üyelik beklenmediği zaman sorun gidermeye yardımcı olur.

## <a name="step-by-step-walk-through"></a>Adım adım izlenecek yol

Başlamak için **Azure Active Directory**  >  **gruplar**'a gidin. Var olan bir dinamik grubu seçin veya yeni bir dinamik grup oluşturun ve dinamik üyelik kuralları ' na tıklayın. Daha sonra **kuralları doğrula** sekmesini görebilirsiniz.

![Kuralları doğrula sekmesini bulun ve mevcut bir kuralla başlayın](./media/groups-dynamic-rule-validation/validate-tab.png)

**Kuralları doğrula** sekmesinde, üyeliklerini doğrulamak için kullanıcılar ' ı seçebilirsiniz. 20 Kullanıcı veya cihaz tek seferde seçilebilir.

![Mevcut kuralı doğrulamak için kullanıcıları ekleyin](./media/groups-dynamic-rule-validation/validate-tab-add-users.png)

Seçicideki kullanıcıları veya aygıtları seçtikten **sonra, doğrulama**otomatik olarak başlatılır ve doğrulama sonuçları görüntülenir.

![Kural doğrulamanın sonuçlarını görüntüleme](./media/groups-dynamic-rule-validation/validate-tab-results.png)

Sonuçlar, bir kullanıcının grubun üyesi olup olmadığını belirtir. Kural geçerli değilse veya bir ağ sorunu varsa, sonuç **bilinmiyor**olarak görünür. **Bilinmeyen**bir durumda, ayrıntılı hata iletisi, gereken sorunu ve eylemleri anlatmaktadır.

![Kural doğrulama sonuçlarının ayrıntılarını görüntüleme](./media/groups-dynamic-rule-validation/validate-tab-view-details.png)

Kuralı değiştirebilir ve üyeliklerin doğrulanması tetiklenecektir. Kullanıcının neden grubun üyesi olmadığını görmek için "Ayrıntıları görüntüle" ye tıklayın ve doğrulama ayrıntıları, kuralı oluşturan her bir ifadenin sonucunu gösterir. Çıkmak için **Tamam** ' ı tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplar için dinamik üyelik kuralları](groups-dynamic-membership.md)
