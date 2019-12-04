---
title: Azure AD şemasını ve özel ifadeleri anlama
description: Bu konuda, sağlama aracısının akan öznitelikleri ve özel ifadeleri Azure AD şeması açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae594bcc20e3c4ed1c6fbd0333699de8c9f4452
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794503"
---
# <a name="understanding-the-azure-ad-schema"></a>Azure AD şemasını anlama
Azure AD 'deki bir nesne, tüm dizin gibi, kullanıcılar, gruplar ve kişiler gibi şeyleri temsil eden bir programlama üst düzey veri yapısıdır.  Azure AD 'de yeni bir kullanıcı veya ilgili kişi oluşturduğunuzda, bu nesnenin yeni bir örneğini oluşturuyorsunuz.  Bu örnekler özelliklerine göre farklılaştırılabilir.

Azure AD 'deki özellikler, Azure AD 'de bir nesnenin örneği hakkındaki bilgileri saklamaktan sorumlu olan öğelerdir.  

Azure AD şeması, bir girişte özelliklerin kullanılabileceği kuralları, bu özelliklerin sahip olabileceği değer türlerini ve kullanıcıların bu değerlerle nasıl etkileşime girebileceğini tanımlar. 

Azure AD 'nin iki tür özelliği vardır.  Özellikler şunlardır:
- **Yerleşik özellikler** – Azure AD şeması tarafından önceden tanımlanan özellikler.  Bu özellikler farklı kullanımlar sağlar ve erişilebilir olmayabilir.
- **Dizin genişletmeleri** – Azure AD 'yi kendi kullanım için özelleştirebilmeniz için sunulan özellikler.  Örneğin, şirket içi Active Directory belirli bir öznitelikle genişlettiyseniz ve bu özniteliği Flow istiyorsanız, belirtilen özel özelliklerden birini kullanabilirsiniz. 

## <a name="attributes-and-expressions"></a>Öznitelikler ve ifadeler
Kullanıcı gibi bir nesne Azure AD 'ye sağlandığında, Kullanıcı nesnesinin yeni bir örneği oluşturulur.  Bu oluşturma, bu nesnenin öznitelikleri olarak da bilinen özelliklerini içerir.  Başlangıçta, yeni oluşturulan nesnenin öznitelikleri, eşitleme kuralları tarafından belirlenen değerlere ayarlanır.  Bu öznitelikler daha sonra bulut sağlama Aracısı aracılığıyla güncel tutulur.

![](media/concept-attributes/attribute1.png)

Örneğin, bir Kullanıcı pazarlama bölümünün parçasıysa, başlangıçta Azure AD departmanı özniteliği sağlandığında oluşturulur ve ardından değer pazarlama olarak ayarlanır.  Ancak sonra altı ay sonra Sales olarak değişir.  Şirket içi AD departmanı özniteliği Sales olarak değiştirilir.  Bu değişiklik daha sonra Azure AD 'ye eşitlenir ve Azure AD kullanıcı nesnesine yansıtılır.

Öznitelik eşitlemesi doğrudan, Azure AD 'deki değerin şirket içi özniteliğinin değerine doğrudan ayarlandığı bir değer olabilir.  Ya da bu eşitlemeyi işleyen bir programlama ifadesi olabilir.  Bir programlı ifade, değeri doldurmak için bazı mantığın veya belirlemenin yapılması gereken durumlarda gereklidir.

Örneğin, posta özniteliktir ("john.smith@contoso.com") ve "@contoso.com" bölümünü sökmek ve yalnızca "John. Smith" değerini akışa almak zorundaysam şuna benzer bir şey kullanıyorum:

`Replace([mail], "@contoso.com", , ,"", ,)`  

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): "john.smith@contoso.com"
* **Çıkış**: "John. Smith"

Ek bilgi için, özel ifadeler yazma hakkında ve söz dizimi, bkz. [Azure Active Directory öznitelik eşlemeleri Için Ifadeler yazma](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data).

Aşağıdaki liste ortak özniteliklerdir ve bunların Azure AD ile nasıl eşitlendikleri açıklanır.


|Şirket içi Active Directory|Eşleme türü|Azure AD|
|-----|-----|-----|
|,|Doğrudan|commonName
|countryCode|Doğrudan|countryCode|
|displayName|Doğrudan|displayName|
|givenName|İfadeler|givenName|
|Objectguıd 'dir|Doğrudan|sourceAnchorBinary|  
|userprincipalName|Doğrudan|userPrincipalName|
|ProxyAdress|Doğrudan|proxyAddress|

## <a name="viewing-the-schema"></a>Şemayı görüntüleme
Şemayı görüntülemek ve doğrulamak için aşağıdaki adımları uygulayın:

1.  [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)'a gidin.
2.  Genel Yönetici hesabınızla oturum açın
3.  Sol tarafta, **izinleri değiştir** ' e tıklayın ve **Directory. ReadWrite. All** ' un tamamen yapıldığından emin olun.
4.  Şu sorguyu çalıştırın: https://graph.microsoft.com/beta/serviceprincipals/.  Bu sorgu, hizmet sorumluları listesinin bir listesini döndürür.
5.  "AppDisplayName": "Active Directory Azure Active Directory sağlamayı" ve "id:" değerini aklınızda bulun.
    ```
    "value": [
            {
                "id": "00d41b14-7958-45ad-9d75-d52fa29e02a1",
                "deletedDateTime": null,
                "accountEnabled": true,
                "appDisplayName": "Active Directory to Azure Active Directory Provisioning",
                "appId": "1a4721b3-e57f-4451-ae87-ef078703ec94",
                "applicationTemplateId": null,
                "appOwnerOrganizationId": "47df5bb7-e6bc-4256-afb0-dd8c8e3c1ce8",
                "appRoleAssignmentRequired": false,
                "displayName": "Active Directory to Azure Active Directory Provisioning",
                "errorUrl": null,
                "homepage": "https://account.activedirectory.windowsazure.com:444/applications/default.aspx?metadata=AD2AADProvisioning|ISV9.1|primary|z",
                "loginUrl": null,
                "logoutUrl": null,
                "notificationEmailAddresses": [],
                "preferredSingleSignOnMode": null,
                "preferredTokenSigningKeyEndDateTime": null,
                "preferredTokenSigningKeyThumbprint": null,
                "publisherName": "Active Directory Application Registry",
                "replyUrls": [],
                "samlMetadataUrl": null,
                "samlSingleSignOnSettings": null,
                "servicePrincipalNames": [
                    "http://adapplicationregistry.onmicrosoft.com/adprovisioningtoaad/primary",
                    "1a4721b3-e57f-4451-ae87-ef078703ec94"
                ],
                "signInAudience": "AzureADMultipleOrgs",
                "tags": [
                    "WindowsAzureActiveDirectoryIntegratedApp"
                ],
                "addIns": [],
                "api": {
                    "resourceSpecificApplicationPermissions": []
                },
                "appRoles": [
                    {
                        "allowedMemberTypes": [
                            "User"
                        ],
                        "description": "msiam_access",
                        "displayName": "msiam_access",
                        "id": "a0326856-1f51-4311-8ae7-a034d168eedf",
                        "isEnabled": true,
                        "origin": "Application",
                        "value": null
                    }
                ],
                "info": {
                    "termsOfServiceUrl": null,
                    "supportUrl": null,
                    "privacyStatementUrl": null,
                    "marketingUrl": null,
                    "logoUrl": null
                },
                "keyCredentials": [],
                "publishedPermissionScopes": [
                    {
                        "adminConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on behalf of the signed-in user.",
                        "adminConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "id": "d40ed463-646c-4efe-bb3e-3fa7d0006688",
                        "isEnabled": true,
                        "type": "User",
                        "userConsentDescription": "Allow the application to access Active Directory to Azure Active Directory Provisioning on your behalf.",
                        "userConsentDisplayName": "Access Active Directory to Azure Active Directory Provisioning",
                        "value": "user_impersonation"
                    }
                ],
                "passwordCredentials": []
            },
    ```
6. {Service Principal ID} değerini değer ile değiştirin ve şu sorguyu çalıştırın: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal id}/synchronization/jobs/`
7. "İd": "AD2AADProvisioning. fd1c9b9e8077402c8bc03a7186c8f976" bölümünü bulun ve "kimlik:" olarak aklınızda edin.
    ```
    {
                "id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976",
                "templateId": "AD2AADProvisioning",
                "schedule": {
                    "expiration": null,
                    "interval": "PT2M",
                    "state": "Active"
                },
                "status": {
                    "countSuccessiveCompleteFailures": 0,
                    "escrowsPruned": false,
                    "code": "Active",
                    "lastSuccessfulExecutionWithExports": null,
                    "quarantine": null,
                    "steadyStateFirstAchievedTime": "2019-11-08T15:48:05.7360238Z",
                    "steadyStateLastAchievedTime": "2019-11-20T16:17:24.7957721Z",
                    "troubleshootingUrl": "",
                    "lastExecution": {
                        "activityIdentifier": "2dea06a7-2960-420d-931e-f6c807ebda24",
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 15,
                        "countEscrowedRaw": 15,
                        "countExported": 0,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "2019-11-20T16:15:21.116098Z",
                        "timeEnded": "2019-11-20T16:17:24.7488681Z"
                    },
                    "lastSuccessfulExecution": {
                        "activityIdentifier": null,
                        "countEntitled": 0,
                        "countEntitledForProvisioning": 0,
                        "countEscrowed": 0,
                        "countEscrowedRaw": 0,
                        "countExported": 5,
                        "countExports": 0,
                        "countImported": 0,
                        "countImportedDeltas": 0,
                        "countImportedReferenceDeltas": 0,
                        "state": "Succeeded",
                        "error": null,
                        "timeBegan": "0001-01-01T00:00:00Z",
                        "timeEnded": "2019-11-20T14:09:46.8855027Z"
                    },
                    "progress": [],
                    "synchronizedEntryCountByType": [
                        {
                            "key": "group to Group",
                            "value": 33
                        },
                        {
                            "key": "user to User",
                            "value": 3
                        }
                    ]
                },
                "synchronizationJobSettings": [
                    {
                        "name": "Domain",
                        "value": "{\"DomainFQDN\":\"contoso.com\",\"DomainNetBios\":\"CONTOSO\",\"ForestFQDN\":\"contoso.com\",\"ForestNetBios\":\"CONTOSO\"}"
                    },
                    {
                        "name": "DomainFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "DomainNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "ForestFQDN",
                        "value": "contoso.com"
                    },
                    {
                        "name": "ForestNetBios",
                        "value": "CONTOSO"
                    },
                    {
                        "name": "QuarantineTooManyDeletesThreshold",
                        "value": "500"
                    }
                ]
            }
    ```
8. Şimdi şu sorguyu çalıştırın: `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal Id}/synchronization/jobs/{AD2AAD Provisioning id}/schema`
 
    Örnek: https://graph.microsoft.com/beta/serviceprincipals/653c0018-51f4-4736-a3a3-94da5dcb6862/synchronization/jobs/AD2AADProvisioning.e9287a7367e444c88dc67a531c36d8ec/schema

 {Service Principal ID} ve {AD2ADD sağlama kimliği} değerlerini değerlerinizle değiştirin.

9. Bu sorgu şemayı döndürecek.
  ![](media/concept-attributes/schema1.png)
 
## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
