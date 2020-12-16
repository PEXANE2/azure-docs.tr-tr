---
title: Azure AD 'de oturum açma tanılaması nedir? | Microsoft Belgeleri
description: Azure AD 'de oturum açma tanılamalarına genel bir bakış sağlar.
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
ms.openlocfilehash: e85de1edd94a0430a4b28b332d9e43b967afba76
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97608927"
---
# <a name="what-is-sign-in-diagnostic-in-azure-ad"></a>Azure AD 'de oturum açma tanılaması nedir?

Azure AD, kullanıcıların yönetilen kaynaklarla neler yapabileceğini denetlemek için esnek bir güvenlik modeli sağlar. Bu kaynaklara erişim, yalnızca **kim** olduğunuz ve aynı zamanda bunlara **nasıl** erişbir şekilde denetlenemez. Genellikle esneklik, sahip olduğunuz yapılandırma seçeneklerinin sayısı nedeniyle belirli bir karmaşıklık derecesi ile birlikte gelir. Karmaşıklık, hatalara karşı riski artırmak için olası bir isteklidir.

BT Yöneticisi olarak, sorunları ortaya çıktığında kolayca tanılamanıza ve çözebilmeniz için, sisteminizdeki etkinliklere yönelik doğru Öngörüler sunan bir çözüme ihtiyacınız vardır. Azure AD 'de oturum açma Tanılaması, böyle bir çözüm örneğidir. Bir oturum açma sırasında ne olduğunu çözümlemek için tanılamayı ve Microsoft desteği 'ni kullanmak zorunda kalmadan sorunları çözmek için gerçekleştirebileceğiniz eylemleri analiz edin.

Bu makalede, çözümün ne yaptığını ve nasıl kullanılabileceğini gösteren bir genel bakış sunulmaktadır.


## <a name="requirements"></a>Gereksinimler

Oturum açma tanılaması Azure AD 'nin tüm sürümlerinde kullanılabilir.<br> Azure AD 'de kullanmak için bir genel yönetici olmanız gerekir.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD 'de, oturum açma **girişimine olan yanıt, sizin ve** kiracınıza **nasıl** erişebilirsiniz. Örneğin, yönetici olarak şirket ağınızdan oturum açtığınızda kiracınızın tüm yönlerini yapılandırabilirsiniz. Ancak, güvenilmeyen bir ağdan aynı hesapla oturum açtığınızda bile engellenmiş olabilirsiniz.
 
Sistemin bir oturum açma girişimine yanıt vermesi için daha fazla esneklik nedeniyle, oturum açma işlemleri için sorun gidermeniz gereken senaryolarda uçtan uca kalabilirsiniz. Oturum açma Tanılaması, şu özelliklere sahiptir:

- Oturum açma işlemlerinin verilerini analiz eder. 

- Ne olduğunu ve sorunların nasıl çözümlendiğini gösterir. 

Azure AD 'nin oturum açma Tanılaması, oturum açma hatalarının kendinden tanılamasını sağlamak üzere tasarlanmıştır. Tanılama işlemini gerçekleştirmek için şunları yapmanız gerekir:

![Oturum açma tanılama işlemi](./media/overview-sign-in-diagnostics/process.png)
 
1. İlgilendiğiniz oturum açma olaylarının kapsamını **tanımlayın**

2. Gözden geçirmek istediğiniz oturum açma **seçeneğini belirleyin**

3. Tanılama sonucunu **gözden geçirin**

4. Eylem **yapın**

 
### <a name="define-scope"></a>Kapsam tanımla

Bu adımın amacı, araştırmak istediğiniz oturum açma işlemlerinin kapsamını tanımlamaktır. Kapsamınız bir kullanıcı veya tanımlayıcıyı (CorrelationId, RequestId) ve zaman aralığını temel alır. Kapsamı daha da daraltmak için bir uygulama adı da belirtebilirsiniz. Azure AD size doğru olayları bulmak için kapsam bilgilerini kullanır.  

### <a name="select-sign-in"></a>Oturum açma seçin  

Azure AD, arama ölçütlerinize göre tüm eşleşen oturum açma işlemlerini alır ve bunları bir kimlik doğrulaması özet listesi görünümünde gösterir. 

![Kimlik doğrulama özeti](./media/overview-sign-in-diagnostics/authentication-summary.png)
 
Bu görünümde görüntülenecek sütunları özelleştirebilirsiniz.

### <a name="review-diagnostic"></a>Tanılamayı gözden geçir 

Azure AD, seçilen oturum açma etkinliği için bir tanılama sonucu sağlar. 

![Tanılama sonuçları](./media/overview-sign-in-diagnostics/diagnostics-results.png)

 
Sonuç bir değerlendirmede başlar. Değerlendirme birkaç cümlede ne olduğunu açıklar. Açıklama, sistemin davranışını anlamanıza yardımcı olur. 

Bir sonraki adım olarak, seçilen oturum açma için uygulanan ilgili koşullu erişim ilkelerinin bir özetini alırsınız. Bu bölüm, sorununuzu çözmek için önerilen düzeltme adımları tarafından tamamlanır. Ek Yardım olmadan sorunları çözümlemek her zaman mümkün olmadığından, bir destek bileti açmak için önerilen bir adım olabilir. 

### <a name="take-action"></a>İşlem yapın 
Bu noktada, sorununuzu çözmeniz için gereken bilgilere sahip olmanız gerekir.


## <a name="scenarios"></a>Senaryolar

Bu bölüm, kapsanan tanılama senaryolarına genel bir bakış sağlar. Aşağıdaki senaryolar uygulanır: 
 
- Koşullu erişim tarafından engellendi

- Koşullu erişim başarısız

- Koşullu erişimden MFA

- Diğer gereksinimlerden MFA

- MFA sağlaması gerekli

- MFA sağlaması gerekli ancak kullanıcı oturum açma denemesi güvenli konumdan değil

- Başarılı oturum açma


### <a name="blocked-by-conditional-access"></a>Koşullu erişim tarafından engellendi

Bu senaryo, koşullu erişim ilkesi tarafından engellenmiş bir oturum açma tabanlıdır.

![Erişimi engelleme](./media/overview-sign-in-diagnostics/block-access.png)

Bu senaryonun tanılama bölümü Kullanıcı oturumu açma ve uygulanan ilkelerle ilgili ayrıntıları gösterir.


### <a name="failed-conditional-access"></a>Koşullu erişim başarısız

Bu senaryo genellikle, bir koşullu erişim ilkesinin gereksinimleri karşılanmadığı için başarısız olan bir oturum açma sonucudur. Sık karşılaşılan örnekler:

![Denetimleri gerektir](./media/overview-sign-in-diagnostics/require-controls.png)

- Karma Azure AD 'ye katılmış cihaz gerektir

- Onaylanan istemci uygulaması gerektir

- Uygulama koruma ilkesi gerektir   


Bu senaryonun tanılama bölümü Kullanıcı oturumu açma ve uygulanan ilkelerle ilgili ayrıntıları gösterir.


### <a name="mfa-from-conditional-access"></a>Koşullu erişimden MFA

Bu senaryo, Multi-Factor Authentication kümesi kullanarak oturum açma gereksinimini içeren bir koşullu erişim ilkesini temel alır.

![Multi-Factor Authentication gerektir](./media/overview-sign-in-diagnostics/require-mfa.png)

Bu senaryonun tanılama bölümü Kullanıcı oturumu açma ve uygulanan ilkelerle ilgili ayrıntıları gösterir.



### <a name="mfa-from-other-requirements"></a>Diğer gereksinimlerden MFA

Bu senaryo, koşullu erişim ilkesi tarafından zorlanan bir Multi-Factor Authentication gereksinimini temel alır. Örneğin, Kullanıcı başına çok faktörlü kimlik doğrulaması.


![Kullanıcı başına çok faktörlü kimlik doğrulaması gerektir](./media/overview-sign-in-diagnostics/mfa-per-user.png)


Bu tanı senaryosunun amacı, hakkında daha fazla ayrıntı sağlamaktır:

- Multi-Factor Authentication kesmesi kaynağı. 
- İstemci etkileşiminin sonucu.

Ayrıca, bu bölüm Kullanıcı oturum açma girişimine ilişkin tüm ayrıntıları da sağlar. 


### <a name="mfa-proof-up-required"></a>MFA sağlaması gerekli

Bu senaryo, Multi-Factor Authentication 'ı ayarlama istekleri tarafından kesintiye uğramış olan oturum açma işlemlerini temel alır. Bu kurulum "kanıtlama" olarak da bilinir.

Multi-Factor Authentication 'ı bir kullanıcının Multi-Factor Authentication kullanması gerektiğinde ancak henüz yapılandırmadığında veya bir yönetici kullanıcıyı yapılandırmak üzere yapılandırdıysa, çok faktörlü kimlik doğrulaması sağlaması oluşur.

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulama kesintisinin ayarlanmak ve kullanıcının kanıtları tamamlaması için öneri sağlaması için gereken öngörüyü sağlamaktır.

### <a name="mfa-proof-up-required-from-a-risky-sign-in"></a>Riskli oturum açma işleminden MFA sağlaması gerekiyor

Bu senaryo, riskli oturum açma işleminden çok faktörlü kimlik doğrulaması ayarlamak için bir istek tarafından kesintiye uğramış olan oturum açma işlemlerinin sonuçlarını alır. 

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulama kesintisini ayarlamaya yönelik bir fikir sağlamaktır, ancak bunu, kullanıcının kanıt için tamamlaması ve riskli görünmeyen bir ağ konumundan yapması önerilir. Örneğin, bir şirket ağı bir adlandırılmış konum olarak tanımlanırsa, bunun yerine kurumsal ağdan Prova yapmayı deneyin.


### <a name="successful-sign-in"></a>Başarılı oturum açma

Bu senaryo, koşullu erişim veya Multi-Factor Authentication tarafından kesintiye uğramayan oturum açma işlemlerini temel alır.

Bu tanı senaryosunun amacı, uygulanması beklenen bir koşullu erişim ilkesi veya ilkeler olması veya Kullanıcı oturum açma işlemini kesintiye uğratmak beklenen yapılandırılmış bir Multi-Factor Authentication olması durumunda kullanıcının oturum açma sırasında sağladıklarına ilişkin Öngörüler sağlamaktır.



## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory raporları nelerdir?](overview-reports.md)
* [Azure Active Directory izleme nedir?](overview-monitoring.md)
