---
title: Özel API'lere çağrıları güvence altına almak için kimlik doğrulama ekleme
description: Azure Logic Apps'tan özel API'lere yapılan aramalariçin güvenliği artırmak için kimlik doğrulaması nasıl ayarlanır?
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 09/22/2017
ms.openlocfilehash: 110a684cf6ad21c13411d3bc2ada84750744f00e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191401"
---
# <a name="increase-security-for-calls-to-custom-apis-from-azure-logic-apps"></a>Azure Logic Apps'tan özel API'lere yapılan aramalar için güvenliği artırın

API'lerinize yapılan aramaların güvenliğini artırmak için, Kodunuzu güncelleştirmek zorunda kalmamak için Azure Etkin Dizin (Azure AD) kimlik doğrulaması'nı Azure portalı üzerinden ayarlayabilirsiniz. İsterseniz, API'nizin kodu aracılığıyla kimlik doğrulamasının gerekli ve zorunlu olmasını da sağlayabilirsiniz.

## <a name="authentication-options-for-your-api"></a>API'niz için kimlik doğrulama seçenekleri

Özel API'nize yapılan aramaların güvenliğini şu şekilde artırabilirsiniz:

* [Kod değişikliği yok](#no-code): ApI'nizi Azure portalı üzerinden [Azure Etkin Dizini (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) ile koruyun, böylece kodunuzu güncelleştirmeniz veya API'nizi yeniden dağıtmanız gerekmesin.

  > [!NOTE]
  > Varsayılan olarak, Azure portalında açtığınız Azure AD kimlik doğrulaması, gelişmiş yetkilendirme sağlamaz. Örneğin, bu kimlik doğrulama, API'nizi belirli bir kullanıcı veya uygulamaya değil, yalnızca belirli bir kiracıya kilitler. 

* [API'nizin kodunu güncelleştirin](#update-code): [Sertifika kimlik doğrulaması,](#certificate) [temel kimlik doğrulaması](#basic)veya kod aracılığıyla [Azure AD kimlik doğrulaması](#azure-ad-code) uygulayarak API'nizi koruyun.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Kodu değiştirmeden API'nize yapılan çağrıları doğrula

Bu yöntemiçin genel adımlar şunlardır:

1. İki Azure Etkin Dizin (Azure AD) uygulama kimliği oluşturun: biri mantık uygulamanız için, diğeri de web uygulamanız (veya API uygulaması) için.

2. API'nize yapılan çağrıları doğrulamak için, mantık uygulamanız için Azure REKLAM uygulama kimliğiyle ilişkili hizmet sorumlusunun kimlik bilgilerini (istemci kimliği ve gizli) kullanın.

3. Uygulama tanımlarını mantık uygulama tanımınıza ekleyin.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Bölüm 1: Mantık uygulamanız için bir Azure REKLAM uygulama kimliği oluşturma

Mantık uygulamanız, Azure AD'ye karşı kimlik doğrulamak için bu Azure AD uygulama kimliğini kullanır. Dizininiz için bu kimliği bir kez ayarlamanız gerekir. Örneğin, her mantık uygulaması için benzersiz kimlikler oluşturabiliyor olsanız bile, tüm mantık uygulamalarınız için aynı kimliği kullanmayı seçebilirsiniz. Bu kimlikleri Azure portalında ayarlayabilir veya [PowerShell'i](#powershell)kullanabilirsiniz.

**Azure portalında mantık uygulamanız için uygulama kimliği oluşturma**

1. Azure [portalında](https://portal.azure.com "https://portal.azure.com") **Azure Etkin Dizin'i**seçin. 

2. Web uygulamanızla veya API uygulamanızla aynı dizinde olduğunuzu doğrulayın.

   > [!TIP]
   > Dizinleri değiştirmek için profilinizi seçin ve başka bir dizin seçin. Veya **Genel Bakış** > **Anahtarı dizinini**seçin.

3. Dizin menüsünde, **Yönet**altında , **Uygulama kayıtları** > **Yeni uygulama kaydı**seçin.

   > [!TIP]
   > Varsayılan olarak, uygulama kayıtları listesi dizininizdeki tüm uygulama kayıtlarını gösterir. Yalnızca uygulama kayıtlarınızı görüntülemek için, arama kutusunun yanında **Uygulamalarım'ı**seçin. 

   ![Yeni uygulama kaydı oluşturma](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Uygulama kimliğinize bir ad verin, **Uygulama türü** kümesini **Web uygulamasına / API'ye**bırakın, **Oturum Açma URL'si**için bir etki alanı olarak biçimlendirilmiş benzersiz bir dize sağlayın ve **Oluştur'u**seçin.

   ![Uygulama kimliği için ad ve oturum açma URL'si sağlama](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   Mantık uygulamanız için oluşturduğunuz uygulama kimliği artık uygulama kayıtları listesinde görünür.

   ![Mantık uygulamanız için uygulama kimliği](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Uygulama kayıtları listesinde yeni uygulama kimliğinizi seçin. Bölüm 3'teki mantık uygulamanız için "istemci kimliği" olarak kullanmak üzere **Uygulama Kimliğini** kopyalayın ve kaydedin.

   ![Mantık uygulaması için uygulama kimliğini kopyalama ve kaydetme](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Uygulama kimlik ayarlarınız görünmüyorsa **Ayarlar** veya **Tüm ayarlar'ı**seçin.

7. **API Access** **altında, Anahtarlar'ı**seçin. **Açıklama**altında, anahtarınız için bir ad sağlayın. **Sona Erme süresi**altında, anahtarınız için bir süre seçin.

   Oluşturduğunuz anahtar, mantık uygulamanız için uygulama kimliğinin "gizli" veya şifresi olarak hareket eder.

   ![Mantık uygulaması kimliği için anahtar oluşturma](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Araç çubuğunda **Kaydet'i**seçin. **Değer**altında, anahtarınız şimdi görünür. 
**Anahtarlar** sayfasından çıktığınızda anahtar gizli olduğundan, anahtarınızı daha sonra kullanmak üzere **kopyalayıp kaydettiğinizden emin olun.**

   Mantık uygulamanızı Bölüm 3'te yapılandırdığınızda, bu anahtarı "gizli" veya parola olarak belirtirsiniz.

   ![Anahtarı daha sonra kopyalama ve kaydetme](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**PowerShell'de mantık uygulamanız için uygulama kimliği oluşturun**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu görevi PowerShell ile Azure Kaynak Yöneticisi aracılığıyla gerçekleştirebilirsiniz. PowerShell'de şu komutları çalıştırın:

1. `Add-AzAccount`

1. `$SecurePassword = Read-Host -AsSecureString`

1. Parola girin ve Enter tuşuna basın.

1. `New-AzADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password $SecurePassword`

1. **Kiracı Kimliğini** (Azure AD kiracınız için GUID), **Uygulama Kimliğini**ve kullandığınız parolayı kopyaladığınızdan emin olun.

Daha fazla bilgi için, [kaynaklara erişmek için PowerShell ile nasıl bir hizmet sorumlusu oluşturabilirsiniz](../active-directory/develop/howto-authenticate-service-principal-powershell.md)öğrenin.

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Bölüm 2: Web uygulamanız veya API uygulamanız için bir Azure REKLAM uygulama kimliği oluşturma

Web uygulamanız veya API uygulamanız zaten dağıtıldıysa, kimlik doğrulamasını açabilir ve Azure portalında uygulama kimliğini oluşturabilirsiniz. Aksi takdirde, [azure kaynak yöneticisi şablonuyla dağıtırken kimlik doğrulamasını açabilirsiniz.](#authen-deploy) 

**Dağıtılan uygulamalar için Uygulama kimliğini oluşturun ve Azure portalında kimlik doğrulamasını açın**

1. Azure [portalında](https://portal.azure.com "https://portal.azure.com")web uygulamanızı veya API uygulamanızı bulun ve seçin. 

2. **Ayarlar**altında **Kimlik Doğrulama/Yetkilendirme'yi**seçin. **App Service Kimlik Doğrulaması**altında kimlik doğrulamasını **açın.** **Kimlik Doğrulama Sağlayıcıları**altında Azure Etkin **Dizini'ni**seçin.

   ![Kimlik doğrulamayı açma](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Şimdi burada gösterildiği gibi web uygulaması veya API uygulaması için bir uygulama kimliği oluşturun. Azure **Etkin Dizin Ayarları** sayfasında, **Yönetim modunu** **Express**olarak ayarlayın. **Yeni AD Uygulaması Oluştur'u**seçin. Uygulama kimliğinize bir ad verin ve **Tamam'ı**seçin. 

   ![Web uygulamanız veya API uygulamanız için uygulama kimliği oluşturma](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. **Kimlik Doğrulama / Yetkilendirme** sayfasında **Kaydet**’i seçin.

Artık web uygulamanızla veya API uygulamanızla ilişkili uygulama kimliği için istemci kimliğini ve kiracı kimliğini bulmanız gerekir. Bölüm 3'te bu t.c. Bu nedenle Azure portalı için bu adımları ile devam edin.

**Azure portalında web uygulamanız veya API uygulamanız için uygulama kimliğinin istemci kimliğini ve kiracı kimliğini bulun**

1. **Kimlik Doğrulama Sağlayıcıları**altında Azure Etkin **Dizini'ni**seçin. 

   !["Azure Active Directory" seçeneğini belirleyin](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Azure **Etkin Dizin Ayarları** sayfasında, **Yönetim modunu** **Gelişmiş**olarak ayarlayın.

3. **İstemci kimliğini**kopyalayın ve bu GUID'i Bölüm 3'te kullanmak üzere kaydedin.

   > [!TIP] 
   > **İstemci Kimliği** ve **Veren Url** görünmüyorsa, Azure portalını yenilemeyi deneyin ve Adım 1'i yineleyin.

4. **Veren Url**altında, kopyalayın ve Bölüm 3 için sadece GUID kaydedin. Gerekirse bu GUID'i web uygulamanızda veya API uygulamanızın dağıtım şablonunda da kullanabilirsiniz.

   Bu GUID, özel kiracınızın GUID'idir ("kiracı Kimliği") ve bu URL'de görünmelidir:`https://sts.windows.net/{GUID}`

5. Değişikliklerinizi kaydetmeden **Azure Etkin Dizin Ayarları** sayfasını kapatın.

<a name="authen-deploy"></a>

**Azure Kaynak Yöneticisi şablonuyla dağıtırken kimlik doğrulamayı açma**

Yine de web uygulamanız veya API uygulamanız için mantık uygulamanızın uygulama kimliğinden farklı bir Azure REKLAM uygulama kimliği oluşturmanız gerekir. Uygulama kimliğini oluşturmak için Azure portalı için Bölüm 2'deki önceki adımları izleyin. 

Bölüm 1'deki adımları da takip edebilirsiniz, ancak web uygulamanızı veya `https://{URL}` API uygulamanızı **oturum açma URL'si** ve App ID **URI**için gerçek olarak kullandığınızdan emin olun. Bu adımlardan, uygulamanızın dağıtım şablonunda ve bölüm 3'te kullanılmak üzere hem istemci kimliğini hem de kiracı kimliğini kaydetmeniz gerekir.

> [!NOTE]
> Web uygulamanız veya API uygulamanız için Azure REKLAM uygulama kimliğini oluşturduğunuzda, PowerShell'i değil Azure portalını kullanmanız gerekir. PowerShell komut leti, kullanıcıları bir web sitesinde imzalamak için gerekli izinleri ayarlamaz.

İstemci kimliğini ve kiracı kimliğini aldıktan sonra, bu kimlikleri dağıtım şablonunuza web uygulamanızın veya API uygulamanızın alt kaynağı olarak ekleyin:

``` json
"resources": [ 
   {
      "apiVersion": "2015-08-01",
      "name": "web",
      "type": "config",
      "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
      "properties": {
         "siteAuthEnabled": true,
         "siteAuthSettings": {
            "clientId": "<client-ID>",
            "issuer": "https://sts.windows.net/<tenant-ID>/"
         }
      }
   } 
]
```

Boş bir web uygulamasını ve bir mantık uygulamasını Azure Active Directory kimlik doğrulaması ile otomatik olarak dağıtmak için [şablonun tamamını buradan görüntüleyin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json)veya burada **Azure'a Dağıt'ı** tıklatın:

[![Azure'a Dağıt](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Bölüm 3: Mantık uygulamanızda Yetkilendirme bölümünü doldurma

Önceki şablonda zaten bu yetkilendirme bölümü zaten ayarlanmıştır, ancak doğrudan mantık uygulamasını yazarsanız, tam yetkilendirme bölümünü eklemeniz gerekir.

Mantık uygulama tanımınızı kod görünümünde açın, **HTTP** eylem tanımına gidin, **Yetkilendirme** bölümünü bulun ve aşağıdaki özellikleri ekleyin:

```json
{
   "tenant": "<tenant-ID>",
   "audience": "<client-ID-from-Part-2-web-app-or-API app>", 
   "clientId": "<client-ID-from-Part-1-logic-app>",
   "secret": "<key-from-Part-1-logic-app>", 
   "type": "ActiveDirectoryOAuth"
}
```

| Özellik | Gerekli | Açıklama | 
| -------- | -------- | ----------- | 
| Kiracı | Evet | Azure AD kiracıiçin GUID | 
| Seyirci | Evet | Web uygulamanızın veya API uygulamanızın uygulama kimliğinden istemci kimliği olan erişmek istediğiniz hedef kaynağın GUID'i | 
| clientId | Evet | Mantık uygulamanızın uygulama kimliğinden istemci kimliği olan erişim isteyen istemci için GUID | 
| gizli dizi | Evet | Erişim jetonunu isteyen istemcinin uygulama kimliğinden anahtar veya parola | 
| type | Evet | Kimlik doğrulama türü. ActiveDirectoryOAuth kimlik doğrulaması için `ActiveDirectoryOAuth`değer. | 
|||| 

Örnek:

``` json
{
   "actions": {
      "HTTP": {
         "inputs": {
            "method": "POST",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         }
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Kod üzerinden güvenli API aramaları

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Sertifika kimlik doğrulaması

Mantık uygulamanızdan gelen istekleri web uygulamanıza veya API uygulamanıza doğrulamak için istemci sertifikalarını kullanabilirsiniz. Kodunuzu ayarlamak için [TLS'nin karşılıklı kimlik doğrulamasını nasıl yapılandırılacaya](../app-service/app-service-web-configure-tls-mutual-auth.md)öğrenin.

**Yetkilendirme** bölümünde, şu özellikleri içerir:

```json
{
   "type": "ClientCertificate",
   "password": "<password>",
   "pfx": "<long-pfx-key>"
} 
```

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| `type` | Evet | Kimlik doğrulama türü. SSL istemci sertifikaları için değer `ClientCertificate`. |
| `password` | Hayır | İstemci sertifikasına (PFX dosyası) erişmek için parola |
| `pfx` | Evet | İstemci sertifikasının temel 64 kodlanmış içeriği (PFX dosyası) |
||||

<a name="basic"></a>

#### <a name="basic-authentication"></a>Temel kimlik doğrulama

Mantık uygulamanızdan gelen istekleri web uygulamanıza veya API uygulamanıza doğrulamak için, kullanıcı adı ve parola gibi temel kimlik doğrulamayı kullanabilirsiniz. Temel kimlik doğrulama yaygın bir desendir ve bu kimlik doğrulamayı web uygulamanızı veya API uygulamanızı oluşturmak için kullanılan herhangi bir dilde kullanabilirsiniz.

**Yetkilendirme** bölümünde, şu özellikleri içerir:

```json
{
   "type": "Basic",
   "username": "<username>",
   "password": "<password>"
}
```

| Özellik | Gerekli | Açıklama | 
| -------- | -------- | ----------- | 
| type | Evet | Kullanmak istediğiniz kimlik doğrulama türü. Temel kimlik doğrulaması için `Basic`değer . | 
| kullanıcı adı | Evet | Kimlik doğrulama için kullanmak istediğiniz kullanıcı adı | 
| password | Evet | Kimlik doğrulama için kullanmak istediğiniz parola | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Kod aracılığıyla Azure Etkin Dizin kimlik doğrulaması

Varsayılan olarak, Azure portalında açtığınız Azure AD kimlik doğrulaması, gelişmiş yetkilendirme sağlamaz. Örneğin, bu kimlik doğrulama, API'nizi belirli bir kullanıcı veya uygulamaya değil, yalnızca belirli bir kiracıya kilitler. 

ApI erişimini kod aracılığıyla mantık uygulamanıza kısıtlamak için, JSON web belirteci (JWT) olan üstbilginin ayıkını ayıklayın. Arayanın kimliğini denetleyin ve eşleşmeyan istekleri reddedin.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/overview-authentication-authorization.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Sonraki adımlar

* [Mantık uygulaması iş akışlarından özel API'leri dağıtma ve arama](../logic-apps/logic-apps-custom-api-host-deploy-call.md)
