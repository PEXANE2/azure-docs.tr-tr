---
title: Azure AD şemasını ve özel ifadeleri anlama
description: Bu makalede, Azure AD şeması, madde sağlayan aracının aktığı öznitelikler ve özel ifadeler açıklanmaktadır.
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
ms.date: 02/18/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ac09fb3faf55be6c07a1e0a88b6e2032c9ab8ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299338"
---
# <a name="understand-the-azure-ad-schema"></a>Azure AD şemasını anlama
Azure Etkin Dizini'ndeki (Azure AD) bir nesne, herhangi bir dizin gibi, kullanıcılar, gruplar ve kişiler gibi şeyleri temsil eden programatik bir üst düzey veri yapısıdır. Azure AD'de yeni bir kullanıcı veya kişi oluşturduğunuzda, bu nesnenin yeni bir örneğini oluşturursunuz. Bu örnekler özelliklerine göre ayırt edilebilir.

Azure AD'deki özellikler, Azure AD'deki bir nesnenin örneği hakkında bilgi depolamaktan sorumlu öğelerdir.

Azure AD şeması, bir girişte hangi özelliklerin kullanılabileceğini, bu özelliklerin sahip olabileceği değer türlerini ve kullanıcıların bu değerlerle nasıl etkileşimde bulunabilirlerini tanımlar. 

Azure AD'nin iki tür özelliği vardır:
- Yerleşik özellikler : Azure AD şeması tarafından önceden tanımlanmış **özellikler.** Bu özellikler farklı kullanımlar sağlar ve erişilebilir olabilir veya olmayabilir.
- **Dizin uzantıları**: Azure AD'yi kendi kullanımınız için özelleştirebilmeniz için sağlanan özellikler. Örneğin, şirket içi Active Directory'nizi belirli bir öznitelikle genişlettiyseniz ve bu öznitelikten akış yapmak istiyorsanız, sağlanan özel özelliklerden birini kullanabilirsiniz. 

## <a name="attributes-and-expressions"></a>Öznitelikler ve ifadeler
Kullanıcı gibi bir nesne Azure AD'ye sağlandığında, kullanıcı nesnesinin yeni bir örneği oluşturulur. Bu oluşturma, öznitelikler olarak da bilinen bu nesnenin özelliklerini içerir. Başlangıçta, yeni oluşturulan nesnenin öznitelikleri eşitleme kuralları tarafından belirlenen değerlere ayarlanmıştır. Bu öznitelikler daha sonra bulut sağlama aracısı aracılığıyla güncel tutulur.

![Nesne sağlama](media/concept-attributes/attribute1.png)

Örneğin, bir kullanıcı Pazarlama bölümünün bir parçası olabilir. Azure AD departman özniteliği başlangıçta sağlandığında oluşturulur ve değer Pazarlama olarak ayarlanır. Altı ay sonra, Satış'a geçiş yaparlarsa, şirket içi Active Directory departman özniteliği Satış olarak değiştirilir. Bu değişiklik Azure AD ile eşitlenir ve Azure AD kullanıcı nesnelerine yansıtılır.

Öznitelik eşitlemesi, Azure AD'deki değerin doğrudan şirket içi öznitelik değerine ayarlandığı doğrudan olabilir. Veya, programatik bir ifade eşitleme işleyebilir. Bazı mantık veya bir belirleme değeri doldurmak için yapılması gereken durumlarda programlı bir ifade gereklidir.

Örneğin, posta özniteliği " "john.smith@contoso.comolsaydı ve " "@contoso.comkısmını ve yalnızca "john.smith" değerini sökmek için gerekli yse, şöyle bir şey kullanırdın:

`Replace([mail], "@contoso.com", , ,"", ,)`  

**Örnek giriş/çıktı:** <br>

* **GIRIŞ** (posta):john.smith@contoso.com" "
* **ÇIKIŞ**: "john.smith"

Özel ifadeler ve sözdizimi nasıl yazılabilenler hakkında daha fazla bilgi için Azure [Etkin Dizini'nde öznitelik eşlemeleri için ifadeler yazma'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data)bakın.

Aşağıdaki tabloda ortak öznitelikler ve Azure AD ile nasıl senkronize edildikleri listelenir.


|Şirket içi Active Directory|Haritalama türü|Azure AD|
|-----|-----|-----|
|Cn|Direct|commonName
|ülkeKodu|Direct|ülkeKodu|
|displayName|Direct|displayName|
|givenName|İfadeler|givenName|
|Objectguıd|Direct|kaynakAnchorBinary|  
|Userprincipalname|Direct|userPrincipalName|
|ProxyAdress|Direct|ProxyAdresi|

## <a name="view-the-schema"></a>Şeayı görüntüle
> [!WARNING]
> Bulut sağlama yapılandırması bir hizmet ilkesi oluşturur. Hizmet ilkesi Azure portalında görülebilir. Azure portalındaki hizmet temel deneyimini kullanarak öznitelik eşlemelerini değiştirmemelisiniz.  Bu özellik desteklenmez.

Şemayı görüntülemek ve doğrulamak için aşağıdaki adımları izleyin.

1.  Grafik [Explorer'a](https://developer.microsoft.com/graph/graph-explorer)gidin.
1.  Genel yönetici hesabınızla oturum açın.
1.  Solda, **izinleri değiştir'i** seçin ve **Directory.ReadWrite.All'ın** *Onay Olduğundan*Emin Olun.
1.  Sorguyu `https://graph.microsoft.com/beta/serviceprincipals/?$filter=startswith(Displayname,'Active')`çalıştırın. Bu sorgu, filtre uygulanmış hizmet ilkelerinin listesini döndürür.
1.  Değeri `"appDisplayName": "Active Directory to Azure Active Directory Provisioning"` bulun ve `"id"`not edin.
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
1. Değerinizle değiştirin `{Service Principal id}` ve sorguyu `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal id}/synchronization/jobs/`çalıştırın.
1. Değeri `"id": "AD2AADProvisioning.fd1c9b9e8077402c8bc03a7186c8f976"` bulun ve `"id"`not edin.
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
1. Şimdi sorguyu `https://graph.microsoft.com/beta/serviceprincipals/{Service Principal Id}/synchronization/jobs/{AD2AAD Provisioning id}/schema`çalıştırın.
 
    Örnek: https://graph.microsoft.com/beta/serviceprincipals/653c0018-51f4-4736-a3a3-94da5dcb6862/synchronization/jobs/AD2AADProvisioning.e9287a7367e444c88dc67a531c36d8ec/schema

   `{Service Principal Id}` Değiştirin `{AD2ADD Provisioning Id}` ve değerlerinizle.

1. Bu sorgu şema döndürür.

   ![İade şema](media/concept-attributes/schema1.png)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
