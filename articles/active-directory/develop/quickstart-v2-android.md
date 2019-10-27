---
title: Microsoft Identity platform Android hızlı başlangıç | Mavisi
description: Android uygulamalarının Microsoft Identity platform uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2019
ms.author: twhitney
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:Android
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2704a6e1af2f06b49b1d3817ad7a30bf53419ffe
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72963999"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı başlangıç: Android uygulamasından kullanıcıların oturum açma ve Microsoft Graph API'sini çağırma

Bu hızlı başlangıç, bir Android uygulamasının kişisel, iş veya okul hesaplarında nasıl oturum açıp bir erişim belirteci edindiğini ve Microsoft Graph API 'sini çağırabileceğini göstermek için bir kod örneği kullanır.

![Örnek uygulamanın screenshoft](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * Android Studio 
> * Android 16 + gereklidir

## <a name="step-1-get-the-sample-app"></a>1\. Adım: örnek uygulamayı edinme

[Kodu kopyalayın](https://github.com/Azure-Samples/ms-identity-android-java.git).

## <a name="step-2-register-your-application"></a>2\. Adım: Uygulamanızı kaydetme

Uygulama nesnesini kaydetmek ve bu uygulama nesnesinin kayıt bilgilerini örnek projeye el ile eklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://aka.ms/MobileAppReg) gidin.
1. [Uygulama kayıtları dikey penceresini](https://portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) açın ve **+ Yeni kayıt**' ye tıklayın.
1. Uygulama kaydınız için bir **ad** girin ve yeniden yönlendirme URI 'Sini ayarlamadan **Kaydet**' e tıklayın.
1. **Yönet** bölümünde **kimlik doğrulama** > + **Android** > **Platform ekleme** ' yi seçin. (Bu ekranı görmek için dikey pencerenin üst kısmındaki **Yeni deneyimi dene** ' yi seçmeniz gerekebilir)
1. Projenizin **paket adını**girin, `com.azuresamples.msalandroidapp`.
1. **Android uygulamanızı yapılandırma** sayfasında bulunan **imza karması** bölümünde, **bir geliştirme imza karması oluşturma**' ya tıklayın ve anahtar aracı komutunu, Android uygulamanızı geliştirirken kullandığınız platformda kullanmak üzere kopyalayın.

   > [!Note]
   > KeyTool. exe, Java Development Kit 'in (JDK) bir parçası olarak yüklenir. Ayrıca, KeyTool komutunu yürütmek için OpenSSL aracını da yüklemelisiniz.  Yolunuzda Keytool ve OpenSSL\bin dizinine ihtiyacınız vardır.

1. Portaldan kopyaladığınız Keytool komutunu bir Terminal penceresinde çalıştırın.
1. **İmza karması**altında portala oluşturulan imza karmasını girin.
1. `Configure` ' a tıklayın ve **msal yapılandırmasının**bir kopyasını oluşturun. Bunu bir sonraki adımda bir yapılandırma dosyasına kopyalayıp yapıştırabilirsiniz. **Bitti**’ye tıklayın.

## <a name="step-3-add-your-app-registration"></a>3\. Adım: uygulama kaydınızı ekleme

1. Örnek projeyi Android Studio açın.
1. **Uygulamanın** içinde > **res** > **RAW**, Open **auth_config_multiple_account. JSON**' ı açın.  MSAL yapılandırmasının içeriğini yapıştırın. Bu, portaldan istemci KIMLIĞI, kiracı KIMLIĞI ve redirect_uri öğesini ekler. Bu, aşağıdakine benzer şekilde görünür, ancak istemci KIMLIĞI, kiracı KIMLIĞI ve redirect_uri için doldurulmuş değerler:

    ```json
    {
      "client_id" : "<your_client_id_here>",
      "authorization_user_agent" : "DEFAULT",
      "redirect_uri" : "<your_redirect_uri_here>",
      "account_mode" : "MULTIPLE",
      "broker_redirect_uri_registered": true,
      "authorities" : [
        {
          "type": "AAD",
          "audience": {
            "type": "AzureADandPersonalMicrosoftAccount",
            "tenant_id": "common"
          }
        }
      ]
    }
    ```

1. **App** > **res** > **RAW**' ı açın, **AUTH_CONFIG_SINGLE_ACCOUNT. JSON**' ı açın ve msal yapılandırmasının içeriğini yapıştırın. Yukarıdaki **auth_config_multiple_account. JSON** dosyası gibi görünür.
1. **Uygulama** > **bildirimleri** > **AndroidManifest. xml**' de `BrowserTabActivity` etkinliğini bulun. Bu giriş, Microsoft 'un, kimlik doğrulamasını tamamladıktan sonra uygulamanıza geri çağırmasını sağlar:

    ```xml
    ...
    <activity android:name="com.microsoft.identity.client.BrowserTabActivity">
                <intent-filter>
                    <action android:name="android.intent.action.VIEW" />
    
                    <category android:name="android.intent.category.DEFAULT" />
                    <category android:name="android.intent.category.BROWSABLE" />
    
                    <!--
                        Add in your scheme/host from registered redirect URI
                        note that the leading "/" is required for android:path
                        For Example:
                        <data
                            android:host="com.azuresamples.msalandroidapp"
                            android:path="/1wIqXSqBj7w+h11ZifsnqwgyKrY="
                            android:scheme="msauth" />
                    -->
    
                    <data
                        android:host="YOUR_PACKAGE_NAME - must be registered at https://aka.ms/MobileAppReg"
                        android:path="/YOUR_DECODED_SIGNATURE_HASH - must be registered at https://aka.ms/MobileAppReg"
                        android:scheme="msauth" />
                </intent-filter>
            </activity>
    ```
    
1. Paket adını, `android:host=` değeri için Azure portal kaydettiğiniz şekilde değiştirin.  Bu durumda, şu şekilde olacaktır: `com.azuresamples.msalandroidapp`.

    > [!IMPORTANT]
    > **Android: Path** değeri önünde bir "/" karakteri **olmalıdır** veya değerin altında kırmızı bir çizgi alırsınız ve örnek uygulama çalışmaz.
     
1. Daha önce Keytool 'u çalıştırarak aldığınız anahtar karmasını değiştirin ve `android:path=` değeri için Azure portal yazın. Imza karması URL kodlamalı olmamalıdır.

## <a name="step-4-run-the-sample-app"></a>4\. Adım: örnek uygulamayı çalıştırma

Android Studio **kullanılabilir cihazlar** açılan menüsünden öykünücü veya cihazınızı seçin ve uygulamayı çalıştırın.

Örnek uygulama, **tek hesap modu** ekranında başlar. Varsayılan olarak, **Kullanıcı. Read**, Microsoft Graph API çağrısı sırasında kendi profil verilerinizi okurken kullanılan varsayılan bir kapsam sağlanır. Microsoft Graph API çağrısı URL 'SI varsayılan olarak sağlanır. İsterseniz bunların her ikisini de değiştirebilirsiniz.

![Tek ve birden çok hesap kullanımını gösteren örnek uygulama MSAL](./media/quickstart-v2-android/quickstart-sample-app.png)

Tek ve birden çok hesap modlarını değiştirmek için uygulama menüsünü kullanın.

Tek hesap modunda bir iş veya giriş hesabı kullanarak oturum açın:

1. Kullanıcıdan kimlik bilgilerini girmesini istemek için **grafik verilerini etkileşimli olarak al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.
2. Oturum açtıktan sonra, kullanıcıdan kimlik bilgilerini yeniden sormadan Microsoft Graph API 'sine çağrı yapmak için **grafik verilerini sessizce al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.

Birden çok hesap modunda aynı adımları tekrarlayabilirsiniz.  Ayrıca, bu hesaba ait önbelleğe alınmış belirteçleri de kaldıran oturum açmış olan hesabı kaldırabilirsiniz.

## <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

Kod, tek ve birden çok hesap MSAL uygulamasının nasıl yazılacağını gösteren parçalar halinde düzenlenir. Kod dosyaları aşağıdaki gibi düzenlenmiştir:

| Dosya  | Leceğini  |
|---------|---------|
| MainActivity | Kullanıcı arabirimini yönetir |
| MSGraphRequestWrapper  | MSAL tarafından sunulan belirteci kullanarak Microsoft Graph API 'sini çağırır |
| Multipleaccountmobirleştirmesi  | Çoklu hesap uygulaması başlatır, bir kullanıcı hesabı yükler ve Microsoft Graph API 'sini çağırmak için bir belirteç alır |
| Singleaccountmobirleştirmesi | Tek hesap uygulaması başlatır, bir kullanıcı hesabı yükler ve Microsoft Graph API 'sini çağırmak için bir belirteç alır |
| Res/auth_config_multiple_account. JSON  | Birden çok hesap yapılandırma dosyası |
| Res/auth_config_single_account. JSON  | Tek hesap yapılandırma dosyası |
| Gradle betikler/Build. Grad (modül: uygulama) | MSAL kitaplığı bağımlılıkları buraya eklenir |

Şimdi bu dosyalara daha ayrıntılı olarak bakacağız ve MSAL 'e özgü kodu her birinde çağıracağız.

### <a name="add-msal-to-the-app"></a>Uygulamaya MSAL ekleme

MSAL ([com. Microsoft. Identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal)), kullanıcıların oturum açmasını ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Gradle 3.0 + **Bağımlılıklar**altında **Build. Gradle (Module: App)**  > **Gradle betiklerine** aşağıdakileri eklediğinizde kitaplığı yükleme:

```gradle  
implementation 'com.microsoft.identity.client:msal:1.0.0'
```

Bunu, Build. Gradle (Module: App) içinde örnek projede görebilirsiniz:

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.0.0-RC7'
    ...
}
```

Bu, Gradle 'in Maven Central 'dan MSAL 'i indirmesini ve oluşturmasını sağlar.

### <a name="msal-imports"></a>MSAL içeri aktarmalar

MSAL kitaplığıyla ilgili içeri aktarmalar `com.microsoft.identity.client.*`.  Örneğin, genel istemci uygulamanızı temsil eden `PublicClientApplication` sınıfının ad alanı olan `import com.microsoft.identity.client.PublicClientApplication;` görürsünüz.

### <a name="singleaccountmodefragmentjava"></a>Singleaccountmobirleştirmesi. Java

Bu dosya, tek bir Account MSAL uygulamasının nasıl oluşturulduğunu ve Microsoft Graph API 'sinin nasıl çağrılacağını gösterir.

Tek hesap uygulamaları yalnızca tek bir kullanıcı tarafından kullanılır.  Örneğin, ile eşleme uygulamanızda oturum açmanız gereken tek bir hesabınız olabilir.

#### <a name="single-account-msal-initialization"></a>Tek hesap MSAL başlatması

`onCreateView()`, `auth_config_single_account.json` dosyasında depolanan yapılandırma bilgileri kullanılarak tek bir hesap `PublicClientApplication` oluşturulur.  Bu, MSAL kitaplığını tek-Account MSAL uygulamasında kullanılmak üzere nasıl başlataöğreneceksiniz:

```java
...
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createSingleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_single_account,
        new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(ISingleAccountPublicClientApplication application) {
                /**
                    * This test app assumes that the app is only going to support one account.
                    * This requires "account_mode" : "SINGLE" in the config json file.
                    **/
                mSingleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                displayError(exception);
            }
        });
```

#### <a name="sign-in-a-user"></a>Kullanıcı oturumu açma

Bir kullanıcının oturum açma kodu `initializeUI()`, `signInButton` tıklama işleyicisi ' nde.

Belirteçleri edinmeyi denemeden önce `signIn()` çağırın. `signIn()`, `acquireToken()` çağrılsa da, kullanıcının oturum açması için etkileşimli bir istem oluşmasına neden olur.

Kullanıcı oturumu açmak zaman uyumsuz bir işlemdir. Microsoft Graph API 'sini çağıran ve Kullanıcı oturum açtığında kullanıcı ARABIRIMINI güncelleştiren bir geri çağırma geçirilir:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Kullanıcı oturumunu kapatma

Bir kullanıcının oturumunu kapatmak için kod `initializeUI()`, `signOutButton` tıklama işleyicisine tıklayın.  Bir kullanıcının imzalanmasının zaman uyumsuz bir işlemdir. Kullanıcının imzalanması, bu hesabın belirteç önbelleğini de temizler. Kullanıcı hesabı oturumu açıldıktan sonra Kullanıcı ARABIRIMINI güncelleştirmek için bir geri çağırma oluşturulur:

```java
mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
    @Override
    public void onSignOut() {
        updateUI(null);
        performOperationOnSignOut();
    }

    @Override
    public void onError(@NonNull MsalException exception) {
        displayError(exception);
    }
});
```

#### <a name="get-a-token-interactively-or-silently"></a>Bir belirteci etkileşimli olarak veya sessizce alın

Kullanıcıya en az sayıda istem sunmak için genellikle sessizce bir belirteç alırsınız. Daha sonra bir hata varsa, belirteci etkileşimli olarak almaya çalışın. Uygulama ilk kez `signIn()`çağırdığında, `acquireToken()`bir çağrı olarak çalışır ve bu da kullanıcıdan kimlik bilgilerini ister.

Kullanıcıdan kendi hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın istediği izinlere izin vermeniz istenebilir:

* Kullanıcı uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir
* Onay iptal edildiğinde
* Uygulamanız açıkça izin gerektiriyorsa
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

Kullanıcıyı içerecek olan UI ile etkileşimli olarak bir belirteci almak için kod, `callGraphApiInteractiveButton` tıklama işleyicisine `initializeUI()`.

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Kullanıcı zaten oturum açmışsa, `acquireTokenSilentAsync()` `initializeUI()`gösterildiği gibi uygulamaların belirteçleri sessizce istemesine izin verir `callGraphApiSilentButton` işleyici ' de tıklayın:

```java
/**
  * Once you've signed the user in,
  * you can perform acquireTokenSilent to obtain resources without interrupting the user.
  */
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Hesap yükleme

Bir hesabı yüklemek için kod `loadAccount()`.  Kullanıcı hesabının yüklenmesi zaman uyumsuz bir işlemdir. bu nedenle, hesap yüklendiğinde, değiştiğinde veya hata oluştuğunda işlenecek geri çağrılar MSAL 'e geçirilir.  Aşağıdaki kod ayrıca, bir hesap kaldırıldığında, Kullanıcı başka bir hesaba değiştiğinde ve bu şekilde gerçekleşen `onAccountChanged()`işler.

```java
private void loadAccount() {
    ...

    mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback() {
        @Override
        public void onAccountLoaded(@Nullable IAccount activeAccount) {
            // You can use the account data to update your UI or your app database.
            updateUI(activeAccount);
        }

        @Override
        public void onAccountChanged(@Nullable IAccount priorAccount, @Nullable IAccount currentAccount) {
            if (currentAccount == null) {
                // Perform a cleanup task as the signed-in account changed.
                performOperationOnSignOut();
            }
        }

        @Override
        public void onError(@NonNull MsalException exception) {
            displayError(exception);
        }
    });
```

#### <a name="call-microsoft-graph"></a>Microsoft Graph çağrısı

Bir Kullanıcı oturum açmışsa, Microsoft Graph çağrısı bir HTTP isteği aracılığıyla `callGraphAPI()`tarafından yapılır. Bu işlev, `authenticationResult` erişim belirtecini alma ve MSGraphRequestWrapper çağrısını paketleme ve çağrının sonuçlarını görüntüleme gibi bazı görevleri gerçekleştirerek örneği basitleştiren bir sarmalayıcıdır.

```java
private void callGraphAPI(final IAuthenticationResult authenticationResult) {
    MSGraphRequestWrapper.callGraphAPIUsingVolley(
            getContext(),
            graphResourceTextView.getText().toString(),
            authenticationResult.getAccessToken(),
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    /* Successfully called graph, process data and send to UI */
                    ...
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    ...
                }
            });
}
```

### <a name="auth_config_single_accountjson"></a>auth_config_single_account. JSON

Bu, tek bir hesabı kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Bu alanların bir açıklaması için bkz. [ANDROID msal yapılandırma dosyasını anlayın](msal-configuration.md) .

Bu uygulamayı tek bir hesap kullanacak şekilde yapılandıran `"account_mode" : "SINGLE"`varlığını aklınızda edin.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

### <a name="multipleaccountmodefragmentjava"></a>Multipleaccountmobirleştirmesi. Java

Bu dosya, birden çok hesap MSAL uygulamasının nasıl oluşturulacağını ve bir Microsoft Graph API 'sinin nasıl çağrılacağını gösterir. 

Birden çok hesap uygulamasına bir örnek, bir iş hesabı ve kişisel hesap gibi birden çok kullanıcı hesabıyla çalışmanıza olanak tanıyan bir posta uygulamasıdır.

#### <a name="multiple-account-msal-initialization"></a>Birden çok hesap MSAL başlatması

`onCreateView()`, `auth_config_multiple_account.json file`depolanan yapılandırma bilgileri kullanılarak birden çok hesap uygulama nesnesi (`IMultipleAccountPublicClientApplication`) oluşturulur:

```java
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

Oluşturulan `MultipleAccountPublicClientApplication` nesnesi, belirteçleri almak ve Kullanıcı hesabını yüklemek ve kaldırmak için MSAL kitaplığıyla etkileşim kurmak üzere kullanılabilmesi için bir sınıf üyesi değişkeninde depolanır.

#### <a name="load-an-account"></a>Hesap yükleme

Birden çok hesap uygulaması genellikle MSAL işlemleri için kullanılacak hesabı seçmek üzere `GetAccounts()` çağırır. Bir hesabı yüklemek için kod `loadAccount()`.  Kullanıcı hesabının yüklenmesi zaman uyumsuz bir işlemdir. Bu nedenle, bir geri arama, hesap yüklendiğinde, değiştiğinde veya bir hata oluştuğunda durumları işler.

```java
/**
    * Load the currently signed-in account, if there's any.
    * In the shared device mode, if the user is signed out from the device, the app can also perform the clean-up work in onAccountChanged().
    */
private void loadAccount() {
    ...
    mMultipleAccountApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            // You can use the account data to update your UI or your app database.
            accountList = result;
            updateUI(accountList);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

#### <a name="get-a-token-interactively-or-silently"></a>Bir belirteci etkileşimli olarak veya sessizce alın

Kullanıcıdan kendi hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın istediği izinlere izin vermeniz istenebilir:

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir 
* Onay iptal edildiğinde 
* Uygulamanız açıkça izin gerektiriyorsa 
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

Birden çok hesap uygulaması genellikle belirteçleri, kullanıcıyı içeren kullanıcı ARABIRIMI olan bir `acquireToken()`çağrısıyla etkileşimli şekilde almalıdır.  Bir belirteci etkileşimli olarak almaya yönelik kod, `callGraphApiInteractiveButton` tıklama işleyicisine `initializeUI()`.

```java
/**
 * Acquire token interactively. It will also create an account object for the silent call as a result (to be obtained by getAccount()).
 *
 * If acquireTokenSilent() returns an error that requires an interaction,
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your SSO refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Uygulamalar, kullanıcının bir belirteç istediklerinde oturum açmasını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, `acquireTokenSilentAsync()` `callGraphApiSilentButton` tıklama işleyicisine `initializeUI()` gösterildiği gibi uygulamaların kullanıcıya sormadan belirteç istemesine izin verir:

```java
/**
 * Performs acquireToken without interrupting the user.
 *
 * This requires an account object of the account you're obtaining a token for.
 * (can be obtained via getAccount()).
 */
mMultipleAccountApp.acquireTokenSilentAsync(getScopes(),
accountList.get(accountListSpinner.getSelectedItemPosition()),
AUTHORITY,
getAuthSilentCallback());
```

#### <a name="remove-an-account"></a>Hesap kaldırma

Hesabı kaldırma kodu ve hesabın önbelleğe alınmış belirteçleri, hesabı Kaldır düğmesinin işleyicisinde `initializeUI()`. Bir hesabı kaldırabilmeniz için önce `getAccounts()` ve `acquireToken()`gibi MSAL işlevlerden elde ettiğiniz bir hesap nesnesi gerekir. Bir hesabı kaldırmak zaman uyumsuz bir işlem olduğundan, Kullanıcı arabirimini güncelleştirmek için `onRemoved` geri çağırma sağlanır.

```java
/**
  * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
  */
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccount();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>auth_config_multiple_account. JSON

Bu, birden çok hesap kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Bu alanların bir açıklaması için bkz. [ANDROID msal yapılandırma dosyasını anlayın](msal-configuration.md) .

[Auth_config_single_account. JSON](#auth_config_single_accountjson) yapılandırma dosyasının aksine, bu yapılandırma dosyası birden çok hesap uygulaması olduğu için `"account_mode" : "SINGLE"` yerine `"account_mode" : "MULTIPLE"` sahiptir.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "MULTIPLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturmaya yönelik adımları öğrenin

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
