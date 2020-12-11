---
title: MS Graph API kullanarak bulut sağlama için gelen eşitleme
description: Bu konuda yalnızca Graph API kullanarak gelen eşitlemenin nasıl etkinleştirileceği açıklanmaktadır
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c6ed106462d7dcac3f31a2ab157e5b634391006
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97093460"
---
# <a name="inbound-synchronization-for-cloud-provisioning-using-ms-graph-api"></a>MS Graph API kullanarak bulut sağlama için gelen eşitleme

Aşağıdaki belge, yalnızca MSGraph API 'Leri kullanarak bir eşitleme profilinin sıfırdan nasıl çoğaltılacağını açıklamaktadır.  
Bunun nasıl yapılacağını gösteren yapı aşağıdaki adımlardan oluşur.  Bunlar:

- [Temel kurulum](#basic-setup)
- [Hizmet sorumluları oluşturma](#create-service-principals)
- [Eşitleme Işi oluştur](#create-sync-job)
- [Hedeflenen etki alanını güncelleştir](#update-targeted-domain)
- [Eşitleme işini Başlat](#start-sync-job)
- [İnceleme durumu](#review-status)

Bu [Windows PowerShell için Microsoft Azure Active Directory modülü](https://docs.microsoft.com/powershell/module/msonline/) komutlarını, bu kiracı Için Yönetim Web hizmetini çağırabilmek üzere bir üretim kiracısı için eşitlemeyi etkinleştirmek üzere kullanın.

## <a name="basic-setup"></a>Temel kurulum

### <a name="enable-tenant-flags"></a>Kiracı bayraklarını etkinleştir

 ```PowerShell
 Connect-MsolService ('-AzureEnvironment <AzureEnvironmnet>')
 Set-MsolDirSyncEnabled -EnableDirSync $true
 ```
Bu iki komutun ilki Azure Active Directory kimlik bilgilerini gerektirir. Bu commandde kiracıyı örtülü olarak tanımlayıp eşitleme için etkinleştirebilmenizi sağlar.

## <a name="create-service-principals"></a>Hizmet sorumlusu oluşturma
Sonra, [AD2AAD uygulaması/hizmet sorumlusunu](/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) oluşturuyoruz

Bu uygulama KIMLIĞI 1a4721b3-e57f-4451-ae87-ef078703ec94 kullanmanız gerekir. DisplayName, portalda kullanılıyorsa (örneğin, contoso.com) AD etki alanı URL 'sidir, ancak başka bir şey adlandırılmış olabilir.

 ```
 POST https://graph.microsoft.com/beta/applicationTemplates/1a4721b3-e57f-4451-ae87-ef078703ec94/instantiate
 Content-type: application/json
 {
    displayName: [your app name here]
 }
 ```


## <a name="create-sync-job"></a>Eşitleme işi oluştur
Yukarıdaki komutun çıktısı, oluşturulan hizmet sorumlusunun ObjectID değerini döndürür. Bu örnekte, ObjectID 614ac0e9-a59b-481f-bd8f-79a73d167e1c olur.  Bu hizmet sorumlusuna bir synchronizationJob eklemek için Microsoft Graph kullanın.  

Eşitleme işi oluşturmaya yönelik belgeler [burada](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-post?view=graph-rest-beta&tabs=http)bulunabilir.

Yukarıdaki KIMLIĞI kaydetmediyseniz, aşağıdaki MS Graf çağrısını çalıştırarak hizmet sorumlusunu bulabilirsiniz. Dizin. Read. bu çağrıyı yapmak için tüm izinlere sahip olmanız gerekir:
 
 `GET https://graph.microsoft.com/beta/servicePrincipals `

Ardından çıktıda Uygulama adınızı arayın.

İki iş oluşturmak için aşağıdaki iki komutu çalıştırın: biri Kullanıcı/Grup sağlama için ve diğeri Parola karması eşitlemesi için. Aynı istek iki kez aynı ancak farklı şablon kimlikleriyle aynı.


Aşağıdaki iki isteği çağırın:

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADProvisioning"
 } 
 ```

 ```
 POST https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs
 Content-type: application/json
 {
 "templateId":"AD2AADPasswordHash"
 }
 ```

Her ikisini de oluşturmak istiyorsanız iki çağrıya ihtiyaç duyarsınız.

Örnek dönüş değeri (sağlama için):

 ```
HTTP 201/Created
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#servicePrincipals('614ac0e9-a59b-481f-bd8f-79a73d167e1c')/synchronization/jobs/$entity",
    "id": "AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da",
    "templateId": "ADDCInPassthrough",
    "schedule": {
        "expiration": null,
        "interval": "PT40M",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "quarantine": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "troubleshootingUrl": null,
        "progress": [],
        "synchronizedEntryCountByType": []
    }
}
```

## <a name="update-targeted-domain"></a>Hedeflenen etki alanını güncelleştir
Bu kiracı için, hizmet sorumlusunun nesne tanımlayıcısı ve uygulama tanımlayıcısı aşağıdaki gibidir:

ObjectID: 8895955e-2e6c-4d79-8943-4d72ca36878f AppID: 00000014-0000-0000-C000-000000000000 DisplayName: testApp

Bu yapılandırmanın hedeflediği etki alanını güncelleştirmemiz gerekiyor, bu nedenle bu etki alanı için gizli dizileri güncelleştirin.

Kullandığınız etki alanı adının, şirket içi etki alanı denetleyiciniz için ayarladığınız URL ile aynı olduğundan emin olun

 ```
 PUT https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/secrets
 Content-type: application/json
 {
  "value": [
    {
      "key": "Domain",
       {"value":[{"key":"Domain","value":"{\"domain\":\"[YOUR_DOMAIN_NAME]\"}"}]}
    }
  ]
 }
 ```

Beklenen yanıt... HTTP 204/içerik yok

Burada, vurgulanan "etki alanı" değeri, girişlerin Azure Active Directory sağlanacak olan şirket içi Active Directory etki alanının adıdır.

## <a name="start-sync-job"></a>Eşitleme işini Başlat
İş aşağıdaki komutla yeniden alınabilir:

 `GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ ` 

İşlerin alınmasına ilişkin belgeler [burada](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-list?view=graph-rest-beta&tabs=http)bulunabilir. 
 
İşi başlatmak için, ilk adımda oluşturulan hizmet sorumlusu ObjectID 'yi ve işi oluşturan istekten döndürülen iş tanımlayıcısını kullanarak bu isteği verin.

Bir işi başlatmaya ilişkin belgeler [burada](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-start?view=graph-rest-beta&tabs=http)bulunabilir. 

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/start
 ```

Beklenen yanıt... HTTP 204/içerik yok.

İşi denetlemeye yönelik diğer komutlar [burada](https://docs.microsoft.com/graph/api/resources/synchronization-synchronizationjob?view=graph-rest-beta)belgelenmiştir.
 
Bir işi yeniden başlatmak için, bunlardan biri kullanılır...

 ```
 POST  https://graph.microsoft.com/beta/servicePrincipals/8895955e-2e6c-4d79-8943-4d72ca36878f/synchronization/jobs/AD2AADProvisioning.fc96887f36da47508c935c28a0c0b6da/restart
 {
   "criteria": {
       "resetScope": "Full"
   }
 }
 ```

## <a name="review-status"></a>İnceleme durumu
İş durumlarını aracılığıyla alın...

 ```
 GET https://graph.microsoft.com/beta/servicePrincipals/[SERVICE_PRINCIPAL_ID]/synchronization/jobs/ 
 ```

İlgili ayrıntılar için dönüş nesnesinin ' durum ' bölümüne bakın

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
- [Dönüşümler](how-to-transformation.md)
- [Azure AD eşitleme API 'SI](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)
