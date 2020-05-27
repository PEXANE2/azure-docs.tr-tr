---
title: SAML tabanlı çoklu oturum açmayı yapılandırmak için Microsoft Graph API 'Leri kullanma
titleSuffix: Azure Active Directory
description: Bir uygulamanın birden çok örneği için SAML tabanlı çoklu oturum açma ayarlanması mı gerekiyor? SAML tabanlı çoklu oturum açma yapılandırmasını otomatik hale getirmek için Microsoft Graph API 'Lerini kullanarak zamandan tasarruf yapmayı öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
ms.reviewer: luleon
ms.openlocfilehash: fd59dcdd566110d1df02333f5701c0c206442d5d
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83846469"
---
# <a name="automate-saml-based-sso-app-configuration-with-microsoft-graph-api"></a>Microsoft Graph API ile SAML tabanlı SSO uygulama yapılandırmasını otomatikleştirme

Bu makalede, Azure Active Directory (Azure AD) galerisinden bir uygulama oluşturmayı ve yapılandırmayı öğreneceksiniz. Bu makale bir örnek olarak AWS kullanır, ancak bu makaledeki adımları Azure AD galerisinde SAML tabanlı herhangi bir uygulama için kullanabilirsiniz.

**SAML tabanlı çoklu oturum açma yapılandırmasını otomatik hale getirmek için Microsoft Graph API 'Lerini kullanma adımları**

| Adım  | Ayrıntılar  |
|---------|---------|
| [1. Galeri uygulamasını oluşturma](#step-1-create-the-gallery-application) | API istemcisinde oturum açın <br> Galeri uygulamasını alma <br> Galeri uygulaması oluşturma|
| [2. çoklu oturum açmayı yapılandırma](#step-2-configure-single-sign-on) | Uygulama nesne KIMLIĞI ve hizmet sorumlusu nesne KIMLIĞINI alma <br> Çoklu oturum açma modunu ayarla <br> Tanımlayıcı, yanıt URL 'SI, oturum açma URL 'SI gibi temel SAML URL 'Lerini ayarlayın <br> Uygulama rolleri ekleme (Isteğe bağlı)|
| [3. talep eşlemesini yapılandırma](#step-3-configure-claims-mapping) | Talep eşleme ilkesi oluştur <br> Hizmet sorumlusuna talep eşleme ilkesi atama|
| [4. imza sertifikası yapılandırma](#step-4-configure-signing-certificate) | Sertifika oluşturma <BR> Özel imza anahtarı ekleme <br> Özel imza anahtarını etkinleştir|
| [5. Kullanıcı atama](#step-5-assign-users) | Uygulamaya kullanıcı ve grup atama
| [6. uygulama tarafını yapılandırma](#step-6-configure-the-application-side)| Azure AD SAML meta verilerini al

**Belgelerde kullanılan tüm API 'lerin listesi**

Aşağıdaki API 'Leri çağırmak için ilgili izinlere sahip olduğunuzdan emin olun.

|Kaynak türü |Yöntem |
|---------|---------|
|[applicationTemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta)|[ApplicationTemplate listesini Listele](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) <br>[ApplicationTemplate örneğini oluştur](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http)|
|[servicePrincipals](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta)|[Hizmet sorumlusunu Güncelleştir](https://docs.microsoft.com/graph/api/serviceprincipal-update?view=graph-rest-beta&tabs=http) <br> [Approlet atamaları oluşturma](https://docs.microsoft.com/graph/api/serviceprincipal-post-approleassignments?view=graph-rest-beta&tabs=http) <br> [ClaimsMappingPolicies atama](https://docs.microsoft.com/graph/api/serviceprincipal-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)|
|[uygulamaları](https://docs.microsoft.com/graph/api/resources/application?view=graph-rest-1.0)|[Uygulamayı güncelleştirme](https://docs.microsoft.com/graph/api/application-update?view=graph-rest-beta&tabs=http)|
|[claimsMappingPolicy](https://docs.microsoft.com/graph/api/resources/claimsmappingpolicy?view=graph-rest-beta)| [ClaimsMappingPolicy oluşturma](https://docs.microsoft.com/graph/api/claimsmappingpolicy-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)

>[!NOTE]
>Bu makalede gösterilen yanıt nesneleri okunabilirlik için kısaltılamayabilir. Tüm özellikler gerçek bir çağrıdan döndürülecek.

## <a name="step-1-create-the-gallery-application"></a>1. Adım: Galeri uygulamasını oluşturma

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Microsoft Graph Explorer (önerilen), Postman veya kullandığınız başka bir API istemcisinde oturum açın

1. [Microsoft Graph Gezginini](https://developer.microsoft.com/graph/graph-explorer) Başlat
2. **Microsoft Ile oturum aç** ' ı seçin ve Azure AD Genel Yöneticisi veya uygulama Yöneticisi kimlik bilgilerini kullanarak oturum açın.
3. Oturum açma başarılı olduğunda, Kullanıcı hesabı ayrıntılarını sol bölmede görürsünüz.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Galeri uygulaması şablon tanımlayıcısını alma

Azure AD uygulama galerisindeki uygulamaların her biri, uygulamanın meta verilerini açıklayan bir [uygulama şablonuna](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) sahiptir. Bu şablonu kullanarak, yönetim için kiracınızda uygulama ve hizmet sorumlusu örneği oluşturabilirsiniz.

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "get_applicationtemplates"
}-->

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates
```

#### <a name="response"></a>Yanıt

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

### <a name="create-the-gallery-application"></a>Galeri uygulaması oluşturma

Son adımda uygulamanız için aldığınız şablon KIMLIĞINI kullanarak kiracınızda uygulamanın ve hizmet sorumlusunun [bir örneğini oluşturun](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) .

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "applicationtemplate_instantiate"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/8b1025e4-1dd2-430b-a150-2ef79cd700f5/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>Yanıt


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

## <a name="step-2-configure-single-sign-on"></a>2. Adım: çoklu oturum açmayı yapılandırma

### <a name="retrieve-app-object-id-and-service-principal-object-id"></a>Uygulama nesne KIMLIĞI ve hizmet sorumlusu nesne KIMLIĞINI alma

Uygulama nesne KIMLIĞI ve hizmet sorumlusu nesne KIMLIĞINI almak ve kaydetmek için önceki çağrıdan gelen yanıtı kullanın.

```
"application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63"
}
"servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e"
}
```
### <a name="set-single-sign-on-mode"></a>Çoklu oturum açma modunu ayarla

Bu örnekte, `saml` [ServicePrincipal kaynak türünde](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta)çoklu oturum açma modu olarak ayarlayacaksınız. Yapılandırabileceğiniz diğer SAML SSO özellikleri şunlardır: `notificationEmailAddresses` , `loginUrl` ve`samlSingleSignOnSettings.relayState`

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: servicePrincipal/json

{
    "preferredSingleSignOnMode": "saml",
    "notificationEmailAddresses": [
        "john@contoso.com"
      ]
}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```

### <a name="set-basic-saml-urls-such-as-identifier-reply-url-sign-on-url"></a>Tanımlayıcı, yanıt URL 'SI, oturum açma URL 'SI gibi temel SAML URL 'Lerini ayarlayın

Uygulama nesnesinde AWS için tanımlayıcı ve yanıt URL 'Lerini ayarlayın.

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/beta/applications/cbc071a6-0fa5-4859-8g55-e983ef63df63
Content-type: applications/json

{
    "web": {
        "redirectUris": [
            "https://signin.aws.amazon.com/saml"
        ] 
    },
    "identifierUris": [
        "https://signin.aws.amazon.com/saml"
    ]    
}
```
#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```
### <a name="add-app-roles-optional"></a>Uygulama rolleri ekleme (Isteğe bağlı)

Uygulama, belirteçte rol bilgilerini gerektiriyorsa, rol tanımını uygulama nesnesine ekleyin. AWS için, bu AWS hesabındaki tüm rolleri getirmek üzere [Kullanıcı sağlamayı etkinleştirebilirsiniz](../app-provisioning/application-provisioning-configure-api.md) . 

Daha fazla bilgi için [SAML belirtecinde verilen rol talebini yapılandırma](../develop/active-directory-enterprise-app-role-management.md) konusunu okuyun

> [!NOTE] 
> Uygulama rolleri eklerken, msiam_access varsayılan uygulama rollerini değiştirmeyin. 

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/beta/serviceprincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: serviceprincipals/json

{
"appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "454dc4c2-8176-498e-99df-8c4efcde41ef",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-admin,arn:aws:iam::212743507312:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Finance,WAAD",
            "displayName": "Finance,WAAD",
            "id": "8642d5fa-18a3-4245-ab8c-a96000c1a217",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-finance,arn:aws:iam::212743507312:saml-provider/WAAD"
        }
    ]

}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-3-configure-claims-mapping"></a>3. Adım: talep eşlemesini yapılandırma

### <a name="create-claims-mapping-policy"></a>Talep eşleme ilkesi oluştur

Temel taleplere ek olarak, Azure AD 'nin SAML belirtecine yaymayı sağlamak için aşağıdaki talepleri yapılandırın:

| Talep adı | Kaynak  |
|---------|---------|
| `https://aws.amazon.com/SAML/Attributes/Role` | atanan| 
| `https://aws.amazon.com/SAML/Attributes/RoleSessionName` | userPrincipalName |
| `https://aws.amazon.com/SAML/Attributes/SessionDuration` | "900" |
| roles | atanan |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` | userPrincipalName |

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/policies/claimsMappingPolicies
Content-type: claimsMappingPolicies/json

{
    "definition":[
            "{\"ClaimsMappingPolicy\": {
                \"Version\": 1,
                \"IncludeBasicClaimSet\": \"true\",
                \"ClaimsSchema\": [
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/Role\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"900\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"appRoles\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"
                    }
                ]
            }
        }"

    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.claimsMappingPolicies",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: claimsMappingPolicies/json

{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#policies/claimsMappingPolicies/$entity",
    "id": "6b33aa8e-51f3-41a6-a0fd-d660d276197a",
    "definition": [
        "{\"ClaimsMappingPolicy\": {\"Version\": 1,\"IncludeBasicClaimSet\": \"true\",\"ClaimsSchema\": [{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"https://aws.amazon.com/SAML/Attributes/Role\"\r\n                    },{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"},{\"Source\": \"user\",\"ID\": \"900\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"},{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"appRoles\"},{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"}]}}"
    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

### <a name="assign-claims-mapping-policy-to-service-principal"></a>Hizmet sorumlusuna talep eşleme ilkesi atama

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/claimsMappingPolicies/$ref

Content-type: claimsMappingPolicies/json

{
  "@odata.id":"https://graph.microsoft.com/beta/policies/claimsMappingPolicies/6b33aa8e-51f3-41a6-a0fd-d660d276197a"
}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-4-configure-signing-certificate"></a>4. Adım: imza sertifikası yapılandırma

[Applicationtemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta) API 'si kullanmak varsayılan olarak bir imza sertifikası oluşturmaz. Özel imza sertifikası oluşturun ve uygulamaya atayın. 

### <a name="create-a-custom-signing-certificate"></a>Özel imza sertifikası oluşturma

Test etmek için, otomatik olarak imzalanan bir sertifika almak için aşağıdaki PowerShell komutunu kullanabilirsiniz. Üretim için bir imza sertifikası oluşturmak üzere şirketinizdeki en iyi güvenlik uygulamasını kullanın.

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [string]$fqdn,
    [Parameter(Mandatory=$true)]
    [string]$pwd,
    [Parameter(Mandatory=$true)]
    [string]$location
) 

if (!$PSBoundParameters.ContainsKey('location'))
{
    $location = "."
} 

$cert = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -DnsName $fqdn
$pwdSecure = ConvertTo-SecureString -String $pwd -Force -AsPlainText
$path = 'cert:\currentuser\my\' + $cert.Thumbprint
$cerFile = $location + "\\" + $fqdn + ".cer"
$pfxFile = $location + "\\" + $fqdn + ".pfx" 

Export-PfxCertificate -cert $path -FilePath $pfxFile -Password $pwdSecure
Export-Certificate -cert $path -FilePath $cerFile
```

### <a name="add-a-custom-signing-key"></a>Özel imza anahtarı ekleme

Hizmet sorumlusuna aşağıdaki bilgileri ekleyin:

* Özel anahtar
* Parola
* Ortak anahtar 

PFX dosyasından kodlanmış özel ve ortak anahtarı Base64 olarak ayıklayın. Özellikler hakkında daha fazla bilgi edinmek için [Keycredential kaynak türünü](https://docs.microsoft.com/graph/api/resources/keycredential?view=graph-rest-1.0)okuyun.

"Imzala" için kullanılan keyCredential öğesinin keyId değerinden passwordCredential öğesinin keyId 'siyle eşleştiğinden emin olun.

`customkeyIdentifier`Sertifikayı, sertifikanın parmak izi karmasını alarak oluşturabilirsiniz.

```csharp
  public string GetSha256FromThumbprint(string thumbprint)
  {
      var message = Encoding.ASCII.GetBytes(thumbprint);
      SHA256Managed hashString = new SHA256Managed();
      return Convert.ToBase64String(hashString.ComputeHash(message));
  }
```

#### <a name="request"></a>İstek

> [!NOTE]
> KeyCredentials özelliğindeki "Key" değeri okunabilirlik için kısaltıldı.

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "keyCredentials":[
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Sign",
            "key":"MIIKIAIBAz.....HBgUrDgMCERE20nuTptI9MEFCh2Ih2jaaLZBZGeZBRFVNXeZmAAgIH0A==",
            "displayName": "CN=awsAPI"
        },
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "e35a7d11-fef0-49ad-9f3e-aacbe0a42c42",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "key": "MIIDJzCCAg+gAw......CTxQvJ/zN3bafeesMSueR83hlCSyg==",
            "displayName": "CN=awsAPI"
        }

    ],
    "passwordCredentials": [
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "endDateTime": "2022-01-27T19:40:33Z",
            "startDateTime": "2020-04-20T19:40:33Z",
            "secretText": "61891f4ee44d"
        }
    ]
}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

### <a name="activate-the-custom-signing-key"></a>Özel imza anahtarını etkinleştir

`preferredTokenSigningKeyThumbprint`Özelliği, Azure AD 'nın SAML Yanıtını imzalamak için kullanmasını istediğiniz sertifikanın parmak izine ayarlamanız gerekir. 

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "preferredTokenSigningKeyThumbprint": "AC09FEF18DDE6983EE2A164FBA3C4DD7518BD787"
}
```

#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```
## <a name="step-5-assign-users"></a>5. Adım: kullanıcıları atama

### <a name="assign-users-and-groups-to-the-application"></a>Uygulamaya kullanıcı ve grup atama

Hizmet sorumlusuna aşağıdaki kullanıcıyı atayın ve AWS_Role1 atayın. 

| Name  | ID  |
|---------|---------|
| Kullanıcı KIMLIĞI (PrincipalId) | 6cad4079-4e79-4A3F-9efb-ea30a14bdb26 |
| Tür (principalType) | Kullanıcı |
| Uygulama rolü KIMLIĞI (Approliıd) | 454dc4c2-8176-498e-99df-8c4efcde41ef |
| Serviceprincipalıd (RESOURCEID) | 515f62cb-d18a-4dca-BEC3-bb0bf31deeea |

#### <a name="request"></a>İstek

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/appRoleAssignments

Content-type: appRoleAssignments/json

{
  "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
  "principalType": "User",
  "appRoleId":"454dc4c2-8176-498e-99df-8c4efcde41ef",
  "resourceId":"515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```
#### <a name="response"></a>Yanıt

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 201 
Content-type: appRoleAssignments/json

{
    "id": "rq7hyzl4yECaNZleMrTpDV-OCe5TEl5Ao_o76XMrRFU",
    "creationTimestamp": "2020-04-23T17:38:13.2508567Z",
    "appRoleId": "454dc4c2-8176-498e-99df-8c4efcde41ef",
    "principalDisplayName": "User 1",
    "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
    "principalType": "User",
    "resourceDisplayName": "AWS API Created",
    "resourceId": "515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```

Daha fazla bilgi için bkz. [Approtaatama](https://docs.microsoft.com/graph/api/resources/approleassignment?view=graph-rest-beta) kaynak türü.

## <a name="step-6-configure-the-application-side"></a>6. Adım: uygulama tarafını yapılandırma

### <a name="get-azure-ad-saml-metadata"></a>Azure AD SAML meta verilerini al

Yapılandırılmış belirli bir uygulama için Azure AD SAML meta verilerini almak üzere aşağıdaki URL 'YI kullanın. Meta veriler, diğer kullanıcıların yanı sıra imza sertifikası, Azure AD EntityId ve Azure AD SingleSignOnService gibi bilgileri içerir.

https://login.microsoftonline.com/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={app-id}

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanıcı sağlamayı yapılandırmak için Microsoft Graph API 'Leri kullanma](../app-provisioning/application-provisioning-configure-api.md)
- [Uygulamaları Azure AD 'ye geçirmek için AD FS uygulama etkinliği raporunu kullanın](migrate-adfs-application-activity.md)
