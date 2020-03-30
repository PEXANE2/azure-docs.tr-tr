---
title: 'Uygulama yönetimi: En iyi uygulamalar ve öneriler | Microsoft Dokümanlar'
description: Azure Active Directory'de uygulamaları yönetmek için en iyi uygulamaları ve önerileri öğrenin. Uygulama Proxy ile şirket içi uygulamaları otomatik olarak sağlama ve yayımlama kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2019
ms.subservice: app-mgmt
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6534efb6fcd07ee3b9f3979cabf2feb77496a8b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74085297"
---
# <a name="application-management-best-practices"></a>Uygulama yönetimi en iyi uygulamalar
Bu makale, Azure Etkin Dizini'ndeki (Azure AD) uygulamaları yönetmek, otomatik sağlama yı kullanmak ve Uygulama Proxy ile şirket içi uygulamaları yayımlama için öneriler ve en iyi uygulamaları içerir.

## <a name="cloud-app-and-single-sign-on-recommendations"></a>Bulut uygulaması ve tek oturum açma önerileri
| Öneri | Yorumlar |
| --- | --- |
| Uygulamalar için Azure AD uygulama galerisini kontrol edin  | Azure AD'de, Enterprise tek oturum açma (SSO) ile etkinleştirilen binlerce önceden tümleşik uygulama içeren bir galeri bulunur. Uygulamaya özel kurulum kılavuzu için [SaaS uygulama öğreticileri listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)bakın.  | 
| Federe SAML tabanlı SSO kullanın  | Bir uygulama bunu desteklediğinde, parola tabanlı SSO ve ADFS yerine Azure AD ile Federe, SAML tabanlı SSO kullanın.  | 
| Sertifika imzalama için SHA-256'yı kullanma  | Azure AD, SAML yanıtını imzalamak için varsayılan olarak SHA-256 algoritmasını kullanır. Uygulama SHA-1 gerektirmedikçe SHA-256'yı kullanın (bkz. [Sertifika imzalama seçenekleri](certificate-signing-options.md) ve Uygulama oturum açma [sorunu](application-sign-in-problem-application-error.md).)  | 
| Kullanıcı atamasını zorunlu kılmasını gerektirin  | Varsayılan olarak, kullanıcılar kurumsal uygulamalarınıza kendilerine atanmadan erişebilir. Ancak, uygulama rolleri ortaya çıkarırsa veya uygulamanın kullanıcının erişim panelinde görünmesini istiyorsanız, kullanıcı ataması gerekir. (Uygulamaları [tümleştirmek için Geliştirici kılavuzuna](developer-guidance-for-integrating-applications.md)bakın.)  | 
| Uygulamalarım erişim panelini kullanıcılarınıza dağıtın | Erişim `https://myapps.microsoft.com` [paneli,](end-user-experiences.md) kullanıcılara atanan bulut tabanlı uygulamaları için tek bir giriş noktası sağlayan web tabanlı bir portaldır. Grup yönetimi ve self servis parola sıfırlama gibi ek özellikler eklendikçe, kullanıcılar bunları erişim panelinde bulabilir. Bkz. [Bir erişim paneli dağıtım Planı.](access-panel-deployment-plan.md)
| Grup ataması kullanma  | Aboneliğinize dahil edildiyseniz, devam eden erişim yönetimini grup sahibine devretmek için gruplar bir uygulamaya atayın. (Uygulamaları [tümleştirmek için Geliştirici kılavuzuna](developer-guidance-for-integrating-applications.md)bakın.)   | 
| Sertifikaları yönetmek için bir süreç oluşturma | İmza sertifikasının maksimum ömrü üç yıldır. Sertifikanın süresinin dolması nedeniyle kesintinin önüne çıkmak veya kesintisi en aza indirmek için, sertifikayla ilgili değişiklik bildirimlerinin yakından izlenmesini sağlamak için rolleri ve e-posta dağıtım listelerini kullanın. |

## <a name="provisioning-recommendations"></a>Tedarik önerileri
| Öneri | Yorumlar |
| --- | --- |
| Bulut uygulamalarıyla sağlama ayarlamak için öğreticileri kullanma | Eklemek istediğiniz galeri uygulaması için sağlama yapılandırma konusunda adım adım rehberlik için [SaaS uygulama öğreticileri listesini](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kontrol edin. |
| Durumu izlemek için sağlama günlüklerini (önizleme) kullanma | [Sağlama günlükleri,](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) tek tek kullanıcıların durumu da dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm eylemler hakkında ayrıntılı bilgi verir. |
| Sağlama bildirimi e-postasına bir dağıtım grubu atama | Sağlama hizmeti tarafından gönderilen kritik uyarıların görünürlüğünü artırmak için Bildirim E-postaları ayarına bir dağıtım grubu atayın. |


## <a name="application-proxy-recommendations"></a>Uygulama Proxy önerileri
| Öneri | Yorumlar |
| --- | --- |
| İç kaynaklara uzaktan erişim için Uygulama Proxy'sini kullanma | Uygulama Proxy uzak kullanıcılara iç kaynaklara erişim vermek için, bir VPN veya ters proxy için ihtiyaç yerine önerilir. Gecikme sonu ekleyebileceğinden, şirket ağı içinden kaynaklara erişmek için tasarlanmamıştır.
| Özel etki alanlarını kullanma | Uygulamalarınız için özel etki alanları ayarlayın (bkz. [özel etki alanlarını yapılandırın)](application-proxy-configure-custom-domain.md)böylece kullanıcılar için URL'ler ve uygulamalar arasında ağınızın içinden veya dışından çalışacaktır. Ayrıca markanızı kontrol edebilecek ve URL'lerinizi özelleştirebilirsiniz.  Özel alan adları kullanırken, Microsoft'a güvenilmeyen bir sertifika yetkilisinden ortak sertifika almayı planlayın. Azure Application Proxy standart,[(joker karakter)](application-proxy-wildcard.md)veya SAN tabanlı sertifikaları destekler. (Bkz. [Uygulama Proxy planlaması.)](application-proxy-deployment-plan.md) |
| Uygulama Proxy'sini dağıtmadan önce kullanıcıları eşitleme | Uygulama proxy'sini dağıtmadan önce, kullanıcı kimliklerini şirket içi bir dizinizinden eşitleyin veya doğrudan Azure AD'de oluşturun. Kimlik eşitlemesi, Azure AD'nin kullanıcılara App Proxy tarafından yayınlanan uygulamalara erişim izni vermeden önce kullanıcıların kimlik doğrulamasını önceden gerçekleştirmesine olanak tanır. Ayrıca, tek oturum açma (SSO) gerçekleştirmek için gerekli kullanıcı tanımlayıcı bilgilerini de sağlar. (Bkz. [Uygulama Proxy planlaması.)](application-proxy-deployment-plan.md) |
| Yüksek kullanılabilirlik ve yük dengeleme için ipuçlarımızı takip edin | Kullanıcılar, Uygulama Proxy bağlayıcıları ve arka uç uygulama sunucuları arasında trafiğin nasıl aktığını öğrenmek ve performans ve yük dengelemesini optimize etmek için ipuçları almak için [Uygulama Proxy bağlayıcılarınızın ve uygulamalarınızın yüksek kullanılabilirlik ve yük dengelemesine](application-proxy-high-availability-load-balancing.md)bakın. |
| Birden fazla konektör kullanma | Daha fazla esneklik, kullanılabilirlik ve ölçek için iki veya daha fazla Uygulama Proxy bağlayıcısı kullanın (bkz. [Uygulama Proxy bağlayıcıları).](application-proxy-connectors.md) Bağlayıcı grupları oluşturun ve her bağlayıcı grubunun en az iki bağlayıcısı olduğundan emin olun (üç bağlayıcı en iyisidir). |
| Bağlayıcı sunucuları uygulama sunucularına yakın bulun ve aynı etki alanında olduğunuzdan emin olun | Performansı en iyi duruma getirmek için, bağlayıcı sunucusunu uygulama sunucularına yakın fiziksel olarak bulun (Bkz. [Ağ topolojisi hususları).](application-proxy-network-topology.md) Ayrıca, bağlayıcı sunucu ve web uygulamaları sunucuları aynı Active Directory etki alanına ait olmalıdır veya güvenilen etki alanlarını kapsamalıdır. Bu yapılandırma, Tümleşik Windows Kimlik Doğrulaması (IWA) ve Kerberos Kısıtlı Delegasyonu (KCD) ile SSO için gereklidir. Sunucular farklı etki alanlarındaysa, SSO için kaynak tabanlı delegasyonu kullanmanız gerekir [(Application Proxy ile tek oturum açma için KCD'ye](application-proxy-configure-single-sign-on-with-kcd.md)bakın). |
| Konektörler için otomatik güncelleştirmeleri etkinleştirme | En son özellikler ve hata düzeltmeleri için bağlayıcılarınız için otomatik güncelleştirmeleri etkinleştirin. Microsoft, en son bağlayıcı sürümü ve daha önce bir sürüm için doğrudan destek sağlar. (Bkz. [Uygulama Proxy sürüm sürüm geçmişi](application-proxy-release-version-history.md).) |
| Şirket içi proxy'nizi atla | Daha kolay bakım için, konektörü şirket içi proxy'nizi doğrudan Azure hizmetlerine bağlayacak şekilde yapılandırın. (Bkz. [Uygulama Proxy bağlayıcıları ve proxy sunucuları.)](application-proxy-configure-connectors-with-proxy-servers.md) |
| Azure AD Uygulama Proxy'yi Web Application Proxy üzerinden kullanma | Şirket içi senaryoların çoğu için Azure AD Uygulama Proxy'yi kullanın. Web Application Proxy yalnızca AD FS için proxy sunucusu gerektiren ve Azure Active Directory'de özel etki alanlarını kullanamadığınız senaryolarda tercih edilir. (Bkz. [Uygulama Proxy geçişi](application-proxy-migration.md).) |
