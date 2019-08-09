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
ms.openlocfilehash: eabf29b10814d19e89c21f27ec66fce5355c9bfb
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880705"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Azure Active Directory 'de özel yönetici rolleri (Önizleme)

Bu makalede, Azure Active Directory (Azure AD) içinde yeni özel rol tabanlı erişim denetimi (RBAC) ve kaynak kapsamlarının nasıl anlaşılması açıklanmaktadır. Özel RBAC rolleri [yerleşik rollerin](directory-assign-admin-roles.md) temel izinlerini oluşturur, böylece kendi özel rollerinizi oluşturabilir ve düzenleyebilirsiniz. Bu yaklaşım, gerektiğinde yerleşik rollerden daha ayrıntılı bir şekilde erişim vermenize olanak tanır. Özel RBAC rollerinin bu ilk sürümü, uygulama kayıtlarının yönetilmesi için izin atamak üzere bir rol oluşturma özelliği içerir. Zamanla, kurumsal uygulamalar, kullanıcılar ve cihazlar gibi kuruluş kaynakları için ek izinler eklenecektir.  

Ayrıca, özel RBAC rolleri, daha geleneksel kuruluş genelinde atamalara ek olarak, atamaları Kaynak temelinde destekler. Bu yaklaşım, tüm kaynaklara (tüm uygulama kayıtları) erişim vermeden bazı kaynakları (örneğin, bir uygulama kaydı) yönetmek için erişim izni vermenizi sağlar.

Azure AD rol tabanlı erişim denetimi, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Azure AD rol tabanlı erişim denetimini anlama

Özel RBAC rolleri kullanarak izin verilmesi, özel bir rol tanımı oluşturmayı ve ardından rol ataması kullanarak atamayı kapsayan iki adımlı bir işlemdir. Özel bir rol tanımı, önceden ayarlanmış bir listeden eklediğiniz izinlerin koleksiyonudur. Bu izinler, yerleşik rollerde kullanılan izinlerdir.  

Rol tanımınızı oluşturduktan sonra bir rol ataması oluşturarak bu kişiye atayabilirsiniz. Rol ataması, birine belirli bir kapsamdaki rol tanımındaki izinleri verir. Bu iki adımlı işlem, tek bir rol tanımı oluşturmanızı ve farklı kapsamlarda birçok kez atamanızı sağlar. Kapsam, rol üyesinin erişimi olan kaynak kümesini tanımlar. En yaygın kapsam, kuruluş genelinde (kuruluş genelinde) kapsamındadır. Özel bir rol kuruluş genelinde bir kapsamda atanabilir, Yani rol üyesi kuruluştaki tüm kaynaklar üzerinde rol izinlerine sahiptir. Özel bir rol, bir nesne kapsamına da atanabilir. Bir nesne kapsamına bir örnek, tek bir uygulama olabilir. Bu şekilde, kuruluştaki tüm uygulamalar üzerinden Sally ' ye ve ardından yalnızca contoso gider raporları uygulamasının üzerine eklenebilir.  

Azure AD RBAC, [Azure rol tabanlı erişim denetimine](../../role-based-access-control/overview.md)benzer kavramlar üzerinde çalışır. Azure RBAC, sanal makineler ve Web siteleri gibi Azure kaynaklarına erişimi denetler ve Azure AD RBAC, Azure AD 'ye erişimi denetler. Her iki sistem de rol tanımları ve rol atamaları kavramından faydalanır.

### <a name="role-assignments"></a>Rol atamaları

Rol ataması, erişim vermek amacıyla belirli bir kapsamdaki kullanıcıya bir rol tanımı ekleme işlemidir. Erişim, rol ataması oluşturularak sağlanır ve rol ataması kaldırıldığında iptal edilir. Rol ataması üç öğeden oluşur:
- Kullanıcı
- Rol tanımı
- Kaynak kapsamı

Azure portal, Azure AD PowerShell veya Graph API kullanarak [rol atamaları oluşturabilirsiniz](roles-create-custom.md) . [Özel bir rol için atamaları da görüntüleyebilirsiniz](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).

Aşağıdaki diyagramda rol ataması örneği gösterilmektedir. Bu örnekte, Chris Green, contoso pencere öğesi Oluşturucu uygulama kaydı kapsamında uygulama kayıt yöneticisi özel rolü atandı. Bu atama, kemal 'e uygulama kayıt yöneticisi rolünün yalnızca bu belirli uygulama kaydında izin verir.

![Rol ataması, izinlerin nasıl zorlanacağını ve üç bölümden oluşur](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Güvenlik sorumlusu

Güvenlik sorumlusu, Azure AD kaynaklarına erişim atanacak kullanıcıyı temsil eder. *Kullanıcı* , Azure Active Directory bir kullanıcı profiline sahip kişidir.

### <a name="role"></a>Role

Rol tanımı veya rol, izin koleksiyonudur. Rol tanımı, oluşturma, okuma, güncelleştirme ve silme gibi Azure AD kaynaklarında gerçekleştirilebilecek işlemleri listeler. Azure AD 'de iki tür rol vardır:

- Microsoft tarafından oluşturulan ve değiştirilemeyen yerleşik roller. Genel yönetici yerleşik rolü tüm Azure AD kaynakları üzerinde tüm izinlere sahiptir.
- Kuruluşunuz tarafından oluşturulan ve yönetilen özel roller.

### <a name="scope"></a>`Scope`

Kapsam, rol atamasının bir parçası olarak belirli bir Azure AD kaynağına izin verilen eylemlerin kısıtlamasıdır. Bir rol atadığınızda, yöneticinin belirli bir kaynağa erişimini sınırlayan bir kapsam belirtebilirsiniz. Örneğin, bir geliştiriciye özel bir rol vermek istiyorsanız, ancak yalnızca belirli bir uygulama kaydını yönetmek için, belirli uygulama kaydını rol atamasında kapsam olarak dahil edebilirsiniz.

  > [!Note]
  > Özel roller, dizin kapsamı ve kaynak kapsamlı olarak atanabilir. Bunlar henüz yönetim birimi kapsamında atanamaz.
  > Yerleşik roller dizin kapsamında ve bazı durumlarda yönetim birimi kapsamında atanabilir. Nesne kapsamında henüz atanamazlar.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal, Azure AD PowerShell ve Graph API](roles-create-custom.md) kullanarak özel rol atamaları oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)
