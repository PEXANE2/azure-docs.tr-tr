---
title: Azure Etkin Dizin için yaygın uygulama yönetimi senaryoları | Microsoft Dokümanlar
description: Azure AD ile uygulama yönetimini merkezileştirin
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
ms.openlocfilehash: d21ff820470765b82e397e56a2458603b8e5a7c7
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657955"
---
# <a name="centralize-application-management-with-azure-ad"></a>Azure AD ile uygulama yönetimini merkezileştirin

Şifreler, hem bir BT kabusu hem de dünya çapında çalışanlar için bir acı. Bu nedenle giderek daha fazla şirket Azure Active Directory' ye, Microsoft' un Kimlik ve Erişim Yönetimi çözümüne bulut ve diğer tüm kaynaklarınıza yöneliyor. Her biri için bir parola girmek zorunda kalmadan uygulamadan uygulamaya atlayın. Outlook'tan, İş Günü'ne, ADP'ye hızlı, hızlı ve güvenli bir şekilde açabildiğiniz kadar hızlı bir şekilde atlayın. Ardından BT'yi aramak zorunda kalmadan iş ortaklarınızla ve hatta kuruluşunuz dışındaki diğer kişilerle işbirliği yapın. Dahası, Azure AD, kim olduğunuzu doğrulamak için kullandığınız uygulamaları çok faktörlü kimlik doğrulama gibi şeylerle güvence altına alarak, nerede olursanız olun ihtiyacınız olan uygulamalara güvenli erişim sağlayan şüpheli oturum açma ları algılamak için sürekli uyarlanabilir makine öğrenimi ve güvenlik istihbaratını kullanarak riski yönetmeye yardımcı olur. Sadece kullanıcılar için değil, BT için de harika. Tam zamanında erişim incelemeleri ve tam ölçekli yönetim paketiyle Azure AD, uyumlu kalmanıza ve ilkeleri de uygulamanıza yardımcı olur. Ve şunu elde edin, hatta kullanıcı hesaplarını otomatikleştirerek erişim yönetimini kolaylaştırabilirsiniz. Müşterinin Azure Active Directory'nin uygulama yönetimi özelliklerini kullandığı yaygın senaryolardan bazılarına göz atın.

**Genel senaryolar**


> [!div class="checklist"]
> * Tüm uygulamalarınız için SSO
> * Sağlama ve deprovisioning otomatikleştirin 
> * Uygulamalarınızı güvenli hale
> * Uygulamalarınıza erişimi yönetin
> * Karma güvenli erişim

## <a name="scenario-1-set-up-sso-for-all-your-applications"></a>Senaryo 1: Tüm uygulamalarınız için SSO'yu ayarlama

Artık parolayı yönetmek yok. Kurumsal kimlik bilgilerinizle ihtiyacınız olan tüm kaynaklara güvenli bir şekilde erişin. 

|Özellik  | Açıklama | Öneri |
|---------|---------|---------|
|SSO|Güvenilir endüstri standartlarını kullanarak standartlara dayalı federe SSO.|Uygulamanız desteklediğinde SSO'yu etkinleştirmek için her zaman [SAML / OIDC](https://docs.microsoft.com/azure/active-directory/manage-apps/isv-choose-multi-tenant-federation) kullanın.|
|Erişim paneli|Kullanıcılarınıza tüm uygulamalarını keşfetmek ve bunlara erişmek için basit bir merkez sunun. Uygulamalara ve gruplara erişim istemek veya başkaları adına kaynaklara erişimi yönetmek gibi self servis özellikleriyle daha üretken olmalarını sağlayabilirsiniz.| Uygulamalarınızı SSO için Azure AD ile tümleştirdikten sonra [kuruluşunuzdaki erişim panelini](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-deployment-plan) dağıtın.|

## <a name="scenario-2-automate-provisioning-and-deprovisioning"></a>Senaryo 2: Sağlama ve deprovisioning otomatikleştirin 


Çoğu uygulama, gereksinim duydukları kaynaklara erişmeden önce bir kullanıcının uygulamaya dahil edilmesini gerektirir. CSV dosyalarını veya karmaşık komut dosyalarını kullanmak maliyetli ve yönetilmesi zor olabilir. Ayrıca, müşterilerin, birinin artık erişimi olmaması gerektiğinde hesapların kaldırıldığından emin olması gerekir. Sağlama ve deprovisioning otomatikleştirmek için aşağıdaki araçlardan yararlanın. 


|Özellik  |Açıklama|Öneri |
|---------|---------|---------|
|SCIM Temini|[SCIM,](https://aka.ms/SICMOverview) kullanıcı sağlamayı otomatikleştirmek için en iyi endüstri uygulamasıdır. SCIM uyumlu tüm uygulamalar Azure AD ile tümleştirilebilir. CSV dosyalarını, özel komut dosyalarını veya prem çözümlerini korumak zorunda kalmadan kullanıcı hesaplarını otomatik olarak oluşturun, güncelleyin ve silin.|Azure AD uygulama [galerisinde, önceden entegre edilmiş](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) uygulamaların büyüyen listesine göz atın|
|Microsoft Graph|Azure AD'nin, uygulamanızı ihtiyaç duyduğu verilerle zenginleştirmek için sahip olduğu veri nefesinden ve derinliğinden yararlanın.|Microsoft ekosisteminden veri almak için [Microsoft grafiğinden](https://developer.microsoft.com/graph/) yararlanın. |


## <a name="scenario-3-secure-your-applications"></a>Senaryo 3: Uygulamalarınızı güvenli hale
Kimlik, güvenliğin temel taşıdır. Eğer bir kimlik açığa çıkarsa, çok geç olmadan domino etkisini durdurmak inanılmaz derecede zordur. Kuruluşların bir uzlaşma olduğunu keşfetmesi ortalama 100 günden fazla sürer. Uygulamalarınızın güvenlik duruşunu iyileştirmek için Azure AD tarafından sağlanan araçları kullanın. 

|Özellik  |Açıklama| Öneri |
|---------|---------| ---------|
|Azure MFA|Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Azure MFA, yönetici onaylı kimlik doğrulama yöntemlerini kullanarak, basit bir oturum açma işlemi talebini karşılarken verilerinize ve uygulamalarınız için erişimi korumaya yardımcı olur.| Kullanıcılarınız için [MFA'yı etkinleştirin.](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124)  |
|Koşullu Erişim|Koşullu Erişim ile, koşullara bağlı olarak bulut uygulamalarınıza kimlerin erişebileceğine ilişkin otomatik erişim denetimi kararlarını uygulayabilirsiniz.| Müşterilerin kullandığı [güvenlik varsayılanlarını](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) ve [yaygın ilkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common) gözden geçirin. | 
|Kimlik Koruması|Kimlik Koruması, Microsoft'un Azure AD'li kuruluşlardaki konumundan, Microsoft Hesapları'ndaki tüketici alanından ve kullanıcılarınızı korumak için Xbox ile oyun oynarken edindiği öğrenmeleri kullanır. Microsoft, müşterileri belirlemek ve tehditlere karşı korumak için günde 6,5 trilyon sinyali analiz eder.|Hizmetimiz tarafından sağlanan [varsayılan kimlik koruma ilkelerini](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-policies) etkinleştirin. | 

## <a name="scenario-4-govern-access-to-your-applications"></a>Senaryo 4: Uygulamalarınıza erişimi yönetin
Kimlik Yönetimi, kuruluşların üretkenlik arasında bir denge kurmasını sağlar - Bir kişi kuruluşuma katıldığında olduğu gibi ihtiyaç duyduğu uygulamalara ne kadar hızlı erişebilir? Ve güvenlik - Bu kişinin istihdam durumundaki değişiklikler gibi erişimleri zaman içinde nasıl değişmeli? 

|Özellik  |Açıklama|Öneri |
|---------|---------| ---------|
|Elm|Azure AD yetkilendirme yönetimi, kuruluşunuz içindeki ve dışındaki kullanıcıların uygulamalarına erişimi daha verimli bir şekilde yönetmelerine yardımcı olabilir.| Yönetici olmayanların uygulamalarına [erişim paketleriyle](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-first)erişmelerini sağlar.|
|Erişim Gözden Geçirmeleri|Kullanıcının uygulamalara erişimi, yalnızca doğru kişilerin sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir.| En hassas uygulamalarınız için [erişimi gözden geçirin.](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) |
|Log Analytics|Verileri veri kaynakları ve zaman içinde ilişkilendirmek için hangi uygulamalara kimin eriştiyi hakkında raporlar oluşturun ve bunları siem aracınızda saklayın.| [Günlük analitiğini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) etkinleştirin ve uygulamalarınız ile ilgili kritik olaylar için uyarılar ayarlayın. |


## <a name="scenario-5-hybrid-secure-access"></a>Senaryo 5: Karma Güvenli Erişim
Kimlik, yalnızca bulut ve şirket içi uygulamalar daki her şeyi birbirine bağlayabiliyorsa, kontrol uçağınız olabilir. Eski tabanlı uygulamalara erişimi güvence altına almak için Azure AD ve iş ortakları tarafından sağlanan araçlardan yararlanın.

|Özellik  |Açıklama|Öneri |
|---------|---------|---------|
|Uygulama Ara Sunucusu|Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Buluttaki SaaS uygulamalarına ve şirket içi kurumsal uygulamalara erişmeleri gerekir. Azure AD Application proxy, pahalı ve karmaşık sanal özel ağlar (VPN'ler) veya arındırılmış bölgeler (DMZ'ler) olmadan bu sağlam erişimi sağlar.|Prem uygulamalarınız için [uzaktan erişim](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) kurulumu yapın. |
|F5, Akamai, Zscaler|Mevcut ağ ve teslim denetleyicinizi kullanarak, iş süreçleriniz için hala kritik olan ancak Azure AD ile daha önce koruyamadığınız eski uygulamaları kolayca koruyabilirsiniz. Bu uygulamaları korumaya başlamak için ihtiyacınız olan her şeye zaten sahip sinizdir.| Akamai, Citrix, F5 veya Zscaler kullanarak? [Önceden oluşturulmuş çözümlerimize](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)göz atın. | 

## <a name="related-articles"></a>İlgili makaleler:

- [Uygulama yönetimi](https://docs.microsoft.com/azure/active-directory/manage-apps/index)
- [Uygulama sağlama](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning)
- [Karma güvenli erişim]()
- [Kimlik idaresi](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview)
- [Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)
- [Kimlik güvenliği](https://docs.microsoft.com/azure/active-directory/conditional-access/index)
