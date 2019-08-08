---
title: Microsoft Identity platform Android hızlı başlangıç | Mavisi
description: Android uygulamalarının Microsoft Identity platform uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2019
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88a0f16a01a7e421558b5cea99daee6944adae11
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853037"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı Başlangıç: Kullanıcıların oturum açması ve Android uygulamasından Microsoft Graph API 'sini çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, bir Android uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * Android Studio 
> * Android 16 + gereklidir 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * Çaba [Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * Kitabında [Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin
> Uygulamanızı kaydetmek için
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AndroidQuickstart`.
>      - Bu sayfadaki diğer konfigürasyonları atlayabilirsiniz. 
>      - `Register` Düğmeye basın.
> 1. Git `Authentication` >Yeni`Add Platform`uygulamaya tıklayın .`Android`  >   >     
>      - Android Studio projenizden paket adını girin. 
>      - Imza karması oluşturun. Yönergeler için portala başvurun.
> 1. Daha `Configure` sonra için ***msal Configuration*** JSON ' ı seçin ve kaydedin. 

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>1\. adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, auth broker ile uyumlu bir yeniden yönlendirme URI 'SI eklemeniz gerekir. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-android/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-the-project"></a>2\. adım: Projeyi indirin

* [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3\. adım: Projenizi yapılandırma

> [!div renderon="docs"]
> Yukarıdaki 1. seçeneği belirlediyseniz, bu adımları atlayabilirsiniz. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. **App**src ana res RAW içinde, auth_config. JSON öğesini açın. >  >  >  > 
> 1. **Auth_config. JSON** öğesini düzenleyin ve Azure Portal JSON ile değiştirin. Bunun yerine, değişiklikleri el ile yapmak istiyorsanız:
>    ```javascript
>    {
>       "client_id" : "Enter_the_Application_Id_Here",
>       "authorization_user_agent" : "DEFAULT",
>       "redirect_uri" : "Enter_the_Redirect_Uri_Here",
>       "authorities" : [
>          {
>             "type": "AAD",
>             "audience": {
>                "type": "Enter_the_Audience_Info_Here",
>                "tenant_id": "Enter_the_Tenant_Info_Here"
>             }
>          }
>       ]
>    }
>    ```
> 
> 1. **Uygulama** > **bildirimleri**içinde **AndroidManifest. xml**' yi açın.
> 1. Aşağıdaki etkinliği **manifest\application** node öğesine yapıştırın: 
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Uygulamayı çalıştırın! 

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıç, Enter_the_Supported_Account_Info_Here destekler.

> [!div renderon="docs"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. **Uygulama** > **res**RAW içinde, auth_config. json ' u açın. > 
> 1. **Auth_config. JSON** öğesini düzenleyin ve Azure Portal JSON ile değiştirin. Bunun yerine, bu değişiklikleri el ile yapmak istiyorsanız:
>    ```javascript
>    "client_id" : "ENTER_YOUR_APPLICATION_ID",
>    "redirect_uri": "ENTER_YOUR_REDIRECT_URI", 
>     ```
> 1. **Uygulama** > **bildirimleri**içinde **AndroidManifest. xml**' yi açın.
> 1. Aşağıdaki etkinliği **manifest\application** düğümüne ekleyin. Bu kod parçacığı, kimlik doğrulaması tamamlandıktan sonra işletim sisteminin uygulamanızı sürdürmesini sağlamak için bir **BrowserTabActivity** kaydeder:
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Decoded_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> 1. `Enter_the_Package_Name` Ve`Enter_the_Signature_Hash` değerlerini Azure Portal kaydettiğiniz değerlerle değiştirin. 
> 1. Uygulamayı çalıştırın! 

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi için aşağıdaki bölümleri okuyun.

### <a name="getting-msal"></a>MSAL alma

MSAL ([com. Microsoft. Identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal)), kullanıcıların oturum açmasını ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. **Bağımlılıklar**altında **Gradle komut dosyası** > **Build. Gradle (Module: App)** bölümüne şunu ekleyerek yüklemek için Gradle 3.0 + kullanabilirsiniz:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.3.+'
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```java
import com.microsoft.identity.client.*;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```java
    sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);
```

> |Konumlar: ||
> |---------|---------|
> |`R.raw.auth_config` | Bu dosya, uygulamanız/Istemci KIMLIĞINIZ, oturum açma hedef kitlesi, yeniden yönlendirme URI 'SI ve diğer çeşitli özelleştirme seçenekleri de dahil olmak üzere uygulamanızın yapılandırmasını içerir. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL’in belirteç almak için kullanılan iki yöntemi vardır: `acquireToken` ve `acquireTokenSilentAsync`

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Belirteci etkileşimli olarak alma

Bazı durumlar, kullanıcıların Microsoft Identity platformu ile etkileşime girmesini gerektirir. Bu durumlarda, son kullanıcının hesabını seçmeniz, kimlik bilgilerini girmesi veya uygulamanızın istediği izinlere izin vermeniz gerekebilir. Örneğin, 

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir 
* Onay iptal edildiğinde 
* Uygulamanız açıkça izin gerektiriyorsa. 
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

#### <a name="acquiretokensilent-getting-a-user-token-silently"></a>acquireTokenSilent: Kullanıcı belirtecini sessizce alma

Uygulamalar, kullanıcıların her belirteç istediklerinde oturum açmasını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, bu yöntem uygulamaların belirteçleri sessizce istemesine izin verir.

```java
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (!accounts.isEmpty()) {
                sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
            } else {
                /* No accounts */
            }
        }
    });
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `getAccounts(...)` | Sessizce belirteç almaya çalıştığınız hesabı içerir |
> | `getAuthSilentCallback()` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

## <a name="next-steps"></a>Sonraki adımlar

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

Uygulama oluşturma işlemiyle ve yeni özelliklerle ilgili eksiksiz adım adım yönergeleri almak için Android öğreticisini deneyin. Orada, bu hızlı başlangıcın tam açıklaması da yer alır.

> [!div class="nextstepaction"]
> [Çağrı Grafı API'si Android öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Android için MSAL kitaplığı wiki'si

Android için MSAL kitaplığı hakkındaki diğer yazıları okuyun:

> [!div class="nextstepaction"]
> [Android için MSAL kitaplığı wiki'si](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)