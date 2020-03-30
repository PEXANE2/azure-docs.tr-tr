---
title: Azure Etkin Dizini'nde özel yönetici rolleri | Microsoft Dokümanlar
description: Kimlik yönetimini atamak için özel Azure REKLAM rollerini önizleyin. Azure portalında, PowerShell'de veya Grafik API'sinde Azure rollerini yönetin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5c7919dcc89e34831cb4cae7921b60b35eb4c69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74024958"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Azure Etkin Dizini'nde özel yönetici rolleri (önizleme)

Bu makalede, rollere dayalı erişim denetimi ve kaynak kapsamlarıyla Azure Etkin Dizini'ndeki (Azure AD) Azure AD özel rollerinin nasıl anlayacağı açıklanmaktadır. Özel Azure AD rolleri, kendi özel rollerinizi oluşturabilmeniz ve düzenleyebilmeniz için [yerleşik rollerin](directory-assign-admin-roles.md)temel izinlerini ortaya çıkar. Bu yaklaşım, gerektiğinde yerleşik rollerden daha ayrıntılı bir şekilde erişim izni vermenize olanak tanır. Azure AD özel rollerinin bu ilk sürümü, uygulama kayıtlarını yönetmek için izin atamak için bir rol oluşturma olanağı içerir. Zaman içinde, kurumsal uygulamalar, kullanıcılar ve aygıtlar gibi kuruluş kaynakları için ek izinler eklenir.  

Ayrıca, Azure AD özel rolleri, daha geleneksel kuruluş çapındaki atamalara ek olarak atamaları kaynak başına destekler. Bu yaklaşım, tüm kaynaklara (tüm uygulama kayıtları) erişim vermeden bazı kaynakları (örneğin, bir uygulama kaydı) yönetmeye erişim izni verme olanağı sağlar.

Azure AD rol tabanlı erişim denetimi, Azure AD'nin genel önizleme özelliğidir ve herhangi bir ücretli Azure AD lisans planında kullanılabilir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="understand-azure-ad-role-based-access-control"></a>Azure AD rol tabanlı erişim denetimini anlama

Özel Azure AD rollerini kullanarak izin vermek, özel bir rol tanımı oluşturmayı ve sonra bir rol ataması kullanarak atamayı içeren iki adımlı bir işlemdir. Özel rol tanımı, önceden ayarlanmış bir listeden eklediğiniz izinler topluluğudur. Bu izinler, yerleşik rollerde kullanılan izinlerle aynıdır.  

Rol tanımınızı oluşturduktan sonra, bir rol ataması oluşturarak bu tanımı kullanıcıya atayabilirsiniz. Rol ataması, kullanıcıya belirli bir kapsamdaki rol tanımındaki izinleri verir. Bu iki adımlı işlem, tek bir rol tanımı oluşturmanıza ve farklı kapsamlarda birçok kez atamanıza olanak tanır. Kapsam, rol üyesinin erişebilmek te olduğu Azure REKLAM kaynakları kümesini tanımlar. En yaygın kapsam kuruluş çapında (org çapında) kapsamdır. Özel bir rol org çapında kapsamda atanabilir, bu da rol üyesinin kuruluştaki tüm kaynaklar üzerinde rol izinlerine sahip olduğu anlamına gelir. Özel bir rol nesne kapsamında da atanabilir. Nesne kapsamıörneği tek bir uygulama olacaktır. Aynı rol, kuruluştaki tüm uygulamalar üzerinden bir kullanıcıya ve daha sonra yalnızca Contoso Gider Raporları uygulaması kapsamına sahip başka bir kullanıcıya atanabilir.  

Azure AD yerleşik ve özel roller, [Azure rol tabanlı erişim denetimine](../../role-based-access-control/overview.md)benzer kavramlarüzerinde çalışır. [Bu iki rol tabanlı erişim denetim sistemi arasındaki fark,](../../role-based-access-control/rbac-and-directory-admin-roles.md) Azure RBAC'ın Azure Kaynak Yönetimi'ni kullanarak sanal makineler veya depolama gibi Azure kaynaklarına erişimi denetlemesi ve Azure AD özel rolleri Grafik API'sini kullanarak Azure REKLAM kaynaklarına erişimi denetlemesidir. Her iki sistem de rol tanımları ve rol atamaları kavramından yararlanır.

### <a name="role-assignments"></a>Rol atamaları

Rol ataması, Azure AD kaynak erişimi sağlamak için belirli bir kapsamdaki bir kullanıcıya rol tanımı ekleyen nesnedir. Erişim, rol ataması oluşturularak sağlanır ve rol ataması kaldırıldığında iptal edilir. Özünde, bir rol ataması üç öğeden oluşur:

- Kullanıcı (Azure Active Directory'de kullanıcı profili olan bir kişi)
- Rol tanımı
- Kaynak kapsamı

Azure portalını, Azure AD PowerShell'i veya Grafik API'sini kullanarak [rol atamaları oluşturabilirsiniz.](roles-create-custom.md) Özel [bir rol için atamaları](roles-view-assignments.md#view-the-assignments-of-a-role)da görüntüleyebilirsiniz.

Aşağıdaki diyagramda rol ataması örneği gösterilmektedir. Bu örnekte, Chris Green Contoso Widget Builder uygulama kaydı kapsamında Uygulama kayıt yöneticisi özel rolü atanmıştır. Atama, Chris'e yalnızca bu özel uygulama kaydı için Uygulama kayıt yöneticisi rolünün izinlerini verir.

![Rol ataması, izinlerin nasıl uygulandığı ve üç bölümden oluşur](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Güvenlik sorumlusu

Güvenlik ilkesi, Azure AD kaynaklarına erişim atanacak kullanıcıyı temsil eder. *Kullanıcı,* Azure Etkin Dizin'de kullanıcı profiliolan bir kişidir.

### <a name="role"></a>Rol

Rol tanımı veya rol, izinler topluluğudur. Rol tanımı, oluşturma, okuma, güncelleştirme ve silme gibi Azure AD kaynaklarında gerçekleştirilebilecek işlemleri listeler. Azure AD'de iki tür rol vardır:

- Microsoft tarafından oluşturulan ve değiştirilemeyecek yerleşik roller.
- Kuruluşunuz tarafından oluşturulan ve yönetilen özel roller.

### <a name="scope"></a>Kapsam

Kapsam, rol atamasının bir parçası olarak belirli bir Azure REKLAM kaynağına izin verilen eylemlerin kısıtlanmasıdır. Bir rol atadığınızda, yöneticinin belirli bir kaynağa erişimini sınırlayan bir kapsam belirtebilirsiniz. Örneğin, bir geliştiriciye özel bir rol vermek, ancak yalnızca belirli bir uygulama kaydını yönetmek istiyorsanız, rol atamasına kapsam olarak belirli uygulama kaydını ekleyebilirsiniz.

  > [!Note]
  > Özel roller dizin kapsamı ve kaynak kapsamı içinde atanabilir. İdari Birim kapsamında henüz atanamazlar.
  > Yerleşik roller dizin kapsamında atanabilir ve bazı durumlarda, Yönetim Birimi kapsamı. Azure AD kaynak kapsamında henüz atanamazlar.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı, Azure AD PowerShell ve Grafik API'sini](roles-create-custom.md) kullanarak özel rol atamaları oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md#view-assignments-of-single-application-scope)
