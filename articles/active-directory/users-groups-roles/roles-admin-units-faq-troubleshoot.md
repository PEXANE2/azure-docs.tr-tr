---
title: İdari birimler sorun giderme ve SSS - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde sınırlı kapsama sahip izinler vermek için yönetim birimlerini araştırın.
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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684851"
---
# <a name="azure-ad-administrative-units-troubleshooting-and-faq"></a>Azure AD yönetim birimleri: Sorun giderme ve SSS

Azure Etkin Dizini'nde (Azure AD) daha ayrıntılı yönetim denetimi için, kullanıcıları bir veya daha fazla yönetim birimiyle (AUs) sınırlı bir kapsamda bir Azure REKLAM rolüne atayabilirsiniz. Ortak görevler için örnek PowerShell komut dosyaları için [bkz.](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Neden bir yönetim birimi oluşturamıyorum?**

**A:** Azure AD'de yalnızca bir *Genel Yönetici* veya Ayrıcalıklı *Rol Yöneticisi* yönetim birimi oluşturabilir. Yönetim birimi oluşturmaya çalışan kullanıcıya Global Administrator veya *Ayrıcalıklı Rol* *Yöneticisi* rolü atanmış olduğundan emin olun.

**S: İdari birime bir grup ekledim. Grup üyeleri neden hala orada görünmüyor?**

**A:** Yönetim birimine bir grup eklediğinizde, bu tüm grubun üyelerinin eklenmesine neden olmaz. Kullanıcılar doğrudan yönetim birimine atanmalıdır.

**S: İdari birimin bir üyesini ekledim (veya kaldırdım). Üye neden kullanıcı arabiriminde görünmüyor (veya hala görünmüyor)**

**A:** Bazen, yönetim biriminin bir veya daha fazla üyesinin eklenmesi veya kaldırılmasının **işlenmesi, Yönetim birimleri** sayfasına yansıtılması birkaç dakika sürebilir. Alternatif olarak, doğrudan ilişkili kaynağın özelliklerine gidebilir ve eylemin tamamlanıp tamamlanmadığını görebilirsiniz. ABD'deki kullanıcılar ve gruplar hakkında daha fazla bilgi için, [bir kullanıcı için yönetim birimlerini listele](roles-admin-units-add-manage-users.md) ve bir grup için yönetim [birimlerini listele'](roles-admin-units-add-manage-groups.md)ye bakın.

**S: İdari bir birimde temsilci parola yöneticisiyim. Belirli bir kullanıcının parolasını neden sıfırlayamıyorum?**

**A:** Bir yönetim biriminin yöneticisi olarak, parolaları yalnızca yönetim biriminize atanan kullanıcılar için sıfırlayabilirsiniz. Parola sıfırlama hatası olan kullanıcının, atandığınız yönetim birimine ait olduğundan emin olun. Kullanıcı aynı yönetim birimine aitse ancak parolasını yine de sıfırlayamazsa, kullanıcıya atanan rolleri denetleyin. 

Ayrıcalık yükselmesini önlemek için, yönetim birimi kapsamlı bir yönetici, kuruluş genelinde bir role atanan bir kullanıcının parolasını sıfırlayamaz.

**S: İdari birimler neden gereklidir? Bir kapsam tanımlamak için güvenlik gruplarını kullanamaz mıydık?**

**A:** Güvenlik gruplarının varolan bir amacı ve yetkilendirme modeli var. Örneğin, Bir *Kullanıcı Yöneticisi*Azure REKLAM kuruluşundaki tüm güvenlik gruplarının üyeliğini yönetebilir. Rol, Salesforce gibi uygulamalara erişimi yönetmek için grupları kullanabilir. *Kullanıcı Yöneticisi* delegasyon modelinin kendisini yönetememelidir ve bu da güvenlik gruplarının "kaynak gruplandırma" senaryolarını destekleyecek şekilde genişletilmesi durumunda ortaya çıkan sonuç olur. Windows Server Active Directory'deki kuruluş birimleri gibi yönetim birimleri, çok çeşitli dizin nesnelerinin kapsam yönetimine yönelik bir yol sağlamayı amaçlamaktadır. Güvenlik gruplarının kendileri kaynak kapsamlarının üyeleri olabilir. Bir yöneticinin yönetebileceği güvenlik grupları kümesini tanımlamak için güvenlik gruplarını kullanmak kafa karıştırıcı olabilir.

**S: Bir yönetim birimine grup eklemek ne anlama gelir?**

**A:** Bir yönetim birimine bir grup eklemek, grubun kendisini, aynı zamanda bu yönetim birimine de kapsamına giren herhangi bir *Kullanıcı Yöneticisinin* yönetim kapsamına getirir. Yönetim biriminin kullanıcı yöneticileri, grubun adını ve üyeliğini yönetebilir. *Kullanıcı* Yöneticisi'ne, grup kullanıcılarını yönetmesi için yönetim birimi izinleri vermez (örneğin, parolalarını sıfırlamak için). *Kullanıcı Yöneticisine* kullanıcıları yönetme olanağı sağlamak için, kullanıcıların yönetim biriminin doğrudan üyesi olması gerekir.

**S: Bir kaynak (kullanıcı veya grup) birden fazla yönetim biriminin üyesi olabilir mi?**

**A:** Evet, bir kaynak birden fazla yönetim biriminin üyesi olabilir. Kaynak, kaynak üzerinde izinleri olan tüm kuruluş çapında ve yönetim birimi kapsamlı yöneticiler tarafından yönetilebilir.

**S: B2C kuruluşlarında yönetim birimleri kullanılabilir mi?**

**A:** Hayır, B2C kuruluşları için yönetim birimleri kullanılamaz.

**S: İç içe yönetim birimleri desteklendi mi?**

**A:** Hayır, iç içe olan yönetim birimleri desteklenmez.

**S: PowerShell ve Grafik API'sinde yönetim birimleri desteklenir mi?**

**Y:** Evet. [PowerShell cmdlet dokümantasyon](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0-preview) ve [örnek komut dosyaları](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0-preview)idari birimler için destek bulacaksınız. 

Microsoft Graph'ta [administrativeUnit kaynak türü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/administrativeunit) için destek bulun.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimleri kullanarak rollerin kapsamını kısıtlama](directory-administrative-units.md)
- [İdari birimleri yönetme](roles-admin-units-manage.md)
