---
title: Azure Active Directory 'de yönetim birimleri | Microsoft Docs
description: Azure Active Directory izinlerin daha ayrıntılı şekilde temsili için yönetim birimleri kullanın.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0fb24d0cd2714969b5a888b1036f524c4c062d76
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "93027627"
---
# <a name="administrative-units-in-azure-active-directory"></a>Azure Active Directory 'de yönetim birimleri

Bu makalede Azure Active Directory (Azure AD) içindeki yönetim birimleri açıklanmaktadır. Yönetim birimi, diğer Azure AD kaynakları için bir kapsayıcı olabilecek bir Azure AD kaynağıdır. Yönetim birimi yalnızca kullanıcılar ve gruplar içerebilir.

Yönetim birimlerini kullanarak, tanımladığınız bir departmanla, bölgeye veya kuruluşunuzun diğer kesimine kısıtlı yönetici izinleri verebilirsiniz. Bölgesel yöneticilerle izinleri devretmek veya ayrıntılı bir düzeyde ilke ayarlamak için yönetim birimlerini kullanabilirsiniz. Örneğin, bir kullanıcı hesabı Yöneticisi profil bilgilerini güncelleştirebilir, parolaları sıfırlayabilir ve yalnızca kendi yönetim biriminde kullanıcılar için lisans atayabilir.

Örneğin, kullanıcıların yalnızca destekledikleri bölgede yönetilmesi kısıtlanan, [Yardım Masası yönetici](permissions-reference.md#helpdesk-administrator) rolü bölgesel destek uzmanlarına temsilci seçebilirsiniz.

## <a name="deployment-scenario"></a>Dağıtım senaryosu

Her türlü bağımsız bölüm oluşturan kuruluşlarda yönetim birimlerini kullanarak yönetim kapsamını kısıtlamak yararlı olabilir. Birçok otonom okuldan (Iş Okulu, mühendisin Okulu vb.) oluşan büyük bir üniversite örneğini göz önünde bulundurun. Her okulda, erişimi denetleyen, kullanıcıları yöneten ve okullarına yönelik ilkeler kuran bir BT yöneticileri ekibi vardır. 

Merkezi yönetici:

- İş okul yönetim birimindeki yalnızca Azure AD kullanıcıları üzerinde yönetici izinlerine sahip bir rol oluşturun.
- Iş Okulu için bir yönetim birimi oluşturun.
- Yönetim birimini yalnızca iş okul öğrencilerine ve personeline göre doldurun.
- İş Okulu BT ekibini, kapsamıyla birlikte rolüne ekleyin.

## <a name="license-requirements"></a>Lisans gereksinimleri

Yönetim birimlerini kullanmak için, her yönetim birimi Yöneticisi için bir Azure Active Directory Premium lisansa ve yönetim birimi üyeleri için Azure Active Directory Ücretsiz lisanslarına sahip olmanız gerekir. Daha fazla bilgi için bkz. [Azure AD Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md)başlama.

## <a name="manage-administrative-units"></a>Yönetim birimlerini yönetme

Azure portal, PowerShell cmdlet 'leri ve betikleri veya Microsoft Graph kullanarak yönetim birimlerini yönetebilirsiniz. Daha fazla bilgi için bkz.

- [Yönetim birimlerine roller oluşturma, kaldırma, doldurma ve ekleme](admin-units-manage.md): tüm nasıl yapılır yordamlarını içerir.
- [Yönetim birimleriyle çalışma](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true): PowerShell kullanarak yönetim birimleriyle çalışmayı ele alır.
- [Yönetim birimi grafik desteği](/graph/api/resources/administrativeunit?view=graph-rest-1.0&preserve-view=true): yönetim birimleri için Microsoft Graph ayrıntılı belgeler sağlar.

### <a name="plan-your-administrative-units"></a>Yönetim birimlerinizi planlayın

Azure AD kaynaklarını mantıksal olarak gruplandırabilmeniz için yönetim birimlerini kullanabilirsiniz. Örneğin, BT departmanı Global olarak dağınık olan bir kuruluş için, bu coğrafi sınırları tanımlayan yönetim birimleri oluşturmak mantıklı olabilir. Çok uluslu bir kuruluşun işlemlerinde yarı otonom olan çeşitli *alt kuruluşların* bulunduğu başka bir senaryoda, yönetim birimi her bir alt kuruluşu temsil edebilir.

Yönetim birimlerinin oluşturulduğu ölçütler, bir kuruluşun benzersiz gereksinimlerine göre yapılır. Yönetim birimleri Microsoft 365 hizmetleri genelinde yapıyı tanımlamanın yaygın bir yoludur. Yönetim birimlerinizi, Microsoft 365 hizmetleri genelinde kullanımı göz önüne alarak hazırlamanızı öneririz. Genel kaynakları bir yönetim birimi altında Microsoft 365 arasında ilişkilendirebileceğiniz zaman, yönetim birimlerinden maksimum değeri alabilirsiniz.

Aşağıdaki aşamaları izleyerek kuruluştaki yönetim birimlerinin oluşturulmasını bekleyebilir:

1. **İlk benimseme** : Kuruluşunuz, ilk ölçütlere göre yönetim birimleri oluşturmaya başlar ve ölçütler iyileştirilyerek yönetim birimlerinin sayısı artacaktır.
1. **Ayıklama** : ölçütler iyi tanımlandıktan sonra artık gerekli olmayan yönetim birimleri silinir.
1. **Sabitlemeyi** : Kurumsal yapınız iyi tanımlanmış ve kısa dönemde yönetim birimlerinin sayısı önemli ölçüde değişmeyeceklerdir.

## <a name="currently-supported-scenarios"></a>Şu anda desteklenen senaryolar

Genel yönetici veya ayrıcalıklı rol yöneticisi olarak, yönetim birimleri oluşturmak, kullanıcıları yönetim birimlerinin üyesi olarak eklemek ve ardından BT personelini yönetim birimi kapsamlı yönetici rollerine atamak için Azure AD portalını kullanabilirsiniz. Yönetim birimi kapsamlı Yöneticiler daha sonra yönetim birimlerindeki kullanıcıların temel yönetiminde Microsoft 365 Yönetim Merkezi 'ni kullanabilir.

Ayrıca, grupları bir yönetim biriminin üyesi olarak ekleyebilirsiniz. Yönetim birimi kapsamlı bir Grup Yöneticisi, PowerShell, Microsoft Graph ve Azure AD portalını kullanarak bunları yönetebilir.

Aşağıdaki tablolar, yönetim birimi senaryoları için geçerli desteği anlatmaktadır:

### <a name="administrative-unit-management"></a>Yönetim Birimi Yönetimi

| İzinler |   Grafik/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi | 
| -- | -- | -- | -- |
| Yönetim birimleri oluşturma ve silme   |    Desteklenir    |   Desteklenir   |    Desteklenmez | 
| Yönetim birimi üyelerini tek tek ekleme ve kaldırma    |   Desteklenir    |   Desteklenir   |    Desteklenmez | 
| CSV dosyalarını kullanarak yönetim birimi üyelerini toplu olarak ekleme ve kaldırma   |    Desteklenmez     |  Desteklenir   |    Destekedilecek plan yok | 
| Yönetim birimi kapsamlı Yöneticiler atama  |     Desteklenir    |   Desteklenir    |   Desteklenmez | 
| Öznitelikleri temel alarak yönetim birimi üyelerini dinamik olarak ekleme ve kaldırma | Desteklenmez | Desteklenmez | Desteklenmez 

### <a name="user-management"></a>Kullanıcı yönetimi

| İzinler |   Grafik/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi |
| -- | -- | -- | -- |
| Kullanıcı özelliklerinin, parolalarının ve lisansların yönetim birimi kapsamlı yönetimi   |    Desteklenir     |  Desteklenir   |   Desteklenir |
| Yönetim birimi kapsamlı engelleme ve Kullanıcı oturum açma işlemleri engellemesini kaldırma    |   Desteklenir   |    Desteklenir   |    Desteklenir |
| Kullanıcı çok faktörlü kimlik doğrulaması kimlik bilgilerinin yönetim birimi kapsamlı yönetimi   |    Desteklenir   |   Desteklenir   |   Desteklenmez |

### <a name="group-management"></a>Grup yönetimi

| İzinler |   Grafik/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi |
| -- | -- | -- | -- |
| Grup özelliklerinin ve üyelerinin yönetim birimi kapsamlı yönetimi     |  Desteklenir   |    Desteklenir    |  Desteklenmez |
| Grup lisanslamanın yönetim birimi kapsamlı yönetimi   |    Desteklenir  |    Desteklenir   |   Desteklenmez |


Yönetim birimleri kapsamı yalnızca yönetim izinlerine uygular. Üyelerin veya yöneticilerin, yönetim birimi dışındaki diğer kullanıcılara, gruplara veya kaynaklara gözatmaları için [Varsayılan Kullanıcı izinlerini](../fundamentals/users-default-permissions.md) kullanmalarını engellemez. Microsoft 365 Yönetim merkezinde, kapsamlı yönetici yönetim birimlerinin dışındaki kullanıcılar filtrelenmez. Ancak Azure AD portalı, PowerShell ve diğer Microsoft hizmetlerinde diğer kullanıcılara da gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimlerini yönetme](admin-units-manage.md)
- [Yönetici birimlerindeki kullanıcıları yönetme](admin-units-add-manage-users.md)
- [Grupları yönetim birimlerinde yönetme](admin-units-add-manage-groups.md)
- [Yönetim birimine kapsamlı roller atama](admin-units-assign-roles.md)
