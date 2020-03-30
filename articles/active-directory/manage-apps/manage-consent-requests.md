---
title: Uygulamalara onay verme ve onay isteklerini değerlendirme - Azure AD
description: Kullanıcı onayı devre dışı bırakıldığında veya kısıtlandığında onay isteklerini nasıl yönetirken ve kiracı çapındaki yönetici isteğinin bir uygulamaya nasıl değerlendirilebildiğini öğrenin.
services: active-directory
author: psignoret
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/27/2019
ms.author: mimart
ms.reviewer: phsignor
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0451fe18629a572c9b49f14924bfa50293f42a2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77367840"
---
# <a name="managing-consent-to-applications-and-evaluating-consent-requests"></a>Başvurulara onay verme ve onay isteklerini değerlendirme

Microsoft, son kullanıcı nın uygulamalara onay vermemasını [önerir.](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#restrict-user-consent-operations) Bu, karar verme sürecini kuruluşunuzun güvenlik ve kimlik yöneticisi ekibiyle merkezileştirir.

Son kullanıcı onayı devre dışı bırakıldıktan veya kısıtlandıktan sonra, kuruluşunuzun iş açısından kritik uygulamaların kullanılmasına izin verirken güvende kalmasını sağlamak için birkaç önemli husus vardır. Bu adımlar, kuruluşunuzun destek ekibi ve BT yöneticileri üzerindeki etkisini en aza indirmek ve üçüncü taraf uygulamalarda yönetilmeyen hesapların kullanımını önlemek için çok önemlidir.

## <a name="process-changes-and-education"></a>Süreç değişiklikleri ve eğitim

 1. Kullanıcıların doğrudan onay ekranından yönetici onayı istemesine izin vermek için [yönetici onayı iş akışını (önizlemeyi)](configure-admin-consent-workflow.md) etkinleştirmeyi düşünün.

 2. Tüm yöneticilerin [izinleri ve onay çerçevesini,](../develop/consent-framework.md) [onay isteminin](../develop/application-consent-experience.md) nasıl çalıştığını ve kiracı çapında yönetici onayı için bir talebi nasıl [değerlendireceklerini](#evaluating-a-request-for-tenant-wide-admin-consent)anladığını sağlayın.
 3. Kullanıcıların bir uygulama için yönetici onayı istemesi için kuruluşunuzun varolan süreçlerini gözden geçirin ve gerekirse güncelleştirmeler yapın. İşlemler değiştirilirse:
    * İlgili belgeleri, izlemeyi, otomasyonu ve benzeri bilgileri güncelleyin.
    * Etkilenen tüm kullanıcılara, geliştiricilere, destek ekiplerine ve BT yöneticilerine işlem değişiklikleri iletin.

## <a name="auditing-and-monitoring"></a>Denetim ve izleme

1. Daha önce hiçbir yersiz veya şüpheli uygulamanın verilere erişmediğinden emin olmak için [uygulamaları ve kuruluşunuzda verilen izinleri denetleyin.](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#audit-apps-and-consented-permissions)

2. OAuth onayı isteyen şüpheli uygulamalara karşı ek en iyi uygulamalar ve güvenlik önlemleri için [Office 365'teki Yasadışı İzin İzinlerini Algıla ve Düzeltin.](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants)

3. Kuruluşunuz uygun lisansa sahipse:

    * Microsoft [Cloud App Security'de ek OAuth uygulama denetimi özelliklerini](https://docs.microsoft.com/cloud-app-security/investigate-risky-oauth)kullanın.
    * İzinleri ve onay verme yle ilgili etkinlikleri [izlemek için Azure Monitor Çalışma Kitaplarını](../reports-monitoring/howto-use-azure-monitor-workbooks.md) kullanın. *Consent Insights* çalışma kitabı, uygulamaların başarısız onay istekleri sayısına göre bir görünüm sağlar. Bu, yöneticilerin gözden geçirmesi ve yönetici onayı verip vermemeye karar vermesi için başvuruları önceliklendirmek yararlı olabilir.

### <a name="additional-considerations-for-reducing-friction"></a>Sürtünmeyi azaltmak için ek hususlar

Halihazırda kullanılmakta olan güvenilir, iş açısından kritik uygulamalar üzerindeki etkiyi en aza indirmek için, çok sayıda kullanıcı onayı verilen uygulamalara yönetici onayı vermeyi proaktif olarak düşünün:

1. Oturum açma günlüklerine veya onay verme hibe etkinliğine bağlı olarak, kuruluşunuza yüksek kullanımla eklenen uygulamaların envanterini alın. PowerShell [komut dosyası,](https://gist.github.com/psignoret/41793f8c6211d2df5051d77ca3728c09) çok sayıda kullanıcı onayı hibesi ile uygulamaları hızlı ve kolay bir şekilde keşfetmek için kullanılabilir.

2. Henüz yönetici onayı verilmeyen en iyi uygulamaları değerlendirin.

   > [!IMPORTANT]
   > Kuruluştaki birçok kullanıcı kendileri için izin vermiş olsa bile, kiracı çapında yönetici onayı vermeden önce bir uygulamayı dikkatle değerlendirin.

3. Onaylanan her uygulama için, aşağıda belgelenen yöntemlerden birini kullanarak kiracı çapında yönetici onayı verin.

4. Onaylanan her uygulama için [kullanıcı erişimini kısıtlamayı](configure-user-consent.md)düşünün.

## <a name="evaluating-a-request-for-tenant-wide-admin-consent"></a>Kiracı çapında yönetici onayı için bir istek değerlendirme

Kiracı çapında yönetici onayı vermek hassas bir işlemdir.  İzinler tüm kuruluş adına verilir ve yüksek ayrıcalıklı işlemleri deneme izinleri içerebilir. Örneğin, rol yönetimi, tüm posta kutularına veya tüm sitelere tam erişim ve tam kullanıcı kimliğe bürünme.

Kiracı genelinde yönetici onayı vermeden önce, size erişim düzeyi için uygulama ve uygulama yayımcısına güvendiğinizi garanti edin. Uygulamayı kimin kontrol ettiğini ve uygulamanın neden izin istediğini anladığınızdan emin değilseniz, *izin vermeyin.*

Aşağıdaki liste, yönetici onayı verme isteğini değerlendirirken göz önünde bulundurulması gereken bazı öneriler sağlar.

* **Microsoft kimlik platformundaki [izinleri ve onay çerçevesini](../develop/consent-framework.md) anlayın.**

* **[Devredilen izinler ile uygulama izinleri](../develop/v2-permissions-and-consent.md#permission-types)arasındaki farkı anlayın.**

   Uygulama izinleri, uygulamanın herhangi bir kullanıcı etkileşimi olmaksızın tüm kuruluş için verilere erişmesine olanak sağlar. Yetki belgesi, uygulamanın bir noktada uygulamaya imza lanmış bir kullanıcı adına hareket etmesine olanak sağlar.

* **İstenilen izinleri anlayın.**

   Başvuru tarafından istenen izinler [onay isteminde](../develop/application-consent-experience.md)listelenir. İzin başlığının genişletilmesi, izin açıklamasını görüntüler. Uygulama izinleri için açıklama genellikle "oturum açmış bir kullanıcı olmadan" sona erer. Devredilen izinler için açıklama genellikle "oturum açmış kullanıcı adına" ile sona erer. Microsoft Graph API'si için izinler [Microsoft Graph Permissions Reference] olarak açıklanmıştır- diğer API'lerin maruz kaldıkları izinleri anlamak için belgelere bakın.

   İstenilen bir izni anlamıyorsanız, *izin vermeyin.*

* **Hangi uygulamanın izin istediğini ve uygulamayı kimin yayımladığını anlayın.**

   Diğer uygulamalar gibi görünmeye çalışan kötü amaçlı uygulamalara karşı dikkatli olun.

   Bir uygulamanın veya yayımcının meşruiyetinden şüphe ediyorsanız, *izin vermeyin.* Bunun yerine, ek onay isteyin (örneğin, doğrudan uygulama yayımcısından).

* **İstenen izinlerin uygulamadan beklediğiniz özelliklere uygun olduğundan emin olun.**

   Örneğin, SharePoint site yönetimini sunan bir uygulama, tüm site koleksiyonlarını okumak için temsilci erişimi gerektirebilir, ancak tüm posta kutularına veya dizindeki tam kimliğe bürünme ayrıcalıklarına tam erişim gerekmez.

   Uygulamanın gerekenden daha fazla izin istediğinden şüpheleniyorsanız, *izin vermeyin.* Daha fazla ayrıntı edinmek için uygulama yayımcısına başvurun.

## <a name="granting-consent-as-an-administrator"></a>Yönetici olarak onay verme

### <a name="granting-tenant-wide-admin-consent"></a>Kiracı çapında yönetici onayı verilmesi

Azure portalından, Azure AD PowerShell'i kullanarak veya onay isteminin kendisinden kiracı çapında yönetici onayı vermek için adım adım talimat lar için grant [kiracı çapında](grant-admin-consent.md) yönetici onayı verilmesine bakın.

### <a name="granting-consent-on-behalf-of-a-specific-user"></a>Belirli bir kullanıcı adına onay verme

Bir yönetici, tüm kuruluş için onay vermek yerine, tek bir kullanıcı adına verilen izinlere izin vermek için [Microsft Graph API'yi](https://docs.microsoft.com/graph/use-the-api) de kullanabilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/graph/auth-v2-user)

## <a name="limiting-user-access-to-applications"></a>Uygulamalara kullanıcı erişimini sınırlama

Kiracıların geneline onay verilmiş olsa bile kullanıcıların uygulamalara erişimi yine de sınırlı olabilir. Bir uygulamaya kullanıcı ataması nasıl gerektireceğiniz hakkında daha fazla bilgi [için, kullanıcı ve grup atama yöntemlerine](methods-for-assigning-users-and-groups.md)bakın.

Ek karmaşık senaryoların nasıl işleyeceğiniz de dahil olmak üzere daha geniş bir genel bakış [için, uygulama erişim yönetimi için Azure AD'yi kullanma](what-is-access-management.md)tarihine bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik altyapınızı güvence altına almak için beş adım](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#before-you-begin-protect-privileged-accounts-with-mfa)

[Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)

[Son kullanıcıların uygulamalara nasıl onay verme sini yapılandırın](configure-user-consent.md)

[Microsoft kimlik platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow'da Azure REKLAM](https://stackoverflow.com/questions/tagged/azure-active-directory)