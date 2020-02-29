---
title: HTTP temel kimlik doğrulaması kullanarak yeniden bir hizmetin güvenliğini sağlama
titleSuffix: Azure AD B2C
description: HTTP temel kimlik doğrulaması kullanarak Azure AD B2C özel REST API talep Değişimlerinizi güvenli hale getirin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 505d92e33606dac327778cae179df44efbfdf853
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78183865"
---
# <a name="secure-your-restful-services-by-using-http-basic-authentication"></a>HTTP temel kimlik doğrulaması kullanarak yeniden takip eden hizmetlerinizin güvenliğini sağlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

İlgili bir [Azure AD B2C makalesinde](rest-api-claims-exchange-dotnet.md), kimlik doğrulaması olmadan Azure Active Directory B2C (Azure AD B2C) Kullanıcı ile tümleşen bir yeniden hizmet (Web API) oluşturabilirsiniz.

Bu makalede, bakım dahil olmak üzere yalnızca doğrulanan kullanıcıların API 'nize erişebilmesi için, yeniden deneme hizmetinize HTTP temel kimlik doğrulaması eklersiniz. HTTP temel kimlik doğrulaması ile, özel ilkenizde Kullanıcı kimlik bilgilerini (uygulama KIMLIĞI ve uygulama gizli anahtarı) ayarlarsınız.

Daha fazla bilgi için bkz. [ASP.NET Web API 'de temel kimlik doğrulaması](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).

## <a name="prerequisites"></a>Önkoşullar

[Azure AD B2C Kullanıcı yolculuğu makalesindeki talep alışverişlerinde tümleştirme REST API](rest-api-claims-exchange-dotnet.md) adımları uygulayın.

## <a name="step-1-add-authentication-support"></a>1\. Adım: kimlik doğrulama desteği ekleme

### <a name="step-11-add-application-settings-to-your-projects-webconfig-file"></a>Adım 1,1: projenizin Web. config dosyasına uygulama ayarları ekleme

1. Daha önce oluşturduğunuz Visual Studio projesini açın.

2. Aşağıdaki uygulama ayarlarını `appSettings` öğesinin altındaki Web. config dosyasına ekleyin:

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. Bir parola oluşturun ve `WebApp:ClientSecret` değeri ayarlayın.

    Karmaşık bir parola oluşturmak için aşağıdaki PowerShell kodunu çalıştırın. Herhangi bir rastgele değeri kullanabilirsiniz.

    ```powershell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a>Adım 1,2: OWıN kitaplıklarını Install

Başlamak için, Visual Studio Paket Yöneticisi konsolunu kullanarak OWıN ara yazılım NuGet paketlerini projeye ekleyin:

```powershell
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-an-authentication-middleware-class"></a>Adım 1,3: kimlik doğrulama ara yazılım sınıfı ekleme

*App_Start* klasörünün altına `ClientAuthMiddleware.cs` sınıfını ekleyin. Bunu yapmak için:

1. *App_Start* klasörüne sağ tıklayın, **Ekle**' yi ve ardından **sınıf**' ı seçin.

   ![App_Start klasöre ClientAuthMiddleware.cs sınıfı ekleme](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

2. **Ad** kutusuna **ClientAuthMiddleware.cs**yazın.

   ![Visual Studio 'da C# yeni öğe Ekle iletişim kutusunda yeni bir sınıf oluşturma](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

3. *App_Start \clientauthmiddleware.cs* dosyasını açın ve dosya içeriğini şu kodla değiştirin:

    ```csharp

    using Microsoft.Owin;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using System.Security.Principal;
    using System.Text;
    using System.Threading.Tasks;
    using System.Web;

    namespace Contoso.AADB2C.API
    {
        /// <summary>
        /// Class to create a custom owin middleware to check for client authentication
        /// </summary>
        public class ClientAuthMiddleware
        {
            private static readonly string ClientID = ConfigurationManager.AppSettings["WebApp:ClientId"];
            private static readonly string ClientSecret = ConfigurationManager.AppSettings["WebApp:ClientSecret"];

            /// <summary>
            /// Gets or sets the next owin middleware
            /// </summary>
            private Func<IDictionary<string, object>, Task> Next { get; set; }

            /// <summary>
            /// Initializes a new instance of the <see cref="ClientAuthMiddleware"/> class.
            /// </summary>
            /// <param name="next"></param>
            public ClientAuthMiddleware(Func<IDictionary<string, object>, Task> next)
            {
                this.Next = next;
            }

            /// <summary>
            /// Invoke client authentication middleware during each request.
            /// </summary>
            /// <param name="environment">Owin environment</param>
            /// <returns></returns>
            public Task Invoke(IDictionary<string, object> environment)
            {
                // Get wrapper class for the environment
                var context = new OwinContext(environment);

                // Check whether the authorization header is available. This contains the credentials.
                var authzValue = context.Request.Headers.Get("Authorization");
                if (string.IsNullOrEmpty(authzValue) || !authzValue.StartsWith("Basic ", StringComparison.OrdinalIgnoreCase))
                {
                    // Process next middleware
                    return Next(environment);
                }

                // Get credentials
                var creds = authzValue.Substring("Basic ".Length).Trim();
                string clientId;
                string clientSecret;

                if (RetrieveCreds(creds, out clientId, out clientSecret))
                {
                    // Set transaction authenticated as client
                    context.Request.User = new GenericPrincipal(new GenericIdentity(clientId, "client"), new string[] { "client" });
                }

                return Next(environment);
            }

            /// <summary>
            /// Retrieve credentials from header
            /// </summary>
            /// <param name="credentials">Authorization header</param>
            /// <param name="clientId">Client identifier</param>
            /// <param name="clientSecret">Client secret</param>
            /// <returns>True if valid credentials were presented</returns>
            private bool RetrieveCreds(string credentials, out string clientId, out string clientSecret)
            {
                string pair;
                clientId = clientSecret = string.Empty;

                try
                {
                    pair = Encoding.UTF8.GetString(Convert.FromBase64String(credentials));
                }
                catch (FormatException)
                {
                    return false;
                }
                catch (ArgumentException)
                {
                    return false;
                }

                var ix = pair.IndexOf(':');
                if (ix == -1)
                {
                    return false;
                }

                clientId = pair.Substring(0, ix);
                clientSecret = pair.Substring(ix + 1);

                // Return whether credentials are valid
                return (string.Compare(clientId, ClientAuthMiddleware.ClientID) == 0 &&
                    string.Compare(clientSecret, ClientAuthMiddleware.ClientSecret) == 0);
            }
        }
    }
    ```

### <a name="step-14-add-an-owin-startup-class"></a>Adım 1,4: bir OWıN başlangıç sınıfı ekleme

API 'ye `Startup.cs` adlı bir OWıN başlangıç sınıfı ekleyin. Bunu yapmak için:
1. Projeye sağ tıklayın, > **Yeni öğe** **Ekle** ' yi seçin ve ardından **owın**' ı arayın.

   ![Visual Studio 'da yeni öğe Ekle iletişim kutusunda OWıN başlangıç sınıfı oluşturma](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-secure-basic-OWIN-startup.png)

2. *Startup.cs* dosyasını açın ve dosya içeriğini şu kodla değiştirin:

    ```csharp
    using Microsoft.Owin;
    using Owin;

    [assembly: OwinStartup(typeof(Contoso.AADB2C.API.Startup))]
    namespace Contoso.AADB2C.API
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                app.Use<ClientAuthMiddleware>();
            }
        }
    }
    ```

### <a name="step-15-protect-the-identity-api-class"></a>Adım 1,5: kimlik API sınıfını koruma

Controllers\ıdentitycontroller.cs dosyasını açın ve denetleyici sınıfına `[Authorize]` etiketini ekleyin. Bu etiket, denetleyiciye erişimi yetkilendirme gereksinimini karşılayan kullanıcılara kısıtlar.

![Denetleyiciye Yetkilendir etiketini ekleyin](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a>2\. Adım: Azure 'da yayımlama

Projenizi yayımlamak için Çözüm Gezgini, **contoso. AADB2C. API** projesine sağ tıklayın ve ardından **Yayımla**' yı seçin.

## <a name="step-3-add-the-restful-services-app-id-and-app-secret-to-azure-ad-b2c"></a>3\. Adım: Azure AD B2C için Restuz Hizmetleri uygulama KIMLIĞI ve uygulama gizli anahtarını ekleyin

Yeniden deneme hizmetiniz istemci KIMLIĞI (Kullanıcı adı) ve gizli anahtar tarafından korunduktan sonra, kimlik bilgilerini Azure AD B2C kiracınızda depolamanız gerekir. Özel ilkeniz, yeniden takip eden hizmetlerinizi istediğinde kimlik bilgilerini sağlar.

### <a name="step-31-add-a-restful-services-client-id"></a>Adım 3,1: yeniden bir hizmet istemci KIMLIĞI ekleyin

1. Azure AD B2C kiracınızda **B2C ayarları** > **kimlik deneyimi çerçevesi**' ni seçin.


2. Kiracınızda kullanılabilir olan anahtarları görüntülemek için **Ilke anahtarlarını** seçin.

3. **Add (Ekle)** seçeneğini belirleyin.

4. **Seçenekler**Için **el ile**' yi seçin.

5. **Ad**için **B2cRestClientId**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenebilir.

6. **Gizli** kutusunda, daha önce TANıMLADıĞıNıZ uygulama kimliğini girin.

7. **Anahtar kullanımı**için **imza**' yı seçin.

8. **Oluştur**’u seçin.

9. `B2C_1A_B2cRestClientId` anahtarını oluşturduğunuzdan emin olun.

### <a name="step-32-add-a-restful-services-client-secret"></a>Adım 3,2: yeniden bir hizmet istemci parolası ekleme

1. Azure AD B2C kiracınızda **B2C ayarları** > **kimlik deneyimi çerçevesi**' ni seçin.

2. Kiracınızda kullanılabilir olan anahtarları görüntülemek için **Ilke anahtarlarını** seçin.

3. **Add (Ekle)** seçeneğini belirleyin.

4. **Seçenekler**Için **el ile**' yi seçin.

5. **Ad**için **B2cRestClientSecret**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenebilir.

6. **Gizli** kutusunda, daha önce tanımladığınız uygulama gizli anahtarını girin.

7. **Anahtar kullanımı**için **imza**' yı seçin.

8. **Oluştur**’u seçin.

9. `B2C_1A_B2cRestClientSecret` anahtarını oluşturduğunuzdan emin olun.

## <a name="step-4-change-the-technical-profile-to-support-basic-authentication-in-your-extension-policy"></a>4\. Adım: uzantı ilkenizde temel kimlik doğrulamasını destekleyecek şekilde teknik profili değiştirme

1. Çalışma dizininizde, uzantı ilkesi dosyasını açın (TrustFrameworkExtensions. xml).

2. `Id="REST-API-SignUp"`içeren `<TechnicalProfile>` düğümünü arayın.

3. `<Metadata>` öğesini bulun.

4. *AuthenticationType* öğesini aşağıdaki gibi *temel*olarak değiştirin:

    ```xml
    <Item Key="AuthenticationType">Basic</Item>
    ```

5. Kapanış `<Metadata>` öğesinden hemen sonra aşağıdaki XML kod parçacığını ekleyin:

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```

    Kod parçacığını ekledikten sonra teknik profiliniz aşağıdaki XML kodu gibi görünmelidir:

    ![Temel kimlik doğrulaması XML öğelerini teknisyen 'e ekleme](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a>5\. Adım: ilkeyi kiracınıza yükleme

1. [Azure Portal](https://portal.azure.com), Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.

1. **Kimlik deneyimi çerçevesini**seçin.

1. **Tüm ilkeleri**açın.

1. **Ilkeyi karşıya yükle**' yi seçin.

1. Varsa **Ilkenin üzerine yaz** onay kutusunu seçin.

1. *TrustFrameworkExtensions. xml* dosyasını karşıya yükleyin ve ardından doğrulamayı geçirdiğinizden emin olun.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>6\. Adım: Şimdi Çalıştır 'ı kullanarak özel ilkeyi test etme

1. **Azure AD B2C ayarları**' nı açın ve ardından **kimlik deneyimi çerçevesi**' ni seçin.

    >[!NOTE]
    >Şimdi Çalıştır, kiracıya en az bir uygulamanın önceden bağlı olmasını gerektirir. Uygulamaları nasıl kaydedeceğinizi öğrenmek için Azure AD B2C [Başlarken](tutorial-create-tenant.md) makalesine veya [uygulama kaydı](tutorial-register-applications.md) makalesine bakın.

2. Karşıya yüklediğiniz bağlı olan taraf (RP) özel ilkesini **B2C_1A_signup_signin**açın ve **Şimdi Çalıştır**' ı seçin.

3. **Verilen ad** kutusuna **Test** yazarak işlemi test edin.
    Azure AD B2C pencerenin üst kısmında bir hata mesajı görüntüler.

    ![Kimlik API 'niz içindeki verilen ad giriş doğrulamasını test etme](./media/secure-rest-api-dotnet-basic-auth/rest-api-netfw-test.png)

4. **Verilen ad** kutusuna bir ad yazın ("test" dışında).
    Azure AD B2C kullanıcıyı imzalar ve sonra uygulamanıza bir bağlılık programı numarası gönderir. Bu örnekteki sayıyı aklınızda:

    ```
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
    }.{
      "exp": 1507125903,
      "nbf": 1507122303,
      "ver": "1.0",
      "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
      "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
      "acr": "b2c_1a_signup_signin",
      "nonce": "defaultNonce",
      "iat": 1507122303,
      "auth_time": 1507122303,
      "loyaltyNumber": "290",
      "given_name": "Emily",
      "emails": ["B2cdemo@outlook.com"]
    }
    ```

## <a name="optional-download-the-complete-policy-files-and-code"></a>Seçim Tüm ilke dosyalarını ve kodunu indirin

* [Özel ilkeleri kullanmaya başlama talimatlarını](custom-policy-get-started.md) tamamladıktan sonra, kendi özel ilke dosyalarınızı kullanarak senaryonuzu oluşturmanızı öneririz. Başvurunuz için [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic)sağladık.
* [Başvuru Için örnek Visual Studio çözümünden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic)tüm kodu indirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeniden takip eden API 'nizin güvenliğini sağlamak için istemci sertifikalarını kullanın](secure-rest-api-dotnet-certificate-auth.md)
