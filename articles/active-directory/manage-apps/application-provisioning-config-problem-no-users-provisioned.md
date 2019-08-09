---
title: Azure AD Galeri uygulamasına hiçbir Kullanıcı sağlanmıyor | Microsoft Docs
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
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: b80539181e6614283b6170b9cd9d4db85f812a5f
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879903"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulamasına hiçbir Kullanıcı sağlanmıyor
Bir uygulama için otomatik sağlama yapılandırıldıktan sonra (Azure AD 'ye uygulamaya bağlanmak için sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulama dahil), kullanıcılar ve/veya grupları uygulamaya sağlanır. Sağlama aşağıdaki noktalara göre belirlenir:

-   Uygulamaya **atanan** kullanıcılar ve gruplar. İç içe gruplar veya Office 365 gruplarının sağlanması desteklenmez. Atama hakkında daha fazla bilgi için bkz. [Azure Active Directory bir kurumsal uygulamaya Kullanıcı veya Grup atama](assign-user-or-group-access-portal.md).
-   **Öznitelik eşlemelerinin** etkinleştirilip etkinleştirilmediği ve geçerli ÖZNITELIKLERI Azure AD 'den uygulamaya eşitlemek üzere yapılandırılıp yapılandırılmadığını belirtir. Öznitelik eşlemeleri hakkında daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulamaları Için Kullanıcı hazırlama öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).
-   Belirli öznitelik değerlerine göre kullanıcılara filtre uygulayan bir **kapsam filtresi** olup olmadığı. Kapsam filtreleri hakkında daha fazla bilgi için bkz. [kapsam filtreleriyle öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md).
  
Kullanıcıların sağlanmakta olmadığını gözlemlerseniz Azure AD 'deki denetim günlüklerine başvurun. Belirli bir kullanıcı için günlük girişlerini arayın.

Sağlama denetim günlüklerine Azure Portal **Azure Active Directory &gt; Enterprise Apps &gt; \[uygulama\] adı&gt; denetim günlükleri** sekmesinde erişilebilir. **Hesap sağlama** kategorisindeki günlüklere yalnızca uygulamanın sağlama olaylarını görmek için filtre uygulayın. Kullanıcılar için, öznitelik eşlemelerinde kendileri için yapılandırılmış "eşleşen KIMLIĞI" temelinde arama yapabilirsiniz. Örneğin, "Kullanıcı asıl adı" veya "e-posta adresi" ni Azure AD tarafında eşleşen öznitelik olarak yapılandırdıysanız ve sağlanmakta olmadığı Kullanıcı "audrey@contoso.com" değerine sahipse, "audrey@contoso.com" için denetim günlüklerinde arama yapın ve girdileri gözden geçirin döndürüldüğünde.

Sağlama denetim günlükleri, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder; sağlama için kapsamdaki Kullanıcı için Azure AD sorgulama, hedef uygulamayı bu kullanıcıların varlığına sorgulama ve kullanıcı nesnelerini karşılaştırma gibi işlemler sistem arasında. Ardından, karşılaştırma temelinde hedef sistemde Kullanıcı hesabını ekleyin, güncelleştirin veya devre dışı bırakın.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate alınması gereken genel sorun alanı
Aşağıda, nereden başlayabileceğiniz hakkında bir fikriniz varsa, detaya gidebileceğiniz genel sorun alanlarının bir listesi verilmiştir.

- [Sağlama hizmeti başlatma olarak görünmüyor](#provisioning-service-does-not-appear-to-start)
- [Denetim günlükleri, atanmış olsalar dahi, kullanıcıların atlandığını ve sağlanmadığını varsayalım](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatma olarak görünmüyor
**Sağlama durumunu** , Azure Portal **Azure Active Directory &gt; Enterprise Apps &gt; \[uygulamaadı\]sağlamabölümünde olacak şekilde ayarlarsanız &gt;** . Ancak, sonraki yeniden yüklemeden sonra bu sayfada başka bir durum ayrıntısı gösterilmez, bu durum büyük olasılıkla hizmetin çalışıyor olması, ancak henüz bir ilk eşitlemeyi tamamlamamamasından kaynaklanıyor olabilir. Hizmetin hangi işlemleri yaptığını ve herhangi bir hata olup olmadığını belirlemek için yukarıda açıklanan **Denetim günlüklerini** denetleyin.

>[!NOTE]
>İlk eşitleme, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakikadan birkaç saate kadar sürebilir. İlk eşitlemeden sonra sonraki eşitlemeler daha hızlıdır, çünkü sağlama hizmeti ilk eşitlemeden sonra her iki sistemin durumunu temsil eden filigranları depolar. İlk eşitleme, sonraki eşitlemeler performansını geliştirir.
>


## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Denetim günlükleri, atanmış olsalar dahi, kullanıcıların atlandığını ve sağlanmadığını varsayalım

Denetim günlüklerinde bir Kullanıcı "atlandı" olarak görünüyorsa, nedenini öğrenmek için günlük iletisindeki genişletilmiş ayrıntıları okumak önemlidir. Aşağıda yaygın nedenler ve çözümler verilmiştir:

- **Kapsam filtresi yapılandırıldı** **Bu, kullanıcıyı bir öznitelik değerine göre filtreleyerek**. Filtrelerin kapsamını belirleme hakkında daha fazla bilgi için bkz. [kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
- **Kullanıcı "etkin değil" olarak belirlenir.** Bu özel hata iletisini görürseniz, bunun nedeni Azure AD 'de depolanan kullanıcı atama kaydında bir sorun olması olabilir. Bu sorunu onarmak için, kullanıcının (veya grup) uygulamadan atamasını kaldırıp yeniden atayın. Atama hakkında daha fazla bilgi için bkz. [Kullanıcı veya grup erişimi atama](assign-user-or-group-access-portal.md).
- **Gerekli bir öznitelik eksik veya Kullanıcı için doldurulmamış.** Sağlamayı ayarlarken göz önünde bulundurmanız gereken önemli bir şey, Azure AD 'den uygulamaya hangi kullanıcı (veya grup) özelliklerinin akmasını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmeniz ve yapılandırmaktır. Bu yapılandırma, iki sistem arasındaki kullanıcıları/grupları benzersiz şekilde tanımlamak ve eşleştirmek için kullanılan "eşleşen özelliği" ayarlamayı içerir. Bu önemli süreç hakkında daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulamaları Için Kullanıcı hazırlama öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).
- **Gruplar için öznitelik eşlemeleri:** Bazı uygulamalarda destekleniyorsa, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen Grup nesneleri **eşlemesini** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. Sağlama grupları etkinleştirilmişse, "eşleşen KIMLIK" için uygun bir alanın kullanıldığından emin olmak üzere öznitelik eşlemelerini gözden geçirdiğinizden emin olun. Eşleşen KIMLIK görünen ad veya e-posta diğer adı olabilir. Eşleşen özellik boşsa veya Azure AD 'de bir grup için doldurulmamışsa, Grup ve üyeleri sağlanmadı.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Connect eşitlemesi: Bildirim Temelli Sağlamayı Anlama](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
