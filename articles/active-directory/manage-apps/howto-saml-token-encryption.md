---
title: Azure Etkin Dizini'nde SAML belirteç şifrelemesi
description: Azure Active Directory SAML belirteç şifrelemesini nasıl yapılandıracağız öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: mimart
ms.reviewer: paulgarn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0082d841faf22745e609d38444f4a97553b3c867
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365875"
---
# <a name="how-to-configure-azure-ad-saml-token-encryption"></a>Nasıl yapIlir: Azure AD SAML belirteç şifrelemesini yapılandırma

> [!NOTE]
> Belirteç şifrelemesi, Azure Etkin Dizin (Azure AD) premium özelliğidir. Azure REKLAM sürümleri, özellikleri ve fiyatlandırması hakkında daha fazla bilgi edinmek için [Azure REKLAM fiyatlandırması'na](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

SAML belirteç şifrelemesi, şifrelenmiş SAML iddialarının kullanılmasını destekleyen bir uygulamayla sağlar. Azure AD, bir uygulama için yapılandırıldığında, Azure AD'de depolanan bir sertifikadan elde edilen ortak anahtarı kullanarak bu uygulama için yayılan SAML iddialarını şifreler. Uygulama, oturum açmış kullanıcı için kimlik doğrulama kanıtı olarak kullanılabilmeden önce belirteci şifresini çözmek için eşleşen özel anahtarı kullanmalıdır.

Azure AD ile uygulama arasındaki SAML iddialarının şifrelenmesi, belirteç içeriğinin ele geçirilemeyecine ve kişisel veya kurumsal verilerin tehlikeye atılamayacağına dair ek güvence sağlar.

Belirteç şifrelemesi olmasa bile, Azure AD SAML belirteçleri ağda hiçbir zaman net olarak geçirilir. Azure AD, IDP, tarayıcı ve uygulama arasındaki iletişimin şifreli bağlantılar üzerinden gerçekleşmesi için jeton isteği/yanıt alışverişinin şifreli HTTPS/TLS kanalları üzerinden gerçekleşmesini gerektirir. Ek sertifikaları yönetmenin genel yüküyle karşılaştırıldığında durumunuz için belirteç şifrelemesinin değerini göz önünde bulundurun.   

Belirteç şifrelemesini yapılandırmak için, uygulamayı temsil eden Azure AD uygulama nesnesinin ortak anahtarını içeren bir X.509 sertifika dosyası yüklemeniz gerekir. X.509 sertifikasını almak için, uygulamanın kendisinden indirebilir veya uygulama satıcısının şifreleme anahtarları sağladığı durumlarda veya uygulamanın özel bir anahtar sağlamanızı beklediği durumlarda uygulama satıcısından alabilirsiniz, şifreleme araçları kullanılarak oluşturulan, uygulamanın anahtar deposuna yüklenen özel anahtar kısmı ve Azure AD'ye yüklenen eşleşen ortak anahtar sertifikası.

Azure AD, SAML iddia verilerini şifrelemek için AES-256 kullanır.

## <a name="configure-saml-token-encryption"></a>SAML belirteç şifrelemesini yapılandırma

SAML belirteç şifrelemesini yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulamada yapılandırılan özel bir anahtarla eşleşen ortak anahtar sertifikası alın.

    Şifreleme için kullanılacak asimetrik bir anahtar çifti oluşturun. Veya, uygulama şifreleme için kullanılacak ortak bir anahtar sağlıyorsa, X.509 sertifikasını indirmek için uygulamanın yönergelerini izleyin.

    Ortak anahtar X.509 sertifika dosyasında .cer biçiminde depolanmalıdır.

    Uygulama, örneğiniz için oluşturduğunuz bir anahtar kullanıyorsa, uygulamanın Azure AD kiracınızdaki belirteçleri çözmek için kullanacağı özel anahtarı yüklemek için uygulamanız tarafından sağlanan yönergeleri izleyin.

1. Azure AD'deki uygulama yapılandırmasına sertifikaekleyin.

### <a name="to-configure-token-encryption-in-the-azure-portal"></a>Azure portalında belirteç şifrelemesini yapılandırmak için

Azure portalında uygulama yapılandırmanıza ortak sertifikayı ekleyebilirsiniz.

1. [Azure portalına](https://portal.azure.com)gidin.

1. **Azure Active Directory > Enterprise uygulamalarına** gidin ve ardından belirteç şifrelemesini yapılandırmak istediğiniz uygulamayı seçin.

1. Uygulamanın sayfasında **Belirteç şifrelemesini**seçin.

    ![Azure portalında belirteç şifreleme seçeneği](./media/howto-saml-token-encryption/token-encryption-option-small.png)

    > [!NOTE]
    > **Belirteç şifreleme** seçeneği yalnızca Azure portalındaki **Kurumsal uygulamalar** bıçak'ından ayarlanan SAML uygulamaları için, Uygulama Galerisi'nden veya Galeri Dışı uygulamadan kullanılabilir. Diğer uygulamalar için bu menü seçeneği devre dışı bırakılır. Azure portalındaki **Uygulama kayıtları** deneyimi üzerinden kaydedilmiş uygulamalar için, uygulama bildirimini kullanarak, Microsoft Graph aracılığıyla veya PowerShell aracılığıyla SAML belirteçleri için şifreleme yi yapılandırabilirsiniz.

1. **Belirteç şifreleme** sayfasında, ortak X.509 sertifikanızı içeren .cer dosyasını almak için **Sertifikayı İçe Aktar'ı** seçin.

    ![X.509 sertifikasını içeren .cer dosyasını alma](./media/howto-saml-token-encryption/import-certificate-small.png)

1. Sertifika alındıktan ve özel anahtar uygulama tarafında kullanılmak üzere yapılandırıldıktan sonra, parmak izi durumunun yanındaki **...** seçeneğini seçerek şifrelemeyi etkinleştirin ve ardından açılır menüdeki seçeneklerden **belirteç şifrelemesini etkinleştir'i** seçin.

1. Belirteç şifreleme sertifikasının etkinleştirmesini onaylamak için **Evet'i** seçin.

1. Uygulama için yayılan SAML iddialarının şifrelenmiş olduğunu doğrulayın.

### <a name="to-deactivate-token-encryption-in-the-azure-portal"></a>Azure portalında belirteç şifrelemesini devre dışı bırakmak için

1. Azure portalında Azure **Active Directory > Enterprise uygulamalarına**gidin ve ardından SAML belirteç şifrelemesi etkinleştirilmiş uygulamayı seçin.

1. Uygulamanın sayfasında, **Token şifrelemesini**seçin, sertifikayı bulun ve ardından açılan menüyü göstermek için **...** seçeneğini seçin.

1. **Belirteç şifrelemesini Devre Dışı Bırak'ı**seçin.

## <a name="configure-saml-token-encryption-using-graph-api-powershell-or-app-manifest"></a>Grafik API, PowerShell veya uygulama bildirimini kullanarak SAML belirteç şifrelemesini yapılandırma

Şifreleme sertifikaları, `encrypt` kullanım etiketiyle Azure AD'deki uygulama nesnesinde depolanır. Birden çok şifreleme sertifikası yapılandırabilirsiniz ve belirteçleri şifrelemek için etkin `tokenEncryptionKeyID` olan özellik tarafından tanımlanır.

Microsoft Graph API veya PowerShell kullanarak belirteç şifrelemesini yapılandırmak için uygulamanın nesne kimliğine ihtiyacınız vardır. Bu değeri programlı olarak veya Azure portalında uygulamanın **Özellikler** sayfasına giderek **nesne kimliği** değerini belirterek bulabilirsiniz.

Grafik, PowerShell veya uygulama bildirimini kullanarak bir tuş Keşetial yapılandırdığınızda, keyId için kullanmak üzere bir GUID oluşturmanız gerekir.

### <a name="to-configure-token-encryption-using-microsoft-graph"></a>Microsoft Graph kullanarak belirteç şifrelemesini yapılandırmak için

1. Şifreleme için X.509 sertifikası ile uygulamanın güncelleştirmesi. `keyCredentials` Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid>

    { 
       "keyCredentials":[ 
          { 
             "type":"AsymmetricX509Cert","usage":"Encrypt",
             "keyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35",    (Use a GUID generator to obtain a value for the keyId)
             "key": "MIICADCCAW2gAwIBAgIQ5j9/b+n2Q4pDvQUCcy3…"  (Base64Encoded .cer file)
          }
        ]
    }
    ```

1. Belirteçleri şifrelemek için etkin olan şifreleme sertifikasını tanımlayın. Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir.

    ```
    Patch https://graph.microsoft.com/beta/applications/<application objectid> 

    { 
       "tokenEncryptionKeyId":"fdf8c5d8-f727-43fd-beaf-0f1521cf3d35" (The keyId of the keyCredentials entry to use)
    }
    ```

### <a name="to-configure-token-encryption-using-powershell"></a>PowerShell kullanarak belirteç şifrelemesini yapılandırmak için

1. Kiracınıza bağlanmak için en son Azure AD PowerShell modülünü kullanın.

1. **[Ayar-AzureApplication](https://docs.microsoft.com/powershell/module/azuread/set-azureadapplication?view=azureadps-2.0-preview)** komutunu kullanarak belirteç şifreleme ayarlarını ayarlayın.

    ```
    Set-AzureADApplication -ObjectId <ApplicationObjectId> -KeyCredentials "<KeyCredentialsObject>"  -TokenEncryptionKeyId <keyID>
    ```

1. Aşağıdaki komutları kullanarak belirteç şifreleme ayarlarını okuyun.

    ```powershell
    $app=Get-AzureADApplication -ObjectId <ApplicationObjectId>
    $app.KeyCredentials
    $app.TokenEncryptionKeyId
    ```

### <a name="to-configure-token-encryption-using-the-application-manifest"></a>Uygulama bildirimini kullanarak belirteç şifrelemesini yapılandırmak için

1. Azure portalından Azure **Active Directory > Uygulama kayıtlarına**gidin.

1. Tüm uygulamaları göstermek için açılır dosyadaki **tüm uygulamaları** seçin ve ardından yapılandırmak istediğiniz kurumsal uygulamayı seçin.

1. Uygulamanın sayfasında, [uygulama bildirimini](../develop/reference-app-manifest.md)sağlamak için **Bildirim'i** seçin.

1. `tokenEncryptionKeyId` Öznitelik için değeri ayarlayın.

    Aşağıdaki örnekte, iki şifreleme sertifikasıyla yapılandırılan bir uygulama bildirimi ve tokenEnryptionKeyId'i kullanarak etkin olarak seçilen ikinci uygulama bildirimi gösterilmektedir.

    ```json
    { 
      "id": "3cca40e2-367e-45a5-8440-ed94edd6cc35",
      "accessTokenAcceptedVersion": null,
      "allowPublicClient": false,
      "appId": "cb2df8fb-63c4-4c35-bba5-3d659dd81bf1",
      "appRoles": [],
      "oauth2AllowUrlPathMatching": false,
      "createdDateTime": "2017-12-15T02:10:56Z",
      "groupMembershipClaims": "SecurityGroup",
      "informationalUrls": { 
         "termsOfService": null, 
         "support": null, 
         "privacy": null, 
         "marketing": null 
      },
      "identifierUris": [ 
        "https://testapp"
      ],
      "keyCredentials": [ 
        { 
          "customKeyIdentifier": "Tog/O1Hv1LtdsbPU5nPphbMduD=", 
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "8be4cb65-59d9-404a-a6f5-3d3fb4030351", 
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest" 
        }, 
        {
          "customKeyIdentifier": "U5nPphbMduDmr3c9Q3p0msqp6eEI=",
          "endDate": "2039-12-31T23:59:59Z", 
          "keyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851",
          "startDate": "2018-10-25T21:42:18Z", 
          "type": "AsymmetricX509Cert", 
          "usage": "Encrypt", 
          "value": <Base64EncodedKeyFile> 
          "displayName": "CN=SAMLEncryptTest2" 
        } 
      ], 
      "knownClientApplications": [], 
      "logoUrl": null, 
      "logoutUrl": null, 
      "name": "Test SAML Application", 
      "oauth2AllowIdTokenImplicitFlow": true, 
      "oauth2AllowImplicitFlow": false, 
      "oauth2Permissions": [], 
      "oauth2RequirePostResponse": false, 
      "orgRestrictions": [], 
      "parentalControlSettings": { 
         "countriesBlockedForMinors": [], 
         "legalAgeGroupRule": "Allow" 
        }, 
      "passwordCredentials": [], 
      "preAuthorizedApplications": [], 
      "publisherDomain": null, 
      "replyUrlsWithType": [], 
      "requiredResourceAccess": [], 
      "samlMetadataUrl": null, 
      "signInUrl": "https://127.0.0.1:444/applications/default.aspx?metadata=customappsso|ISV9.1|primary|z" 
      "signInAudience": "AzureADMyOrg",
      "tags": [], 
      "tokenEncryptionKeyId": "6b9c6e80-d251-43f3-9910-9f1f0be2e851" 
    }  
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Azure [AD'nin SAML protokolünü nasıl kullandığını](../develop/active-directory-saml-protocol-reference.md) öğrenin
* [Azure AD'de SAML belirteçlerinin](../develop/reference-saml-tokens.md) biçimini, güvenlik özelliklerini ve içeriğini öğrenin