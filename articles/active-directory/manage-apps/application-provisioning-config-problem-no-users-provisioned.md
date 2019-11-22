---
title: Azure AD Galeri uygulamasına hiçbir Kullanıcı sağlanmıyor
description: Azure AD ile Kullanıcı sağlaması için yapılandırdığınız bir Azure AD Galeri uygulamasında görünen kullanıcıları görmezseniz karşılaşılan yaygın sorunları giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1683f645e1ed81a8a415214fbb83ee80572be3ee
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275834"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulamasına hiçbir Kullanıcı sağlanmıyor
Bir uygulama için otomatik sağlama yapılandırıldıktan sonra (Azure AD 'ye uygulamaya bağlanmak için sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulama dahil), kullanıcılar ve/veya grupları uygulamaya sağlanır. Sağlama aşağıdaki noktalara göre belirlenir:

-   Uygulamaya **atanan** kullanıcılar ve gruplar. İç içe gruplar veya Office 365 gruplarının sağlanması desteklenmez. Atama hakkında daha fazla bilgi için bkz. [Azure Active Directory bir kurumsal uygulamaya Kullanıcı veya Grup atama](assign-user-or-group-access-portal.md).
-   **Öznitelik eşlemelerinin** etkinleştirilip etkinleştirilmediği ve geçerli ÖZNITELIKLERI Azure AD 'den uygulamaya eşitlemek üzere yapılandırılıp yapılandırılmadığını belirtir. Öznitelik eşlemeleri hakkında daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulamaları Için Kullanıcı hazırlama öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).
-   Belirli öznitelik değerlerine göre kullanıcılara filtre uygulayan bir **kapsam filtresi** olup olmadığı. Kapsam filtreleri hakkında daha fazla bilgi için bkz. [kapsam filtreleriyle öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md).
  
Kullanıcıların sağlanmakta olmadığını gözlemlerseniz, Azure AD 'de [sağlama günlükleri (Önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) bölümüne başvurun. Belirli bir kullanıcı için günlük girişlerini arayın.

**Etkinlik** bölümünde &gt; **Kurumsal uygulamalar** &gt; **sağlama günlükleri (Önizleme)** **Azure Active Directory** ' ni seçerek Azure Portal sağlama günlüklerine erişebilirsiniz. Sağlama verilerini kullanıcı adına veya kaynak sistemde ya da hedef sistemde tanımlayıcı temelinde arayabilirsiniz. Ayrıntılar için bkz. [sağlama günlükleri (Önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context). 

Sağlama günlükleri, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder ve sağlama için kapsamdaki kullanıcılara yönelik Azure AD sorgulama, hedef uygulamayı bu kullanıcıların varlığına sorgulama ve kullanıcı nesnelerini arasında karşılaştırma gibi sistem. Ardından, karşılaştırma temelinde hedef sistemde Kullanıcı hesabını ekleyin, güncelleştirin veya devre dışı bırakın.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate alınması gereken genel sorun alanı
Aşağıda, nereden başlayabileceğiniz hakkında bir fikriniz varsa, detaya gidebileceğiniz genel sorun alanlarının bir listesi verilmiştir.

- [Sağlama hizmeti başlatma olarak görünmüyor](#provisioning-service-does-not-appear-to-start)
- [Sağlama günlükleri, atanmış olsalar dahi, kullanıcıların atlandığını ve sağlanmadığını belirtdiklerini varsayalım](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatma olarak görünmüyor
**Sağlama durumunu** **Azure Active Directory &gt; Enterprise Apps &gt; \[uygulama adı\] &gt;sağlama** **bölümünde olacak şekilde** ayarlarsanız. Ancak, sonraki yeniden yüklemeden sonra bu sayfada başka bir durum ayrıntısı gösterilmez, büyük olasılıkla hizmetin çalışıyor olması ancak henüz bir başlangıç döngüsünü tamamlamamamasından kaynaklanıyor olabilir. Hizmetin hangi işlemleri yaptığını ve herhangi bir hata olup olmadığını belirlemek için yukarıda açıklanan **sağlama günlüklerini (Önizleme)** denetleyin.

>[!NOTE]
>Bir başlangıç döngüsünün, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakikadan birkaç saate kadar herhangi bir zaman geçmesi gerekir. İlk döngüden sonraki eşitlemeler daha hızlı olduktan sonra, sağlama hizmeti ilk döngüden sonra her iki sistemin durumunu temsil eden filigranları depolar. Başlangıç çevrimi, sonraki eşitlemeler performansını geliştirir.
>


## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Kaynak sağlama, kullanıcıların atlandığını ve bunlara atanmış olsalar da sağlanmadığını söyleyin

Bir Kullanıcı sağlama günlüklerinde "atlandı" olarak görünüyorsa, nedenini öğrenmek için günlüğün **adımlar** sekmesini gözden geçirmeniz önemlidir. Aşağıda yaygın nedenler ve çözümler verilmiştir:

- Bir **öznitelik değerine göre kullanıcıya filtre**uygulanan **bir kapsam filtresi yapılandırıldı** . Filtrelerin kapsamını belirleme hakkında daha fazla bilgi için bkz. [kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
- **Kullanıcı "etkin değil" olarak belirlenir.** Bu özel hata iletisini görürseniz, bunun nedeni Azure AD 'de depolanan kullanıcı atama kaydında bir sorun olması olabilir. Bu sorunu onarmak için, kullanıcının (veya grup) uygulamadan atamasını kaldırıp yeniden atayın. Atama hakkında daha fazla bilgi için bkz. [Kullanıcı veya grup erişimi atama](assign-user-or-group-access-portal.md).
- **Gerekli bir öznitelik eksik veya Kullanıcı için doldurulmamış.** Sağlamayı ayarlarken göz önünde bulundurmanız gereken önemli bir şey, Azure AD 'den uygulamaya hangi kullanıcı (veya grup) özelliklerinin akmasını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmeniz ve yapılandırmaktır. Bu yapılandırma, iki sistem arasındaki kullanıcıları/grupları benzersiz şekilde tanımlamak ve eşleştirmek için kullanılan "eşleşen özelliği" ayarlamayı içerir. Bu önemli süreç hakkında daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulamaları Için Kullanıcı hazırlama öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).
- **Gruplar Için öznitelik eşlemeleri:** Bazı uygulamalarda destekleniyorsa, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen Grup nesneleri **eşlemesini** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. Sağlama grupları etkinleştirilmişse, "eşleşen KIMLIK" için uygun bir alanın kullanıldığından emin olmak üzere öznitelik eşlemelerini gözden geçirdiğinizden emin olun. Eşleşen KIMLIK görünen ad veya e-posta diğer adı olabilir. Eşleşen özellik boşsa veya Azure AD 'de bir grup için doldurulmamışsa, Grup ve üyeleri sağlanmadı.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Connect eşitleme: bildirime dayalı sağlamayı anlama](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
