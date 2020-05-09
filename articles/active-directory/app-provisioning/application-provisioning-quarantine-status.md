---
title: Karantina uygulama sağlama durumu | Microsoft Docs
description: Otomatik Kullanıcı sağlama için bir uygulama yapılandırdığınızda, karantinanın sağlama durumunun ne anlama geldiğini ve nasıl temizleyeceğinizi öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: mimart
ms.reviewer: arvinh
ms.openlocfilehash: c1e0039133b7f9a7ae827e348640f6379b7f10ac
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82593939"
---
# <a name="application-provisioning-in-quarantine-status"></a>Karantina durumunda uygulama sağlama

Azure AD sağlama hizmeti yapılandırmanızın sistem durumunu izler ve sağlıksız uygulamaları bir "Karantina" durumuna koyar. Hedef sisteme karşı yapılan çağrıların çoğu veya hepsi bir hata nedeniyle sürekli olarak başarısız olursa, örneğin, geçersiz yönetici kimlik bilgileri gibi, sağlama işi karantinada olarak işaretlenir.

Karantinaya alma sırasında, artımlı döngülerin sıklığı günde bir kez yavaş şekilde azaltılır. Hazırlama işi, tüm hatalar düzeltildikten ve sonraki eşitleme döngüsünün başlamasından sonra karantinadan kaldırılır. Sağlama işi dört haftadan uzun bir süreyle karantinayla kalırsa, sağlama işi devre dışı bırakılır (çalışmayı sonlandırır).

## <a name="how-do-i-know-if-my-application-is-in-quarantine"></a>Nasıl yaparım? uygulamamın karantinaya alınsın mı?

Bir uygulamanın karantinada olup olmadığını denetlemek için üç yol vardır:
  
- Azure Portal, **Azure Active Directory** > **Kurumsal uygulamalar** > &lt;*uygulama adı*&gt; > **sağlama** ' ya gidin ve karantina iletisi için ilerleme çubuğunu gözden geçirin.   

  ![Karantina durumunu gösteren sağlama durumu çubuğu](./media/application-provisioning-quarantine-status/progress-bar-quarantined.png)

- Azure Portal, **Azure Active Directory** > **Denetim günlüklerine** gidin > etkinliğe filtre uygula **:** karantinaya alma ve karantina geçmişini gözden geçirme. Yukarıda açıklanan ilerleme çubuğundaki görünüm, sağlama işleminin şu anda karantinada olup olmadığını gösterir. denetim günlükleri, bir uygulamanın karantina geçmişini görmenizi sağlar. 

- Sağlama işinin durumunu programlı bir şekilde almak için Microsoft Graph isteği [Al işini](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-get?view=graph-rest-beta&tabs=http) kullanın:

        `GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/`

- E-postanızı kontrol edin. Bir uygulama karantinada yerleştirildiğinde, bir kerelik bildirim e-postası gönderilir. Karantina nedeni değişirse, karantinanın yeni nedenini gösteren güncelleştirilmiş bir e-posta gönderilir. E-posta görmüyorsanız:

  - Uygulamanın sağlama yapılandırmasında geçerli bir **bildirim e-postası** belirttiğinizden emin olun.
  - Bildirim e-postası gelen kutusunda istenmeyen posta filtrelemesi olmadığından emin olun.
  - E-postalardan aboneliği kaldırdığınızdan emin olun.

## <a name="why-is-my-application-in-quarantine"></a>Uygulamamın neden karantinaya alınsın?

|Açıklama|Önerilen Eylem|
|---|---|
|**SCIM uyumluluk sorunu:** Beklenen HTTP/200 Tamam yanıtı yerine bir HTTP/404 bulunamadı yanıtı döndürüldü. Bu durumda, Azure AD sağlama hizmeti hedef uygulamaya bir istek yaptı ve beklenmeyen bir yanıt aldı.|Uygulamanın kiracı URL 'sini belirtmesini gerektirip gerektirmediğini ve URL 'nin doğru olduğundan emin olmak için yönetici kimlik bilgileri bölümüne bakın. Bir sorun görmüyorsanız, hizmetin SCıM uyumlu olduğundan emin olmak için lütfen uygulama geliştiricisine başvurun. https://tools.ietf.org/html/rfc7644#section-3.4.2 |
|**Geçersiz kimlik bilgileri:** Hedef uygulamaya erişim yetkisi verme girişiminde, belirtilen kimlik bilgilerinin geçersiz olduğunu belirten hedef uygulamadan bir yanıt aldık.|Lütfen sağlama yapılandırma Kullanıcı arabiriminin yönetici kimlik bilgileri bölümüne gidin ve geçerli kimlik bilgileriyle erişime yeniden yetki verin. Uygulama Galeri 'de ise, gereken ek adımlar için uygulama yapılandırma öğreticisini gözden geçirin.|
|**Yinelenen roller:** Salesforce ve Zendesk gibi belirli uygulamalardan içeri aktarılan roller benzersiz olmalıdır. |Azure portal uygulama [bildirimine](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest) gidin ve yinelenen rolü kaldırın.|

 Sağlama işinin durumunu almak için bir Microsoft Graph isteği, karantinaya alma işleminin aşağıdaki nedenini gösterir:

- `EncounteredQuarantineException`geçersiz kimlik bilgilerinin sağlandığını belirtir. Sağlama Hizmeti, kaynak sistemle hedef sistem arasında bir bağlantı kuramıyor.

- `EncounteredEscrowProportionThreshold`sağlamanın Emanet eşiğini aştığını gösterir. Bu durum, sağlama olaylarının %60 ' inden fazlası başarısız olduğunda meydana gelir.

- `QuarantineOnDemand`uygulamanızla ilgili bir sorun tespit ettiğimiz ve bunu el ile karantinaya ayarlamış olduğumuz anlamına gelir.

## <a name="how-do-i-get-my-application-out-of-quarantine"></a>Uygulamamı karantinaya alma Nasıl yaparım??

İlk olarak, uygulamanın karantinada yerleştirilmesine neden olan sorunu çözün.

- [Geçerli yönetici kimlik bilgilerini girdiğinizden](../app-provisioning/configure-automatic-user-provisioning-portal.md#configuring-automatic-user-account-provisioning)emin olmak için uygulamanın sağlama ayarlarını kontrol edin. Azure AD, hedef uygulamayla bir güven kurabilmesi gerekir. Geçerli kimlik bilgilerini girdiğinizden ve hesabınızda gerekli izinlere sahip olduğunuzdan emin olun.

- Hangi hataların karantinaya neden olduğunu araştırmak ve hatayı gidermek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) gözden geçirin. **Etkinlik** bölümündeki **Azure Active Directory** &gt; **Kurumsal uygulamalar** &gt; **sağlama günlükleri (Önizleme)** bölümüne giderek Azure Portal sağlama günlüklerine erişin.

Sorunu çözdükten sonra, sağlama işini yeniden başlatın. Uygulamanın sağlama ayarlarında öznitelik eşlemeleri veya kapsam filtreleri gibi bazı değişiklikler, sağlamayı sizin için otomatik olarak yeniden başlatacak. Uygulamanın **sağlama** sayfasındaki ilerleme çubuğu, sağlamanın en son ne zaman başlatıldığını gösterir. Sağlama işini el ile yeniden başlatmanız gerekiyorsa aşağıdaki yöntemlerden birini kullanın:  

- Sağlama işini yeniden başlatmak için Azure portal kullanın. Uygulamanın **sağlama** sayfasında, **Ayarlar**' ın altında, **durumu temizle ve eşitlemeyi yeniden Başlat** ' ı seçin ve **sağlama durumunu** **Açık**olarak ayarlayın. Bu eylem, sağlama hizmetini tamamen yeniden başlatır ve bu işlem biraz zaman alabilir. Tam bir başlangıç döngüsünün yeniden çalışması için, escrows 'yi temizler, uygulamayı karantinadan kaldırır ve tüm filigranları temizler.

- [Sağlama işini yeniden başlatmak](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http)için Microsoft Graph kullanın. Yeniden başlatdıklarınız üzerinde tam denetime sahip olacaksınız. Et 'ları kaldırmayı seçebilirsiniz (karantina durumuna göre tahakkuk eden Emanet sayacını yeniden başlatmak için), karantinayı temizleyebilir (uygulamayı karantinadan kaldırmak için) veya filigranları temizleyebilirsiniz. Aşağıdaki isteği kullanın:
 
       `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`
