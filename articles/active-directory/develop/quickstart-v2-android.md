---
title: Microsoft kimlik platformu Android quickstart | Azure
description: Android uygulamalarının Microsoft kimlik platformu bitiş noktası tarafından erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin.
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
ms.openlocfilehash: c9e6c6a3eac3193ca503a23d523b78881f6a6738
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473826"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı başlangıç: Android uygulamasından kullanıcıların oturum açma ve Microsoft Graph API'sini çağırma

Bu hızlı başlangıç, bir Android uygulamasının Microsoft kimlik platformını kullanarak kişisel, iş veya okul hesaplarında nasıl oturum açabileceğini göstermek ve ardından bir erişim belirteci almak ve Microsoft Graph API'yi aramak için bir kod örneği kullanır. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)

Microsoft kimlik platformunun uygulamanız için belirteçler sağlayabilmesi için uygulamaların Azure Etkin Dizini'nde bir uygulama nesnesi tarafından temsil edilmesi gerekir.

> [!div renderon="docs"]
> Kolaylık sağlamak amacıyla, kod örneği, `redirect_uri` önce kendi `AndroidManifest.xml` uygulama nesnenizi kaydetmek zorunda kalmamak için dosyada önceden yapılandırılmış varsayılan bir varsayılan la birlikte gelir. A `redirect_uri` kısmen uygulamanızın imza anahtarına dayanır. Örnek proje, sağlanan `redirect_uri` projenin çalışması için bir imzalama anahtarıyla önceden yapılandırılmıştır. Bir uygulama nesnesini kaydetme ve uygulamanızla tümleştirme hakkında daha fazla bilgi edinmek için Oturum Aç kullanıcılarına bakın [ve bir Android uygulama öğreticisinden Microsoft Graph'ı arayın.](tutorial-v2-android.md)


> [!NOTE]
> **Ön koşullar**
> * Android Studio 
> * Android 16+

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Adım 1: Uygulamanızı Azure portalında yapılandırın 
>  Bu hızlı başlatmanın işe yaraması için Auth aracısıyla uyumlu bir yeniden yönlendirme URI eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-android/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış
>
> ### <a name="step-2-download-the-project"></a>2. Adım: Projeyi indirme 
> [!div class="sxs-lookup" renderon="portal"]
> Android Studio kullanarak proje çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip)
>
> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Adım 3: Uygulamanız yapılandırıldı ve çalışmaya hazır
> Projenizi uygulamanızın özellikleriyle yapılandırdık ve çalışmaya hazır. 
> Örnek uygulama Tek **Hesap Modu** ekranında başlar. Varsayılan kapsam olan **user.read,** varsayılan olarak sağlanır ve microsoft graph API çağrısı sırasında kendi profil verilerinizi okurken kullanılır. Microsoft Graph API çağrısının URL'si varsayılan olarak sağlanır. İstersen ikisini de değiştirebilirsin.
>
> ![Tek ve çoklu hesap kullanımını gösteren MSAL örnek uygulaması](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Tek ve birden çok hesap modu arasında geçiş yapmak için uygulama menüsünü kullanın.
>
> Tek hesap modunda, bir iş veya ev hesabı kullanarak oturum açın:
>
> 1. Kullanıcıdan kimlik bilgilerini almak için **grafik verilerini etkileşimli olarak al'ı** seçin. Aramadan Microsoft Graph API'ye yapılan çıktıyı ekranın alt kısmında görürsünüz.
> 2. Oturum açtıktan sonra, kullanıcıdan kimlik bilgilerini yeniden sormadan Microsoft Graph API'yi aramak için **grafik verilerini sessizce al'ı** seçin. Aramadan Microsoft Graph API'ye yapılan çıktıyı ekranın alt kısmında görürsünüz.
>
> Birden çok hesap modunda, aynı adımları yineleyebilirsiniz.  Ayrıca, oturum açmış hesabı kaldırabilirsiniz ve bu hesap için önbelleğe alınmış belirteçleri de kaldırabilirsiniz.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> ## <a name="step-1-get-the-sample-app"></a>Adım 1: Örnek uygulamayı alın
>
> [Kodu indirin.](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip)
>
> ## <a name="step-2-run-the-sample-app"></a>Adım 2: Örnek uygulamayı çalıştırın
>
> Android Studio'nun **mevcut cihazlarından** emülatörünüzü veya fiziksel cihazınızı seçin ve uygulamayı çalıştırın.
>
> Örnek uygulama Tek **Hesap Modu** ekranında başlar. Varsayılan kapsam olan **user.read,** varsayılan olarak sağlanır ve microsoft graph API çağrısı sırasında kendi profil verilerinizi okurken kullanılır. Microsoft Graph API çağrısının URL'si varsayılan olarak sağlanır. İstersen ikisini de değiştirebilirsin.
>
> ![Tek ve çoklu hesap kullanımını gösteren MSAL örnek uygulaması](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Tek ve birden çok hesap modu arasında geçiş yapmak için uygulama menüsünü kullanın.
>
> Tek hesap modunda, bir iş veya ev hesabı kullanarak oturum açın:
>
> 1. Kullanıcıdan kimlik bilgilerini almak için **grafik verilerini etkileşimli olarak al'ı** seçin. Aramadan Microsoft Graph API'ye yapılan çıktıyı ekranın alt kısmında görürsünüz.
> 2. Oturum açtıktan sonra, kullanıcıdan kimlik bilgilerini yeniden sormadan Microsoft Graph API'yi aramak için **grafik verilerini sessizce al'ı** seçin. Aramadan Microsoft Graph API'ye yapılan çıktıyı ekranın alt kısmında görürsünüz.
>
> Birden çok hesap modunda, aynı adımları yineleyebilirsiniz.  Ayrıca, oturum açmış hesabı kaldırabilirsiniz ve bu hesap için önbelleğe alınmış belirteçleri de kaldırabilirsiniz.

## <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Örnek uygulamanın ekran görüntüsü](media/quickstart-v2-android/android-intro.svg)


Kod, tek ve birden çok hesap MSAL uygulamasının nasıl yazılabildiğini gösteren parçalar halinde düzenlenir. Kod dosyaları aşağıdaki gibi düzenlenmiştir:

| Dosya  | Gösteriler  |
|---------|---------|
| Ana Aktivite | UI'yi yönetir |
| MSGraphRequestWrapper  | MSAL tarafından sağlanan belirteci kullanarak Microsoft Graph API'yi çağırır |
| MultipleAccountModeFragment  | Çok hesaplı bir uygulamayı başlatir, kullanıcı hesabı yükler ve Microsoft Graph API'yi aramak için bir belirteç alır |
| SingleAccountModeFragment | Tek hesaplı bir uygulamayı başlatir, kullanıcı hesabı yükler ve Microsoft Graph API'yi aramak için bir belirteç alır |
| res/auth_config_multiple_account.json  | Çoklu hesap yapılandırma dosyası |
| res/auth_config_single_account.json  | Tek hesap yapılandırma dosyası |
| Gradle Scripts/build.grade (Modül:app) | MSAL kitaplık bağımlılıkları buraya eklenir |

Şimdi bu dosyalara daha ayrıntılı olarak bakacağız ve her birinde MSAL'a özgü kodu çağıracağız.

### <a name="adding-msal-to-the-app"></a>Uygulamaya MSAL ekleme

MSAL ([com.microsoft.identity.client),](https://javadoc.io/doc/com.microsoft.identity.client/msal)microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan kullanıcı ve istek belirteçleri oturum açmada kullanılan kitaplıktır. Gradle 3.0+ **Aşağıdakileri** **Gradle Scripts** > **build.gradle(Modül: uygulama)** bağımlılıklar altında eklediğinizde kitaplığı yükler:

```gradle  
implementation 'com.microsoft.identity.client:msal:1.+'
```

Bunu build.gradle (Modül: uygulama) örnek projede görebilirsiniz:

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.+'
    ...
}
```

Bu, Gradle'a Maven Central'dan MSAL'ı indirmesini ve oluşturmasını sağlar.

### <a name="msal-imports"></a>MSAL ithalatı

MSAL kitaplığı ile ilgili içeri `com.microsoft.identity.client.*`aktarma.  Örneğin, ortak istemci `import com.microsoft.identity.client.PublicClientApplication;` uygulamanızı temsil eden `PublicClientApplication` sınıfın ad alanının hangisi olduğunu görürsünüz.

### <a name="singleaccountmodefragmentjava"></a>SingleAccountModeFragment.java

Bu dosya, tek bir hesap MSAL uygulaması nın nasıl oluşturulup microsoft grafiği API'sini nasıl çağırılabildiğini gösterir.

Tek hesap uygulamaları yalnızca tek bir kullanıcı tarafından kullanılır.  Örneğin, haritauygulamanızda oturum açabileceğiniz tek bir hesabınız olabilir.

#### <a name="single-account-msal-initialization"></a>Tek hesap MSAL başlatma

Dosyada `auth_config_single_account.json` `onCreateView()` `auth_config_single_account.json` depolanan config bilgileri kullanılarak tek bir hesap `PublicClientApplication` oluşturulur.  MSAL kitaplığını tek hesaplı bir MSAL uygulamasında kullanılmak üzere bu şekilde başlatmanız dır:

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

#### <a name="sign-in-a-user"></a>Kullanıcıda oturum açma

Kullanıcıda `SingleAccountModeFragment.java`oturum açılabilmek için `initializeUI()`kod, `signInButton` tıklama işleyicisinde.

Belirteçleri elde etmeye çalışmadan önce arayın. `signIn()` `signIn()`çağrılır gibi, `acquireToken()` kullanıcının oturum açması için etkileşimli bir istem le sonuçlanır.

Kullanıcıda oturum açma, eşzamanlı bir işlemdir. Microsoft Graph API'yi çağıran ve kullanıcı aşağıdakileri imzaladığında Kullanıcı Arabirimi'ni güncelleştiren bir geri arama geçirilir:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Bir kullanıcıyı oturum aç

Kullanıcıyı `SingleAccountModeFragment.java`oturum alabilmek için `initializeUI()`kod, `signOutButton` tıklama işleyicisinde.  Bir kullanıcıyı dışarı atamak eşzamanlı bir işlemdir. Kullanıcıyı imzalamak, bu hesabın belirteç önbelleğini de temizler. Kullanıcı hesabı oturum dışı çıktıktan sonra Kullanıcı Arabirimi'ni güncelleştirmek için bir geri arama oluşturulur:

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

#### <a name="get-a-token-interactively-or-silently"></a>Etkileşimli veya sessizce bir belirteç alın

Kullanıcıya en az sayıda istem sunmak için genellikle sessizce bir belirteç alırsınız. Ardından, bir hata varsa, etkileşimli olarak belirteç almaya çalış. Uygulama ilk kez `signIn()`aradığında, kullanıcıyı kimlik `acquireToken()`bilgileri için harekete geçecek olan bir çağrı görevi görür.

Kullanıcıdan hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın istediği izinleri kabul etmesi istenebileceği durumlar şunlardır:

* Kullanıcı uygulamaya ilk kez girdiği zaman
* Bir kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir
* İzin iptal edilirse
* Uygulamanız açıkça onay gerektiriyorsa
* Uygulamanız ilk kez bir kaynağa erişim istediğinde
* MFA veya diğer Koşullu Erişim ilkeleri gerektiğinde

Kullanıcıyı içerecek kullanıcıyı içeren Kullanıcı UI ile etkileşimli olarak bir `SingleAccountModeFragment.java`belirteç elde etmek için `initializeUI()`kod, içinde , tıklama işleyicisi `callGraphApiInteractiveButton` içinde:

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

Kullanıcı zaten oturum açmışsa, `acquireTokenSilentAsync()` uygulamaların tıklama işleyicisinde gösterildiği `initializeUI()`gibi `callGraphApiSilentButton` sessizce belirteçler istemesine izin verir:

```java
/**
 * Once you've signed the user in,
 * you can perform acquireTokenSilent to obtain resources without interrupting the user.
 **/
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Hesap yükleme

Bir hesabı yüklemek için `SingleAccountModeFragment.java` kod `loadAccount()`içinde.  Kullanıcının hesabını yüklemek eşzamanlı bir işlemdir, bu nedenle hesap yüklendiğinde, değiştiğinde veya bir hata oluştuğunda işleyeceğimiz geri aramalar MSAL'a aktarılır.  Aşağıdaki kod, bir `onAccountChanged()`hesap kaldırıldığında ortaya çıkan, kullanıcının başka bir hesaba değiştiği ve benzeri kuralları da işler.

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

#### <a name="call-microsoft-graph"></a>Microsoft Graph'ı arayın

Bir kullanıcı oturum açtığında, Microsoft Graph'a yapılan arama, `callGraphAPI()` 'de `SingleAccountModeFragment.java`tanımlanan bir HTTP isteği aracılığıyla yapılır. Bu işlev, erişim belirteci alma `authenticationResult` ve msgraphRequestWrapper için arama paketleme ve arama sonuçlarını görüntüleme gibi bazı görevleri yaparak örnek basitleştirir bir sarmalayıcıdır.

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

### <a name="auth_config_single_accountjson"></a>auth_config_single_account.json

Bu, tek bir hesap kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Bu alanların açıklaması için [Android MSAL yapılandırma dosyasını anlayın.](msal-configuration.md)

Bu uygulamayı `"account_mode" : "SINGLE"`tek bir hesap kullanacak şekilde yapılandıran varlığına dikkat edin.

`"client_id"`Microsoft'un koruduğu bir uygulama nesnesi kaydını kullanmak için önceden yapılandırılmıştır.
`"redirect_uri"`kod örneği ile sağlanan imzalama anahtarını kullanmak için önceden yapılandırılmıştır.

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

### <a name="multipleaccountmodefragmentjava"></a>MultipleAccountModeFragment.java

Bu dosya, birden çok hesaplı MSAL uygulamasının nasıl oluşturulup Microsoft Graph API'sini nasıl çağırılabildiğini gösterir.

Birden çok hesap uygulamasına örnek olarak, iş hesabı ve kişisel hesap gibi birden çok kullanıcı hesabıyla çalışmanızı sağlayan bir posta uygulaması örneğidir.

#### <a name="multiple-account-msal-initialization"></a>Çoklu hesap MSAL başlatma

Dosyada, `MultipleAccountModeFragment.java` `onCreateView()`içinde, birden çok hesap`IMultipleAccountPublicClientApplication`uygulaması nesnesi ( ) içinde `auth_config_multiple_account.json file`depolanan config bilgileri kullanılarak oluşturulur:

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

Oluşturulan `MultipleAccountPublicClientApplication` nesne, jeton elde etmek ve kullanıcı hesabını yüklemek ve kaldırmak için MSAL kitaplığıyla etkileşimde kullanılmak üzere bir sınıf üye değişkeninde depolanır.

#### <a name="load-an-account"></a>Hesap yükleme

Birden çok hesap `getAccounts()` uygulaması genellikle MSAL işlemleri için kullanılacak hesabı seçmek için çağrı yapar. Bir hesabı yüklemek için kod `MultipleAccountModeFragment.java` dosyada, `loadAccounts()`içinde.  Kullanıcının hesabını yüklemek eşzamanlı bir işlemdir. Bu nedenle, geri arama, hesabın yüklendiği, değiştiği veya bir hata oluştuğu durumları işler.

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

#### <a name="get-a-token-interactively-or-silently"></a>Etkileşimli veya sessizce bir belirteç alın

Kullanıcıdan hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın istediği izinleri kabul etmesi istenebileceği durumlar şunlardır:

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Bir kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir 
* İzin iptal edilirse 
* Uygulamanız açıkça onay gerektiriyorsa 
* Uygulamanız ilk kez bir kaynağa erişim istediğinde
* MFA veya diğer Koşullu Erişim ilkeleri gerektiğinde

Birden çok hesap uygulaması genellikle etkileşimli olarak belirteçleri elde etmelidir, bu `acquireToken()`kullanıcı içeren Kullanıcı Arabirimi ile, bir çağrı ile .  Bir belirteç etkileşimli olarak almak `MultipleAccountModeFragment.java` için `initializeUI()`kod, `callGraphApiInteractiveButton` tıklama işleyicisi içinde dosya da:

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

Uygulamalar, kullanıcının her jeton talebinde oturum açmasını gerektirmemelidir. Kullanıcı `acquireTokenSilentAsync()` zaten oturum açmışsa, uygulamaların `MultipleAccountModeFragment.java` dosyada gösterildiği gibi kullanıcıdan tıklama işleyicisinde`initializeUI()` `callGraphApiSilentButton` belirteçleri istemeden istekte bulunmasına izin verir:

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

#### <a name="remove-an-account"></a>Hesabı kaldırma

Hesabı kaldırmak için kod ve hesabın önbelleğe alınmış belirteçleri, `initializeUI()` hesabı kaldır düğmesinin işleyicisindeki `MultipleAccountModeFragment.java` dosyada dır. Bir hesabı kaldırmadan önce, MSAL gibi yöntemlerden elde ettiğiniz `getAccounts()` `acquireToken()`bir hesap nesnesi gerekir. Bir hesabı kaldırmak eşzamanlı bir işlem olduğundan, `onRemoved` geri arama Kullanıcı Arabirimi'ni güncelleştirmek için sağlanır.

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

### <a name="auth_config_multiple_accountjson"></a>auth_config_multiple_account.json

Bu, birden çok hesap kullanan bir MSAL uygulamasının yapılandırma dosyasıdır.

Çeşitli alanların açıklaması için [Android MSAL yapılandırma dosyasını anlayın.](msal-configuration.md)

[auth_config_single_account.json](#auth_config_single_accountjson) yapılandırma dosyasının aksine, bu `"account_mode" : "MULTIPLE"` config dosyası birden çok hesap uygulaması olduğu için yerine `"account_mode" : "SINGLE"` sahiptir.

`"client_id"`Microsoft'un koruduğu bir uygulama nesnesi kaydını kullanmak için önceden yapılandırılmıştır.
`"redirect_uri"`kod örneği ile sağlanan imzalama anahtarını kullanmak için önceden yapılandırılmıştır.

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

Oturum Açma kullanıcılarını deneyin ve erişim jetonuna sahip bir Android uygulaması oluşturmak için adım adım kılavuz için [bir Android uygulaması öğreticisinden Microsoft](tutorial-v2-android.md) Graph'ı arayın ve microsoft graph API'yi aramak için kullanır.

> [!div class="nextstepaction"]
> [Çağrı Grafı API'si Android öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Android için MSAL kitaplığı wiki'si

Android için MSAL kitaplığı hakkındaki diğer yazıları okuyun:

> [!div class="nextstepaction"]
> [Android için MSAL kitaplığı wiki'si](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
