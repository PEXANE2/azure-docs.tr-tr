---
title: Uygulama onayını yönetme ve izin isteklerini değerlendirme-Azure AD
description: Kullanıcı onayı devre dışı bırakıldığında veya kısıtlandıktan sonra izin isteklerini yönetmeyi ve bir uygulamaya kiracı genelinde yönetici onayı için bir istek değerlendirmeyi öğrenin.
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
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367840"
---
# <a name="managing-consent-to-applications-and-evaluating-consent-requests"></a>Uygulamalara onayı yönetme ve izin isteklerini değerlendirme

Microsoft, uygulamalara son kullanıcı onayını devre dışı [bırakmanızı öneriyor](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#restrict-user-consent-operations) . Bu, karar verme sürecini kuruluşunuzun güvenlik ve kimlik yöneticisi ekibi ile merkezileştirmek için kullanılır.

Son Kullanıcı izni devre dışı bırakıldıktan veya kısıtlandıktan sonra, iş açısından kritik uygulamaların kullanılmasına hala izin verirken kuruluşunuzun güvende kalmasını sağlamaya yönelik birkaç önemli nokta vardır. Bu adımlar, kuruluşunuzun destek ekibi ve BT yöneticileri üzerindeki etkiyi en aza indirmek için önemlidir, ancak üçüncü taraf uygulamalarda yönetilmeyen hesapların kullanılmasını önler.

## <a name="process-changes-and-education"></a>Değişiklikleri ve eğitimi işleme

 1. Kullanıcıların doğrudan onay ekranından yönetici onayı istemesine izin vermek için [yönetici onayı iş akışını (Önizleme)](configure-admin-consent-workflow.md) etkinleştirmeyi düşünün.

 2. Tüm yöneticilerin [izinleri ve onay çerçevesini](../develop/consent-framework.md)anlamalarına, [izin isteminin](../develop/application-consent-experience.md) nasıl çalıştığını ve [kiracı genelindeki yönetici onayı için bir isteğin nasıl değerlendirileceğini](#evaluating-a-request-for-tenant-wide-admin-consent)doğrulayın.
 3. Kullanıcıların bir uygulama için yönetici onayı istemesi ve gerekirse güncelleştirmeler yapması için kuruluşunuzun mevcut süreçlerini gözden geçirin. İşlem değiştirilirse:
    * İlgili belgeleri, izlemeyi, Otomasyonu vb. güncelleştirin.
    * Tüm etkilenen kullanıcılar, geliştiriciler, destek takımları ve BT yöneticileri için işlem değişikliklerini iletişim kurun.

## <a name="auditing-and-monitoring"></a>Denetleme ve izleme

1. Daha önce verilere erişim izni verilmemesini sağlamak için [uygulamaları denetleyin ve kuruluşunuzdaki izinleri](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#audit-apps-and-consented-permissions) kabul edin.

2. Ek en iyi uygulamalar ve OAuth onayı isteyen şüpheli uygulamalara karşı koruma için, [Office 365 ' de Illicit onay Izin verdiği Izinleri algılayın ve düzeltin](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants) .

3. Kuruluşunuzun uygun lisansı varsa:

    * Microsoft Cloud App Security ek [OAuth uygulama denetimi özellikleri](https://docs.microsoft.com/cloud-app-security/investigate-risky-oauth)kullanın.
    * [İzinleri ve onayı ilgili etkinlikleri izlemek Için Azure Izleyici çalışma kitaplarını](../reports-monitoring/howto-use-azure-monitor-workbooks.md) kullanın. *Onay öngörüleri* çalışma kitabı, başarısız izin istekleri sayısına göre uygulamaların bir görünümünü sağlar. Bu, yöneticilerin Yöneticiler için izin vermesini ve BT yöneticilerine yönetici izni verip vermeyeceğine karar vermesini sağlamak için yararlı olabilir.

### <a name="additional-considerations-for-reducing-friction"></a>Uçuşmayı azaltmaya yönelik ek konular

Zaten kullanımda olan güvenilen, iş açısından kritik uygulamalar üzerindeki etkileri en aza indirmek için, yüksek sayıda kullanıcı izni veren uygulamalara yönetici onayı vermeyi tercih etmeyi düşünün:

1. Oturum açma günlükleri veya onay verme etkinliğine bağlı olarak, yüksek kullanım ile kuruluşunuza zaten eklenmiş olan uygulamaların envanterini alın. PowerShell [betiği](https://gist.github.com/psignoret/41793f8c6211d2df5051d77ca3728c09) , çok sayıda kullanıcı izin verimiyle uygulamaları hızlı ve kolay bir şekilde keşfetmesi için kullanılabilir.

2. Henüz yönetici izni verilmemiş olan en iyi uygulamaları değerlendirin.

   > [!IMPORTANT]
   > Kuruluştaki çok sayıda kullanıcının kendileri için zaten onay vermiş olması durumunda bile, kiracı genelinde yönetici onayı vermeden önce bir uygulamayı dikkatle değerlendirin.

3. Onaylanan her uygulama için aşağıda belgelenen yöntemlerden birini kullanarak kiracı genelinde yönetici onayı verin.

4. Onaylanan her uygulama için, [Kullanıcı erişimini kısıtlamayı](configure-user-consent.md)göz önünde bulundurun.

## <a name="evaluating-a-request-for-tenant-wide-admin-consent"></a>Kiracı genelinde yönetici onayı için bir isteği değerlendirme

Kiracı genelinde yönetici onayı verme, hassas bir işlemdir.  İzinler tüm kuruluş adına verilecek ve yüksek ayrıcalıklı işlemler gerçekleştirmeye yönelik izinleri içerebilir. Örneğin, rol yönetimi, tüm posta kutularına veya tüm sitelere tam erişim ve tam kullanıcı kimliğine bürünme.

Kiracı genelinde yönetici onayı vermeden önce, verdiğiniz erişim düzeyi için uygulamaya ve uygulama yayımcısına güvendiğinizden emin olmanız gerekir. Uygulamayı kimin denetlediğini ve uygulamanın neden izinleri istediğini anladığınızdan emin değilseniz, *izin vermeyin*.

Aşağıdaki listede, yönetici onayı verme isteği değerlendirilirken göz önünde bulundurmanız gereken bazı öneriler verilmiştir.

* **Microsoft Identity platformunda [izinleri ve onay çerçevesini](../develop/consent-framework.md) anlayın.**

* **[Temsilci izinleri ve uygulama izinleri](../develop/v2-permissions-and-consent.md#permission-types)arasındaki farkı anlayın.**

   Uygulama izinleri, uygulamanın tüm Kullanıcı etkileşimi olmadan tüm kuruluş için verilere erişmesine izin verir. Temsilci izinleri, uygulamanın bir noktada uygulamaya imzalandığı bir kullanıcı adına işlem yapmasına izin verir.

* **İstenen izinleri anlayın.**

   Uygulama tarafından istenen izinler [onay isteminde](../develop/application-consent-experience.md)listelenir. İzin başlığını genişleterek izin açıklaması görüntülenir. Uygulama izinlerinin açıklaması genellikle "oturum açmış kullanıcı olmadan" ile sona acaktır. Temsilci izinleri için açıklama genellikle "oturum açmış kullanıcı adına" ile sona acaktır. Microsoft Graph API izinleri [Microsoft Graph Izinleri başvurusu] içinde açıklanmaktadır-sergiledikleri izinleri anlamak için diğer API 'Lerin belgelerine bakın.

   İstenmekte olan bir izni anladıysanız izin *vermeyin*.

* **Hangi uygulamanın izin istediğini ve uygulamayı yayımladığınızı anlayın.**

   Diğer uygulamalar gibi görünmeye çalışan kötü amaçlı uygulamalar konusunda dikkatli olun.

   Bir uygulamanın veya yayımcısının yasallığını şüpheederseniz *izin vermeyin*. Bunun yerine, ek onay arayın (örneğin, doğrudan uygulama yayımcısı).

* **İstenen izinlerin uygulamadan beklemiş özelliklerle uyumlu olduğundan emin olun.**

   Örneğin, SharePoint site yönetimi sunan bir uygulama, tüm site koleksiyonlarını okumak için temsilci erişimi gerektirebilir, ancak tüm posta kutularına tam erişim veya dizindeki tam kimliğe bürünme ayrıcalıklarına ihtiyaç duymayabilir.

   Uygulamanın gerekenden daha fazla izin istediğini kuşkulanıyorsanız, *izin vermeyin*. Daha fazla bilgi edinmek için uygulama yayımcısına başvurun.

## <a name="granting-consent-as-an-administrator"></a>Yönetici olarak izin verme

### <a name="granting-tenant-wide-admin-consent"></a>Kiracı genelinde yönetici onayı verme

Bkz. Azure AD PowerShell kullanarak veya onay isteminin kendisinden Azure portal Kiracı genelinde yönetici onayı verme konusunda adım adım yönergeler için bkz. [bir uygulamaya kiracı genelinde yönetici onayı verme](grant-admin-consent.md) .

### <a name="granting-consent-on-behalf-of-a-specific-user"></a>Belirli bir kullanıcı adına izin verme

Yönetici, tüm kuruluş için izin vermek yerine, tek bir kullanıcı adına temsilci izinleri izni vermek için [Microsft Graph API](https://docs.microsoft.com/graph/use-the-api) de kullanabilir. Daha fazla bilgi için bkz. [Kullanıcı adına erişim edinme](https://docs.microsoft.com/graph/auth-v2-user).

## <a name="limiting-user-access-to-applications"></a>Kullanıcıların uygulamalara erişimini sınırlandırma

Kullanıcıların uygulamalara erişimi, kiracı genelinde yönetici izni verildiğinde bile hala sınırlı olabilir. Bir uygulamaya Kullanıcı Ataması gerektirme hakkında daha fazla bilgi için bkz. [kullanıcıları ve grupları atamaya yönelik yöntemler](methods-for-assigning-users-and-groups.md).

Ek karmaşık senaryoların nasıl işleneceği hakkında daha geniş bir genel bakış için bkz. [uygulama erişim yönetimi Için Azure ad kullanma](what-is-access-management.md).

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik altyapınızı güvenli hale getirmenin beş adımı](https://docs.microsoft.com/azure/security/fundamentals/steps-secure-identity#before-you-begin-protect-privileged-accounts-with-mfa)

[Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)

[Son kullanıcıların uygulamalara nasıl izin vermesini yapılandırın](configure-user-consent.md)

[Microsoft Identity platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow üzerinde Azure AD](https://stackoverflow.com/questions/tagged/azure-active-directory)