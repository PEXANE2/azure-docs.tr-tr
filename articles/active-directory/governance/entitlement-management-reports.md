---
title: Azure AD Yetkilendirme Yönetimi 'nde raporları ve günlükleri görüntüleme (Önizleme)-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) Kullanıcı atamaları raporu ve denetim günlüklerini görüntülemeyi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: ajburnle
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: d33b4751b421f5af1536af9a88d15e060ab59bdb
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489071"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde raporları ve günlükleri görüntüleme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="view-resources-a-user-has-access-to"></a>Kullanıcının erişimi olan kaynakları görüntüleme

1. **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **Kullanıcı atamaları raporu**' na tıklayın.

1. Kullanıcıları Seç bölmesini açmak için **Kullanıcıları Seç** ' e tıklayın.

1. Erişimi olan kaynakları görüntülemek istediğiniz kullanıcıyı listede bulun.

1. Kullanıcıya tıklayın ve ardından **Seç**' e tıklayın.

    Kullanıcının erişimi olan kaynakların listesi görüntülenir. Erişim paketi, ilke ve tarihleri içerir.

    ![Kullanıcı atamaları raporu](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Kullanıcının isteğinin durumunu belirleme

Bir kullanıcının erişim paketine erişim isteği ve erişimi alma hakkında daha fazla bilgi edinmek için Azure AD denetim günlüğünü kullanabilirsiniz. Özellikle, `EntitlementManagement` ve `UserManagement` kategorilerindeki günlük kayıtlarını, her istek için işleme adımlarıyla ilgili ek ayrıntılar almak için kullanabilirsiniz.  

1. **Azure Active Directory** ' a ve sonra **Denetim günlükleri**' ne tıklayın.

1. En üstte, aradığınız denetim kaydına  bağlı olarak kategoriyi `EntitlementManagement` veya `UserManagement`ya da olarak değiştirin.  

1. **Uygula**'ya tıklayın.

1. Günlükleri indirmek için **İndir**' e tıklayın.

Azure AD yeni bir istek aldığında, **kategorinin** `EntitlementManagement` olduğu ve **etkinliğin** genellikle `User requests access package assignment`olduğu bir denetim kaydı yazar.  Azure Portal doğrudan atama durumunda, denetim kaydının **etkinlik** alanı olur `Administrator directly assigns user to access package`ve atamayı gerçekleştiren kullanıcı **actoruserprincipalname**tarafından tanımlanır.

Azure AD, istek sürerken aşağıdakiler de dahil olmak üzere ek denetim kayıtları yazacak:

| Category | Etkinlik | İstek durumu |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | İstek onay gerektirmiyor |
| `UserManagement` | `Create request approval` | İstek onay gerektiriyor |
| `UserManagement` | `Add approver to request approval` | İstek onay gerektiriyor |
| `EntitlementManagement` | `Approve access package assignment request` | İstek onaylandı |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |İstek onaylandı veya onay gerektirmiyor |

Bir kullanıcıya erişim atandığında Azure AD, `EntitlementManagement` **etkinlik** `Fulfill access package assignment`içeren kategori için bir denetim kaydı yazar.  Erişimi alan Kullanıcı **Actoruserprincipalname** alanı tarafından tanımlanır.

Erişim atanmamışsa, Azure AD, etkinlik bir onaylayan `EntitlementManagement` tarafından reddedildiyse veya `Access package assignment request timed out (no approver action taken)`bir onaylayanın onaylanmadan önce zaman `Deny access package assignment request`aşımına uğradığından, **etkinlik** içeren kategori için bir denetim kaydı yazar.

Kullanıcının erişim paketi atamasının süresi dolarsa, Kullanıcı tarafından iptal edilir veya bir yönetici tarafından kaldırıldıktan sonra Azure AD, `EntitlementManagement` **etkinliği** `Remove access package assignment`olan kategori için bir denetim kaydı yazar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yetkilendirme yönetimi sorunlarını giderme](entitlement-management-troubleshoot.md)
- [Yaygın senaryolar](entitlement-management-scenarios.md)
