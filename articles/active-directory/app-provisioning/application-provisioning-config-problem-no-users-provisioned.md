---
title: Kullanıcılar benim uygulamamda sağlanmıyor
description: Azure AD ile kullanıcı sağlama için yapılandırdığınız bir Azure REKLAM Galerisi Uygulamasında görünen kullanıcıları görmediğinizde karşılaşılan sık karşılaşılan sorunları nasıl giderebilirsiniz?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac6d4f24d3b6c21828ccb11785005df736c6b070
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680345"
---
# <a name="no-users-are-being-provisioned"></a>Hiçbir kullanıcı sağlanmıyor 
>[!NOTE]
>16.04.2020'den itibaren varsayılan erişim rolü atanan kullanıcıların davranışını değiştirdik. Ayrıntılar için lütfen aşağıdaki bölüme bakın. 
>
Bir uygulama için otomatik sağlama yapılandırıldıktan sonra (uygulamaya bağlanmak için Azure AD'ye sağlanan uygulama kimlik bilgilerinin geçerli olduğunu doğrulamak da dahil olmak üzere), kullanıcılar ve/veya gruplar uygulamaya sağlanır. Sağlama aşağıdaki şeyler tarafından belirlenir:

-   Uygulamaya hangi kullanıcı ve gruplar **atandı.** İç içe gruplar veya Office 365 grupları sağlamanın desteklenmediğini unutmayın. Atama hakkında daha fazla bilgi için azure [Etkin Dizin'deki bir kurumsal uygulamaya bir kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)'ya bakın.
-   **Öznitelik eşlemelerinin** etkin olup olmadığı ve Azure AD'den uygulamaya geçerli öznitelikleri eşitlemek üzere yapılandırılıp yapılandırılmadığı. Öznitelik eşlemeleri hakkında daha fazla bilgi için, [Azure Etkin Dizini'ndeki SaaS Uygulamaları için Kullanıcı Sağlama Özellik Eşlemelerini Özelleştirme](customize-application-attributes.md)konusuna bakın.
-   Belirli öznitelik **değerlerine** göre kullanıcıları filtreleyen bir kapsam filtresi olup olmadığı. Kapsam filtreleri hakkında daha fazla bilgi için, [kapsam filtreleri ile Öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.
  
  
Kullanıcıların sağlanmadığını gözlemlerseniz, Azure AD'deki [Sağlama günlüklerine (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) başvurun. Belirli bir kullanıcı için günlük girişlerini arayın.

**Etkinlik** bölümünde **Azure Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerini (önizleme)** seçerek Azure portalındaki sağlama günlüklerine erişebilirsiniz. Sağlama verilerini, kaynak sistemde veya hedef sistemde kullanıcının adına veya tanımlayıcıya göre arayabilirsiniz. Ayrıntılar için, [Sağlama günlükleri (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)bakın. 

Sağlama günlükleri, sağlama kapsamında olan atanmış kullanıcılar için Azure AD'yi sorgulamak, bu kullanıcıların varlığı için hedef uygulamayı sorgulamak, sistem arasındaki kullanıcı nesnelerini karşılaştırmak da dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder. Ardından, karşılaştırmaya dayalı olarak hedef sistemdeki kullanıcı hesabını ekleyin, güncelleyin veya devre dışı edin.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Dikkate Alınması Gereken Geçici Genel Sorun Alanları
Aşağıda nereden başlayacağınızı bir fikriniz varsa içine matkap olabilir genel sorunlu alanların bir listesidir.

- [Sağlama hizmeti başlatılamıyor](#provisioning-service-does-not-appear-to-start)
- [Sağlama günlükleri, kullanıcıların atansalar bile atlanmadığını ve sağlanmadığını söylüyor](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatılamıyor
Azure portalının **Azure Active Directory &gt; Enterprise Apps &gt; \[\] &gt;Uygulama Adı Sağlama** bölümünde Sağlama **Durumunu** **A)'da** olacak şekilde ayarlarsanız. Ancak, sonraki yeniden yüklemelerden sonra o sayfada başka durum ayrıntıları gösterilmez, hizmetin çalışıyor olması olasıdır ancak henüz bir başlangıç döngüsünü tamamlamamıştır. Hizmetin hangi işlemleri gerçekleştirdiğini ve herhangi bir hata olup olmadığını belirlemek için yukarıda açıklanan **Sağlama günlüklerini (önizleme)** denetleyin.

>[!NOTE]
>İlk döngü, Azure REKLAM dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakika ile birkaç saat arasında sürebilir. Sağlama hizmeti, ilk döngüden sonra her iki sistem durumunu temsil eden filigranları depoladıktan sonra sonraki eşitlemeler daha hızlıdır. İlk döngü sonraki eşitlemelerin performansını artırır.
>


## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Sağlama günlükleri, kullanıcıların atlanır ve atanmış olsalar bile sağlanmadığını söyler

Bir kullanıcı sağlama günlüklerinde "atlanan" olarak ortaya çıktığında, nedenini belirlemek için günlüğün **Adımlar** sekmesini gözden geçirmek önemlidir. Aşağıda yaygın nedenler ve kararlar verilmiştir:

- Bir **öznitelik değerine göre kullanıcıyı filtreleyen bir** **kapsam filtresi yapılandırıldı.** Kapsam filtreleri hakkında daha fazla bilgi için [kapsam filtreleri'ne](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.
- **Kullanıcı "etkili bir şekilde yetkili değildir".** Bu özel hata iletisini görüyorsanız, bunun nedeni Azure AD'de depolanan kullanıcı atama kaydıyla ilgili bir sorun olmasıdır. Bu sorunu gidermek için, kullanıcıyı (veya grubu) uygulamadan atayın ve yeniden atayın. Atama hakkında daha fazla bilgi için [bkz.](../manage-apps/assign-user-or-group-access-portal.md)
- **Bir kullanıcı için gerekli öznitelik eksik veya doldurulmadı.** Sağlama yı ayarlarken göz önünde bulundurulması gereken önemli bir şey, Azure AD'den uygulamaya hangi kullanıcının (veya grup) özelliklerinin aktığını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmek ve yapılandırmaktır. Bu yapılandırma, iki sistem arasındaki kullanıcıları/grupları benzersiz olarak tanımlamak ve eşleştirmek için kullanılan "eşleşen özelliği" ayarlamayı içerir. Bu önemli işlem hakkında daha fazla bilgi için, [Azure Etkin Dizinindeki SaaS Uygulamaları için Kullanıcı Sağlama Özellik Eşlemelerini Özelleştirme](customize-application-attributes.md)konusuna bakın.
- **Gruplar için öznitelik eşlemeleri:** Bazı uygulamalar için desteklenirse, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen grup nesneleri için **Eşlemeyi** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. Sağlama grupları etkinse, "eşleşen kimlik" için uygun bir alanın kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirin. Eşleşen kimlik, görüntü adı veya e-posta takma adı olabilir. Grup ve üyeleri, eşleşen özellik boşsa veya Azure AD'deki bir grup için doldurulmuyorsa sağlanmaz.
## <a name="provisioning-users-assigned-to-the-default-access-role"></a>Varsayılan erişim rolüne atanan kullanıcıları sağlama
Galeriden bir uygulamadaki varsayılan role "varsayılan erişim" rolü denir. Tarihsel olarak, bu role atanan kullanıcılar sağlanmaz ve "etkin bir şekilde hak sahibi olmadığı" için [sağlama günlüklerinde](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) atlanmış olarak işaretlenir. 

**16/04/2020 tarihinden sonra oluşturulan yapılandırmaları sağlama davranışı:** Varsayılan erişim rolüne atanan kullanıcılar, diğer tüm rollerle aynı şekilde değerlendirilir. Varsayılan erişim atanan bir kullanıcı "etkin bir şekilde hak sahibi değil" olarak atlanmaz. 

**16/04/2020'den önce oluşturulan yapılandırmaları sağlama davranışı:** Önümüzdeki 3 ay boyunca, davranış bugün olduğu gibi devam edecektir. Varsayılan erişim rolüne sahip kullanıcılar, etkin bir şekilde hak sahibi olmadığı için atlanır. Temmuz 2020'den sonra, davranış tüm uygulamalar için tek düze olacaktır. "Etkin bir şekilde hak sahibi olmadığımız" için varsayılan erişim rolünü sağlayan kullanıcıları atlamayız. Bu değişiklik Microsoft tarafından yapılacak tır ve müşteri eylemi gerekmez. Bu değişiklikten sonra bile bu kullanıcıların atlandırılmaya devam etmesini istiyorsanız, lütfen uygun kapsam filtrelerini uygulayın veya kapsam dışında olduğundan emin olmak için kullanıcıyı uygulamadan atayın.  

Bu değişikliklerle ilgili sorularınız için lütfenprovisioningfeedback@microsoft.com
## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Connect eşitlemi: Bildirimsel Sağlama'yı Anlama](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
