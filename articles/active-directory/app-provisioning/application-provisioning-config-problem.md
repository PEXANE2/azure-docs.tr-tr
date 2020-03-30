---
title: Kullanıcı sağlamayı bir Azure AD Galerisi uygulamasına yapılandırma sorunu
description: Azure AD Uygulama Galerisi'nde listelenen bir uygulamaya kullanıcı sağlama yapılandırması yaparken karşılaşılan sık karşılaşılan sorunları giderme
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
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3955b96e4a7edfc79a229d927523bdd4473409d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522773"
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a>Kullanıcı sağlamayı azure AD Galerisi uygulamasına yapılandırma sorunu

Bir uygulama için [otomatik kullanıcı sağlamanın](user-provisioning.md) (desteklendiği yerlerde) yapılandırılması, uygulamayı otomatik sağlama için hazırlamak için belirli yönergelerin izlenmesini gerektirir. Ardından, kullanıcı hesaplarını uygulamaya eşitlemek için sağlama hizmetini yapılandırmak için Azure portalını kullanabilirsiniz.

Her zaman uygulamanız için bulma ayarlama özel kurulum öğretici bularak başlamalıdır. Ardından, hem uygulamayı hem de Azure AD'yi sağlama bağlantısını oluşturmak için yapılandırmak için bu adımları izleyin. Uygulama öğreticilerinin bir listesini, [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi'nde](../saas-apps/tutorial-list.md)bulunabilir.

## <a name="how-to-see-if-provisioning-is-working"></a>Sağlamanın çalışıp çalışmadığını görme 

Hizmet yapılandırıldıktan sonra, hizmetin çalışmasına ilişkin çoğu kavrayış iki yerden çizilebilir:

-   **Sağlama günlükleri (önizleme)** – [Sağlama günlükleri,](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) sağlama kapsamında olan atanmış kullanıcılar için Azure AD'yi sorgulamak da dahil olmak üzere, sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri kaydeder. Sistem arasındaki kullanıcı nesnelerini karşılaştırarak, bu kullanıcıların varlığı için hedef uygulamayı sorgulayın. Ardından, karşılaştırmaya dayalı olarak hedef sistemdeki kullanıcı hesabını ekleyin, güncelleyin veya devre dışı edin. **Etkinlik** bölümünde **Azure Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerini (önizleme)** seçerek Azure portalındaki sağlama günlüklerine erişebilirsiniz.

-   **Mevcut durum –** Belirli bir uygulama için son sağlama çalışmasının bir özeti, hizmet ayarları altında ekranın alt kısmındaki **Azure Active Directory &gt; Enterprise Apps Uygulama &gt; \[\] &gt;Adı Sağlama** bölümünde görülebilir. Geçerli Durum bölümü, bir sağlama döngüsünün kullanıcı hesaplarını sağlamaya başlayıp başlamadığını gösterir. Döngünün ilerlemesini izleyebilir, kaç kullanıcı ve grubun sağlandığını görebilir ve kaç rol oluşturulduğunu görebilirsiniz. Herhangi bir hata varsa, ayrıntılar [Sağlama günlüklerinde bulunabilir (... /reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context).

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Genel sorunlu alanlar ile birlikte dikkate alınması gereken

Aşağıda nereden başlayacağınızı bir fikriniz varsa içine matkap olabilir genel sorunlu alanların bir listesidir.

* [Sağlama hizmeti başlatılamıyor](#provisioning-service-does-not-appear-to-start)
* Uygulama kimlik bilgileri çalışmadığı için yapılandırmayı kaydedemiyorum
* [Sağlama günlükleri, kullanıcıların atanmış olsalar bile "atlandıklarını" ve sağlanmadıklarını söylüyor](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Sağlama hizmeti başlatılamıyor

Azure portalının **Azure Active Directory &gt; Enterprise Apps &gt; \[\] &gt;Uygulama Adı Sağlama** bölümünde Sağlama **Durumunu** **A)'da** olacak şekilde ayarlarsanız. Ancak, sonraki yeniden yüklemeler sonrasında bu sayfada başka durum ayrıntıları gösterilmez. Hizmetin çalışıyor olması olasıdır, ancak henüz bir başlangıç döngüsünü tamamlamamıştır. Hizmetin hangi işlemleri gerçekleştirdiğini ve herhangi bir hata olup olmadığını belirlemek için yukarıda açıklanan **Sağlama günlüklerini** denetleyin.

>[!NOTE]
>İlk döngü, Azure REKLAM dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak 20 dakika ile birkaç saat arasında sürebilir. Sonraki eşitlemeler, sonraki eşitlemelerin performansını artırarak, ilk döngüden sonra her iki sistem durumunu temsil eden filigranları depolayan, ilk döngüden sonraki eşitlemelerden sonra daha hızlı olur.
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a>Uygulama kimlik bilgileri çalışmadığı için yapılandırmayı kaydedemiyorum

Sağlamanın işe yaraması için Azure AD, bu uygulama tarafından sağlanan bir kullanıcı yönetimi API'sine bağlanmasına olanak tanıyan geçerli kimlik bilgileri gerektirir. Bu kimlik bilgileri çalışmıyorsa veya ne olduklarını bilmiyorsanız, daha önce açıklanan bu uygulamayı ayarlamak için öğreticiyi inceleyin.

## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Sağlama günlükleri, kullanıcıların atlanır ve atanmış olsalar bile sağlanmadığını söyler

Bir kullanıcı sağlama günlüklerinde "atlanan" olarak ortaya çıktığında, nedenini belirlemek için günlük iletisinde genişletilmiş ayrıntıları okumak çok önemlidir. Aşağıda yaygın nedenler ve kararlar verilmiştir:

- Bir **öznitelik değerine göre kullanıcıyı filtreleyen bir** **kapsam filtresi yapılandırıldı.** Daha fazla bilgi için, [kapsam filtreleri ile Attribute tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.

- **Kullanıcı "etkili bir şekilde yetkili değildir".** Bu özel hata iletisini görüyorsanız, bunun nedeni Azure AD'de depolanan kullanıcı atama kaydıyla ilgili bir sorun olmasıdır. Bu sorunu gidermek için, kullanıcıyı (veya grubu) uygulamadan atayın ve yeniden atayın. Daha fazla bilgi için [bkz.](../manage-apps/assign-user-or-group-access-portal.md)

- **Bir kullanıcı için gerekli öznitelik eksik veya doldurulmadı.** Sağlama yı ayarlarken göz önünde bulundurulması gereken önemli bir şey, Azure AD'den uygulamaya hangi kullanıcının (veya grup) özelliklerinin aktığını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmek ve yapılandırmaktır. Bu, iki sistem arasındaki kullanıcıları/grupları benzersiz olarak tanımlamak ve eşleştirmek için kullanılan "eşleşen özelliği" ayarlamayı içerir. Bu önemli işlem hakkında daha fazla bilgi için [bkz.](../app-provisioning/customize-application-attributes.md)

  * **Gruplar için öznitelik eşlemeleri:** Bazı uygulamalar için desteklenirse, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen grup nesneleri için **Eşlemeyi** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. Sağlama grupları etkinse, "eşleşen kimlik" için uygun bir alanın kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirin. Bu, görüntü adı veya e-posta takma adı olabilir), çünkü eşleşen özellik Boşsa veya Azure AD'deki bir grup için doldurulmuyorsa grup ve üyeleri sağlanmaz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Sağlama ve Deprovisioning'i otomatikleştirin](user-provisioning.md)
