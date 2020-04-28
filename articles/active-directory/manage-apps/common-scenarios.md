---
title: Azure Active Directory için genel uygulama yönetimi senaryoları | Microsoft Docs
description: Azure AD ile uygulama yönetimini merkezileştirme
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/02/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21caec50bf9c20ec60194976b1bfbf16e4815914
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81261049"
---
# <a name="centralize-application-management-with-azure-ad"></a>Azure AD ile uygulama yönetimini merkezileştirme

Parolalar, hem It nightmhem de dünyanın her yerindeki çalışanlar için bir sorun. Bunun nedeni, Microsoft 'un bulut ve diğer tüm kaynaklarınız için kimlik ve erişim yönetimi çözümü olan Azure Active Directory daha fazla şirket, Microsoft 'un kimlik ve erişim yönetimi çözümünü de kapatıyor. Her biri için bir parola girmek zorunda kalmadan uygulamadan uygulamaya atlayın. Outlook 'tan Workday 'e, hızlı ve güvenli bir şekilde açabileceğiniz en hızlı şekilde ADP 'ye atlayın. Daha sonra iş ortaklarıyla işbirliği yapmak zorunda kalmadan iş ortakları ve hatta diğer kullanıcılarla işbirliği yapın. Azure AD, ne olursa olsun, ihtiyacınız olan uygulamalara güvenli erişim sağlayan şüpheli oturum açma işlemlerini tespit etmek için sürekli Uyarlamalı makine öğrenimi ve güvenlik zekasını kullanarak, sizin tarafınızdan nerede olduğunu doğrulamak üzere çok faktörlü kimlik doğrulaması gibi şeyler ile, kullandığınız uygulamaların güvenliğini sağlamaya yardımcı olur. Yalnızca kullanıcılar için de harika değildir. Tam zamanında erişim İncelemeleri ve tam ölçekli bir idare paketi sayesinde Azure AD, uyumluluk ve ilkeleri zorunlu tutmanıza yardımcı olur. Bunun yanı sıra, sağlama Kullanıcı hesapları da otomatikleştirerek, erişim yönetiminin bir Breeze olmasını sağlayabilirsiniz. müşterinin için Azure Active Directory Uygulama yönetimi özelliklerini kullanmasının bazı yaygın senaryolarına göz atın.

**Yaygın senaryolar**


> [!div class="checklist"]
> * Tüm uygulamalarınız için SSO
> * Sağlamayı otomatik hale getirme ve sağlamayı kaldırma 
> * Uygulamalarınızın güvenliğini sağlama
> * Uygulamalarınıza erişimi yönetir
> * Karma güvenli erişim

## <a name="scenario-1-set-up-sso-for-all-your-applications"></a>Senaryo 1: tüm uygulamalarınız için SSO 'yu ayarlama

Daha fazla parola yönetilme. Şirket kimlik bilgilerinizle ihtiyacınız olan tüm kaynaklara güvenli bir şekilde erişin. 

|Özellik  | Açıklama | Öneri |
|---------|---------|---------|
|SSO|Güvenilen sektör standartları kullanarak standartlara dayalı Federasyon SSO 'SU.|Uygulamanız destekliyorsa, SSO 'yu etkinleştirmek için her zaman [SAML/OıDC](https://docs.microsoft.com/azure/active-directory/manage-apps/isv-choose-multi-tenant-federation) kullanın.|
|Erişim paneli|Kullanıcılarınıza tüm uygulamalarını keşfetmek ve bunlara erişmek için basit bir hub sunun. Uygulamalar ve gruplar için erişim isteğinde bulunma veya diğerleri adına kaynaklara erişimi yönetme gibi self servis özellikleri ile daha üretken olmalarını sağlayın.| Uygulamalarınızı SSO için Azure AD ile tümleştirdikten sonra [erişim panelini](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-deployment-plan) kuruluşunuza dağıtın.|

## <a name="scenario-2-automate-provisioning-and-deprovisioning"></a>Senaryo 2: sağlamayı ve sağlamayı kaldırmayı otomatikleştirme 


Çoğu uygulama, ihtiyaç duydukları kaynaklara erişmeden önce bir kullanıcının uygulamaya sağlanması gerekir. CSV dosyalarını veya karmaşık betikleri kullanmak maliyetli ve yönetimi zor olabilir. Ayrıca, müşterilerin artık erişimleri olmaması durumunda hesapların kaldırılmasını sağlamak gerekir. Sağlama ve sağlamayı kaldırma işlemlerini otomatikleştirmek için aşağıdaki araçlardan yararlanın. 


|Özellik  |Açıklama|Öneri |
|---------|---------|---------|
|SCıM sağlama|[SCIM](https://aka.ms/SICMOverview) , Kullanıcı sağlamasını otomatikleştirmek için sektör açısından en iyi uygulamadır. Herhangi bir SCıM uyumlu uygulama, Azure AD ile tümleştirilebilir. CSV dosyalarını, özel betikleri veya şirket içi çözümleri sürdürmek zorunda kalmadan kullanıcı hesaplarını otomatik olarak oluşturun, güncelleştirin ve silin.|Azure AD uygulama galerisinde [önceden tümleştirilmiş](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) uygulamalar listesini gözden geçirin|
|Microsoft Graph|Azure AD 'nin uygulamanızı, ihtiyaç duyacağı verilerle zenginleştirmesini sağlayan verilerin genişliği ve derinliğine yararlanın.|Microsoft ekosistemi üzerinden veri almak için [Microsoft Graph](https://developer.microsoft.com/graph/) 'tan yararlanın. |


## <a name="scenario-3-secure-your-applications"></a>Senaryo 3: uygulamalarınızın güvenliğini sağlama
Kimlik, güvenlik için linchpın 'dir. Bir kimlik tehlikeye atılırsa, çok geç olmadan önce Domino efektini durdurmak inanılmaz zordur. Kuruluşların bir güvenliğinin aşılmasına neden olduğunu fark etmeden önce 100 gün üzerinden geçen süre. Uygulamalarınızın güvenlik duruşunu geliştirmek için Azure AD tarafından sunulan araçları kullanın. 

|Özellik  |Açıklama| Öneri |
|---------|---------| ---------|
|Azure MFA|Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Azure MFA, yönetici onaylı kimlik doğrulama yöntemlerini kullanarak, basit bir oturum açma işlemi talebini karşılarken verilerinize ve uygulamalarınıza erişimi korumaya yardımcı olur.| Kullanıcılarınız için [MFA 'Yı etkinleştirin](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124) .  |
|Koşullu Erişim|Koşullu erişimle, koşullara göre bulut uygulamalarınıza kimlerin erişebileceği otomatik erişim denetimi kararları uygulayabilirsiniz.| Müşterilerin kullandığı [güvenlik varsayılanlarını](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) ve [ortak ilkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common) gözden geçirin. | 
|Kimlik Koruması|Kimlik koruması, Microsoft 'un kuruluşların konumlarından Azure AD, Microsoft hesaplarıyla tüketici alanı ve Xbox ile oyun oynarken kullanıcılarınızı koruduğu dersleri kullanır. Microsoft analizler 6.500.000.000.000 sinyalleri, müşterileri tespit etmek ve tehditlere karşı korumak için günde.|Hizmetimizin tarafından sunulan [varsayılan kimlik koruması ilkelerini](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-policies) etkinleştirin. | 

## <a name="scenario-4-govern-access-to-your-applications"></a>Senaryo 4: uygulamalarınıza erişimi yönetir
Kimlik yönetimi, kuruluşların üretkenlik arasında bir denge elde etmesine yardımcı olur ve kuruluşa ne zaman katılabilecekleri gibi bir kişinin ihtiyacı olan uygulamalara ne kadar hızlı erişmesini sağlayabilir? Ve güvenlik-bu kişinin iş durumundaki değişiklikler nedeniyle, zaman içinde erişimin ne zaman içinde değiştirilmesi gerekir? 

|Özellik  |Açıklama|Öneri |
|---------|---------| ---------|
|KARA|Azure AD Yetkilendirme Yönetimi, kuruluşunuzun içindeki ve dışındaki kullanıcıların uygulamalarına erişimi daha verimli bir şekilde yönetmesine yardımcı olabilir.| Yönetici olmayan uygulamalara erişim [paketleriyle](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-first)erişimi yönetmesine izin verin.|
|Erişim Gözden Geçirmeleri|Kullanıcıların uygulamalara erişimi, yalnızca doğru kişilerin erişmeye devam ettiğinden emin olmak için düzenli olarak incelenebilir.| En hassas uygulamalarınıza [erişimi gözden geçirin](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) . |
|Log Analytics|Hangi uygulamalara kimin eriştiğini ve verileri veri kaynakları arasında ve zaman içindeki verileri ilişkilendirmek için onu tercih ettiğiniz SıEM aracında depolayan raporlar oluşturun.| [Log Analytics](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) 'i etkinleştirin ve uygulamalarınızla ilgili kritik olaylar için Uyarılar ayarlayın. |


## <a name="scenario-5-hybrid-secure-access"></a>Senaryo 5: karma güvenli erişim
Kimlik yalnızca bulut ve şirket içi uygulamalardaki her şeyi bağlayabildiği durumlarda denetim düzledir. Eski kimlik doğrulama tabanlı uygulamalara güvenli erişim sağlamak için Azure AD ve iş ortakları tarafından sunulan araçlardan yararlanın.

|Özellik  |Açıklama|Öneri |
|---------|---------|---------|
|Uygulama Ara Sunucusu|Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Bulut ve şirket içi uygulamalarda SaaS uygulamalarına erişmesi gerekir. Azure AD uygulama proxy 'si, pahalı ve karmaşık sanal özel ağlar (VPN) veya sivil bölgeler (DMZs) olmadan bu güçlü erişimi mümkün hale getirmenizi sunar.|Şirket içi uygulamalarınız için [Uzaktan erişim](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) ayarlayın. |
|F5, Akamai, Zscaler|Mevcut ağ ve teslim denetleyicinizi kullanarak, İş süreçleriniz için hala kritik olan eski uygulamaları kolayca koruyabilir ancak Azure AD ile önce koruyamazsınız. Bu uygulamaları korumaya başlamak için ihtiyacınız olan her şeye zaten sahipsiniz.| Akamai, Citrix, F5 veya Zscaler kullanılıyor mu? [Önceden oluşturulmuş çözümlerimize](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)göz atın. | 

## <a name="related-articles"></a>İlgili makaleler:

- [Uygulama yönetimi](https://docs.microsoft.com/azure/active-directory/manage-apps/index)
- [Uygulama sağlama](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning)
- [Karma güvenli erişim](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
- [Kimlik yönetimi](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview)
- [Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)
- [Kimlik güvenliği](https://docs.microsoft.com/azure/active-directory/conditional-access/index)
