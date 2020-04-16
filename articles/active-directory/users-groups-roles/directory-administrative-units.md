---
title: Yönetim birimleri yönetimi (önizleme) - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde daha ayrıntılı izin ler delegasyonu için yönetim birimlerini kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12101b170066bc52e75f4aa281c6f1c21283c004
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406448"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Azure Active Directory'de yönetim birimleri yönetimi (önizleme)

Bu makalede, Azure Etkin Dizin (Azure AD) yönetim birimleri açıklanmaktadır. Yönetim birimi, diğer Azure REKLAM kaynakları için kapsayıcı olabilecek bir Azure REKLAM kaynağıdır. Bu önizleme sürümünde, yönetim birimi yalnızca kullanıcıları ve grupları içerebilir.

Yönetim birimleri, kuruluşunuzun tanımladığınız bir bölümü, bölgesi veya başka bir kesimiyle sınırlı yönetici izinleri vermenize olanak sağlar. İzinleri bölge yöneticilerine devretmek veya ilkeyi parçalı düzeyde ayarlamak için yönetim birimlerini kullanabilirsiniz. Örneğin, bir Kullanıcı hesabı yöneticisi profil bilgilerini güncelleyebilir, parolaları sıfırlayabilir ve yalnızca yönetim birimindeki kullanıcılar için lisansatabilir.

 Örneğin, bölgesel destek uzmanlarına atanan [Helpdesk Administrator](directory-assign-admin-roles.md#helpdesk-administrator) rolü, yalnızca destek verdikleri bölgedeki kullanıcıları yönetmekle sınırlıdır.

## <a name="deployment-scenario"></a>Dağıtım senaryosu

İdari birimleri kullanarak yönetim kapsamının kısıtlanması, her türlü bağımsız bölümden oluşan kuruluşlarda yararlı olabilir. Her birinin erişimi kontrol eden, kullanıcıları yöneten ve okulları için politikalar belirleyen BT yöneticilerinden oluşan bir ekibine sahip olduğu birçok özerk okuldan (School of Business, School of Engineering vb.) oluşan büyük bir üniversite örneğini düşünün. Merkezi bir yönetici şunları yapabilir:

- İşletme fakültesi yönetim biriminde yalnızca Azure AD kullanıcıları üzerinde yönetim izinleriyle bir rol oluşturma
- İşletme Fakültesi için bir idari birim oluşturma
- Yönetici birimini yalnızca işletme okulu öğrencileri ve personeliyle doldurun
- İş okulu BT ekibini kapsamlarıyla role ekleyin

## <a name="license-requirements"></a>Lisans gereksinimleri

Yönetim birimlerinin kullanılması için her yönetim birimi yöneticisi için bir Azure Active Directory Premium lisansı ve yönetim birimi üyeleri için Azure Active Directory Free lisansları gerektirir. Daha fazla bilgi için Azure [AD Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md)bakın.

## <a name="manage-administrative-units"></a>İdari birimleri yönetme

Bu önizleme sürümünde, Azure portalını, PowerShell cmdlets ve komut dosyalarını veya Microsoft Graph'ı kullanarak yönetim birimlerini yönetebilirsiniz. Ayrıntılar için belgelerimize bakabilirsiniz:

- [Yönetim birimlerine rol oluşturma, kaldırma, doldurma ve ekleme](roles-admin-units-manage.md): Nasıl yapılır işlemlerini tamamla
- [Yönetici Birimleri ile Çalışma](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0): PowerShell kullanarak yönetim birimleri ile nasıl çalışIlir
- [Yönetim Birimi Grafiği desteği](https://docs.microsoft.com/graph/api/resources/administrativeunit?view=graph-rest-beta): İdari birimler için Microsoft Graph'ta ayrıntılı dokümantasyon.

### <a name="planning-your-administrative-units"></a>İdari birimlerinizi planlama

Yönetim birimleri, Azure REKLAM kaynaklarını mantıksal olarak gruplandırmak için kullanılabilir. Örneğin, BT departmanı genel olarak dağılmış bir kuruluş için, bu coğrafi sınırları tanımlayan yönetim birimleri oluşturmak mantıklı olabilir. Çok uluslu bir kuruluşun operasyonlarda yarı özerk olan farklı "alt kuruluşlara" sahip olduğu başka bir senaryoda, her alt kuruluş bir yönetim birimi tarafından temsil edilebilir.

Yönetim birimlerinin oluşturulduğu ölçütler, bir kuruluşun benzersiz gereksinimleri tarafından yönlendirilir. Yönetim Birimleri, M365 hizmetleri genelinde yapıyı tanımlamanın yaygın bir yoludur. İdari birimlerinizi M365 hizmetleri nde kullanımı göz önünde bulundurularak hazırlamanızı öneririz. M365 genelindeki ortak kaynakları bir yönetim birimi altında ilişkilendirebildiğinizde, yönetim birimlerinden maksimum değer elde edebilirsiniz.

Kuruluşta yönetim birimlerinin oluşturulmasının aşağıdaki aşamalardan geçmesini bekleyebilirsiniz:

1. İlk Benimseme: Kuruluşunuz ilk ölçütlere göre yönetim birimleri oluşturmaya başlayacak ve ölçütler rafine edildikçe yönetim birimlerinin sayısı artacaktır.
1. Budama: Ölçütler iyi tanımlandıktan sonra, artık gerekli olmayan yönetim birimleri silinir.
1. Stabilizasyon: Organizasyon yapınız iyi tanımlanmıştır ve idari birimlerin sayısı kısa süreler içinde önemli ölçüde değişmeyecek.

## <a name="currently-supported-scenarios"></a>Şu anda desteklenen senaryolar

Genel yöneticiler veya Ayrıcalıklı rol yöneticileri, yönetim birimleri oluşturmak, kullanıcıları yönetim birimlerinin üyesi olarak eklemek ve bt personelini yönetim birimi kapsamlı yönetici rollerine atamak için Azure AD portalını kullanabilir. İdari birim kapsamlı yöneticiler, daha sonra yönetim birimlerindeki kullanıcıların temel yönetimi için Office 365 portalını kullanabilir.

Ayrıca, gruplar yönetim biriminin üyeleri olarak eklenebilir ve yönetici birimi kapsamlı grup yöneticisi bunları PowerShell, Microsoft Graph ve Azure AD portalını kullanarak yönetebilir.

Aşağıdaki tabloda yönetim birimi senaryoları için geçerli destek açıklanmaktadır.

### <a name="administrative-unit-management"></a>İdari birim yönetimi

İzinler |   MS Grafiği/PowerShell   | Azure AD portalı | Microsoft 365 yönetim merkezi
----------- | ----------------------- | --------------- | -----------------
Yönetim birimleri oluşturma ve silme   |    Destekleniyor    |   Destekleniyor   |    Desteklenmiyor
İdari birim üyelerinin tek tek eklenmesi ve kaldırılması    |   Destekleniyor    |   Destekleniyor   |    Desteklenmiyor
.csv dosyalarını kullanarak yönetim birimi üyelerini toplu ekleme ve kaldırma   |    Desteklenmiyor     |  Destekleniyor   |    Destek için bir plan yok
İdari birim kapsamlı yöneticilerin atanması  |     Destekleniyor    |   Destekleniyor    |   Desteklenmiyor
Özniteliklere dayalı olarak AÜ üyelerini dinamik olarak ekleme ve kaldırma | Desteklenmiyor | Desteklenmiyor | Desteklenmiyor

### <a name="user-management"></a>Kullanıcı yönetimi

İzinler |   MS Grafiği/PowerShell   | Azure AD portalı | Microsoft 365 yönetim merkezi
----------- | ----------------------- | --------------- | -----------------
kullanıcı özellikleri, şifreler, lisanslar idari birim kapsamlı yönetimi   |    Destekleniyor     |  Destekleniyor   |   Destekleniyor
yönetim birimi kapsamlı engelleme ve kullanıcı oturum açma engellemelerinin engellenmesi    |   Destekleniyor   |    Destekleniyor   |    Destekleniyor
kullanıcı MFA kimlik bilgilerinin yönetim birimi kapsamlı yönetimi   |    Destekleniyor   |   Destekleniyor   |   Desteklenmiyor

### <a name="group-management"></a>Grup yönetimi

İzinler |   MS Grafiği/PowerShell   | Azure AD portalı | Microsoft 365 yönetim merkezi
----------- | ----------------------- | --------------- | -----------------
grup özelliklerinin ve üyelerinin yönetim birimi kapsamlı yönetimi     |  Destekleniyor   |    Destekleniyor    |  Desteklenmiyor
grup lisanslama nın yönetim birimi kapsamlı yönetimi   |    Destekleniyor  |    Destekleniyor   |   Desteklenmiyor

> [!NOTE]
>
> Yönetim birimi kapsamına sahip yöneticiler dinamik grup üyelik kurallarını yönetemez.

Yönetim birimleri kapsamı yalnızca yönetim izinlerine uygular. Üyelerin veya yöneticilerin, yönetim birimi dışındaki diğer kullanıcılara, gruplara veya kaynaklara göz atmak için [varsayılan kullanıcı izinlerini](../fundamentals/users-default-permissions.md) kullanmalarını engellemezler. Office 365 portalında, kapsamlı bir yöneticinin yönetim birimlerinin dışındaki kullanıcılar filtrelenir, ancak Azure AD portalı, PowerShell ve diğer Microsoft hizmetlerindeki diğer kullanıcılara göz atabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [AUs'u Yönetme](roles-admin-units-manage.md)
- [ABD'deki kullanıcıları yönetme](roles-admin-units-add-manage-users.md)
- [AUs'taki grupları yönetme](roles-admin-units-add-manage-groups.md)
- [Kapsamlı rolleri BIR AU'ya atama](roles-admin-units-assign-roles.md)