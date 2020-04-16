---
title: Sorun giderme yönetim birimleri ve SSS - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde kapsamı kısıtlı olan izinlerin delegasyonuna yönetim birimlerini araştırma
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
ms.openlocfilehash: 5c9770b60edad6f956ae99c91b94a232cc48bbbd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428153"
---
# <a name="troubleshooting-and-faq-for-administrative-units-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki yönetim birimleri için sorun giderme ve SSS

Azure Etkin Dizini'nde (Azure AD) daha ayrıntılı yönetim denetimi için, kullanıcıları bir veya daha fazla yönetim birimiyle (AUs) sınırlı bir kapsamla Azure AD rolüne atayabilirsiniz. Ortak görevler için örnek PowerShell komut https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0dosyalarını .

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: İdari bir birim oluşturamayacağım**

**A:** Azure AD'de yalnızca bir Genel Yönetici veya Ayrıcalıklı Rol Yöneticisi yönetim birimi oluşturabilir. Yönetim birimi oluşturmaya çalışan kullanıcıya Global Administrator veya Ayrıcalıklı Rol Yöneticisi rolü atanmış olup olmadığını denetleyin.

**S: İdari birime bir grup ekledim, ancak grup üyeleri hala yönetim biriminde görünmüyor**

**A:** Yönetim birimine bir grup eklediğinizde, bu, grubun tüm üyelerinin yönetim birimine eklenmesiyle sonuçlanmaz. Kullanıcılar doğrudan yönetim birimine atanmalıdır.

**S: Ben sadece ekledi / idari birimin bir üyesi kaldırıldı ve hala UI gösteriliyor**

**A:** Bazen yönetim biriminin bir veya daha fazla üyesinin eklenmesi veya kaldırılmasının **işlenmesi, Yönetim birimleri** sayfasının altında yansıtmak için birkaç dakika sürebilir. İdari birimler altında yansıtılabilmek için birkaç dakika beklemeyi tercih edebilirsiniz. Alternatif olarak, doğrudan ilişkili kaynağın özelliklerine gidebilir ve eylemin tamamlanıp tamamlanmadığını görebilirsiniz. ABD'deki kullanıcılar ve gruplar hakkında daha fazla bilgi için, [bir kullanıcı için yönetim birimlerini listele](roles-admin-units-add-manage-users.md) ve bir grup için yönetim [birimlerini listele'](roles-admin-units-add-manage-groups.md)ye bakın.

**S: İdari bir birimde temsilci parola yöneticisi olarak, belirli bir kullanıcının parolasını sıfırlayamıyorum**

**A:** Bir yönetim birimi üzerinden atanan bir yönetici, parolayı yalnızca yönetim biriminize atanan kullanıcılar için sıfırlayabilirsiniz. Parola sıfırlama nın başarısız olduğu kullanıcının, rolün atandığı yönetim birimlerine ait olduğundan emin olun. Kullanıcı aynı yönetim birimine aitse ve kullanıcının parolasını hala sıfırlayamıyorsanız, kullanıcıya atanan rolleri denetleyin. Ayrıcalık yükselmesini önlemek için, yönetim birimi kapsamı na sahip yönetici, kuruluş genelinde bir role atanan bir kullanıcının parolasını sıfırlayamaz.

**S: İdari birimler neden gereklidir? Bir kapsam tanımlamak için güvenlik gruplarını kullanamaz mıydık?**

**A:** Güvenlik gruplarının varolan bir amacı ve yetkilendirme modeli var. Örneğin, bir Kullanıcı yöneticisi, Salesforce gibi uygulamalara erişimi yönetmek için grupları kullanmak gibi Azure REKLAM kuruluşundaki tüm güvenlik gruplarının üyeliğini yönetebilir. Kullanıcı yöneticisi, "kaynak gruplandırma" senaryolarını desteklemek üzere güvenlik grupları genişletildiyse sonucu oluşan delegasyon modelini yönetme yeteneğine sahip olmamalıdır. Windows Server Active Directory'deki Kuruluş Birimleri gibi yönetim birimleri, çok çeşitli dizin nesnelerinin kapsam yönetimine yönelik bir yol sağlamayı amaçlamaktadır. Güvenlik gruplarının kendileri kaynak kapsamlarının üyeleri olabilir. Bir yöneticinin yönetebileceği güvenlik grupları kümesini tanımlamak için güvenlik gruplarının kullanılması kafa karıştırıcı olabilir.

**S: Bir yönetim birimine grup eklemek ne anlama gelir?**

**A:** Bir yönetim birimine bir grup eklemek, grubun kendisini, aynı zamanda bu yönetici birimine de kapsamına giren herhangi bir Kullanıcı yöneticisinin yönetim kapsamına getirir. Yönetim biriminin kullanıcı yöneticileri, grubun adını ve üyeliğini yönetebilir. Kullanıcı yöneticisine yönetim birimi için grubun kullanıcılarını yönetme izni vermez (örneğin, parolalarını sıfırlama). Kullanıcı Yöneticisine kullanıcıları yönetme olanağı sağlamak için, kullanıcıların yönetim biriminin doğrudan üyesi olması gerekir.

**S: Bir kaynak (kullanıcı veya grup) birden fazla yönetim biriminin üyesi olabilir mi?**

**A:** Evet, bir kaynak birden fazla yönetim biriminin üyesi olabilir. Kaynak, kaynak üzerinde izinleri olan tüm kuruluş çapında ve yönetim birimi kapsamındaki yöneticiler tarafından yönetilebilir.

**S: B2C kuruluşlarında yönetim birimleri kullanılabilir mi?**

**A:** Hayır, B2C kuruluşları için yönetim birimleri kullanılamaz.

**S: İç içe yönetim birimleri desteklendi mi?**

**A:** İç içe olan yönetim birimleri desteklenmez.

**S: PowerShell ve Grafik API'sinde yönetim birimleri desteklenir mi?**

**Y:** Evet. PowerShell [cmdlet belgelerinde](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0-preview) ve [örnek komut dosyalarında](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0-preview)Yönetim Birimleri desteği bulunur ve [destek, administrativeUnit kaynak türü](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/administrativeunit)için Microsoft Graph'ta dır.

## <a name="next-steps"></a>Sonraki adımlar

- [Rollere genel bakış için kapsamı kısıtlayan yönetim birimleri](directory-administrative-units.md)
- [İdari birimleri yönetme](roles-admin-units-manage.md)