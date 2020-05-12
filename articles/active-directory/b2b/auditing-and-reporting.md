---
title: B2B işbirliği kullanıcısını denetleme ve raporlama-Azure AD
description: Konuk Kullanıcı özellikleri Azure Active Directory B2B işbirliğinde yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67b543bd46b195a0944959f3f462a2bdc7d150ba
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83199473"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>B2B işbirliği kullanıcısını denetleme ve raporlama
Konuk kullanıcılar ile, üye kullanıcılarla aynı denetim özelliklerine sahip olursunuz. 

## <a name="access-reviews"></a>Erişim gözden geçirmeleri
Erişim gözden geçirmeleri kullanarak, konuk kullanıcıların kaynaklarınıza erişmeye devam edilip edilmeyeceğini düzenli aralıklarla doğrulayabilirsiniz. **Erişim gözden geçirmeleri** özelliği, **Kurumsal ilişkiler** **Azure Active Directory**  >  **erişim gözden geçirmeleri** (veya **dış kimlikler**  >  **erişim İncelemeleri** ) altında Azure Active Directory kullanılabilir. Ayrıca, Azure portal **tüm hizmetlerden** "erişim gözden geçirmeleri" için arama yapabilirsiniz. Erişim incelemelerini nasıl kullanacağınızı öğrenmek için bkz. [Azure AD erişim gözden geçirmeleri ile konuk erişimini yönetme](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Denetim günlükleri

Azure AD denetim günlükleri, Konuk kullanıcılar tarafından başlatılan etkinlikler dahil olmak üzere sistem ve Kullanıcı etkinliklerinin kayıtlarını sağlar. Denetim günlüklerine erişmek için, **Azure Active Directory**' de, **Izleme**altında, **Denetim günlükleri**' ni seçin. Davetli Sam oogle daveti ve kullanım geçmişine bir örnek aşağıda verilmiştir:

![Denetim günlüğü çıkışını gösteren ve örnek ekran görüntüsü](./media/auditing-and-reporting/audit-log.png)

Ayrıntıları almak için bu olayların her birini kullanabilirsiniz. Örneğin, kabul ayrıntılarına bakalım.

![Etkinlik ayrıntıları çıkışını gösteren ve örnek ekran görüntüsü](./media/auditing-and-reporting/activity-details.png)

Ayrıca, bu günlükleri Azure AD 'den dışarı aktarabilir ve özelleştirilmiş raporlar almak için seçtiğiniz raporlama aracını kullanabilirsiniz.

### <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

