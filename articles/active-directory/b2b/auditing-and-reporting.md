---
title: B2B işbirliği kullanıcılarını denetleme ve raporlama - Azure AD
description: Azure Active Directory B2B işbirliğinde konuk kullanıcı özellikleri yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74d2473d37dcf0d435ab61763e72a64111845956
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74273283"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>B2B işbirliği kullanıcılarını denetleme ve raporlama
Konuk kullanıcılarda, üye kullanıcılara benzer denetim özelliklerine sahipsiniz. 

## <a name="access-reviews"></a>Erişim gözden geçirmeleri
Konuk kullanıcıların kaynaklarınıza hala erişmeye ihtiyacı olup olmadığını düzenli aralıklarla doğrulamak için erişim değerlendirmelerini kullanabilirsiniz. **Access incelemeleri** özelliği,**Kurumsal İlişkileri** **Yönet** > altında **Azure Etkin Dizini'nde** kullanılabilir. (Azure portalındaki **tüm hizmetlerden** "erişim yorumlarını" da arayabilirsiniz.) Erişim yorumlarını nasıl kullanacağınızı öğrenmek için Azure [AD erişim yorumlarıyla konuk erişimini yönet'e](../governance/manage-guest-access-with-access-reviews.md)bakın.

## <a name="audit-logs"></a>Denetim günlükleri

Azure AD denetim günlükleri, konuk kullanıcılar tarafından başlatılan etkinlikler de dahil olmak üzere sistem ve kullanıcı etkinliklerinin kayıtlarını sağlar. Denetim günlüklerine erişmek için, Azure **Etkin Dizini'nde** **, İzleme**altında Denetim **günlüklerini**seçin. İşte davet ve davetSam Oogle kurtuluş geçmişi bir örnek:

![Denetim günlüğü çıktısının ekran görüntüsü ve örneği](./media/auditing-and-reporting/audit-log.png)

Ayrıntıları almak için bu olayların her birine dalabilir. Örneğin, kabul ayrıntılarına bakalım.

![Ekran görüntüsü gösterimi ve etkinlik ayrıntıları çıktısı örneği](./media/auditing-and-reporting/activity-details.png)

Ayrıca bu günlükleri Azure AD'den dışa aktarabilir ve özelleştirilmiş raporlar almak için seçtiğiniz raporlama aracını kullanabilirsiniz.

### <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

