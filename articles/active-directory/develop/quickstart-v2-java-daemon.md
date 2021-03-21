---
title: "Hızlı başlangıç: Java Daemon 'dan Microsoft Graph çağırma | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, Java uygulamasının bir erişim belirtecini nasıl alabileceğinizi ve uygulamanın kendi kimliğini kullanarak Microsoft Identity platform uç noktası tarafından korunan bir API 'YI nasıl çağırabileceğinizi öğrenirsiniz.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 01/22/2021
ms.author: nacanuma
ms.custom: aaddev, scenarios:getting-started, languages:Java, devx-track-java
ms.openlocfilehash: 196b80a704b8a270a4cbb7d3505d5f9be1e23479
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99820333"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-java-console-app-using-apps-identity"></a>Hızlı başlangıç: uygulamanın kimliğini kullanarak bir Java konsol uygulamasından belirteç alma ve Microsoft Graph API çağırma

Bu hızlı başlangıçta, Java uygulamasının, Microsoft Graph API 'sini çağırmak ve dizindeki [kullanıcıların listesini](/graph/api/user-list) göstermek için uygulamanın kimliğini kullanarak bir erişim belirteci alma ve çalıştırma şeklini gösteren bir kod örneği indirirsiniz. Kod örneği, katılımsız bir işin veya Windows hizmetinin bir kullanıcının kimliği yerine bir uygulama kimliğiyle nasıl çalışacağını gösterir. 

> [!div renderon="docs"]
> ![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-java-daemon/java-console-daemon.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için şunlar gerekir:

- [Java Development Kit (JDK)](https://openjdk.java.net/) 8 veya üzeri
- [Maven](https://maven.apache.org/)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (aşağıdaki 1. seçenek) ve manuel (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. <a href="https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavaDaemonQuickstartPage/sourceType/docs" target="_blank">Azure portal uygulama kayıtları</a> hızlı başlangıç deneyimine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.
> 1. Uygulamanız için bir **ad** girin (örneğin,) `Daemon-console` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
> 1. **Kaydet**’i seçin.
> 1. **Yönet** altında **Sertifikalar & gizlilikler**' ı seçin.
> 1. **İstemci gizli** dizileri altında **yeni istemci parolası**' nı seçin, bir ad girin ve ardından **Ekle**' yi seçin. Daha sonraki bir adımda kullanmak üzere gizli bir konuma gizli değeri kaydedin.
> 1. **Yönet** altında **API izinleri**  >  **bir izin Ekle**' yi seçin. **Microsoft Graph** seçin.
> 1. **Uygulama izinleri**' ni seçin.
> 1. **Kullanıcı** düğümü altında **User. Read. All**' ı seçin ve ardından **izin Ekle**' yi seçin.

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-the-quickstart-app"></a>Hızlı başlangıç uygulamasını indirme ve yapılandırma
>
> #### <a name="step-1-configure-the-application-in-azure-portal"></a>1. Adım: uygulamayı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, bir istemci parolası oluşturmanız ve Graph API **Kullanıcı. Read. All** uygulama iznini eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-netcore-daemon/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-java-project"></a>2. Adım: Java projesini Indirme

> [!div renderon="docs"]
> [Java Daemon projesini indirin](https://github.com/Azure-Samples/ms-identity-java-daemon/archive/master.zip)

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-java-daemon/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`


> [!div renderon="docs"]
> #### <a name="step-3-configure-the-java-project"></a>3. Adım: Java projesini yapılandırma
>
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin *C:\Azure-Samples*) ayıklayın.
> 1. **Msal-Client-Credential-Secret** alt klasörüne gidin.
> 1. *Src\mainresources\application.exe* ' i düzenleyin ve alanları `AUTHORITY` , `CLIENT_ID` ve değerlerini aşağıdaki kod parçacığıyla değiştirin `SECRET` :
>
>    ```
>    AUTHORITY=https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/
>    CLIENT_ID=Enter_the_Application_Id_Here
>    SECRET=Enter_the_Client_Secret_Here
>    ```
>    Konum:
>    - `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
>    - `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com).
>    - `Enter_the_Client_Secret_Here` -Bu değeri, 1. adımda oluşturulan istemci gizli anahtarı ile değiştirin.
>
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) kimliği** değerlerini bulmak Için Azure Portal uygulamanın **genel bakış** sayfasına gidin. Yeni bir anahtar oluşturmak için **sertifikalar & gizlilikler** sayfasına gidin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>3. Adım: yönetici onayı

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>4. Adım: yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz, *HTTP 403-Yasak* hatası: ' ı alırsınız `Insufficient privileges to complete the operation` . Bu hata, *yalnızca uygulama Izni* yönetici onayı gerektirdiğinden oluşur: dizininizin genel Yöneticisi uygulamanıza onay vermelidir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini belirleyin:

##### <a name="global-tenant-administrator"></a>Genel Kiracı Yöneticisi

> [!div renderon="docs"]
> Küresel kiracı yöneticisiyseniz, Azure portal **uygulama kayıtları** Içindeki **API izinleri** sayfasına gidin ve **{Tenant Name} için yönetici onayı ver** ' i seçin (burada {Tenant Name} dizininizin adıdır).

> [!div renderon="portal" class="sxs-lookup"]
> Genel yöneticiyseniz, **API izinleri** sayfasına gidin **Enter_the_Tenant_Name_Here Için yönetici onayı ver**' i seçin.
> > [!div id="apipermissionspage"]
> > [API Izinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart Kullanıcı

Kiracınızın standart bir kullanıcısı kullanıyorsanız, uygulamanız için yönetici onayı vermesini istemek üzere bir genel yönetici yapmanız gerekir. Bunu yapmak için, yöneticinize aşağıdaki URL 'YI verin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Konum:
>> * `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>4. Adım: uygulamayı çalıştırma

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>5. Adım: uygulamayı çalıştırma

Örnek olarak, IDE 'nizden ClientCredentialGrant. Java ' ın Main yöntemini çalıştırarak örneği test edebilirsiniz.

Kabuk veya komut satırınızdan:

```
$ mvn clean compile assembly:single
```

Bu,/targets dizininizde bir msal-Client-Credential-Secret-1.0.0. jar dosyası oluşturur. Bunu aşağıdaki gibi Java yürütülebilir dosyanızı kullanarak çalıştırın:

```
$ java -jar msal-client-credential-secret-1.0.0.jar
```

Çalıştırdıktan sonra uygulama, yapılandırılan Kiracıdaki kullanıcıların listesini görüntülemelidir.


> [!IMPORTANT]
> Bu hızlı başlangıç uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci gizli anahtarı kullanır. İstemci parolası proje dosyalarınıza düz metin olarak eklendiğinden, güvenlik nedenleriyle, uygulamayı üretim uygulaması olarak düşünmeden önce istemci parolası yerine bir sertifika kullanmanız önerilir. Sertifika kullanma hakkında daha fazla bilgi için, bu örnek için aynı GitHub deposunda, ancak ikinci klasörde **msal-Client-Credential-Certificate** içindeki [Bu yönergelere](https://github.com/Azure-Samples/ms-identity-java-daemon/tree/master/msal-client-credential-certificate) bakın.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msal-java"></a>MSAL Java

[Msal Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) , kullanıcıların oturum açması ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemek için kullanılan bir kitaplıktır. Bu hızlı başlangıç, açıklandığı şekilde, uygulamanın temsilci izinleri yerine kendi kimliğini kullanarak belirteçleri ister. Bu örnekte kullanılan kimlik doğrulama akışı, *[istemci kimlik bilgileri OAuth akışı](v2-oauth2-client-creds-grant-flow.md)* olarak bilinir. MSAL Java 'Yı Daemon uygulamalarıyla kullanma hakkında daha fazla bilgi için [Bu makaleye](scenario-daemon-overview.md)bakın.

Uygulamanızın pom.xml (Maven) veya Build. Gradle (Gradle) dosyasında aşağıdaki değişiklikleri yaparak bağımlılıklarınızı yönetmek için Maven veya Gradle kullanarak uygulamanıza MSAL4J ekleyin.

pom.xml:

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

Build. Gradle içinde:

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="msal-initialization"></a>MSAL başlatma

MSAL4J kullanacağınız dosyanın en üstüne aşağıdaki kodu ekleyerek Java için MSAL öğesine bir başvuru ekleyin:

```Java
import com.microsoft.aad.msal4j.*;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```Java
IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

> | Konum: |Description |
> |---------|---------|
> | `CLIENT_SECRET` | Azure portalında uygulama için istemci gizli dizisi oluşturulmuştur. |
> | `CLIENT_ID` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `AUTHORITY`    | Kimlik doğrulaması yapılacak kullanıcı için STS uç noktası. Genellikle `https://login.microsoftonline.com/{tenant}` {Tenant}, kiracınızın adı veya kiracı kimliğiniz olduğu genel bulut için.|

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak bir belirteç istemek için `acquireToken` yöntemi kullanın:

```Java
IAuthenticationResult result;
     try {
         SilentParameters silentParameters =
                 SilentParameters
                         .builder(SCOPE)
                         .build();

         // try to acquire token silently. This call will fail since the token cache does not
         // have a token for the application you are requesting an access token for
         result = cca.acquireTokenSilently(silentParameters).join();
     } catch (Exception ex) {
         if (ex.getCause() instanceof MsalException) {

             ClientCredentialParameters parameters =
                     ClientCredentialParameters
                             .builder(SCOPE)
                             .build();

             // Try to acquire a token. If successful, you should see
             // the token information printed out to console
             result = cca.acquireToken(parameters).join();
         } else {
             // Handle other exceptions accordingly
             throw ex;
         }
     }
     return result;
```

> |Konum:| Description |
> |---------|---------|
> | `SCOPE` | İstenen kapsamları içerir. Gizli istemciler için, `{Application ID URI}/.default` istenen kapsamların Azure Portal (Microsoft Graph için, işaret edilen uygulama nesnesi içinde statik olarak tanımlandığını göstermek için şuna benzer biçimi kullanması gerekir `{Application ID URI}` `https://graph.microsoft.com` ). Özel Web API 'Leri için, `{Application ID URI}` Azure portalındaki uygulama kayıtları **bir API 'yi kullanıma** sunma  bölümünde tanımlanmıştır.|

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daemon uygulamaları hakkında daha fazla bilgi için bkz. senaryo giriş sayfası.

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran Daemon uygulaması](scenario-daemon-overview.md)
