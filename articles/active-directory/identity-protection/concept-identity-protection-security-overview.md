---
title: Azure Active Directory Identity Protection security'ye genel bakış
description: Güvenlik genel görünümünün kuruluşunuzun güvenlik duruşu hakkında nasıl bir fikir verdiğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9550684ad154f28a02ee347fd0a79c1ec286beb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74382192"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Azure Active Directory Identity Protection - Güvenlik genel bakış

Azure portalındaki [Güvenlik genel bakışı,](https://aka.ms/IdentityProtectionRefresh) kuruluşunuzun güvenlik duruşu hakkında bir fikir verir. Olası saldırıları belirlemeye ve politikalarınızın etkinliğini anlamanıza yardımcı olur.

'Güvenlik genel bakışı' genel olarak iki bölüme ayrılmıştır:

- Eğilimler, solda, kuruluşunuzdaki risk zaman çizelgesini sağlar.
- Sağdaki kutucuklar, kuruluşunuzdaki devam eden önemli sorunları vurgulayın ve hızlı bir şekilde nasıl harekete geçilen önerin.

![Güvenliğe genel bakış](./media/concept-identity-protection-security-overview/01.png)
  
## <a name="trends"></a>Eğilimler

### <a name="new-risky-users-detected"></a>Yeni riskli kullanıcılar algılandı

Bu grafik, seçilen zaman dilimi içinde algılanan yeni riskli kullanıcı sayısını gösterir. Bu grafiğin görünümünü kullanıcı risk düzeyine göre filtreleyebilirsiniz (düşük, orta, yüksek). O gün için algılanan riskli kullanıcı sayısını görmek için UTC tarih artışlarının üzerine çıkın. Bu grafiğe yapılacak bir tıklama sizi 'Riskli kullanıcılar' raporuna getirir. Risk altındaki kullanıcıları düzeltmek için parolalarını değiştirmeyi düşünün.

### <a name="new-risky-sign-ins-detected"></a>Yeni riskli oturum açmalar algılandı

Bu grafik, seçilen zaman dilimi içinde algılanan riskli oturum açma sayısını gösterir. Bu grafiğin görünümünü oturum açma risk türüne (gerçek zamanlı veya toplam) ve oturum açma risk düzeyine (düşük, orta, yüksek) göre filtreleyebilirsiniz. Korumasız oturum açmalar, MFA'ya meydan okunmayan başarılı gerçek zamanlı risk oturum açmalarıdır. (Not: Çevrimdışı algılamalar nedeniyle riskli olan oturum açma lar oturum açma riski ilkeleri yle gerçek zamanlı olarak korunamaz). O gün için risk altında olan oturum açma sayısını görmek için UTC tarih artışlarının üzerine çıkın. Bu grafiğe yapılacak bir tıklama sizi 'Riskli oturum açma' raporuna getirir.

## <a name="tiles"></a>Kutucuklar
 
### <a name="high-risk-users"></a>Yüksek riskli kullanıcılar

'Yüksek riskli kullanıcılar' döşemesi, kimlik uzlaşma olasılığı yüksek olan kullanıcıların en son sayısını gösterir. Bunlar soruşturma için en önemli öncelik olmalı. 'Yüksek riskli kullanıcılar' döşemesine yapılan bir tıklama, yalnızca risk düzeyi yüksek olan kullanıcıları gösteren 'Riskli kullanıcılar' raporunun filtrelenmiş görünümüne yönlendirilir. Bu raporu kullanarak, daha fazla bilgi edinebilir ve bu kullanıcıları parola sıfırlama ile düzeltebilirsiniz.

![Güvenliğe genel bakış](./media/concept-identity-protection-security-overview/02.png)

### <a name="medium-risk-users"></a>Orta riskli kullanıcılar
'Orta riskli kullanıcılar' döşemesi, kimlik uzlaşması olasılığı orta olan kullanıcıların en son sayısını gösterir. 'Orta riskli kullanıcılar' döşemesine yapılan bir tıklama, yalnızca orta risk düzeyine sahip kullanıcıları gösteren 'Riskli kullanıcılar' raporunun filtrelenmiş görünümüne yönlendirilir. Bu raporu kullanarak, bu kullanıcıları daha fazla araştırabilir ve düzeltebilirsiniz.

### <a name="unprotected-risky-sign-ins"></a>Korunmasız riskli oturum açma

'Korumasız riskli oturum açma' döşemesi, geçen haftaki, engellenmemiş veya Koşullu Erişim ilkesi, Kimlik Koruması risk ilkesi veya kullanıcı başına MFA tarafından engellenmemiş başarılı, gerçek zamanlı riskli oturum açma sayısını gösterir. Bunlar, MFA'nın meydan okumadığı, başarılı olabilecek potansiyel olarak tehlikeye atılmış oturum açmalardır. Gelecekte bu tür oturum açmaları korumak için oturum açma riski ilkesi uygulayın. 'Korumasız riskli oturum açma' döşemesine yapılan bir tıklama, oturum açma risk ilkesi ni belirli bir risk düzeyine sahip bir oturum açma gereksinimi gerektirecek şekilde oturum açma risk ilkesini yapılandırabileceğiniz oturum açma risk ilkesi yapılandırma bıçağına yönlendirecektir.

### <a name="legacy-authentication"></a>Eski kimlik doğrulaması

'Eski kimlik doğrulaması' döşemesi, kuruluşunuzdaki son haftanın eski kimlik doğrulama sayısını gösterir. Eski kimlik doğrulama protokolleri MFA gibi modern güvenlik yöntemlerini desteklemez. Eski kimlik doğrulamasını önlemek için Koşullu Erişim ilkesi uygulayabilirsiniz. 'Eski kimlik doğrulama' döşemesine yapılacak bir tıklama yla sizi 'Kimlik Güvenli Puanı'na yönlendirir.

### <a name="identity-secure-score"></a>Kimlik Güvenli Puanı

Identity Secure Score, güvenlik duruşunuzu endüstri kalıplarıyla ölçer ve karşılaştırır. 'Identity Secure Score (Preview)' döşemesine tıklarsanız, güvenlik duruşunuzu geliştirme hakkında daha fazla bilgi edinebileceğiniz 'Identity Secure Score' bıyığı'na yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Risk nedir](concept-identity-protection-risks.md)

- [Riskleri azaltmak için kullanılabilen politikalar](concept-identity-protection-policies.md)
