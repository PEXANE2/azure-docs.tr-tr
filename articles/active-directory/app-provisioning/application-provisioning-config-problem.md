---
title: Azure AD Galeri uygulamasına kullanıcı sağlamayı yapılandırma sorunu
description: Azure AD uygulama galerisinde zaten listelenen bir uygulamaya Kullanıcı sağlamayı yapılandırırken karşılaştığı yaygın sorunları giderme
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: asteen
ms.openlocfilehash: e6fb5fbecc9b2917f4fde2d1ccb76d6962a0af18
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82593973"
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulamasına kullanıcı sağlamayı yapılandırma sorunu

Bir uygulama için [Otomatik Kullanıcı sağlamayı](user-provisioning.md) yapılandırma (desteklenir), uygulamayı otomatik sağlama için hazırlamak üzere belirli yönergelerin izlenmesini gerektirir. Daha sonra, Kullanıcı hesaplarını uygulamayla eşitlemeye yönelik sağlama hizmetini yapılandırmak için Azure portal kullanabilirsiniz.

Uygulamanız için sağlamayı ayarlamaya özgü kurulum öğreticisini bularak her zaman başlamanız gerekir. Ardından, sağlama bağlantısı oluşturmak için hem uygulamayı hem de Azure AD 'yi yapılandırmak için bu adımları izleyin. [SaaS uygulamalarını Azure Active Directory Ile tümleştirme hakkında öğreticiler listesinde](../saas-apps/tutorial-list.md)uygulama öğreticilerinin listesi bulunabilir.

## <a name="how-to-see-if-provisioning-is-working"></a>Sağlamanın çalışıp çalışmadığını nasıl görebileceğinizi belirleme 

Hizmet yapılandırıldıktan sonra, hizmetin çalışması ile ilgili birçok öngörü iki konumdan çizilebilirler:

-   **Sağlama günlükleri (Önizleme)** – sağlama [günlüğü](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) , sağlama kapsamında olan ATANMıŞ kullanıcılar için Azure AD sorgulama da dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder. Bu kullanıcıların varlığı için hedef uygulamayı sorgulayın ve sistem arasındaki kullanıcı nesnelerini karşılaştırır. Ardından, karşılaştırma temelinde hedef sistemde Kullanıcı hesabını ekleyin, güncelleştirin veya devre dışı bırakın. **Etkinlik** bölümünde **Kurumsal uygulamalar** &gt; **sağlama günlükleri (Önizleme)** **Azure Active Directory** &gt; ' ni seçerek Azure Portal sağlama günlüklerine erişebilirsiniz.

-   **Geçerli durum –** Belirli bir uygulama için son sağlama çalıştırmasının Özeti, ekranın alt kısmındaki **Azure Active Directory &gt; kurumsal uygulamalar &gt; \[\] &gt;uygulama adı sağlama** bölümünde görülebilir. Geçerli durum bölümü, bir sağlama döngüsünün Kullanıcı hesapları sağlamaya başlayıp başlamadığına ilişkin gösterir. Döngüsünün ilerlemesini izleyebilir, kaç Kullanıcı ve grup sağlandığını görebilir ve kaç rolün oluşturulduğunu görebilirsiniz. Herhangi bir hata oluşursa, [sağlama günlükleri (.. /Reports-Monitoring/Concept-provisioning-logs.MD? Context = Azure/Active-Directory/Manage-Apps/Context/Manage-Apps-context).

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate alınması gereken genel sorun alanı

Aşağıda, nereden başlayabileceğiniz hakkında bir fikriniz varsa, detaya gidebileceğiniz genel sorun alanlarının bir listesi verilmiştir.

* [Sağlama hizmeti başlatma olarak görünmüyor](#provisioning-service-does-not-appear-to-start)
* Uygulama kimlik bilgileri çalışmaması nedeniyle yapılandırma kaydedilemiyor
* [Sağlama günlükleri, atanmış olsalar dahi, kullanıcıların "atlandı" ve sağlanmamış olduğunu söylüyor](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatma olarak görünmüyor

**Sağlama durumunu** , Azure Portal **Azure Active Directory &gt; Enterprise &gt; \[Apps uygulama adı\] &gt;sağlama** **bölümünde olacak şekilde** ayarlarsanız. Ancak, sonraki yeniden yüklemeden sonra bu sayfada başka bir durum ayrıntısı gösterilmez. Hizmetin çalışıyor olması, ancak henüz bir başlangıç döngüsünü tamamlamamasından kaynaklanıyor olabilir. Hizmetin hangi işlemleri yaptığını ve herhangi bir hata olup olmadığını belirlemek için yukarıda açıklanan **sağlama günlüklerini** denetleyin.

>[!NOTE]
>Bir başlangıç döngüsünün, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakikadan birkaç saate kadar herhangi bir zaman geçmesi gerekir. İlk döngüden sonra sonraki eşitlemeler daha hızlı olur, çünkü sağlama hizmeti başlangıç döngüsünden sonra her iki sistemin durumunu temsil eden filigranları depolar, sonraki eşitlemeler performansını geliştirir.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Uygulama kimlik bilgileri çalışmaması nedeniyle yapılandırma kaydedilemiyor

Sağlamanın çalışması için Azure AD, uygulamanın bu uygulama tarafından verilen bir kullanıcı yönetimi API 'sine bağlanmasına izin veren geçerli kimlik bilgileri gerektirir. Bu kimlik bilgileri işe yaradığını veya ne olduğunu bilmiyorsanız, daha önce açıklanan bu uygulamayı ayarlama öğreticisini gözden geçirin.

## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Kaynak sağlama, kullanıcıların atlandığını ve bunlara atanmış olsalar da sağlanmadığını söyleyin

Bir Kullanıcı sağlama günlüklerinde "atlandı" olarak görünüyorsa, nedenini öğrenmek için günlük iletisindeki genişletilmiş ayrıntıları okumak çok önemlidir. Aşağıda yaygın nedenler ve çözümler verilmiştir:

- Bir **öznitelik değerine göre kullanıcıya filtre**uygulanan **bir kapsam filtresi yapılandırıldı** . Daha fazla bilgi için bkz. [kapsam filtreleri Ile öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

- **Kullanıcı "etkin değil" olarak belirlenir.** Bu özel hata iletisini görürseniz, bunun nedeni Azure AD 'de depolanan kullanıcı atama kaydında bir sorun olması olabilir. Bu sorunu onarmak için, Kullanıcı (veya grubu) uygulamadan atamayı kaldırın ve yeniden atayın. Daha fazla bilgi için bkz. [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md).

- **Gerekli bir öznitelik eksik veya Kullanıcı için doldurulmamış.** Sağlama yaparken göz önünde bulundurmanız gereken önemli bir şey, Azure AD 'den uygulamaya hangi kullanıcı (veya grup) özelliklerinin akmasını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmek ve yapılandırmak için gereken bir şeydir. Bu, iki sistem arasında kullanıcıları/grupları benzersiz şekilde tanımlamak ve eşleştirmek için kullanılan "eşleşen özelliği" ayarlamayı içerir. Bu önemli süreç hakkında daha fazla bilgi için bkz. [Kullanıcı sağlama özniteliği-eşlemeler özelleştirme](../app-provisioning/customize-application-attributes.md).

  * **Gruplar Için öznitelik eşlemeleri:** Bazı uygulamalarda destekleniyorsa, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen Grup nesneleri **eşlemesini** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. Sağlama grupları etkinleştirilmişse, "eşleşen KIMLIK" için uygun bir alanın kullanıldığından emin olmak üzere öznitelik eşlemelerini gözden geçirdiğinizden emin olun. Bu, görünen ad veya e-posta diğer adı olabilir), eşleşen özellik boşsa veya Azure AD 'de bir grup için doldurulmamışsa grup ve üyeleri sağlanmamıştır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS uygulamalarına Kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatikleştirme](user-provisioning.md)
