---
title: Azure Active Directory için oturum açma tanılaması nedir?
description: Azure Active Directory 'de oturum açma tanılaması hakkında genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/15/2020
ms.author: markvi
ms.reviewer: tspring
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdef3e1f1a60c9eb0c751855837e9cbe77e015e9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98572298"
---
# <a name="what-is-the-sign-in-diagnostic-in-azure-ad"></a>Azure AD 'de oturum açma tanılaması nedir?

Azure Active Directory (Azure AD), kullanıcıların yönetilen kaynaklarla neler yapabileceğini denetlemek için esnek bir güvenlik modeli sağlar. Bu kaynaklara erişim yalnızca *kim oldukları kişiler* tarafından değil, bunlara *nasıl* eriştikleri de denetlenir. Genellikle, sahip olduğunuz yapılandırma seçeneklerinin sayısı nedeniyle esnek bir model belirli bir karmaşıklık derecesine sahip olacaktır. Karmaşıklık, hatalara karşı riski artırmak için olası bir isteklidir.

BT Yöneticisi olarak, sisteminizdeki etkinliklere ilişkin Öngörüler sağlayan bir çözüme ihtiyacınız vardır. Bu görünürlük sorunları ortaya çıktığında tanılamanıza ve çözmenize olanak sağlayabilir. Azure AD 'de oturum açma Tanılaması, bu tür bir çözüme örnektir. Bir oturum açma girişimi sırasında ne olduğunu çözümlemek ve Microsoft desteği 'ni dahil etmek zorunda kalmadan sorunları çözmeye yönelik öneriler almak için tanılamayı kullanabilirsiniz.

Bu makalede, çözümün ne yaptığını ve nasıl kullanılabileceğini gösteren bir genel bakış sunulmaktadır.

## <a name="requirements"></a>Gereksinimler

Oturum açma tanılaması Azure AD 'nin tüm sürümlerinde kullanılabilir.

Azure AD 'de kullanmak için bir genel yönetici olmanız gerekir.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD 'de, oturum açma girişimine olan yanıt, *kimin* oturum açtığı ve kiracıya *nasıl* eriştikleri ile bağlantılıdır. Örneğin, bir yönetici genellikle şirket ağından oturum açtıklarında kiracının tüm yönlerini yapılandırabilir. Ancak, güvenilmeyen bir ağdan aynı hesapla oturum açtıklarında aynı kullanıcı engellenebilir.

Sistemin bir oturum açma girişimine yanıt vermesi için daha fazla esneklik nedeniyle, oturum açma işlemleri için sorun gidermeniz gereken senaryolarda uçtan uca kalabilirsiniz. Oturum açma Tanılaması, şu özelliklere sahiptir:

- Oturum açma olaylarından verileri analiz eder.

- Ne olduğunu gösterir.

- Sorunların nasıl çözümleneceği hakkında öneriler sağlar.

Azure AD 'nin oturum açma Tanılaması, oturum açma hatalarının kendinden tanılamasını sağlamak üzere tasarlanmıştır. Tanılama işlemini gerçekleştirmek için şunları yapmanız gerekir:

![Oturum açma tanılaması gösteren diyagram.](./media/overview-sign-in-diagnostics/process.png)

1. İlgilendiğiniz oturum açma olaylarının kapsamını tanımlayın.

2. Gözden geçirmek istediğiniz oturum açma ' yı seçin.

3. Tanılama sonuçlarını gözden geçirin.

4. İşlem yapın.

### <a name="define-scope"></a>Kapsam tanımla

Bu adımın amacı, araştırmak için oturum açma olaylarının kapsamını tanımlamaktır. Kapsamınız bir kullanıcıya ya da bir tanımlayıcıya (CorrelationId, RequestId) ve zaman aralığına göre belirlenir. Kapsamı daha da daraltmak için bir uygulama adı belirtebilirsiniz. Azure AD size doğru olayları bulmak için kapsam bilgilerini kullanır.  

### <a name="select-sign-in"></a>Oturum açma seçin  

Azure AD, arama ölçütlerinize göre tüm eşleşen oturum açma olaylarını alır ve bunları bir kimlik doğrulaması özet listesi görünümünde gösterir.

![Kimlik doğrulama özeti bölümünü gösteren kısmi ekran görüntüsü.](./media/overview-sign-in-diagnostics/authentication-summary.png)

Bu görünümde görüntülenecek sütunları özelleştirebilirsiniz.

### <a name="review-diagnostic"></a>Tanılamayı gözden geçir

Azure AD, seçilen oturum açma etkinliği için Tanılama sonuçları sağlar.

![Tanılama sonuçları bölümünü gösteren kısmi ekran görüntüsü.](./media/overview-sign-in-diagnostics/diagnostics-results.png)

Bu sonuçlar, birkaç cümlede ne olduğunu açıklayan bir değerlendirmede başlar. Açıklama, sistemin davranışını anlamanıza yardımcı olur.

Ardından, seçilen oturum açma olayına uygulanan ilgili koşullu erişim ilkelerinin bir özetini alırsınız. Tanılama sonuçları, sorununuzu çözmek için önerilen düzeltme adımlarını da içerir. Sorunları daha fazla yardım olmadan çözmek her zaman mümkün olmadığından, önerilen bir adım bir destek bileti açmak olabilir.

### <a name="take-action"></a>İşlem yapın

Bu noktada, sorununuzu çözmeniz için gereken bilgilere sahip olmanız gerekir.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki senaryolar, oturum açma tanısı tarafından ele alınmıştır:

- Koşullu erişim tarafından engellendi

- Koşullu erişim başarısız

- Koşullu erişimden çok faktörlü Authentication (MFA)

- Diğer gereksinimlerden MFA

- MFA sağlaması gerekli

- MFA sağlaması gerekli (riskli oturum açma konumu)

- Başarılı oturum açma

### <a name="blocked-by-conditional-access"></a>Koşullu erişim tarafından engellendi

Bu senaryoda bir oturum açma denemesi, koşullu erişim ilkesi tarafından engellendi.

![Blok erişimi olan erişim yapılandırmasını gösteren ekran görüntüsü.](./media/overview-sign-in-diagnostics/block-access.png)

Bu senaryonun Tanılama bölümünde Kullanıcı oturum açma olayı ve uygulanan ilkeler hakkındaki ayrıntılar gösterilmektedir.

### <a name="failed-conditional-access"></a>Koşullu erişim başarısız

Bu senaryo genellikle, bir koşullu erişim ilkesinin gereksinimleri karşılanmadığı için başarısız olan oturum açma girişiminin bir sonucudur. Sık karşılaşılan örnekler:

![Ortak ilke örnekleriyle erişim yapılandırmasını gösteren ve seçilen erişime Izin gösteren ekran görüntüsü.](./media/overview-sign-in-diagnostics/require-controls.png)

- Karma Azure AD 'ye katılmış cihaz gerektir

- Onaylanan istemci uygulaması gerektir

- Uygulama koruma ilkesi gerektir

Bu senaryonun Tanılama bölümünde Kullanıcı oturum açma girişimine ve uygulanan ilkelere ilişkin ayrıntılar gösterilmektedir.

### <a name="mfa-from-conditional-access"></a>Koşullu erişimden MFA

Bu senaryoda, koşullu erişim ilkesinin çok faktörlü kimlik doğrulaması kümesini kullanarak oturum açması gereksinimi vardır.

![Çok faktörlü kimlik doğrulaması ıste seçiliyken erişim yapılandırmasını gösteren ekran görüntüsü.](./media/overview-sign-in-diagnostics/require-mfa.png)

Bu senaryonun Tanılama bölümünde Kullanıcı oturum açma girişimine ve uygulanan ilkelere ilişkin ayrıntılar gösterilmektedir.

### <a name="mfa-from-other-requirements"></a>Diğer gereksinimlerden MFA

Bu senaryoda, bir çok faktörlü kimlik doğrulama gereksinimi, koşullu erişim ilkesi tarafından zorlanmaz. Örneğin, çok faktörlü kimlik doğrulaması, Kullanıcı başına esasına göre yapılır.

![Kullanıcı Yapılandırması başına çok faktörlü kimlik doğrulamasını gösteren ekran görüntüsü.](./media/overview-sign-in-diagnostics/mfa-per-user.png)

Bu tanı senaryosunun amacı, hakkında daha fazla ayrıntı sağlamaktır:

- Çok faktörlü kimlik doğrulama kesmesi kaynağı
- İstemci etkileşiminin sonucu

Kullanıcı oturum açma girişiminin tüm ayrıntılarını da görüntüleyebilirsiniz.

### <a name="mfa-proof-up-required"></a>MFA sağlaması gerekli

Bu senaryoda, oturum açma girişimleri, çok faktörlü kimlik doğrulamasını ayarlama istekleri tarafından kesintiye uğradı. Bu kurulum, düzeltme olarak da bilinir.

Çok faktörlü kimlik doğrulaması sağlaması, bir kullanıcının çok faktörlü kimlik doğrulaması kullanması gerektiğinde, ancak henüz yapılandırılmadığı ya da bir yöneticinin kullanıcıyı yapılandırmasını sağlaması gerektiğinde gerçekleşir.

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulaması kesintisinin Kullanıcı yapılandırmasının olmamasından kaynaklanıyor olduğunu ortaya çıkarmaktır. Önerilen çözüm, kullanıcının prova 'yi tamamlaması içindir.

### <a name="mfa-proof-up-required-risky-sign-in-location"></a>MFA sağlaması gerekli (riskli oturum açma konumu)

Bu senaryoda, oturum açma girişimleri, riskli oturum açma konumundan çok faktörlü kimlik doğrulamasını ayarlama isteği tarafından kesintiye uğradı.

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulaması kesintisinin Kullanıcı yapılandırmasının olmamasından kaynaklanıyor olduğunu ortaya çıkarmaktır. Önerilen çözüm, özellikle riskli olarak görünmeyen bir ağ konumundan kullanıcının prova 'yi tamamlaması içindir.

Örneğin, bir şirket ağı adlandırılmış bir konum olarak tanımlanmışsa, Kullanıcı bunun yerine şirket ağından Prova yapmayı denemelidir.

### <a name="successful-sign-in"></a>Başarılı oturum açma

Bu senaryoda, oturum açma olayları koşullu erişim veya çok faktörlü kimlik doğrulaması tarafından kesintiye uğratılmaz.

Bu tanılama senaryosunda, koşullu erişim ilkeleri veya çok faktörlü kimlik doğrulaması nedeniyle kesintiye uğratılmasını beklenen Kullanıcı oturum açma olayları hakkında ayrıntılı bilgi sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory raporları nelerdir?](overview-reports.md)
- [Azure Active Directory izleme nedir?](overview-monitoring.md)
