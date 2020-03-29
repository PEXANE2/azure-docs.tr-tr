---
title: Uygulamaları Azure Etkin Dizine geçirmek için kaynaklar | Microsoft Dokümanlar
description: Uygulama erişimini ve kimlik doğrulamasını Azure Etkin Dizin'e (Azure AD) geçirmenize yardımcı olacak kaynaklar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 02/29/2020
ms.author: mimart
ms.reviewer: baselden
ms.collection: M365-identity-device-management
ms.openlocfilehash: b30469858a5dd83f7f5f707f74466302b3000510
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968719"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Uygulamaları Azure Etkin Dizini'ne geçirmek için kaynaklar

Uygulama erişimini ve kimlik doğrulamasını Azure Etkin Dizin'e (Azure AD) geçirmenize yardımcı olacak kaynaklar. Bu kısa anketihttps://aka.ms/AppsMigrationFeedback) (uygulamaları Azure AD'ye geçirme deneyiminiz hakkında geri bildirim sağlamak için (geçiş edoğru engelleyiciler, araç kullanma / yönlendirme gereksinimi veya şirket içi IDP'nizi koruma nedenleri dahil) sağlayın. 

| Kaynak  | Açıklama  |
|:-----------|:-------------|
|[Uygulamalarınızı Azure AD'ye geçirme](https://aka.ms/migrateapps/whitepaper) | Bu teknik inceleme, göçün faydalarını sunar ve açıkça özetlenen dört aşamada geçişin nasıl planlanır: keşif, sınıflandırma, göç ve sürekli yönetim. Süreci nasıl düşüneceğiniz ve projenizi tüketilmesi kolay parçalara ayırma konusunda size rehberlik edilecektir. Belge boyunca yol boyunca size yardımcı olacak önemli kaynaklara bağlantılar vardır. |
|[Çözüm kılavuzu: Uygulamaları Active Directory Federation Services 'ten (AD FS) Azure AD'ye geçirme](https://aka.ms/migrateapps/adfssolutionguide) | Bu çözüm kılavuzu, geçiş teknik incelemesinde daha yüksek bir düzeyde açıklanan bir uygulama geçiş projesini planlamanın ve yürütmenin aynı dört aşamasında size yol gösterin. Bu kılavuzda, bu aşamaları, bir uygulamayı Azure Dizin Federe Hizmetleri'nden (AD FS) Azure AD'ye taşıma hedefine nasıl uygulayacağınızı öğreneceksiniz.|
| [Araç: Aktif Dizin Federasyon Hizmetleri Geçiş Hazırlık Script](https://aka.ms/migrateapps/adfstools) | Bu, Azure AD'ye geçiş için uygulamaların hazır olup olmadığını belirlemek için şirket içi Active Directory Federation Services (AD FS) sunucunuzda çalıştırabileceğiniz bir komut dosyasıdır.|
| [Dağıtım planı: AD FS'den parola karma eşitlemesine geçiş](https://aka.ms/ADFSTOPHSDPDownload) | Parola karma senkronizasyonu ile, kullanıcı parolalarının karmaları şirket içi Active Directory'den Azure AD'ye eşitlenir. Bu, Azure AD'nin şirket içi Active Directory ile etkileşimde olmadan kullanıcıların kimliğini doğrulamasını sağlar.| 
| [Dağıtım planı: AD FS'den geçiş kimlik doğrulaması'na geçiş](https://aka.ms/ADFSTOPTADPDownload)|Azure AD geçiş kimlik doğrulaması, kullanıcıların aynı parolayı kullanarak hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmalarına yardımcı olur. Bu özellik, kullanıcılarınızın hatırlanacak bir parolaları daha az olduğundan daha iyi bir deneyim sağlar. Ayrıca, kullanıcıların yalnızca bir parolayı hatırlamaları gerektiğinde oturum açma olasılığını unutma olasılığı daha düşük olduğundan BT yardım masası maliyetlerini de azaltır. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
| [Dağıtım planı: Azure AD ile Bir SaaS uygulamasında Tek Oturum Açma'yı etkinleştirme](https://aka.ms/SSODPDownload) | Tek oturum açma (SSO), tek bir kullanıcı hesabı kullanarak yalnızca bir kez oturum açarak, iş yapmak için ihtiyacınız olan tüm uygulamalara ve kaynaklara erişmenize yardımcı olur. Örneğin, bir kullanıcı oturum açsa, kullanıcı ikinci kez kimlik doğrulamadan (örneğin parola yazma) Microsoft Office'ten SalesForce'a, Kutu'ya taşınabilir. 
| [Dağıtım planı: Uygulama Proxy ile uygulamaları Azure AD'ye genişletme](https://aka.ms/AppProxyDPDownload)| Çalışan dizüstü bilgisayarlarından ve diğer cihazlardan şirket içi uygulamalara erişim sağlamak geleneksel olarak sanal özel ağları (VPN'ler) veya arındırılmış bölgeleri (DMZ'ler) içeriyordu. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Azure AD Application Proxy, şirket içi uygulamalara erişmenizi kolaylaştırır. |
| [Dağıtım planları](../fundamentals/active-directory-deployment-plans.md) | Çok Faktörlü kimlik doğrulama, Koşullu Erişim, kullanıcı sağlama, sorunsuz SSO, self servis parola sıfırlama ve daha fazlası gibi özellikleri dağıtmak için daha fazla dağıtım planı bulun! |


