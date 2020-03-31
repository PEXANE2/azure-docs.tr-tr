---
title: Sağlama yapılandırması için Microsoft Graph API'lerini kullanın - Azure Active Directory | Microsoft Dokümanlar
description: Bir uygulamanın birden çok örneği için sağlama ayarlamanız mı gerekiyor? Otomatik sağlama yapılandırmasını otomatikleştirmek için Microsoft Graph API'lerini kullanarak zamandan nasıl tasarruf edebilirsiniz öğrenin.
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
ms.date: 11/15/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: c72217a565071f9531281af1862ba3681e353a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481475"
---
# <a name="configure-provisioning-using-microsoft-graph-apis"></a>Microsoft Graph API'lerini kullanarak sağlama yı yapılandırma

Azure portalı, tek tek uygulamalar için sağlamayı teker teker yapılandırmanın kullanışlı bir yoludur. Ancak, bir uygulamanın birkaç hatta yüzlerce örneğini oluşturuyorsanız, Uygulama oluşturma yı ve yapılandırmayı Microsoft Graph API'leriyle otomatikleştirmek daha kolay olabilir. Bu makalede, API'ler aracılığıyla sağlama yapılandırması otomatikleştirilen özetler. Bu yöntem genellikle Amazon Web [Services](../saas-apps/amazon-web-service-tutorial.md#configure-azure-ad-sso)gibi uygulamalar için kullanılır.

**Sağlama yapılandırmasını otomatikleştirmek için Microsoft Graph API'lerini kullanma adımlarına genel bakış**


|Adım  |Ayrıntılar  |
|---------|---------|
|[Adım 1. Galeri uygulamasını oluşturma](#step-1-create-the-gallery-application)     |API istemcisinde oturum açma <br> Galeri uygulama şablonu alma <br> Galeri uygulamasını oluşturma         |
|[Adım 2. Şablona dayalı sağlama işi oluşturma](#step-2-create-the-provisioning-job-based-on-the-template)     |Sağlama bağlayıcısı için şablonu alma <br> Sağlama işini oluşturma         |
|[Adım 3. Erişimi yetkilendirme](#step-3-authorize-access)     |Uygulamaya bağlantıyı test etme <br> Kimlik bilgilerini kaydetme         |
|[Adım 4. İşe başlama](#step-4-start-the-provisioning-job)     |İşi başlatma         |
|[Adım 5. İzleme sağlama](#step-5-monitor-provisioning)     |Sağlama işinin durumunu kontrol edin <br> Sağlama günlüklerini alma         |

> [!NOTE]
> Bu makalede gösterilen yanıt nesneleri okunabilirlik için kısaltılabilir. Tüm özellikler gerçek bir çağrıdan döndürülür.

## <a name="step-1-create-the-gallery-application"></a>Adım 1: Galeri uygulamasını oluşturma

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Microsoft Graph Explorer (önerilen), Postacı veya kullandığınız diğer API istemcilerinde oturum açın

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başlat
1. "Microsoft ile Oturum Aç" düğmesini seçin ve Azure AD global yöneticisi veya App Yöneticisi kimlik bilgilerini kullanarak oturum açın.

    ![Grafik Oturum Açma](./media/application-provisioning-configure-api/wd_export_02.png)

1. Başarılı oturum açma dan sonra, kullanıcı hesabı ayrıntılarını sol bölmede görürsünüz.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Galeri uygulama şablonu tanımlayıcısını alma
Azure AD uygulama galerisindeki uygulamaların her birinde, bu uygulamanın meta verilerini açıklayan bir [uygulama şablonu](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) vardır. Bu şablonu kullanarak, yönetim için kiracınızda uygulama ve hizmet sorumlusunun bir örneğini oluşturabilirsiniz.

#### <a name="request"></a>*İstek*

<!-- {
  "blockType": "request",
  "name": "get_applicationtemplates"
}-->

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates
```

#### <a name="response"></a>*Yanıt*

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationTemplate",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
  "value": [
  {
    "id": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "Amazon Web Services (AWS)",
        "homePageUrl": "http://aws.amazon.com/",
        "supportedSingleSignOnModes": [
             "password",
             "saml",
             "external"
         ],
         "supportedProvisioningTypes": [
             "sync"
         ],
         "logoUrl": "https://az495088.vo.msecnd.net/app-logo/aws_215.png",
         "categories": [
             "developerServices"
         ],
         "publisher": "Amazon",
         "description": null    
  
}
```

### <a name="create-the-gallery-application"></a>Galeri uygulamasını oluşturma

Kiracınızda uygulama ve hizmet sorumlusunun bir [örneğini oluşturmak](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) için son adımda uygulamanız için alınan şablon kimliğini kullanın.

#### <a name="request"></a>*İstek*

<!-- {
  "blockType": "request",
  "name": "applicationtemplate_instantiate"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/{id}/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>*Yanıt*


<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationServicePrincipal"
} -->

```http
HTTP/1.1 201 OK
Content-type: application/json


{
    "application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63",
        "appId": "92653dd4-aa3a-3323-80cf-e8cfefcc8d5d",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "AWS Contoso",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "logoutUrl": null,
        "samlMetadataUrl": null,
    },
    "servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e",
        "appDisplayName": "AWS Contoso",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "appRoleAssignmentRequired": true,
        "displayName": "My custom name",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "servicePrincipalNames": [
            "93653dd4-aa3a-4323-80cf-e8cfefcc8d7d"
        ],
        "tags": [
            "WindowsAzureActiveDirectoryIntegratedApp"
        ],
    }
}
```

## <a name="step-2-create-the-provisioning-job-based-on-the-template"></a>Adım 2: Şablona dayalı sağlama işini oluşturma

### <a name="retrieve-the-template-for-the-provisioning-connector"></a>Sağlama bağlayıcısı için şablonu alma

Sağlama için etkinleştirilen galerideki uygulamaların yapılandırmayı kolaylaştırmak için şablonları vardır. [Sağlama yapılandırması için şablonu almak için](https://docs.microsoft.com/graph/api/synchronization-synchronizationtemplate-list?view=graph-rest-beta&tabs=http)aşağıdaki isteği kullanın. Kimliği sağlamanız gerekeceğini unutmayın. Kimlik, bu durumda ServicePrincipal olan önceki kaynağa başvurur. 

#### <a name="request"></a>*İstek*

<!-- {
  "blockType": "request",
  "name": "get_synchronizationtemplate"
}-->
```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/templates
```


#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationTemplate",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK

{
    "value": [
        {
            "id": "aws",
            "factoryTag": "aws",
            "schema": {
                    "directories": [],
                    "synchronizationRules": []
                    }
        }
    ]
}
```

### <a name="create-the-provisioning-job"></a>Sağlama işini oluşturma
Sağlamayı etkinleştirmek için öncelikle [bir iş oluşturmanız](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-post?view=graph-rest-beta&tabs=http)gerekir. Bir sağlama işi oluşturmak için aşağıdaki isteği kullanın. İş için kullanılacak şablonu belirtirken önceki adımdaki şablonId'i kullanın.

#### <a name="request"></a>*İstek*
<!-- {
  "blockType": "request",
  "name": "create_synchronizationjob_from_synchronization"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs
Content-type: application/json

{ 
    "templateId": "aws"
}
```

#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationJob"
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "NotConfigured",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    }
}
```

## <a name="step-3-authorize-access"></a>Adım 3: Erişimi yetkilendirme

### <a name="test-the-connection-to-the-application"></a>Uygulamaya bağlantıyı test etme

Üçüncü taraf uygulamasıyla bağlantıyı test edin. Aşağıdaki örnek clientSecret ve secretToken gerektiren bir uygulama içindir. Her uygulamanın gereksinimleri vardır. Uygulamalar genellikle ClientSecret yerine BaseAddress'i kullanır. Uygulamanızın hangi kimlik bilgilerini gerektirdiğini belirlemek için, uygulamanız için sağlama yapılandırma sayfasına gidin ve geliştirici modunda test bağlantısını tıklatın. Ağ trafiği kimlik bilgileri için kullanılan parametreleri gösterir. Kimlik bilgilerinin tam listesini [burada](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-validatecredentials?view=graph-rest-beta&tabs=http)bulabilirsiniz. 

#### <a name="request"></a>*İstek*
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{id}/validateCredentials
{ 
    credentials: [ 
        { key: "ClientSecret", value: "xxxxxxxxxxxxxxxxxxxxx" },
        { key: "SecretToken", value: "xxxxxxxxxxxxxxxxxxxxx" }
    ]
}
```
#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```

### <a name="save-your-credentials"></a>Kimlik bilgilerinizi kaydedin

Sağlamanın yapılandırılması, Azure AD ile uygulama arasında bir güven oluşturulmasını gerektirir. Üçüncü taraf uygulamasına erişim yetkisi ver. Aşağıdaki örnek clientSecret ve secretToken gerektiren bir uygulama içindir. Her uygulamanın gereksinimleri vardır. Kullanılabilir seçenekleri görmek için [API belgelerini](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-validatecredentials?view=graph-rest-beta&tabs=http) gözden geçirin. 

#### <a name="request"></a>*İstek*
```msgraph-interactive
PUT https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/secrets 
 
{ 
    value: [ 
        { key: "ClientSecret", value: "xxxxxxxxxxxxxxxxxxxxx" },
        { key: "SecretToken", value: "xxxxxxxxxxxxxxxxxxxxx" }
    ]
}
```

#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```

## <a name="step-4-start-the-provisioning-job"></a>Adım 4: Sağlama işini başlatın
Şimdi sağlama işi yapılandırıldığından, işi başlatmak için aşağıdaki [komutu](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-start?view=graph-rest-beta&tabs=http)kullanın. 


#### <a name="request"></a>*İstek*
<!-- {
  "blockType": "request",
  "name": "synchronizationjob_start"
}-->
```http
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/start
```

#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.None"
} -->
```http
HTTP/1.1 204 No Content
```


## <a name="step-5-monitor-provisioning"></a>Adım 5: Sağlamayı izleme

### <a name="monitor-the-provisioning-job-status"></a>Sağlama iş durumunu izleme

Sağlama işi artık çalışıyorsa, geçerli sağlama döngüsünün ilerlemesini ve hedef sistemde oluşturulan kullanıcı ve grup sayısı gibi istatistikleri izlemek için aşağıdaki komutu kullanın. 

#### <a name="request"></a>*İstek*
<!-- {
  "blockType": "request",
  "name": "get_synchronizationjob"
}-->
```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.synchronizationJob"
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 2577

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "progress": [],
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    },
    "synchronizationJobSettings": [
      {
          "name": "QuarantineTooManyDeletesThreshold",
          "value": "500"
      }
    ]
}
```


### <a name="monitor-provisioning-events-using-the-provisioning-logs"></a>Sağlama günlüklerini kullanarak sağlama olaylarını izleme
Sağlama işinin durumunu izlemeye ek olarak, oluşan tüm olayları sorgulamak için [sağlama günlüklerini](https://docs.microsoft.com/graph/api/provisioningobjectsummary-list?view=graph-rest-beta&tabs=http) kullanabilirsiniz (örn. belirli bir kullanıcı için sorgu ve bunların başarıyla sağlanıp sağlanmadığını belirleme).

#### <a name="request"></a>*İstek*
```msgraph-interactive
GET https://graph.microsoft.com/beta/auditLogs/provisioning
```
#### <a name="response"></a>*Yanıt*
<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.provisioningObjectSummary",
  "name": "list_provisioningobjectsummary_error"
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#auditLogs/provisioning",
    "value": [
        {
            "id": "gc532ff9-r265-ec76-861e-42e2970a8218",
            "activityDateTime": "2019-06-24T20:53:08Z",
            "tenantId": "7928d5b5-7442-4a97-ne2d-66f9j9972ecn",
            "jobId": "BoxOutDelta.7928d5b574424a97ne2d66f9j9972ecn",
            "cycleId": "44576n58-v14b-70fj-8404-3d22tt46ed93",
            "changeId": "eaad2f8b-e6e3-409b-83bd-e4e2e57177d5",
            "action": "Create",
            "durationInMilliseconds": 2785,
            "sourceSystem": {
                "id": "0404601d-a9c0-4ec7-bbcd-02660120d8c9",
                "displayName": "Azure Active Directory",
                "details": {}
            },
            "targetSystem": {
                "id": "cd22f60b-5f2d-1adg-adb4-76ef31db996b",
                "displayName": "Box",
                "details": {
                    "ApplicationId": "f2764360-e0ec-5676-711e-cd6fc0d4dd61",
                    "ServicePrincipalId": "chc46a42-966b-47d7-9774-576b1c8bd0b8",
                    "ServicePrincipalDisplayName": "Box"
                }
            },
            "initiatedBy": {
                "id": "",
                "displayName": "Azure AD Provisioning Service",
                "initiatorType": "system"
            },
            "sourceIdentity": {
                "id": "5e6c9rae-ab4d-5239-8ad0-174391d110eb",
                "displayName": "Self-service Pilot",
                "identityType": "Group",
                "details": {}
            },
            "targetIdentity": {
                "id": "",
                "displayName": "",
                "identityType": "Group",
                "details": {}
            },
            "statusInfo": {
                "@odata.type": "#microsoft.graph.statusDetails",
                "status": "failure",
                "errorCode": "BoxEntryConflict",
                "reason": "Message: Box returned an error response with the HTTP status code 409.  This response indicates that a user or a group already exisits with the same name. This can be avoided by identifying and removing the conflicting user from Box via the Box administrative user interface, or removing the current user from the scope of provisioning either by removing their assignment to the Box application in Azure Active Directory or adding a scoping filter to exclude the user.",
                "additionalDetails": null,
                "errorCategory": "NonServiceFailure",
                "recommendedAction": null
            },
            "provisioningSteps": [
                {
                    "name": "EntryImportAdd",
                    "provisioningStepType": "import",
                    "status": "success",
                    "description": "Received Group 'Self-service Pilot' change of type (Add) from Azure Active Directory",
                    "details": {}
                },
                {
                    "name": "EntrySynchronizationAdd",
                    "provisioningStepType": "matching",
                    "status": "success",
                    "description": "Group 'Self-service Pilot' will be created in Box (Group is active and assigned in Azure Active Directory, but no matching Group was found in Box)",
                    "details": {}
                },
                {
                    "name": "EntryExportAdd",
                    "provisioningStepType": "export",
                    "status": "failure",
                    "description": "Failed to create Group 'Self-service Pilot' in Box",
                    "details": {
                        "ReportableIdentifier": "Self-service Pilot"
                    }
                }
            ],
            "modifiedProperties": [
                {
                    "displayName": "objectId",
                    "oldValue": null,
                    "newValue": "5e0c9eae-ad3d-4139-5ad0-174391d110eb"
                },
                {
                    "displayName": "displayName",
                    "oldValue": null,
                    "newValue": "Self-service Pilot"
                },
                {
                    "displayName": "mailEnabled",
                    "oldValue": null,
                    "newValue": "False"
                },
                {
                    "displayName": "mailNickname",
                    "oldValue": null,
                    "newValue": "5ce25n9a-4c5f-45c9-8362-ef3da29c66c5"
                },
                {
                    "displayName": "securityEnabled",
                    "oldValue": null,
                    "newValue": "True"
                },
                {
                    "displayName": "Name",
                    "oldValue": null,
                    "newValue": "Self-service Pilot"
                }
            ]
       }
    ]
}

```
## <a name="related-articles"></a>İlgili makaleler:

- [Eşitleme Microsoft Graph belgelerini gözden geçirme](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)
- [Azure AD ile özel bir SCIM uygulamasını tümleştirme](use-scim-to-provision-users-and-groups.md)
