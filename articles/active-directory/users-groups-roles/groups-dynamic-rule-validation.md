---
title: Dinamik grup üyeliği için kuralları doğrulayın (önizleme) - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde dinamik bir grup için üyeleri üyelik kuralına göre nasıl sınayabilirsiniz?
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/10/2020
ms.author: curtand
ms.reviewer: yukarppa
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84e081f62949f4cf3f7ae375b2c60ee5b032d974
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115527"
---
# <a name="validate-a-dynamic-group-membership-rule-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde dinamik bir grup üyelik kuralını (önizleme) doğrulama

Azure Etkin Dizin (Azure AD) artık dinamik grup kurallarını (genel önizlemede) doğrulamak için araçlar sağlar. Kuralları **Doğrula** sekmesinde, kuralın beklendiği gibi çalıştığını doğrulamak için örnek grup üyelerine karşı dinamik kuralınızı doğrulayabilirsiniz. Yöneticiler dinamik grup kuralları oluştururken veya güncellerken, bir kullanıcının veya aygıtın grubun bir üyesi olup olmayacağını bilmek ister. Bu, kullanıcının veya aygıtın kural ölçütlerini karşılayıp karşılamadığını değerlendirmeye ve üyelik beklenmediği durumlarda sorun gidermeye yardımcı olur.

## <a name="step-by-step-walk-through"></a>Adım adım gezinme

Başlamak için Azure **Etkin Dizin** > **Grupları'na**gidin. Varolan dinamik bir grup seçin veya yeni bir dinamik grup oluşturun ve Dinamik üyelik kurallarını tıklatın. Daha sonra **Kuralları Doğrula** sekmesini görebilirsiniz.

![Kuralları Doğrula sekmesini bulun ve varolan bir kuralla başlayın](./media/groups-dynamic-rule-validation/validate-tab.png)

**Kuralları Doğrula** sekmesinde, kullanıcıların üyeliklerini doğrulamasını seçebilirsiniz. Aynı anda 20 kullanıcı veya cihaz seçilebilir.

![Karşı varolan kuralı doğrulamak için kullanıcıları ekleme](./media/groups-dynamic-rule-validation/validate-tab-add-users.png)

Seçiciden kullanıcıları veya cihazları seçtikten sonra ve **Seç,** doğrulama otomatik olarak başlar ve doğrulama sonuçları görüntülenir.

![Kural doğrulama sonuçlarını görüntüleme](./media/groups-dynamic-rule-validation/validate-tab-results.png)

Sonuçlar, kullanıcının grubun bir üyesi olup olmadığını söyler. Kural geçerli değilse veya bir ağ sorunu varsa, sonuç **Bilinmeyen**olarak gösterecektir. **Bilinmiyor**durumunda, ayrıntılı hata iletisi sorunu ve gerekli eylemleri açıklar.

![Kural doğrulama sonuçlarının ayrıntılarını görüntüleme](./media/groups-dynamic-rule-validation/validate-tab-view-details.png)

Kuralı değiştirebilirsiniz ve üyeliklerin doğrulanması tetiklenir. Kullanıcının neden grubun bir üyesi olmadığını görmek için "Ayrıntıları görüntüle"yi tıklatın ve doğrulama ayrıntıları kuralı oluşturan her ifadenin sonucunu gösterir. Çıkmak için **Tamam'ı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplar için dinamik üyelik kuralları](groups-dynamic-membership.md)
