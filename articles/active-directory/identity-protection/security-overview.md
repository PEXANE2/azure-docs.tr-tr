---
title: Azure Active Directory (Azure AD) Identity Protection güvenliğine genel bakış | Microsoft Docs
description: "' Güvenlik genel bakış ' ' ın size kuruluşunuzun güvenlik duruşuna ilişkin bir Öngörüler nasıl sağladığını öğrenin."
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: b894f7020083dd6ca46c394ec2930a3da36bc76d
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335210"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Azure Active Directory Kimlik Koruması-güvenliğe genel bakış

[Güvenliğe genel bakış](https://aka.ms/IdentityProtectionRefresh) , kuruluşunuzun güvenlik duruşuna ilişkin bir fikir verir. Olası saldırıları belirlemenize ve ilkelerinizin verimliliğini anlamanıza yardımcı olur.

' Güvenlik genel bakışı ' iki bölüme büyük ölçüde ayrılmıştır:

- Eğilimler, solda, kuruluşunuzda risk zaman çizelgesi sağlar.
- Kutucuklar, sağda, kuruluşunuzda devam eden sorunları vurgulayabilir ve hızlı bir şekilde nasıl işlem yapılacağını önerir.

![Güvenliğe genel bakış](./media/security-overview/01.png)
  
## <a name="trends"></a>Eğilimler

### <a name="new-risky-users-detected"></a>Yeni riskli kullanıcılar algılandı

Bu grafik, seçilen dönemde algılanan yeni riskli Kullanıcı sayısını gösterir. Bu grafiğin görünümünü Kullanıcı risk düzeyine göre filtreleyebilirsiniz (düşük, orta, yüksek). Söz konusu gün için algılanan riskli Kullanıcı sayısını görmek için UTC Tarih artışlarının üzerine gelin. Bu grafiğe tıklama sizi ' riskli kullanıcılar ' raporuna getirecek. Risk altında olan kullanıcıları düzeltmek için parolalarını değiştirmeyi göz önünde bulundurun.

### <a name="new-risky-sign-ins-detected"></a>Yeni riskli oturum açma işlemleri algılandı

Bu grafik, seçilen dönemde algılanan riskli oturum açma işlemlerinin sayısını gösterir. Bu grafiğin görünümünü oturum açma risk türüne (gerçek zamanlı veya toplu) ve oturum açma riski düzeyine (düşük, orta, yüksek) göre filtreleyebilirsiniz. Korumasız oturum açma işlemleri, MFA 'dan doğan başarılı gerçek zamanlı risk oturum açma bileşenlerlidir. (Not: Çevrimdışı algılamalar nedeniyle riskli olan oturum açma işlemleri, oturum açma riski ilkelerine göre gerçek zamanlı olarak korunamaz. Bu güne yönelik risk altında algılanan oturum açma işlemlerinin sayısını görmek için UTC Tarih artışlarının üzerine gelin. Bu grafiğe tıklama, sizi ' riskli oturum açma ' raporuna getirecek.

## <a name="tiles"></a>Kutucuklar
 
### <a name="high-risk-users"></a>Yüksek riskli kullanıcılar

' Yüksek riskli kullanıcılar ' kutucuğunda, kimlik güvenliğinin en son olasılığı yüksek olan kullanıcı sayısı gösterilir. Bunlar, araştırma için bir üst öncelik olmalıdır. ' Yüksek riskli kullanıcılar ' kutucuğuna tıkladığınızda ' riskli kullanıcılar ' raporunun yalnızca risk düzeyine sahip kullanıcıları gösteren filtrelenmiş bir görünümüne yeniden yönlendirilir. Bu raporu kullanarak daha fazla bilgi alabilir ve parolayı sıfırlama ile bu kullanıcıları düzeltebilirsiniz.

![Güvenliğe genel bakış](./media/security-overview/02.png)

### <a name="medium-risk-users"></a>Orta riskli kullanıcılar
' Orta riskli kullanıcılar ' kutucuğu, en son kullanıcı sayısını orta düzeyde kimlik güvenliğinin aşılmasına neden gösterir. ' Orta riskli kullanıcılar ' kutucuğuna tıklama, yalnızca risk düzeyine sahip kullanıcıları gösteren ' riskli kullanıcılar ' raporunun filtrelenmiş görünümüne yönlendirilir. Bu raporu kullanarak bu kullanıcıları daha fazla araştırıp düzeltebilirsiniz.

### <a name="unprotected-risky-sign-ins"></a>Korunmayan ve riskli oturum açma işlemleri

' Korumasız riskli oturum açma ' kutucuğunda, bir koşullu erişim ilkesi, kimlik koruması risk ilkesi veya Kullanıcı başına MFA ile ilgili olarak engellenmeyen veya MFA 'dan doğan başarılı, gerçek zamanlı riskli oturum açma işlemlerinin sayısı gösterilir. Bunlar, başarılı olan ve MFA 'nın başarıya düşmesine yol açmayan oturumlardır. Gelecekte bu tür oturum açma işlemlerini korumak için bir oturum açma risk ilkesi uygulayın. ' Korumasız riskli oturum açma ' kutucuğunda bir tıklama, oturum açma risk ilkesini, belirtilen risk düzeyiyle bir oturum açma üzerinde MFA 'yı gerektirecek şekilde yapılandırabileceğiniz oturum açma risk ilkesi yapılandırma dikey penceresine yönlendirilir.

### <a name="legacy-authentication"></a>Eski kimlik doğrulaması

' Eski kimlik doğrulama ' kutucuğu, son haftanın kuruluşunuzdaki eski kimlik doğrulama sayısını gösterir. Eski kimlik doğrulama protokolleri MFA gibi modern güvenlik yöntemlerini desteklemez. Eski kimlik doğrulamasını engellemek için, koşullu erişim ilkesi uygulayabilirsiniz. ' Eski kimlik doğrulaması ' kutucuğuna tıklama sizi ' kimlik güvenli puanı ' öğesine yönlendirir.

### <a name="identity-secure-score"></a>Kimlik Güvenliği Puanı

Kimlik güvenli puanı ölçer ve güvenlik duruşunuzu sektör desenleriyle karşılaştırır. ' Kimlik güvenli puanı (Önizleme) ' kutucuğuna tıklarsanız, güvenlik duruşunuzu geliştirme hakkında daha fazla bilgi edinmek için ' kimlik güvenli puanı (Önizleme) ' dikey penceresine yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kanal 9: Azure AD ve kimlik göster: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
- [Azure Active Directory Kimlik Koruması etkinleştiriliyor](enable.md)
