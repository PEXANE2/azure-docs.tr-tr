---
title: B2B işbirliğini ve B2C'yi karşılaştırın - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği ile Azure AD B2C arasındaki fark nedir?
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 07/22/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e7f5aa324db869e30a8b2fe214416129baca8d9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68380739"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Azure Active Directory’de B2B işbirliğini ve B2C’yi karşılaştırma

Hem Azure Active Directory (Azure AD) B2B işbirliği hem de Azure AD B2C, Azure AD’de dış kullanıcılarla çalışmanıza olanak sağlar. O halde bunların benzerlikleri ve farkları nelerdir?

**Azure AD B2B**, işbirliği yapabilmeleri için harici kullanıcılarla dosya ve kaynakları güvenli şekilde paylaşmak isteyen işletmeler içindir. Azure yöneticisi, Azure portalda B2B’yi ayarlar ve Azure AD, işletmeniz ile şirket dışındaki iş ortağınız arasındaki federasyon işleminden yararlanır. Kullanıcılar, iş veya okul hesabı ya da herhangi bir e-posta hesabı ile basit bir davet ve kullanım işlemini kullanarak paylaşılan kaynaklarda oturum açar.
 
**Azure AD B2C**, öncelikle müşteriye yönelik uygulamalar oluşturan işletmeler ve geliştiriciler içindir. Azure AD B2C ile geliştiriciler, bir yandan müşterilerin önceden belirlediği bir kimlikle (Facebook veya Gmail gibi) oturum açmasını sağlarken diğer yandan uygulamaları için tam özellikli kimlik sistemi olarak Azure AD’yi de kullanabilir.

Aşağıdaki tabloda ayrıntılı bir karşılaştırma sağlanmıştır.


B2B işbirliği özellikleri |     Azure AD B2C bağımsız teklifi
-------- | --------
Hedeflenen: Kimlik sağlayıcısına bakılmaksızın bir üst kuruluştaki kullanıcıların kimliğini doğrulayabilmek isteyen kuruluşlar. | Hedeflenen: İster birey, isterse kurumsal müşteriler olsun, mobil ve web uygulamalarınızın müşterilerini Azure AD’ye davet etme.
Desteklenen kimlikler: İş veya okul hesapları olan çalışanlar, iş ya da okul hesapları veya herhangi bir e-posta adresi olan iş ortakları. Yakında doğrudan federasyon desteklenecektir.  | Desteklenen kimlikler: Yerel uygulama hesapları (herhangi bir e-posta adresi veya kullanıcı adı) ya da doğrudan federasyon özellikli herhangi bir desteklenen sosyal kimliği olan tüketici kullanıcılar.
Dış kullanıcılar çalışanlarla aynı dizinde yönetilir, ancak özel olarak açıklamalı olarak açıklanır. Çalışanlarla aynı şekilde yönetilebilirler, aynı gruplara eklenebilirler ve böylece  | Dış kullanıcılar uygulama dizininde yönetilir. Bunlar, kuruluşun çalışan ve iş ortağı dizininden ayrı olarak yönetilir (varsa).
Tüm Azure AD ile bağlantılı uygulamalarda çoklu oturum açma (SSO) desteklenir. Örneğin, Office 365 veya şirket içi uygulamalara ve Salesforce ya da Workday gibi diğer SaaS uygulamalarına erişim sağlayabilirsiniz.  |  Azure AD B2C kiracılarında müşterilere ait uygulamalarda SSO desteklenir. SSO'dan Office 365'e veya diğer Microsoft SaaS uygulamalarına desteklenmez.
İş ortağı yaşam döngüsü: Ana bilgisayar/davet eden kuruluş tarafından yönetilir.  | Müşteri yaşam döngüsü: Self servistir veya uygulama tarafından yönetilir.
Güvenlik ilkesi ve uyumluluk: Ana bilgisayar/davet eden kuruluş tarafından yönetilir (örneğin, [Koşullu Erişim ilkeleriyle).](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)  | Güvenlik ilkesi ve uyumluluk: Uygulama tarafından yönetilir.
Marka: Ana bilgisayar/davet eden kuruluşun markası kullanılır.  |    Marka: Uygulama tarafından yönetilir. Genellikle kuruluşun arka planda soluk olduğu şekilde ürün markalı olma eğilimindedir.
Daha fazla bilgi: [Blog gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Belgeler](what-is-b2b.md)  | Daha fazla bilgi: [Ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [Belgeler](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

