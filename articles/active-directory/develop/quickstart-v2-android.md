---
title: Microsoft Identity platform Android hızlı başlangıç | Mavisi
description: Android uygulamalarının Microsoft Identity platform uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/15/2019
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:Android
ms.openlocfilehash: a46cd1b916edeae8a24fb997db46e5a0651567cb
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88115280"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı başlangıç: Android uygulamasından kullanıcıların oturum açma ve Microsoft Graph API'sini çağırma

Bu hızlı başlangıç, bir Android uygulamasının Microsoft Identity platformunu kullanarak kişisel, iş veya okul hesaplarında nasıl oturum açıp bir erişim belirteci edindiğini ve Microsoft Graph API 'sini çağırabileceğini göstermek için bir kod örneği kullanır. (Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.)

Uygulamalar, Microsoft Identity platformunun uygulamanıza belirteç sağlayabilmesi için Azure Active Directory bir uygulama nesnesi tarafından temsil etmelidir.

> [!div renderon="docs"]
> Bir kolaylık olması halinde, kod örneği dosyada önceden yapılandırılmış bir varsayılan ile birlikte gelir, `redirect_uri` `AndroidManifest.xml` böylece öncelikle kendi uygulama nesneniz kaydetmeniz gerekmez. `redirect_uri`, Kısmen uygulamanızın imzalama anahtarına dayanır. Örnek proje, bir imzalama anahtarıyla önceden yapılandırılmıştır ve bu sayede, belirtilen çalışacaktır `redirect_uri` . Uygulama nesnesini kaydetme ve uygulamanızla tümleştirme hakkında daha fazla bilgi edinmek için, [oturum açma kullanıcılarına bakın ve Android uygulama öğreticisindeki Microsoft Graph çağırın](tutorial-v2-android.md) .


> [!NOTE]
> **Önkoşullar**
> * Android Studio 
> * Android 16 +

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma 
>  Bu hızlı başlangıçta çalışması için kod örneği için, auth broker ile uyumlu bir yeniden yönlendirme URI 'SI eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-android/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış
>
> ### <a name="step-2-download-the-project"></a>2. Adım: Projeyi indirme 
> [!div class="sxs-lookup" renderon="portal"]
> Android Studio kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip)
>
> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor. 
> Örnek uygulama, **tek hesap modu** ekranında başlar. Varsayılan olarak, **Kullanıcı. Read**, Microsoft Graph API çağrısı sırasında kendi profil verilerinizi okurken kullanılan varsayılan bir kapsam sağlanır. Microsoft Graph API çağrısı URL 'SI varsayılan olarak sağlanır. İsterseniz bunların her ikisini de değiştirebilirsiniz.
>
> ![Tek ve birden çok hesap kullanımını gösteren örnek uygulama MSAL](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Tek ve birden çok hesap modlarını değiştirmek için uygulama menüsünü kullanın.
>
> Tek hesap modunda bir iş veya giriş hesabı kullanarak oturum açın:
>
> 1. Kullanıcıdan kimlik bilgilerini girmesini istemek için **grafik verilerini etkileşimli olarak al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.
> 2. Oturum açtıktan sonra, kullanıcıdan kimlik bilgilerini yeniden sormadan Microsoft Graph API 'sine çağrı yapmak için **grafik verilerini sessizce al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.
>
> Birden çok hesap modunda aynı adımları tekrarlayabilirsiniz.  Ayrıca, bu hesaba ait önbelleğe alınmış belirteçleri de kaldıran oturum açmış olan hesabı kaldırabilirsiniz.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> ## <a name="step-1-get-the-sample-app"></a>1. Adım: örnek uygulamayı edinme
>
> [Kodu indirin](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip).
>
> ## <a name="step-2-run-the-sample-app"></a>2. Adım: örnek uygulamayı çalıştırma
>
> Android Studio **kullanılabilir cihazlar** açılan menüsünden öykünücü veya fiziksel cihazınızı seçin ve uygulamayı çalıştırın.
>
> Örnek uygulama, **tek hesap modu** ekranında başlar. Varsayılan olarak, **Kullanıcı. Read**, Microsoft Graph API çağrısı sırasında kendi profil verilerinizi okurken kullanılan varsayılan bir kapsam sağlanır. Microsoft Graph API çağrısı URL 'SI varsayılan olarak sağlanır. İsterseniz bunların her ikisini de değiştirebilirsiniz.
>
> ![Tek ve birden çok hesap kullanımını gösteren örnek uygulama MSAL](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Tek ve birden çok hesap modlarını değiştirmek için uygulama menüsünü kullanın.
>
> Tek hesap modunda bir iş veya giriş hesabı kullanarak oturum açın:
>
> 1. Kullanıcıdan kimlik bilgilerini girmesini istemek için **grafik verilerini etkileşimli olarak al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.
> 2. Oturum açtıktan sonra, kullanıcıdan kimlik bilgilerini yeniden sormadan Microsoft Graph API 'sine çağrı yapmak için **grafik verilerini sessizce al** ' ı seçin. Ekranın alt kısmındaki Microsoft Graph API 'sine yapılan çağrıdan gelen çıktıyı görürsünüz.
>
> Birden çok hesap modunda aynı adımları tekrarlayabilirsiniz.  Ayrıca, bu hesaba ait önbelleğe alınmış belirteçleri de kaldıran oturum açmış olan hesabı kaldırabilirsiniz.

## <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Örnek uygulamanın ekran görüntüsü](media/quickstart-v2-android/android-intro.svg)


Kod, tek ve birden çok hesap MSAL uygulamasının nasıl yazılacağını gösteren parçalar halinde düzenlenir. Kod dosyaları aşağıdaki gibi düzenlenmiştir:

| Dosya  | Gösteriler  |
|---------|---------|
| MainActivity | Kullanıcı arabirimini yönetir |
| MSGraphRequestWrapper  | MSAL tarafından sunulan belirteci kullanarak Microsoft Graph API 'sini çağırır |
| Multipleaccountmobirleştirmesi  | Çoklu hesap uygulaması başlatır, bir kullanıcı hesabı yükler ve Microsoft Graph API 'sini çağırmak için bir belirteç alır |
| Singleaccountmobirleştirmesi | Tek hesap uygulaması başlatır, bir kullanıcı hesabı yükler ve Microsoft Graph API 'sini çağırmak için bir belirteç alır |
| Kaynak/auth_config_multiple_account.jsaçık  | Birden çok hesap yapılandırma dosyası |
| Kaynak/auth_config_single_account.jsaçık  | Tek hesap yapılandırma dosyası |
| Gradle betikler/Build. Grad (modül: uygulama) | MSAL kitaplığı bağımlılıkları buraya eklenir |

Şimdi bu dosyalara daha ayrıntılı olarak bakacağız ve MSAL 'e özgü kodu her birinde çağıracağız.

### <a name="adding-msal-to-the-app"></a>Uygulamaya MSAL ekleme

MSAL ([com. Microsoft. Identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal)), kullanıcıların oturum açmasını ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Gradle 3.0 +, bağımlılıklar altında **Gradle komut dosyası**  >  **Build. Gradle (Module: App)** öğesine aşağıdakileri eklediğinizde kitaplığı kurar: **Dependencies**

```gradle  
implementation 'com.microsoft.identity.client:msal:1.+'
```

Bunu, Build. Gradle (Module: App) içinde örnek projede görebilirsiniz:

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.+'
    ...
}
```

Bu, Gradle 'in Maven Central 'dan MSAL 'i indirmesini ve oluşturmasını sağlar.

### <a name="msal-imports"></a>MSAL içeri aktarmalar

MSAL kitaplığıyla ilgili içeri aktarmalar şunlardır `com.microsoft.identity.client.*` .  Örneğin, `import com.microsoft.identity.client.PublicClientApplication;` `PublicClientApplication` genel istemci uygulamanızı temsil eden sınıfının ad alanı olduğunu görürsünüz.

### <a name="singleaccountmodefragmentjava"></a>Singleaccountmobirleştirmesi. Java

Bu dosya, tek bir Account MSAL uygulamasının nasıl oluşturulduğunu ve Microsoft Graph API 'sinin nasıl çağrılacağını gösterir.

Tek hesap uygulamaları yalnızca tek bir kullanıcı tarafından kullanılır.  Örneğin, ile eşleme uygulamanızda oturum açmanız gereken tek bir hesabınız olabilir.

#### <a name="single-account-msal-initialization"></a>Tek hesap MSAL başlatması

İçinde, `auth_config_single_account.json` içinde, `onCreateView()` `PublicClientApplication` dosyasında depolanan yapılandırma bilgileri kullanılarak tek bir hesap oluşturulur `auth_config_single_account.json` .  Bu, MSAL kitaplığını tek-Account MSAL uygulamasında kullanılmak üzere nasıl başlataöğreneceksiniz:

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

' De `SingleAccountModeFragment.java` , bir kullanıcının oturum açması için kullanılacak kod `initializeUI()` , `signInButton` tıklama işleyicisinde ' de bulunur.

`signIn()`Belirteçleri edinmeyi denemeden önce çağırın. `signIn()`çağrıldı gibi davranır `acquireToken()` , kullanıcının oturum açması için etkileşimli bir istem ile sonuçlanır.

Kullanıcı oturumu açmak zaman uyumsuz bir işlemdir. Microsoft Graph API 'sini çağıran ve Kullanıcı oturum açtığında kullanıcı ARABIRIMINI güncelleştiren bir geri çağırma geçirilir:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Kullanıcı oturumunu kapatma

' De `SingleAccountModeFragment.java` , bir kullanıcının imzalanme kodu `initializeUI()` ' nda, `signOutButton` tıklama işleyicisinde.  Bir kullanıcının imzalanmasının zaman uyumsuz bir işlemdir. Kullanıcının imzalanması, bu hesabın belirteç önbelleğini de temizler. Kullanıcı hesabı oturumu açıldıktan sonra Kullanıcı ARABIRIMINI güncelleştirmek için bir geri çağırma oluşturulur:

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

Kullanıcıya en az sayıda istem sunmak için genellikle sessizce bir belirteç alırsınız. Daha sonra bir hata varsa, belirteci etkileşimli olarak almaya çalışın. Uygulama ilk kez çağırdığında, ' `signIn()` a bir çağrı görevi görür ve `acquireToken()` kullanıcıdan kimlik bilgilerini ister.

Kullanıcıdan kendi hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın istediği izinlere izin vermeniz istenebilir:

* Kullanıcı uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir
* Onay iptal edildiğinde
* Uygulamanız açıkça izin gerektiriyorsa
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

Kullanıcı arabirimini içeren UI ile etkileşimli bir belirteci almak için kullanılan kod, ' de, `SingleAccountModeFragment.java` `initializeUI()` ' ın `callGraphApiInteractiveButton` tıklama işleyicisidir:

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 **/
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Kullanıcı zaten oturum açmışsa, `acquireTokenSilentAsync()` `initializeUI()` ' ın tıklama işleyicisinde gösterildiği gibi uygulamaların belirteçleri sessizce istemesine izin verir `callGraphApiSilentButton` :

```java
/**
 * Once you've signed the user in,
 * you can perform acquireTokenSilent to obtain resources without interrupting the user.
 **/
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Hesap yükleme

Bir hesabın yükleneceği kod ' de bulunur `SingleAccountModeFragment.java` `loadAccount()` .  Kullanıcı hesabının yüklenmesi zaman uyumsuz bir işlemdir. bu nedenle, hesap yüklendiğinde, değiştiğinde veya hata oluştuğunda işlenecek geri çağrılar MSAL 'e geçirilir.  Aşağıdaki kod ayrıca `onAccountChanged()` , bir hesap kaldırıldığında, Kullanıcı başka bir hesaba değiştirdiğinde ve bu şekilde devam eden bir durum işler.

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

Bir Kullanıcı oturum açmışsa, Microsoft Graph çağrısı ' de tanımlanan bir HTTP isteği aracılığıyla yapılır `callGraphAPI()` `SingleAccountModeFragment.java` . Bu işlev, ' dan erişim belirtecini alma `authenticationResult` ve MSGraphRequestWrapper çağrısını paketleme ve çağrının sonuçlarını görüntüleme gibi bazı görevleri gerçekleştirerek örneği basitleştiren bir sarmalayıcıdır.

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

### <a name="auth_config_single_accountjson"></a>Üzerinde auth_config_single_account.js

Bu, tek bir hesabı kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Bu alanların bir açıklaması için bkz. [ANDROID msal yapılandırma dosyasını anlayın](msal-configuration.md) .

`"account_mode" : "SINGLE"`Bu uygulamayı tek bir hesap kullanacak şekilde yapılandıran, varlığını dikkate alın.

`"client_id"`, Microsoft 'un koruduğu bir uygulama nesnesi kaydını kullanacak şekilde önceden yapılandırılmıştır.
`"redirect_uri"`, kod örneğiyle birlikte sunulan imzalama anahtarını kullanacak şekilde önceden yapılandırılmıştır.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
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

Dosyasında, `MultipleAccountModeFragment.java` içinde `onCreateView()` depolanan yapılandırma bilgileri kullanılarak bir çoklu hesap uygulama nesnesi ( `IMultipleAccountPublicClientApplication` ) oluşturulur `auth_config_multiple_account.json file` :

```java
// Creates a PublicClientApplication object with res/raw/auth_config_multiple_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccounts();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

Oluşturulan `MultipleAccountPublicClientApplication` nesne, belirteçleri almak ve Kullanıcı hesabını yüklemek ve kaldırmak IÇIN msal kitaplığıyla etkileşim kurmak üzere kullanılabilmesi için bir sınıf üyesi değişkeninde depolanır.

#### <a name="load-an-account"></a>Hesap yükleme

Birden çok hesap uygulaması genellikle `getAccounts()` msal işlemleri için kullanılacak hesabı seçmek üzere çağrı yapılır. Bir hesabın yükleneceği kod, `MultipleAccountModeFragment.java` içindeki dosyasında bulunur `loadAccounts()` .  Kullanıcı hesabının yüklenmesi zaman uyumsuz bir işlemdir. Bu nedenle, bir geri arama, hesap yüklendiğinde, değiştiğinde veya bir hata oluştuğunda durumları işler.

```java
/**
 * Load currently signed-in accounts, if there's any.
 **/
private void loadAccounts() {
    if (mMultipleAccountApp == null) {
        return;
    }

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

Birden çok hesap uygulaması genellikle, kullanıcıyı içeren kullanıcı ARABIRIMI ile, ' a çağrı içeren belirteçleri etkileşimli olarak elde etmelidir `acquireToken()` .  Bir belirteci etkileşimli olarak almaya yönelik kod, `MultipleAccountModeFragment.java` içindeki dosyasında `initializeUI()` , `callGraphApiInteractiveButton` tıklama işleyicisinde bulunur:

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
 **/
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Uygulamalar, kullanıcının bir belirteç istediklerinde oturum açmasını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, `acquireTokenSilentAsync()` uygulamanın, dosyasında gösterildiği gibi, `MultipleAccountModeFragment.java` `initializeUI()` tıklama işleyicisindeki içinde gösterildiği gibi, kullanıcılara sormadan belirteç istemesine izin verir `callGraphApiSilentButton` :

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

Hesap için bir hesabı ve tüm önbelleğe alınmış belirteçleri kaldırma kodu, hesap `MultipleAccountModeFragment.java` `initializeUI()` kaldırma düğmesi için işleyicisindeki dosyasında bulunur. Bir hesabı kaldırabilmeniz için önce ve gibi MSAL metotlarından elde ettiğiniz bir hesap nesnesi gerekir `getAccounts()` `acquireToken()` . Bir hesabı kaldırmak zaman uyumsuz bir işlem olduğundan, `onRemoved` Kullanıcı arabirimini güncelleştirmek için geri çağırma sağlanır.

```java
/**
 * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
 **/
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccounts();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>Üzerinde auth_config_multiple_account.js

Bu, birden çok hesap kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Çeşitli alanların bir açıklaması için bkz. [ANDROID msal yapılandırma dosyasını anlayın](msal-configuration.md) .

Yapılandırma dosyasındaki [auth_config_single_account.js](#auth_config_single_accountjson) farklı olarak, bu yapılandırma dosyası `"account_mode" : "MULTIPLE"` bunun yerine `"account_mode" : "SINGLE"` birden çok hesap uygulaması olduğundan bunun yerine.

`"client_id"`, Microsoft 'un koruduğu bir uygulama nesnesi kaydını kullanacak şekilde önceden yapılandırılmıştır.
`"redirect_uri"`, kod örneğiyle birlikte sunulan imzalama anahtarını kullanacak şekilde önceden yapılandırılmıştır.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
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

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

[Oturum açma kullanıcılarını deneyin ve bir Android uygulaması öğreticiden Microsoft Graph çağırarak,](tutorial-v2-android.md) bir erişim belirteci alan ve Microsoft Graph API 'sini çağırmak için onu kullanan bir Android uygulaması oluşturmaya yönelik adım adım kılavuz.

> [!div class="nextstepaction"]
> [Çağrı Grafı API'si Android öğreticisi](./tutorial-v2-android.md)

### <a name="msal-for-android-library-wiki"></a>Android için MSAL kitaplığı wiki'si

Android için MSAL kitaplığı hakkındaki diğer yazıları okuyun:

> [!div class="nextstepaction"]
> [Android için MSAL kitaplığı wiki'si](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]