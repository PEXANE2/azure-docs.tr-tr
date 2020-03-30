---
title: Karantina Başvuru Durumu | Microsoft Dokümanlar
description: Otomatik kullanıcı sağlama için bir uygulama yapılandırdığınızda, Karantina'nın sağlama durumunun ne anlama geldiğini ve nasıl temizlenerek temize çıktığınızı öğrenin.
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
ms.date: 10/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 563c049bf3d1606e87db54e3b003dac987594610
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154636"
---
# <a name="application-provisioning-in-quarantine-status"></a>Karantina durumunda başvuru sağlama

Azure AD sağlama hizmeti yapılandırmanızın durumunu izler ve sağlıksız uygulamaları bir "karantina" durumuna yerleştirir. Hedef sisteme karşı yapılan çağrıların çoğu veya tümü bir hata nedeniyle sürekli olarak başarısız olursa (örneğin geçersiz yönetici kimlik bilgileri), sağlama işi karantinada olarak işaretlenir.

Karantina sırasında, artımlı döngülerin sıklığı yavaş yavaş günde bir kez azalır. Tüm hatalar düzeltildikten ve bir sonraki eşitleme döngüsü başladıktan sonra sağlama işi karantinadan kaldırılır. Sağlama işi dört haftadan uzun süre karantinada kalırsa, sağlama işi devre dışı bırakılır (çalışmayı durdurur).

## <a name="how-do-i-know-if-my-application-is-in-quarantine"></a>Başvurumun karantinada olup olmadığını nasıl anlarım?

Bir uygulamanın karantinada olup olmadığını denetlemenin üç yolu vardır:
  
- Azure portalında, **Azure Active Directory** > **Enterprise uygulamalarına** > &lt;gidin*uygulama adı*&gt; > **Sağlama'ya** gidin ve en alttaki ilerleme çubuğuna gidin.  

  ![Karantina durumunu gösteren hüküm çubuğu](./media/application-provisioning-quarantine-status/progress-bar-quarantined.png)

- Azure portalında, **Etkinlik: Karantina** ve karantina geçmişini gözden geçirin > **Azure Etkin Dizin** > **Denetim Günlükleri'ne** gidin. Yukarıda açıklandığı gibi ilerleme çubuğundaki görünüm, sağlamanın şu anda karantinada olup olmadığını gösterirken, denetim günlükleri bir uygulamaiçin karantina geçmişini görmenizi sağlar. 

- Sağlama [işinin](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-get?view=graph-rest-beta&tabs=http) durumunu programlı bir şekilde almak için Microsoft Graph isteğini kullan

        `GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/`

- E-postanızı kontrol edin. Bir uygulama karantinaya alındığı zaman, tek seferlik bir bildirim e-postası gönderilir. Karantina nedeni değişirse, karantina için yeni nedeni gösteren güncelleştirilmiş bir e-posta gönderilir. Bir e-posta görmüyorsanız:

  - Uygulama için sağlama yapılandırmasında geçerli bir **Bildirim E-postası** belirttiğinizi unutmayın.
  - Bildirim e-posta gelen kutusunda istenmeyen posta filtresi olmadığından emin olun.
  - E-postaaboneliğini iptal etmediğinizden emin olun.

## <a name="why-is-my-application-in-quarantine"></a>Başvurum neden karantinada?

Sağlama işinin durumunu almak için microsoft grafiği isteği karantina için aşağıdaki nedeni gösterir:

- `EncounteredQuarantineException`geçersiz kimlik bilgilerinin sağlandığını gösterir. Sağlama hizmeti, kaynak sistem ile hedef sistem arasında bağlantı kuramadı.

- `EncounteredEscrowProportionThreshold`hükmün emanet eşiğini aştığını gösterir. Bu durum, sağlama olaylarının %60'ından fazlası başarısız olduğunda oluşur.

- `QuarantineOnDemand`uygulamanızla ilgili bir sorun tespit ettiğimiz ve el ile karantinaya aldığımız anlamına gelir.

## <a name="how-do-i-get-my-application-out-of-quarantine"></a>Başvurumu karantinadan nasıl çıkaracağım?

İlk olarak, uygulamanın karantinaya alınmasına neden olan sorunu giderin.

- [Geçerli Yönetici Kimlik Bilgileri girdiğinizi](../app-provisioning/configure-automatic-user-provisioning-portal.md#configuring-automatic-user-account-provisioning)emin olmak için uygulamanın sağlama ayarlarını denetleyin. Azure AD hedef uygulama ile bir güven kurmak gerekir. Geçerli kimlik bilgilerini girdiğinizden ve hesabınızda gerekli izinlere sahip olduğundan emin olun.

- Karantinaya neden olan hataları daha fazla araştırmak ve hatayı gidermek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) gözden geçirin. **Etkinlik** bölümündeki **Azure Active Directory** &gt; **Enterprise Apps** &gt; **Sağlama günlüklerine (önizleme)** giderek Azure portalındaki sağlama günlüklerine erişin.

Sorunu çözdükten sonra, sağlama işini yeniden başlatın. Öznitelik eşlemeleri veya kapsam filtreleri gibi uygulamanın sağlama ayarlarında yapılan belirli değişiklikler, sizin için hükmü otomatik olarak yeniden başlatacaktır. Uygulamanın **Sağlama** sayfasındaki ilerleme çubuğu, sağlamanın en son ne zaman başlatılacağı dır. Sağlama işini el ile yeniden başlatmanız gerekiyorsa, aşağıdaki yöntemlerden birini kullanın:  

- Sağlama işini yeniden başlatmak için Azure portalını kullanın. **Uygulamanın Ayarlar**altındaki **Sağlama** sayfasında, **Durumu Temizle'yi seçin ve eşitlemayı yeniden başlatın** ve **Sağlama Durumunu** **Açık**olarak ayarlayın. Bu eylem, biraz zaman alabilir sağlama hizmeti tamamen yeniden başlatır. Tam bir ilk döngü tekrar çalışır, bu da emanetleri temizler, uygulamayı karantinadan kaldırır ve filigranları temizler.

- Sağlama işini yeniden başlatmak için Microsoft [Graph'ı](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http)kullanın. Yeniden başlatacağınız şey üzerinde tam kontrole sahip olacaksınız. Emanetleri temizlemeyi (karantina durumuna doğru tahakkuk eden emanet sayacını yeniden başlatmak için), karantinayı (uygulamayı karantinadan kaldırmak için) veya filigranları temizlemeyi seçebilirsiniz. Aşağıdaki isteği kullanın:
 
       `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`
