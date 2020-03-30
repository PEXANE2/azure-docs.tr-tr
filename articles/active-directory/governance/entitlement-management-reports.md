---
title: Yetkilendirme yönetiminde raporları & günlükleri görüntüleme - Azure AD
description: Azure Active Directory yetkilendirme yönetiminde kullanıcı atamaları raporunu ve denetim günlerini nasıl görüntüleyebilirsiniz öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 514f8e86d6bd28cc5212e0f0058f00e270f43e35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128429"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde raporları ve günlükleri görüntüleme

Azure AD yetkilendirme yönetimi raporları ve Azure AD denetim günlüğü, kullanıcıların hangi kaynaklara erişebildikleri hakkında ek ayrıntılar sağlar. Yönetici olarak, bir kullanıcıiçin erişim paketlerini ve kaynak atamalarını görüntüleyebilir ve denetim amacıyla veya kullanıcının isteğinin durumunu belirlemek için istek günlüklerini görüntüleyebilirsiniz. Bu makalede, yetkilendirme yönetimi raporlarının ve Azure AD denetim günlüklerinin nasıl kullanılacağı açıklanmaktadır.

Yetkilendirme yönetiminde kullanıcıların hangi kaynaklara erişebildiğini görmek için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.youtube.com/embed/omtNJ7ySjS0]

## <a name="view-access-packages-for-a-user"></a>Bir kullanıcı için erişim paketlerini görüntüleme

Bu rapor, bir kullanıcının isteyebileceği tüm erişim paketlerini ve şu anda kullanıcıya atanan erişim paketlerini listelemenizi sağlar.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Raporlar'ı**tıklatın.

1. **Bir kullanıcı için Access paketlerine**tıklayın.

1. Kullanıcıları Seç bölmesini açmak için **kullanıcıları seç'i** tıklatın.

1. Listedeki kullanıcıyı bulun ve sonra **Seç'i**tıklatın.

    **Can isteği** sekmesi, kullanıcının isteyebileceği erişim paketlerinin listesini görüntüler. Bu liste, erişim paketleri için tanımlanan [istek ilkeleri](entitlement-management-access-package-request-policy.md#for-users-in-your-directory) tarafından belirlenir. 

    ![Bir kullanıcı için erişim paketleri](./media/entitlement-management-reports/access-packages-report.png)

1. Bir erişim paketi için birden fazla kaynak rolü veya ilke varsa, seçim ayrıntılarını görmek için kaynak rollerini veya ilke girişini tıklatın.

1. Kullanıcıya **Assigned** atanan erişim paketlerinin listesini görmek için Atananseksekmesi'ni tıklatın. Bir erişim paketi bir kullanıcıya atandığında, bu kullanıcının erişim paketindeki tüm kaynak rollerine erişebildiğim anlamına gelir.

## <a name="view-resource-assignments-for-a-user"></a>Bir kullanıcı için kaynak atamalarını görüntüleme

Bu rapor, yetkilendirme yönetiminde bir kullanıcıya atanan kaynakları listelemenizi sağlar. Bu raporun yetkilendirme yönetimiyle yönetilen kaynaklar için olduğunu unutmayın. Kullanıcı, yetkilendirme yönetimi dışında dizininizdeki diğer kaynaklara erişebilir.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Raporlar'ı**tıklatın.

1. **Bir kullanıcı için Kaynak atamaları'nı**tıklatın.

1. Kullanıcıları Seç bölmesini açmak için **kullanıcıları seç'i** tıklatın.

1. Listedeki kullanıcıyı bulun ve sonra **Seç'i**tıklatın.

    Şu anda kullanıcıya atanan kaynakların listesi görüntülenir. Liste ayrıca, erişim için başlangıç ve bitiş tarihiyle birlikte kaynak rolünü aldıkları erişim paketini ve ilkesini de gösterir.
    
    Bir kullanıcı iki veya daha fazla pakette aynı kaynağa erişebiliyorsa, her paketi ve ilkeyi görmek için bir oku tıklatabilirsiniz.

    ![Bir kullanıcı için kaynak atamaları](./media/entitlement-management-reports/resource-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Kullanıcı isteğinin durumunu belirleme

Bir kullanıcının bir erişim paketini nasıl istekte aldığı ve bu pakete nasıl eriştüğü hakkında ek bilgi almak için Azure AD denetim günlüğünü kullanabilirsiniz. Özellikle, her istek için işleme `EntitlementManagement` adımları `UserManagement` hakkında ek ayrıntılar almak için günlük kayıtlarını ve kategorilerini kullanabilirsiniz.  

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Denetim günlüklerini**tıklatın.

1. En üstte, Aradığınız denetim `EntitlementManagement` `UserManagement`kaydına bağlı olarak **Kategoriyi** ya da  

1. **Uygula**’ya tıklayın.

1. Günlükleri indirmek için **İndir'i**tıklatın.

Azure AD yeni bir istek aldığında, **Kategori'nin** olduğu `EntitlementManagement` ve **Etkinliğin** genellikle `User requests access package assignment`olduğu bir denetim kaydı yazar.  Azure portalında oluşturulan doğrudan bir atama durumunda, denetim kaydının `Administrator directly assigns user to access package` **Etkinlik** alanı dır ve atamayı gerçekleştiren kullanıcı **ActorUserPrincipalName**tarafından tanımlanır.

Azure AD, istek devam ederken aşağıdakiler de dahil olmak üzere ek denetim kayıtları yazar:

| Kategori | Etkinlik | İstek durumu |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | İstek onay gerektirmez |
| `UserManagement` | `Create request approval` | İstek onay gerektirir |
| `UserManagement` | `Add approver to request approval` | İstek onay gerektirir |
| `EntitlementManagement` | `Approve access package assignment request` | İstek onaylandı |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |İstek onaylandı veya onay gerektirmez |

Bir kullanıcıya erişim atandığında, Azure AD `EntitlementManagement` **Etkinlik** `Fulfill access package assignment`kategorisi için bir denetim kaydı yazar.  Erişimi alan kullanıcı **ActorUserPrincipalName** alanı tarafından tanımlanır.

Erişim atanmamışsa, Azure AD, isteğin `EntitlementManagement` bir onaylayıcı `Deny access package assignment request` `Access package assignment request timed out (no approver action taken)`tarafından reddedilmesi veya bir onaylayıcının onaylayabilmesi için istek zaman dolduysa, **Etkinlik** içeren kategori için bir denetim kaydı yazar.

Kullanıcının erişim paketi atamasının süresi dolduğunda, kullanıcı tarafından iptal edildiğinde veya bir yönetici tarafından kaldırıldığında, Azure `Remove access package assignment`AD etkinliği **olan** `EntitlementManagement` kategori için bir denetim kaydı yazar.

## <a name="next-steps"></a>Sonraki adımlar

- [Arşiv raporları ve Günlükler](entitlement-management-logs-and-reporting.md)
- [Azure AD yetkilendirme yönetimiyle ilgili sorun giderme](entitlement-management-troubleshoot.md)
- [Genel senaryolar](entitlement-management-scenarios.md)
