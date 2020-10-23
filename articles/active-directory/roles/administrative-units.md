---
title: Azure Active Directory 'de yönetim birimleri | Microsoft Docs
description: Azure Active Directory izinlerin daha ayrıntılı olarak temsili için yönetim birimleri kullanma
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
ms.openlocfilehash: bc7f28f9369d54c65f633bd24da5443415a80625
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92379347"
---
# <a name="administrative-units-in-azure-active-directory"></a>Azure Active Directory 'de yönetim birimleri

Bu makalede Azure Active Directory (Azure AD) içindeki yönetim birimleri açıklanmaktadır. Yönetim birimi, diğer Azure AD kaynakları için bir kapsayıcı olabilecek bir Azure AD kaynağıdır. Yönetim birimi yalnızca kullanıcılar ve gruplar içerebilir.

Yönetim birimleri bir departman, bölge veya tanımladığınız kuruluşunuzun diğer kesimiyle kısıtlanan yönetici izinleri vermenizi sağlar. Bölgesel yöneticilerle izinleri devretmek veya ayrıntılı bir düzeyde ilke ayarlamak için yönetim birimlerini kullanabilirsiniz. Örneğin, bir kullanıcı hesabı Yöneticisi profil bilgilerini güncelleştirebilir, parolaları sıfırlayabilir ve yalnızca kendi yönetim biriminde kullanıcılar için lisans atayabilir.

Örneğin, bölgesel destek uzmanlarına temsilci seçme, [Yardım Masası yönetici](permissions-reference.md#helpdesk-administrator) rolü, yalnızca destekledikleri bölgedeki kullanıcıları yönetmeye kısıtlıdır.

## <a name="deployment-scenario"></a>Dağıtım senaryosu

Yönetim birimlerini kullanarak yönetim kapsamını kısıtlamak, her türlü bağımsız bölüm oluşturan kuruluşlarda yararlı olabilir. Birçok otonom okuldan (Iş Okulu, mühendisin Okulu vb.) oluşan büyük bir üniversite örneğini, her birinin erişimi denetleyen, kullanıcıları yöneten ve okulların ilkelerini ayarlayabilen bir BT yöneticileri takımına sahip olduğunu göz önünde bulundurun. Merkezi yönetici:

- İş okulunun yönetim birimindeki yalnızca Azure AD kullanıcıları üzerinde yönetici izinlerine sahip bir rol oluşturma
- Iş Okulu için yönetim birimi oluşturma
- Yönetici birimini yalnızca iş okul öğrencilerine ve personeline göre doldurun
- Iş Okulu BT ekibini, kapsamında role ekleyin

## <a name="license-requirements"></a>Lisans gereksinimleri

Yönetim birimlerinin kullanılması için her yönetim birimi Yöneticisi için bir Azure Active Directory Premium Lisansı ve yönetim birimi üyeleri için Azure Active Directory Ücretsiz Lisansları gerekir. Daha fazla bilgi için bkz. [Azure AD Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md)başlama.

## <a name="manage-administrative-units"></a>Yönetim birimlerini yönetme

Azure portal, PowerShell cmdlet 'leri ve betikleri ya da Microsoft Graph kullanarak yönetim birimlerini yönetebilirsiniz. Ayrıntılar için belgelerimize başvurabilirsiniz:

- [Yönetim birimlerine roller oluşturma, kaldırma, doldurma ve ekleme](admin-units-manage.md): nasıl yapılır yordamlarını doldurun
- [Yönetici birimleriyle çalışma](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true): PowerShell kullanarak yönetim birimleriyle çalışma
- [Yönetim birimi grafik desteği](/graph/api/resources/administrativeunit?view=graph-rest-1.0&preserve-view=true): yönetim birimleri için Microsoft Graph ayrıntılı belgeler.

### <a name="planning-your-administrative-units"></a>Yönetim birimlerinizi planlama

Yönetim birimleri, Azure AD kaynaklarını mantıksal olarak gruplamak için kullanılabilir. Örneğin, BT departmanı Global olarak dağınık olan bir kuruluş için, bu coğrafi sınırları tanımlayan yönetim birimleri oluşturmak mantıklı olabilir. Çok milli bir kuruluşun, işlemlerde yarı otonom olan farklı "alt kuruluşlar" olduğu başka bir senaryoda, her bir alt kuruluş bir yönetim birimiyle temsil edilebilir.

Yönetim birimlerinin oluşturulduğu ölçütler, bir kuruluşun benzersiz gereksinimlerine göre kılavuzluk eder. Yönetim birimleri Microsoft 365 hizmetleri genelinde yapıyı tanımlamanın yaygın bir yoludur. Yönetim birimlerinizi, Microsoft 365 hizmetleri genelinde kullanımı göz önüne alarak hazırlamanızı öneririz. Genel kaynakları bir yönetim birimi altında Microsoft 365 arasında ilişkilendirebileceğiniz zaman, yönetim birimlerinden maksimum değeri alabilirsiniz.

Aşağıdaki aşamaları izleyerek kuruluştaki yönetim birimlerinin oluşturulmasını bekleyebilir:

1. İlk benimseme: Kuruluşunuz, ilk ölçütlere göre yönetim birimleri oluşturmaya başlar ve ölçüt iyileştirilyerek yönetim birimlerinin sayısı artacaktır.
1. Ayıklama: ölçüt iyi tanımlandıktan sonra artık gerekli olmayan yönetim birimleri silinir.
1. Sabitlemeyi: Kurumsal yapınız iyi tanımlanmış ve yönetim birimlerinin sayısı kısa süreler boyunca önemli ölçüde değişmeyecek.

## <a name="currently-supported-scenarios"></a>Şu anda desteklenen senaryolar

Genel Yöneticiler veya ayrıcalıklı rol yöneticileri, yönetim birimleri oluşturmak, kullanıcıları yönetim birimlerinin üyesi olarak eklemek ve ardından BT personelini yönetim birimi kapsamlı yönetici rollerine atamak için Azure AD portalını kullanabilir. Yönetim birimi kapsamındaki Yöneticiler daha sonra yönetim birimlerindeki kullanıcıların temel yönetimi için Microsoft 365 Yönetim merkezini kullanabilir.

Ayrıca, gruplar yönetim biriminin üyeleri olarak eklenebilir ve yönetici birim kapsamlı Grup Yöneticisi bunları PowerShell, Microsoft Graph ve Azure AD portalını kullanarak yönetebilir.

Aşağıdaki tabloda, yönetim birimi senaryoları için geçerli destek açıklanmaktadır.

### <a name="administrative-unit-management"></a>Yönetim Birimi Yönetimi

İzinler |   MS Graph/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi
----------- | ----------------------- | --------------- | -----------------
Yönetim birimleri oluşturma ve silme   |    Desteklenir    |   Desteklenir   |    Desteklenmez
Yönetim birimi üyelerini tek tek ekleme ve kaldırma    |   Desteklenir    |   Desteklenir   |    Desteklenmez
. Csv dosyası kullanarak yönetim birimi üyelerini toplu ekleme ve kaldırma   |    Desteklenmez     |  Desteklenir   |    Destekedilecek plan yok
Yönetim birimi kapsamlı Yöneticiler atama  |     Desteklenir    |   Desteklenir    |   Desteklenmez
AU üyelerini dinamik olarak özniteliklere göre ekleme ve kaldırma | Desteklenmez | Desteklenmez | Desteklenmez

### <a name="user-management"></a>Kullanıcı yönetimi

İzinler |   MS Graph/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi
----------- | ----------------------- | --------------- | -----------------
Kullanıcı özelliklerinin, parolaların, lisansların yönetim birimi kapsamlı yönetimi   |    Desteklenir     |  Desteklenir   |   Desteklenir
yönetim birimi kapsamlı engelleme ve Kullanıcı oturum açma işlemleri engellemesini kaldırma    |   Desteklenir   |    Desteklenir   |    Desteklenir
Kullanıcı MFA kimlik bilgilerinin yönetim birimi kapsamlı yönetimi   |    Desteklenir   |   Desteklenir   |   Desteklenmez

### <a name="group-management"></a>Grup yönetimi

İzinler |   MS Graph/PowerShell   | Azure AD portalı | Microsoft 365 yönetici merkezi
----------- | ----------------------- | --------------- | -----------------
Grup özelliklerinin ve üyelerinin yönetim birimi kapsamlı yönetimi     |  Desteklenir   |    Desteklenir    |  Desteklenmez
Grup lisanslamanın yönetim birimi kapsamlı yönetimi   |    Desteklenir  |    Desteklenir   |   Desteklenmez


Yönetim birimleri kapsamı yalnızca yönetim izinlerine uygular. Üyelerin veya yöneticilerin, yönetim birimi dışındaki diğer kullanıcılara, gruplara veya kaynaklara gözatmaları için [Varsayılan Kullanıcı izinlerini](../fundamentals/users-default-permissions.md) kullanmalarını engellemez. Microsoft 365 Yönetim merkezinde, kapsamlı yönetici yönetim birimlerinin dışındaki kullanıcılar filtrelenmektedir, ancak Azure AD portalındaki, PowerShell ve diğer Microsoft hizmetlerinde diğer kullanıcılara da gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Avustralya yönetimi](admin-units-manage.md)
- [Avustralya 'da kullanıcıları yönetme](admin-units-add-manage-users.md)
- [Avustralya 'da grupları yönetme](admin-units-add-manage-groups.md)
- [Bir AU 'ya kapsamlı roller atama](admin-units-assign-roles.md)
