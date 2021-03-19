---
title: Karantina uygulama sağlama durumu | Microsoft Docs
description: Otomatik Kullanıcı sağlama için bir uygulama yapılandırdığınızda, karantinanın sağlama durumunun ne anlama geldiğini ve nasıl temizleyeceğinizi öğrenin.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 03/18/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 49590c46588ad0d0f1c1b7b095679a3c3fce96eb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104579510"
---
# <a name="application-provisioning-in-quarantine-status"></a>Karantina durumunda uygulama sağlama

Azure AD sağlama hizmeti, yapılandırmanızın sistem durumunu izler. Ayrıca uygun olmayan uygulamaları bir "Karantina" durumuna koyar. Hedef sisteme karşı yapılan çağrıların çoğu veya hepsi sürekli olarak başarısız olursa, sağlama işi karantinada olarak işaretlenir. Hatalı yönetici kimlik bilgileri nedeniyle hata örneği alındı.

Karantinaya alma sırasında:
- Artımlı döngülerin sıklığı günde bir kez yavaş olarak düşürülür.
- Hazırlama işi, tüm hatalar düzeltildikten ve sonraki eşitleme döngüsünün başlamasından sonra karantinadan kaldırılır. 
- Sağlama işi dört haftadan uzun bir süreyle karantinayla kalırsa, sağlama işi devre dışı bırakılır (çalışmayı sonlandırır).

## <a name="how-do-i-know-if-my-application-is-in-quarantine"></a>Nasıl yaparım? uygulamamın karantinaya alınsın mı?

Bir uygulamanın karantinada olup olmadığını denetlemek için üç yol vardır:
  
- Azure Portal, **Azure Active Directory**  >  **Kurumsal uygulamalar**  >  &lt; *uygulama adı* sağlama ' ya gidin &gt;  >   ve karantina iletisi için ilerleme çubuğunu gözden geçirin.   

  ![Karantina durumunu gösteren sağlama durumu çubuğu](./media/application-provisioning-quarantine-status/progress-bar-quarantined.png)

- Azure Portal, **Azure Active Directory**  >  **Denetim günlüklerine** gidin > **etkinliğe** filtre uygula: karantinaya alma ve karantina geçmişini gözden geçirme. Yukarıda açıklanan ilerleme çubuğundaki görünüm, sağlama işleminin zaten karantinaya alındı olup olmadığını gösterir. Denetim günlükleri bir uygulamanın karantina geçmişini gösterir. 

- Sağlama işinin durumunu programlı bir şekilde almak için Microsoft Graph isteği [Al işini](/graph/api/synchronization-synchronizationjob-get?tabs=http&view=graph-rest-beta&preserve-view=true) kullanın:

```microsoft-graph
        GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

- E-postanızı kontrol edin. Bir uygulama karantinada yerleştirildiğinde, bir kerelik bildirim e-postası gönderilir. Karantina nedeni değişirse, karantinanın yeni nedenini gösteren güncelleştirilmiş bir e-posta gönderilir. E-posta görmüyorsanız:

  - Uygulamanın sağlama yapılandırmasında geçerli bir **bildirim e-postası** belirttiğinizden emin olun.
  - Bildirim e-postası gelen kutusunda istenmeyen posta filtrelemesi bulunmadığından emin olun.
  - E-postalardan aboneliği kaldırmadığınızdan emin olun.
  - E-postaları denetle `azure-noreply@microsoft.com`

## <a name="why-is-my-application-in-quarantine"></a>Uygulamamın neden karantinaya alınsın?

|Description|Önerilen Eylem|
|---|---|
|**SCIM uyumluluk sorunu:** Beklenen HTTP/200 Tamam yanıtı yerine bir HTTP/404 bulunamadı yanıtı döndürüldü. Bu durumda, Azure AD sağlama hizmeti hedef uygulamaya bir istek yaptı ve beklenmeyen bir yanıt aldı.|Yönetici kimlik bilgileri bölümünü denetleyin. Uygulamanın kiracı URL 'sini belirtmesini gerektirip gerektirmediğini ve URL 'nin doğru olup olmadığını görün. Bir sorun görmüyorsanız, hizmetinin SCıM uyumlu olduğundan emin olmak için uygulama geliştiricisine başvurun. https://tools.ietf.org/html/rfc7644#section-3.4.2 |
|**Geçersiz kimlik bilgileri:** Hedef uygulamaya erişim izni verirken, hedef uygulamadan, belirtilen kimlik bilgilerinin geçersiz olduğunu belirten bir yanıt aldık.|Sağlama yapılandırma Kullanıcı arabiriminin yönetici kimlik bilgileri bölümüne gidin ve geçerli kimlik bilgileriyle erişimi yeniden Yetkilendir. Uygulama Galeri 'de ise, artık gerekli adımlar için uygulama yapılandırma öğreticisini gözden geçirin.|
|**Yinelenen roller:** Salesforce ve Zendesk gibi belirli uygulamalardan içeri aktarılan roller benzersiz olmalıdır. |Azure portal uygulama [bildirimine](../develop/reference-app-manifest.md) gidin ve yinelenen rolü kaldırın.|

 Sağlama işinin durumunu almak için bir Microsoft Graph isteği, karantinaya alma işleminin aşağıdaki nedenini gösterir:
- `EncounteredQuarantineException` geçersiz kimlik bilgilerinin sağlandığını belirtir. Sağlama Hizmeti, kaynak sistemle hedef sistem arasında bir bağlantı kuramıyor.
- `EncounteredEscrowProportionThreshold` sağlamanın Emanet eşiğini aştığını gösterir. Bu durum, sağlama olaylarının %40 ' inden fazlası başarısız olduğunda meydana gelir. Daha fazla bilgi için bkz. aşağıdaki eşik ayrıntılarını emanet.
- `QuarantineOnDemand` uygulamanızla ilgili bir sorun tespit ettiğimiz ve bunu el ile karantinaya ayarlamış olduğumuz anlamına gelir.

**Emanet eşikleri**

Orantılı Emanet eşiği karşılanıyorsa, sağlama işi karantinaya alınacaktır. Bu mantık değişebilir, ancak aşağıda açıklandığı gibi kabaca işe yarar: 

Bir iş, yönetici kimlik bilgileri veya SCıM uyumluluğu gibi sorunlar nedeniyle hata sayımlarından bağımsız olarak karantinaya alabilir. Ancak, genel olarak 5.000 başarısızlık çok fazla başarısızlık nedeniyle karantinaya alma için en düşük gerekliliktir. Örneğin, 4.000 arızaya sahip bir iş karantinaya almaya devam etmek zorunda değildir. Ancak, 5.000 arızaya sahip bir iş bir değerlendirmeyi tetikleyecektir. Bir değerlendirme aşağıdaki ölçütleri kullanır:  
- Sağlama olaylarının %40 ' inden fazlası başarısız olursa veya 40.000 ' den fazla hata varsa, sağlama işi karantinaya alınacaktır. %40 eşiğin veya 40.000 eşiğinin bir parçası olarak başvuru hatası sayılmaz. Örneğin, bir yöneticiyi veya grup üyesini güncelleştirme hatası bir başvuru hatasıdır.
- 45.000 kullanıcının sağlaması başarısız olan bir iş, 40.000 eşiğini aştığından karantinaya almaya neden olur.
- 30.000 kullanıcının sağlama başarısız olduğu ve 5.000 ' nin başarılı olduğu bir iş,%% Threshold 5.000 ve en düşük 40 değerini aştığından karantinaya almaya yol açacaktı.
- 20.000 hata ve 100.000 başarılı olan bir iş 40,% hata eşiğini veya 40.000 hata en büyük değerini aşmadığından karantinaya almaya devam etmez.  
- Hem başvuru hem de başvuru olmayan hatalara yönelik hesapların 60.000 hatası vardır. Örneğin, 40.000 Kullanıcı sağlanamadı ve 21.000 yönetici güncelleştirmesi başarısız oldu. Toplam 61.000 hatadır ve 60.000 limitini aşıyor.


## <a name="how-do-i-get-my-application-out-of-quarantine"></a>Uygulamamı karantinaya alma Nasıl yaparım??

İlk olarak, uygulamanın karantinada yerleştirilmesine neden olan sorunu çözün.

- [Geçerli yönetici kimlik bilgilerini girdiğinizden](../app-provisioning/configure-automatic-user-provisioning-portal.md#configuring-automatic-user-account-provisioning)emin olmak için uygulamanın sağlama ayarlarını kontrol edin. Azure AD, hedef uygulamayla bir güven sağlamalıdır. Geçerli kimlik bilgilerini girdiğinizden ve hesabınızda gerekli izinlere sahip olduğunuzdan emin olun.

- Hangi hataların karantinaya neden olduğunu araştırmak ve hatayı gidermek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) gözden geçirin. Etkinlik bölümünde **Azure Active Directory** &gt; **Kurumsal uygulamalar** &gt; **sağlama günlükleri (Önizleme)** bölümüne gidin  .

Sorunu çözdükten sonra, sağlama işini yeniden başlatın. Uygulamanın sağlama ayarlarında öznitelik eşlemeleri veya kapsam filtreleri gibi bazı değişiklikler, sağlamayı sizin için otomatik olarak yeniden başlatacak. Uygulamanın **sağlama** sayfasındaki ilerleme çubuğu, sağlamanın en son ne zaman başlatıldığını gösterir. Sağlama işini el ile yeniden başlatmanız gerekiyorsa aşağıdaki yöntemlerden birini kullanın:  

- Sağlama işini yeniden başlatmak için Azure portal kullanın. Uygulamanın **sağlama** sayfasında, **sağlamayı yeniden Başlat**' ı seçin. Bu eylem, sağlama hizmetini tamamen yeniden başlatır ve bu işlem biraz zaman alabilir. Tam bir başlangıç döngüsünün yeniden çalışması için, escrows 'yi temizler, uygulamayı karantinadan kaldırır ve tüm filigranları temizler. Daha sonra hizmet, kaynak sistemdeki tüm kullanıcıları yeniden değerlendirir ve sağlama kapsamında olup olmadıklarını saptacaktır. Bu makalede açıklandığı gibi, uygulamanız Şu anda karantinaya alınır veya öznitelik eşlemelerinizde bir değişiklik yapmanız gerekiyorsa, bu yararlı olabilir. İlk döngüsünün değerlendirilmesi gereken nesne sayısı nedeniyle, tipik artımlı döngüden daha uzun sürdüğüne göz önünde unutmayın. [Burada](application-provisioning-when-will-provisioning-finish-specific-user.md)ilk ve artımlı döngülerin performansı hakkında daha fazla bilgi edinebilirsiniz.

- [Sağlama işini yeniden başlatmak](/graph/api/synchronization-synchronizationjob-restart?tabs=http&view=graph-rest-beta&preserve-view=true)için Microsoft Graph kullanın. Yeniden başlatdıklarınız üzerinde tam denetime sahip olacaksınız. Et 'ları kaldırmayı seçebilirsiniz (karantina durumuna göre tahakkuk eden Emanet sayacını yeniden başlatmak için), karantinayı temizleyebilir (uygulamayı karantinadan kaldırmak için) veya filigranları temizleyebilirsiniz. Aşağıdaki isteği kullanın:
 
```microsoft-graph
        POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart
```

"{ID}" öğesini uygulama KIMLIĞI değeriyle değiştirin ve "{JobId}" öğesini [eşitleme Işinin kimliğiyle](/graph/api/resources/synchronization-configure-with-directory-extension-attributes?tabs=http&view=graph-rest-beta&preserve-view=true#list-synchronization-jobs-in-the-context-of-the-service-principal)değiştirin.
