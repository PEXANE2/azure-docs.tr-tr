---
title: Azure AD 'de oturum açma tanılaması nelerdir? | Microsoft Belgeleri
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
ms.openlocfilehash: e113753bee5c0a94fbec47a2ea14b98c1779a394
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97578180"
---
# <a name="what-are-sign-in-diagnostics-in-azure-ad"></a>Azure AD 'de oturum açma tanılaması nelerdir?

Azure AD, kullanıcıların yönetilen kaynaklarla neler yapabileceğini denetlemek için esnek bir güvenlik modeli sağlar. Bu kaynaklara erişim, yalnızca kim olduğunuz ve aynı zamanda bunlara nasıl erişbir şekilde denetlenemez. Genellikle esneklik, sahip olduğunuz yapılandırma seçeneklerinin sayısı nedeniyle belirli bir karmaşıklık derecesi ile birlikte gelir. Karmaşıklık, hatalara karşı riski artırmak için olası bir isteklidir.

BT Yöneticisi olarak, sorunları ortaya çıktığında kolayca tanılamanıza ve çözebilmeniz için, sisteminizdeki etkinliklere yönelik doğru Öngörüler sunan bir çözüme ihtiyacınız vardır. Azure AD 'de oturum açma Tanılaması, böyle bir araç için bir örnektir. Bir oturum açma sırasında ne olduğunu çözümlemek için tanılamayı ve Microsoft desteği 'ni kullanmak zorunda kalmadan sorunları çözmek için gerçekleştirebileceğiniz eylemleri analiz edin.

Bu makalede, bu aracın ne yaptığını ve bunu nasıl kullanabileceğinizi gösteren bir genel bakış sunulmaktadır.


## <a name="requirements"></a>Gereksinimler

Oturum açma tanılaması Azure AD 'nin tüm sürümlerinde kullanılabilir.<br> Bu aracı kullanmak için Azure AD 'de bir genel yönetici üyesi olmanız gerekir.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD 'de, oturum açma girişimine olan yanıt, kiracınıza nasıl eriş, ancak aynı zamanda kiracınıza bağlı değildir. Örneğin, bir yönetici olarak, şirket ağınızdan oturum açtığınızda kiracınızın tüm yönlerini yapılandırabilirsiniz. Ancak, güvenilmeyen bir ağdan aynı hesabı kullanarak oturum açtığınızda bile engellenmiş olabilirsiniz. Sistemin bir oturum açma girişimine yanıt vermek daha fazla esneklik nedeniyle, oturum açma denemelerinde sorun gidermeye gerek duyduğunuz senaryolarda uçtan uca kalabilirsiniz. Oturum açma Tanılaması, oturum açma işleminden verileri çözümleyen ve ne olduğu hakkında belirli iletiler sunan bir özelliktir ve sorunları çözmek için ne yapmanız gerekenler hakkında öneriler sağlar. Azure AD 'de oturum açma Tanılaması, oturum açma hatalarının kendinden tanılamasını sağlamak için tasarlanmıştır. Tanılama işlemi dört ana yapı blobundan oluşur:

![Oturum açma tanılama işlemi](./media/overview-sign-in-diagnostics/process.png)
 
1. İlgilendiğiniz oturum açma olaylarının kapsamını **tanımlayın**

2. İncelemek istediğiniz olayı **seçin**

3. Tanısı **gözden geçir**

4. Eylem **yapın**

 
### <a name="define-sign-ins"></a>Oturum açma işlemlerini tanımlama

Bu adımın amacı, araştırmak istediğiniz oturum açma olaylarının kapsamını tanımlamaktır. Kapsamınız bir kullanıcı veya tanımlayıcıyı (CorrelationId, RequestId) ve zaman aralığını temel alır. Ayrıca, bir uygulama adı da belirtebilirsiniz. Azure AD size doğru olayları bulmak için kapsam bilgilerini kullanır.  

### <a name="select-sign-in-event"></a>Oturum açma olayı seçin 

Azure AD, arama ölçütlerinize göre tüm eşleşen oturum açma olaylarını alır ve bunları bir kimlik doğrulaması özet listesi görünümünde gösterir. 

![Kimlik doğrulama özeti](./media/overview-sign-in-diagnostics/authentication-summary.png)
 
Bu görünümde görüntülenecek sütunları özelleştirebilirsiniz.

### <a name="review-diagnosis"></a>Tanısı gözden geçir

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

Bu senaryo, koşullu erişim ilkesi tarafından engellenmiş bir oturum açma işlemi nedeniyle oluşur.

![Erişimi engelleme](./media/overview-sign-in-diagnostics/block-access.png)

Tanılama bölümünde Kullanıcı oturum açma ve uygulanan ilkelerle ilgili ayrıntılar gösterilir.


### <a name="failed-conditional-access"></a>Koşullu erişim başarısız

Bu senaryo genellikle, bir koşullu erişim ilkesinin gereksinimleri karşılanmadığı için başarısız olan bir oturum açma sonucudur. Sık karşılaşılan örnekler:

![Denetimleri gerektir](./media/overview-sign-in-diagnostics/require-controls.png)

- Karma Azure AD 'ye katılmış cihaz gerektir

- Onaylanan istemci uygulaması gerektir

- Uygulama koruma ilkesi gerektir   


Tanılama, Kullanıcı oturumu açma ve uygulanan ilke ya da ilkelerle ilgili ayrıntıları sunar.


### <a name="mfa-from-conditional-access"></a>Koşullu erişimden MFA

Bu senaryo, Multi-Factor Authentication kümesi kullanarak oturum açma gereksinimini içeren bir koşullu erişim ilkesi nedeniyle oluşur.

![Multi-Factor Authentication gerektir](./media/overview-sign-in-diagnostics/require-mfa.png)

Tanılama bölümünde Kullanıcı oturum açma ve uygulanan ilkelerle ilgili ayrıntılar gösterilir.



### <a name="mfa-from-other-requirements"></a>Diğer gereksinimlerden MFA

Bu senaryo, çok faktörlü kimlik doğrulaması bir koşullu erişim ilkesi tarafından zorlandığında oluşur. Örneğin, kiracınızda Kullanıcı başına temelinde Multi-Factor Authentication 'ı yapılandırdınız.


![Kullanıcı başına çok faktörlü kimlik doğrulaması gerektir](./media/overview-sign-in-diagnostics/mfa-per-user.png)


Bu tanı senaryosunun amacı, hakkında daha fazla ayrıntı sağlamaktır:

- Multi-Factor Authentication kesmesi kaynağı. 
- İstemci etkileşiminin sonucu.

Ayrıca, bu bölüm Kullanıcı oturum açma girişimine ilişkin tüm ayrıntıları da sağlar. 


### <a name="mfa-proof-up-required"></a>MFA sağlaması gerekli

Bu senaryo, Multi-Factor Authentication 'ı ayarlama istekleri tarafından kesintiye uğramış olan oturum açma işlemlerinin sonucu olarak sonuçlanır. Bu kurulum işlemi "kanıtlama" olarak da bilinir.

Multi-Factor Authentication kanıtlama, bir kullanıcının Multi-Factor Authentication kullanması gerektiğinde, ancak daha önce hiç Ayarlamamışsa veya bir yönetici kullanıcıyı ayarlama gerektirecek şekilde yapılandırdıysa oluşur.

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulama kesintisinin ayarlanmak ve kullanıcının kanıtları tamamlaması için öneri sağlaması için gereken öngörüyü sağlamaktır.

### <a name="mfa-proof-up-required-but-user-sign-in-attempt-is-not-from-secure-location"></a>MFA sağlaması gerekli ancak kullanıcı oturum açma denemesi güvenli konumdan değil

Bu senaryo, Multi-Factor Authentication 'ı ayarlama isteği tarafından kesintiye uğramış, ancak oturum açma riskli olarak görüldüğü oturum açma işlemlerinin sonuçlarını elde ediyor. 

Multi-Factor Authentication kanıtlama, bir kullanıcının Multi-Factor Authentication kullanması gerektiğinde, ancak daha önce hiç Ayarlamamışsa veya bir yönetici kullanıcıyı ayarlama gerektirecek şekilde yapılandırdıysa oluşur.

Bu tanı senaryosunun amacı, çok faktörlü kimlik doğrulama kesintisini ayarlamaya yönelik bir fikir sağlamaktır, ancak bunu, kullanıcının kanıt için tamamlaması ve riskli görünmeyen bir ağ konumundan yapması önerilir. Örneğin, bir şirket ağı bir adlandırılmış konum olarak tanımlanırsa, bunun yerine kurumsal ağdan Prova yapmayı deneyin.


### <a name="successful-sign-in"></a>Başarılı oturum açma

Bu senaryo, koşullu erişim veya Multi-Factor Authentication 'dan kesinti olmadan Azure AD oturum açma konularını ele alır.

Bu tanı senaryosunun amacı, uygulanması beklenen bir koşullu erişim ilkesi veya ilkeler olması veya Kullanıcı oturum açma işlemini kesintiye uğratmak beklenen yapılandırılmış bir Multi-Factor Authentication olması durumunda kullanıcının oturum açma sırasında sağladıklarına ilişkin Öngörüler sağlamaktır.



## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory raporları nelerdir?](overview-reports.md)
* [Azure Active Directory izleme nedir?](overview-monitoring.md)
