---
title: Yönetim birimleri sorun giderme ve SSS-Azure Active Directory | Microsoft Docs
description: Azure Active Directory, kısıtlanmış kapsama sahip izinler vermek için yönetim birimlerini araştırın.
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
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 022658306d6e4d69174cc616d230cfe4892f1204
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81684851"
---
# <a name="azure-ad-administrative-units-troubleshooting-and-faq"></a>Azure AD yönetim birimleri: sorun giderme ve SSS

Azure Active Directory (Azure AD) içinde daha ayrıntılı yönetim denetimi için, bir veya daha fazla yönetim birimiyle (AU) sınırlı bir kapsama sahip bir Azure AD rolüne kullanıcı atayabilirsiniz. Ortak görevlere yönelik örnek PowerShell betikleri için bkz. [çalışma, yönetim birimleriyle çalışma](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: neden yönetim birimi oluşturamıyorum?**

Y **:** Yalnızca bir *genel yönetici* veya *ayrıcalıklı rol YÖNETICISI* , Azure AD 'de bir yönetim birimi oluşturabilir. Yönetim birimini oluşturmaya çalışan kullanıcının *genel yönetici* ya da *ayrıcalıklı rol yöneticisi* rolüne atandığından emin olun.

**S: yönetim birimine bir grup ekledim. Grup üyeleri neden hala gösterilmiyor?**

Y **:** Yönetim birimine bir grup eklediğinizde, bu, gruba eklenen tüm üyelerin oluşmasına neden olmaz. Kullanıcıların doğrudan yönetim birimine atanması gerekir.

**S: yönetici biriminin bir üyesini yeni ekledim (veya kaldırdım). Üye Kullanıcı arabiriminde neden gösterilmiyor (veya hala gösterilmiyor)?**

Y **:** Bazen, yönetim biriminin bir veya daha fazla üyesinin eklenmesi veya kaldırılması işleminin işlenmesi, **yönetim birimleri** sayfasında yansıtılmayı birkaç dakika sürebilir. Alternatif olarak, doğrudan ilişkili kaynağın özelliklerine gidebilir ve eylemin tamamlanıp tamamlanmadığını görebilirsiniz. Au 'daki kullanıcılar ve gruplar hakkında daha fazla bilgi için bkz. [bir kullanıcının yönetim birimlerini listeleme](roles-admin-units-add-manage-users.md) ve [bir grup Için yönetim birimlerini listeleme](roles-admin-units-add-manage-groups.md).

**S: bir yönetim biriminde Temsilcili bir parola yöneticisidir. Belirli bir kullanıcının parolasını neden sıfırlayamıyorum?**

Y **:** Bir yönetim biriminin Yöneticisi olarak, yalnızca yönetim biriminizdeki kullanıcılara atanan kullanıcılar için parolaları sıfırlayabilirsiniz. Parola sıfırlama başarısız olan kullanıcının, atandığı yönetim birimine ait olduğundan emin olun. Kullanıcı aynı yönetim birimine aitse ancak parolasını sıfırlamadıysanız, kullanıcıya atanan rolleri kontrol edin. 

Ayrıcalık yükselmesine engel olmak için, yönetim birimi kapsamlı bir yönetici, kuruluş genelinde bir kapsama sahip bir role atanan kullanıcının parolasını sıfırlayabilir.

**S: yönetim birimleri neden gereklidir? Kapsam tanımlama yolu olarak güvenlik grupları kullanılamadı mu?**

Y **:** Güvenlik grupları, mevcut bir amaç ve yetkilendirme modeline sahiptir. Örneğin, bir *Kullanıcı Yöneticisi*, Azure AD kuruluşundaki tüm güvenlik gruplarının üyeliğini yönetebilir. Rol, Salesforce gibi uygulamalara erişimi yönetmek için grupları kullanabilir. Bir *Kullanıcı Yöneticisi* , yetkilendirme modelinin kendisini yönetebilmelidir, bu da güvenlik grupları "kaynak gruplandırması" senaryolarını destekleyecek şekilde genişletilmişse sonuç olur. Windows Server Active Directory 'daki kuruluş birimleri gibi yönetim birimlerinin, çok çeşitli dizin nesnelerinin kapsamını yönetmek için bir yol sağlaması amaçlanmıştır. Güvenlik grupları, kaynak kapsamlarının üyesi olabilir. Yöneticinin yönetebileceği güvenlik grupları kümesini tanımlamak için güvenlik gruplarının kullanılması kafa karıştırıcı hale gelebilir.

**S: yönetim birimine bir grup eklemek ne anlama geliyor?**

Y **:** Bir yönetim birimine Grup eklemek, grubun kendisini o yönetim birimi kapsamındaki herhangi bir *Kullanıcı yöneticisinin* yönetim kapsamına taşır. Yönetim birimi için Kullanıcı yöneticileri, grubun adını ve üyeliğini yönetebilir. Yönetici birimi izinleri için *Kullanıcı yöneticisinin* grup kullanıcılarını (örneğin parolalarını sıfırlamasına) yönetmesine izin vermez. *Kullanıcı yöneticisine* kullanıcıları yönetme olanağı vermek için kullanıcıların doğrudan yönetim biriminin üyesi olması gerekir.

**S: bir kaynak (Kullanıcı veya grup) birden fazla yönetim biriminin üyesi olabilir mi?**

Y **:** Evet, kaynak birden fazla yönetim biriminin üyesi olabilir. Kaynak, kaynak üzerinde izinlere sahip olan tüm kuruluş genelindeki ve yönetim birimi kapsamlı yöneticilerle yönetilebilir.

**S: B2C kuruluşları 'nda yönetim birimleri kullanılabilir mi?**

Y **:** Hayır, B2C kuruluşları için yönetim birimleri kullanılamaz.

**S: iç içe geçmiş yönetim birimleri destekleniyor mu?**

Y **:** Hayır, iç içe geçmiş yönetim birimleri desteklenmez.

**S: PowerShell ve Graph API desteklenen yönetim birimleri var mı?**

**Y:** Evet. [PowerShell cmdlet belgeleri](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0-preview) ve [örnek betiklerdeki](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0-preview)yönetim birimleri için destek bulacaksınız. 

Microsoft Graph ' de [Yönetimtiveunit kaynak türü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/administrativeunit) için destek bulun.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimlerini kullanarak roller için kapsamı kısıtlama](directory-administrative-units.md)
- [Yönetim birimlerini yönetme](roles-admin-units-manage.md)
