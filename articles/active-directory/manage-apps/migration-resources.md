---
title: Azure Active Directory uygulamasına uygulama geçirme kaynakları | Microsoft Docs
description: Uygulama erişimini ve kimlik doğrulamasını Azure Active Directory (Azure AD) ile geçirmenize yardımcı olacak kaynaklar.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 02/29/2020
ms.author: kenwith
ms.reviewer: baselden
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3fe21b6509080e88c63d9c02c8116f3882fb64dd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763117"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Azure Active Directory uygulamalara geçiş kaynakları

Uygulama erişimini ve kimlik doğrulamasını Azure Active Directory (Azure AD) ile geçirmenize yardımcı olacak kaynaklar. Bu kısa ankete katılın ( https://aka.ms/AppsMigrationFeedback) geçiş için engelleyiciler dahil olmak üzere Azure AD 'ye uygulama geçirme, alet kullanımı/yönergeler veya şirket ıçı ıDP 'nizi sürdürme nedenleri dahil) hakkında geri bildirimde bulunmak için. 

| Kaynak  | Açıklama  |
|:-----------|:-------------|
|[Uygulamalarınızı Azure AD 'ye geçirme](https://aka.ms/migrateapps/whitepaper) | Bu Teknik İnceleme, geçişin avantajlarının yanı sıra, açıkça Seviyelendirilmiş dört aşamada geçişin nasıl planlanacağını açıklar: bulma, sınıflandırma, geçiş ve devam eden yönetim. İşlemin nasıl düşündüğünü ve projenizi kullanımı kolay parçalara nasıl başlayacağınızı öğreneceksiniz. Belge genelinde, bu şekilde size yardımcı olacak önemli kaynakların bağlantıları bulunur. |
|[Çözüm Kılavuzu: uygulamaları Active Directory Federasyon Hizmetleri (AD FS) (AD FS) 'den Azure AD 'ye geçirme](https://aka.ms/migrateapps/adfssolutionguide) | Bu çözüm kılavuzu, geçiş teknik incelemesi 'nde daha yüksek bir düzeyde açıklanan uygulama geçiş projesini planlama ve yürütmenin aynı dört aşamada size kılavuzluk eder. Bu kılavuzda, bir uygulamayı Azure Directory Federasyon Hizmetleri 'nden (AD FS) Azure AD 'ye taşımaya yönelik belirli bir hedefe bu aşamaları nasıl uygulayacağınızı öğreneceksiniz.|
| [Araç: Active Directory Federasyon Hizmetleri (AD FS) geçiş hazırlığı betiği](https://aka.ms/migrateapps/adfstools) | Bu, şirket içi Active Directory Federasyon Hizmetleri (AD FS) (AD FS) sunucunuzda çalıştırabileceğiniz ve Azure AD 'ye geçiş için uygulamaların hazır olduğunu belirleyebilmeniz için kullanabileceğiniz bir betiktir.|
| [Dağıtım planı: AD FS 'den Parola karması eşitlemesine geçiş](https://aka.ms/ADFSTOPHSDPDownload) | Parola karması eşitlemeyle, kullanıcı parolalarının karmaları şirket içi Active Directory Azure AD 'ye eşitlenir. Bu, Azure AD 'nin şirket içi Active Directory etkileşime girmeden kullanıcıların kimliğini doğrulamasına olanak sağlar.| 
| [Dağıtım planı: AD FS 'den geçişli kimlik doğrulamaya geçiş](https://aka.ms/ADFSTOPTADPDownload)|Azure AD geçişli kimlik doğrulaması, kullanıcıların aynı parolayı kullanarak hem şirket içi hem de bulut tabanlı uygulamalarda oturum açmalarına yardımcı olur. Bu özellik, daha az bir parolaya sahip olmaları gerektiğinden kullanıcılarınıza daha iyi bir deneyim sağlar. Ayrıca, kullanıcıların yalnızca bir parolayı hatırlamaları gerektiğinde nasıl oturum açabileceğini unutmaları daha az olduğu için BT yardım masası maliyetlerini de azaltır. Kullanıcılar Azure AD'de oturum açtığında bu özellik parolaları doğrudan şirket için Active Directory dizininizde doğrular.|
| [Dağıtım planı: Azure AD ile bir SaaS uygulamasında çoklu oturum açmayı etkinleştirme](https://aka.ms/SSODPDownload) | Çoklu oturum açma (SSO), tek bir kullanıcı hesabı kullanarak yalnızca bir kez oturum açarak iş yapmak için ihtiyacınız olan tüm uygulamalara ve kaynaklara erişmenize yardımcı olur. Örneğin, bir Kullanıcı oturum açtıktan sonra Kullanıcı Microsoft Office, SalesForce 'a (örneğin, bir parola yazarak) ikinci bir kez geçebilir. 
| [Dağıtım planı: uygulama proxy 'Si ile uygulamaları Azure AD 'ye genişletme](https://aka.ms/AppProxyDPDownload)| Çalışan dizüstü bilgisayarlardan ve diğer cihazlardan şirket içi uygulamalara erişim sağlamak, geleneksel olarak sanal özel ağlar (VPN 'Ler) veya sivilleştirilmiş bölgeler (DMZs) ile ilgilidir. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir. Azure AD Uygulama Ara Sunucusu, şirket içi uygulamalara erişmeyi kolaylaştırır. |
| [Dağıtım planları](../fundamentals/active-directory-deployment-plans.md) | Multi-Factor Authentication, koşullu erişim, Kullanıcı sağlama, sorunsuz SSO, self servis parola sıfırlama ve daha fazlası gibi özellikleri dağıtmaya yönelik daha fazla dağıtım planı bulabilirsiniz! |


