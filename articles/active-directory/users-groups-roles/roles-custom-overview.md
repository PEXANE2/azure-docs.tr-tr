---
title: Özelleştirilebilir izinlerle Azure yönetici rollerini önizleyin-Azure Active Directory | Microsoft Docs
description: Kimlik yönetimi temsilcisi seçme için özel Azure AD rollerini önizleyin. Azure portal, PowerShell veya Graph API Azure rollerini yönetin.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 546abdae5d7c03bb0d4b49f9485fe06b521fdc1d
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722206"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Azure Active Directory 'de özel yönetici rolleri (Önizleme)

Bu makalede, yeni özel RBAC (rol tabanlı erişim denetimi) ve Azure Active Directory (Azure AD) içindeki kaynak kapsamlarının nasıl anlaşılması açıklanmaktadır. Özel RBAC rolleri [yerleşik rollerin](directory-assign-admin-roles.md) temel izinlerini oluşturur, böylece kendi özel rollerinizi oluşturabilir ve düzenleyebilirsiniz. Kaynak kapsamları, bazı kaynakları (örneğin, bir uygulama) tüm kaynaklara (tüm uygulamalar) erişim vermeden yönetmek için özel rol atamanız için bir yol sağlar.

Özel RBAC rollerini kullanarak izin verilmesi iki adımlı bir işlemdir. İlk olarak, özel bir rol tanımı oluşturup önceden ayarlanmış listesinden buna izinleri eklersiniz. Bunlar, yerleşik rollerde kullanılan izinlerdir. Rolünüzü oluşturduktan sonra bir rol ataması oluşturarak bu kişiye atayabilirsiniz. Bu iki adımlı işlem, tek bir rol oluşturmanıza ve farklı kapsamlarda birçok kez atamanıza olanak tanır. Dizin kapsamında özel bir rol atanabilir veya bir nesne kapsamında atanabilir. Bir nesne kapsamına bir örnek, tek bir uygulama olabilir. Bu şekilde, aynı rol dizindeki tüm uygulamalar üzerinden Sally ve ardından yalnızca contoso gider raporları uygulamasının üzerinde yer alabilir.

Özel RBAC rollerinin bu ilk sürümü, uygulama kayıtlarının yönetilmesi için izin atamak üzere bir rol oluşturma özelliği içerir. Zamanla, kurumsal uygulamalar, kullanıcılar ve cihazlar gibi kuruluş kaynakları için ek izinler eklenecektir.

Önizleme özellikleri:

- Özel Roller oluşturmak ve yönetmek ve bunları kuruluş genelinde kapsama göre kullanıcılara atamak için Portal Kullanıcı Arabirimi güncelleştirmeleri
- Yeni cmdlet 'leri ile bir önizleme PowerShell modülü:
  - Özel roller oluşturma ve yönetme
  - Kuruluş genelinde veya uygulama başına kayıt kapsamıyla özel roller atama
  - Kuruluş genelindeki bir kapsamda yerleşik roller atama (GA cmdlet 'leriyle eşlik)
  - Azure AD Graph API desteği

Azure AD rol tabanlı erişim denetimi, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Azure AD rol tabanlı erişim denetimini anlama

Azure AD rol tabanlı erişim denetimi, yalnızca tek bir Azure AD kaynağı türünde izin verilen eylemlere izin vermek üzere özelleştirilmiş roller atamanıza olanak tanır. Azure AD rol tabanlı erişim, Azure rol tabanlı erişim denetimi (Azure kaynak erişimi için[Azure RBAC](../../role-based-access-control/overview.md) ) ile benzer kavramlar üzerinde çalışır, ancak Azure AD rol tabanlı erişim denetimi Microsoft Graph tabanlıdır ve azure RBAC Azure Resource Manager tabanlıdır. Ancak, her iki sistem kendi işlevlerini rol atamalarına dayandırın.

### <a name="role-assignments"></a>Rol atamaları

Azure AD rol tabanlı erişim denetimi kullanarak erişimi denetleme yönteminiz, izinleri zorlamak için kullanılan **rol atamaları**oluşturmaktır. Rol ataması üç öğeden oluşur:

- Güvenlik sorumlusu
- Rol tanımı
- Kaynak kapsamı

Erişim, rol ataması oluşturularak sağlanır ve rol ataması kaldırıldığında iptal edilir. Azure portal, Azure AD PowerShell ve Graph API kullanarak [rol atamaları oluşturabilirsiniz](roles-create-custom.md) . [Özel bir rol için atamaları](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)ayrı ayrı görüntüleyebilirsiniz.

Aşağıdaki diyagramda rol ataması örneği gösterilmektedir. Bu örnekte, Chris Green, SalesForce uygulaması kapsamında [Uygulama Yöneticisi](directory-assign-admin-roles.md#application-administrator) rolü atandı. Chris, farklı bir rol atamasının parçası olmadıkları müddetçe başka herhangi bir uygulamayı yönetmek için erişime sahip değildir.

![Rol ataması, izinlerin nasıl zorlanacağını ve üç bölümden oluşur](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Güvenlik sorumlusu

Güvenlik sorumlusu, Azure AD kaynaklarına erişim atanacak Kullanıcı veya hizmet sorumlusunu temsil eder. *Kullanıcı* , Azure Active Directory bir kullanıcı profiline sahip kişidir. *Hizmet sorumlusu* , uygulamalar veya hizmetler tarafından belırlı Azure AD kaynaklarına erişmek için kullanılan bir güvenlik kimliğidir.

Bir güvenlik sorumlusu, Kullanıcı adı ve parola veya sertifikayı temsil eden kullanıcı kimliğine benzer, ancak kullanıcı yerine bir uygulama veya hizmet için kullanılır.

### <a name="role"></a>Role

Rol tanımı veya rol, izin koleksiyonudur. Rol tanımı, oluşturma, okuma, güncelleştirme ve silme gibi Azure AD kaynaklarında gerçekleştirilebilecek işlemleri listeler. Azure AD 'de iki tür rol vardır:

- Microsoft tarafından oluşturulan ve değiştirilemeyen yerleşik roller. Genel yönetici yerleşik rolü tüm Azure AD kaynakları üzerinde tüm izinlere sahiptir.
- Kuruluşunuz tarafından oluşturulan ve yönetilen özel roller.

### <a name="scope"></a>`Scope`

Kapsam, belirli bir Azure AD kaynağı üzerinde izin verilen eylemlerin kısıtlamasıdır. Bir rol atadığınızda, bir eylem kapsamı tanımlayarak yöneticinin izin verilen eylemlerini sınırlamak için rolü özelleştirebilirsiniz. Örneğin, geliştiricilerinizin tüm uygulamaları tamamen yönetmesi gerekmiyorsa, yalnızca uygulama kayıtlarını yönetmesine izin vermek için Azure AD özel rollerini kullanabilirsiniz.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal, Azure AD PowerShell ve Graph API](roles-create-custom.md) kullanarak özel rol atamaları oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)
