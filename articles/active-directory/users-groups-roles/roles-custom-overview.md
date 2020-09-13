---
title: Azure Active Directory 'de özel yönetici rolleri | Microsoft Docs
description: Kimlik yönetimi temsilcisi seçme için özel Azure AD rollerini önizleyin. Azure portal, PowerShell veya Graph API Azure rollerini yönetin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: overview
ms.date: 09/11/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e962bea893e8a658b876aabf417e64b11a31dfc1
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033515"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Azure Active Directory 'de özel yönetici rolleri (Önizleme)

Bu makalede, rol tabanlı erişim denetimi ve kaynak kapsamları ile Azure Active Directory (Azure AD) içinde Azure AD özel rollerinin nasıl anlaşılması açıklanmaktadır. Özel Azure AD rolleri, [yerleşik rollerin](directory-assign-admin-roles.md)temel alınan izinlerini yüzeye koyarak kendi özel rollerinizi oluşturabilir ve düzenleyebilirsiniz. Bu yaklaşım, gerektiğinde yerleşik rollerden daha ayrıntılı bir şekilde erişim vermenize olanak tanır. Azure AD özel rollerinin bu ilk sürümü, uygulama kayıtlarının yönetilmesi için izin atamak üzere bir rol oluşturma özelliğini içerir. Zamanla, kurumsal uygulamalar, kullanıcılar ve cihazlar gibi kuruluş kaynakları için ek izinler eklenecektir.  

Ayrıca, Azure AD özel rolleri, daha geleneksel kuruluş genelinde atamalara ek olarak, atamaları Kaynak temelinde destekler. Bu yaklaşım, tüm kaynaklara (tüm uygulama kayıtları) erişim vermeden bazı kaynakları (örneğin, bir uygulama kaydı) yönetmek için erişim izni vermenizi sağlar.

Azure AD rol tabanlı erişim denetimi, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Azure AD rol tabanlı erişim denetimini anlama

Özel Azure AD rolleri kullanarak izin verilmesi, özel bir rol tanımı oluşturmayı ve ardından rol ataması kullanarak atamayı kapsayan iki adımlı bir işlemdir. Özel bir rol tanımı, önceden ayarlanmış bir listeden eklediğiniz izinlerin koleksiyonudur. Bu izinler, yerleşik rollerde kullanılan izinlerdir.  

Rol tanımınızı oluşturduktan sonra bir rol ataması oluşturarak bunu bir kullanıcıya atayabilirsiniz. Rol ataması, kullanıcıya belirtilen kapsamdaki bir rol tanımında izinleri verir. Bu iki adımlı işlem, tek bir rol tanımı oluşturmanızı ve farklı kapsamlarda birçok kez atamanızı sağlar. Kapsam, rol üyesinin erişimi olan Azure AD kaynakları kümesini tanımlar. En yaygın kapsam, kuruluş genelinde (kuruluş genelinde) kapsamındadır. Özel bir rol kuruluş genelinde bir kapsamda atanabilir, Yani rol üyesi kuruluştaki tüm kaynaklar üzerinde rol izinlerine sahiptir. Özel bir rol, bir nesne kapsamına da atanabilir. Bir nesne kapsamına bir örnek, tek bir uygulama olabilir. Aynı rol, kuruluştaki tüm uygulamalarda bir kullanıcıya ve sonra yalnızca contoso gider raporları uygulamasının kapsamına sahip başka bir kullanıcıya atanabilir.  

Azure AD yerleşik ve özel rolleri, [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md)ile benzer kavramlar üzerinde çalışır. [Bu iki rol tabanlı erişim denetimi sistemi arasındaki fark](../../role-based-access-control/rbac-and-directory-admin-roles.md) , Azure RBAC 'Nin Azure Kaynak Yönetimi 'ni kullanarak sanal makineler veya depolama gibi Azure kaynaklarına erişimi denetlemesini ve Azure AD özel rollerinin Graph API kullanarak Azure AD kaynaklarına erişimini denetlerleridir. Her iki sistem de rol tanımları ve rol atamaları kavramından faydalanır.

### <a name="how-azure-ad-determines-if-a-user-has-access-to-a-resource"></a>Azure AD 'nin bir kullanıcının kaynağa erişip erişemeyeceğini belirleme

Aşağıda, Azure AD 'nin bir yönetim kaynağına erişiminizin olup olmadığını belirlemede kullandığı üst düzey adımlar verilmiştir. Erişim sorunlarını gidermek için bu bilgileri kullanın.

1. Bir Kullanıcı (veya hizmet sorumlusu) Microsoft Graph veya Azure AD Graph uç noktası için bir belirteç alır.

1. Kullanıcı, verilen belirteci kullanarak Microsoft Graph veya Azure AD Graph aracılığıyla Azure Active Directory (Azure AD) için bir API çağrısı yapar.

1. Durumunuza bağlı olarak, Azure AD aşağıdaki eylemlerden birini alır:

    - Kullanıcının erişim belirtecindeki [wids talebine](../develop/access-tokens.md) göre Kullanıcı rolü üyeliklerini değerlendirir.
    - Kullanıcı için, doğrudan veya grup üyeliği aracılığıyla, eylemin alındığı kaynağa uygulanan tüm rol atamalarını alır.

1. Azure AD, API çağrısındaki eylemin kullanıcının bu kaynak için sahip olduğu rollere dahil edilip edilmediğini belirler.
1. Kullanıcının istenen kapsamda eyleme sahip bir rolü yoksa erişime izin verilmez. Aksi takdirde erişim izni verilir.

### <a name="role-assignments"></a>Rol atamaları

Rol ataması, Azure AD kaynak erişimi sağlamak için belirli bir kapsamdaki bir kullanıcıya rol tanımı ekleyen nesnedir. Erişim, rol ataması oluşturularak sağlanır ve rol ataması kaldırıldığında iptal edilir. Bir rol ataması, temel tarafında üç öğeden oluşur:

- Kullanıcı (Azure Active Directory bir kullanıcı profiline sahip olan kişi)
- Rol tanımı
- Kaynak kapsamı

Azure portal, Azure AD PowerShell veya Graph API kullanarak [rol atamaları oluşturabilirsiniz](roles-create-custom.md) . [Özel bir rol için atamaları da görüntüleyebilirsiniz](roles-view-assignments.md#view-the-assignments-of-a-role).

Aşağıdaki diyagramda rol ataması örneği gösterilmektedir. Bu örnekte, Chris Green, contoso pencere öğesi Oluşturucu uygulama kaydı kapsamında uygulama kayıt yöneticisi özel rolü atandı. Atama, kemal 'e uygulama kayıt yöneticisi rolünün yalnızca bu belirli uygulama kaydı için izin verir.

![Rol ataması, izinlerin nasıl zorlanacağını ve üç bölümden oluşur](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Güvenlik sorumlusu

Güvenlik sorumlusu, Azure AD kaynaklarına erişim atanacak kullanıcıyı temsil eder. *Kullanıcı* , Azure Active Directory bir kullanıcı profiline sahip kişidir.

### <a name="role"></a>Rol

Rol tanımı veya rol, izin koleksiyonudur. Rol tanımı, oluşturma, okuma, güncelleştirme ve silme gibi Azure AD kaynaklarında gerçekleştirilebilecek işlemleri listeler. Azure AD 'de iki tür rol vardır:

- Microsoft tarafından oluşturulan ve değiştirilemeyen yerleşik roller.
- Kuruluşunuz tarafından oluşturulan ve yönetilen özel roller.

### <a name="scope"></a>Kapsam

Kapsam, rol atamasının bir parçası olarak belirli bir Azure AD kaynağına izin verilen eylemlerin kısıtlamasıdır. Bir rol atadığınızda, yöneticinin belirli bir kaynağa erişimini sınırlayan bir kapsam belirtebilirsiniz. Örneğin, bir geliştiriciye özel bir rol vermek istiyorsanız, ancak yalnızca belirli bir uygulama kaydını yönetmek için, belirli uygulama kaydını rol atamasında kapsam olarak dahil edebilirsiniz.

  > [!Note]
  > Özel roller, dizin kapsamı ve kaynak kapsamlı olarak atanabilir. Bunlar henüz yönetim birimi kapsamında atanamaz.
  > Yerleşik roller dizin kapsamında, bazı durumlarda ise yönetim birimi kapsamında atanabilir. Henüz Azure AD kaynak kapsamında atanamazlar.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal, Azure AD PowerShell ve Graph API](roles-create-custom.md) kullanarak özel rol atamaları oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md#view-assignments-of-single-application-scope)