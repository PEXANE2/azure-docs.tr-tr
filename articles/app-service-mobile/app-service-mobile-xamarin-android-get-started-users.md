---
title: Xamarin Android'de kimlik doğrulamaya başlayın
description: Xamarin Android uygulamanızın kullanıcılarını AAD, Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla doğrulamak için Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: fa70b7419e1877ab2daba49ad154cdfd5a8d2cba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458962"
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Genel Bakış
Bu konu, bir Mobil Uygulamanın kullanıcılarını istemci uygulamanızdan nasıl doğruladığınızı gösterir. Bu eğitimde, Azure Mobile Apps tarafından desteklenen bir kimlik sağlayıcısını kullanarak hızlı başlatma projesine kimlik doğrulama eklersiniz. Mobil Uygulama'da başarılı bir şekilde kimlik doğrulaması ve yetkilendirildikten sonra kullanıcı kimliği değeri görüntülenir.

Bu öğretici Mobil Uygulama quickstart dayanmaktadır. Ayrıca ilk öğretici [bir Xamarin.Android uygulaması oluşturun]tamamlamak gerekir. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, kimlik doğrulama uzantısı paketini projenize eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><a name="register"></a>Uygulamanızı kimlik doğrulama ve uygulama hizmetlerini yapılandırmak için kaydedin
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><a name="redirecturl"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir. Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır. Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız. Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz. Mobil uygulamanıza özgü olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalında], Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `url_scheme_of_your_app://easyauth.callback`  Bu dizedeki **url_scheme_of_your_app,** mobil uygulamanızın URL Düzenidir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

4. **Tamam**'a tıklayın.

5. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a><a name="permissions"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studio veya Xamarin Studio'da, istemci projesini bir aygıt veya emülatör üzerinde çalıştırın. Uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durum ortaya çıkın. Bu durum, uygulamanın Gerçek leştirilmiş bir kullanıcı olarak Mobil Uygulamanızın arka ucuna erişmeye çalıştığı için gerçekleşir. *TodoItem* tablosu artık kimlik doğrulaması gerektirir.

Ardından, kimlik doğrulaması yapılan bir kullanıcıyla Mobil Uygulama arka ucundan kaynak istemek için istemci uygulamasını güncellersiniz.

## <a name="add-authentication-to-the-app"></a><a name="add-authentication"></a>Uygulamaya kimlik doğrulama ekleme
Uygulama, kullanıcıların **Oturum Aç** düğmesine dokunmalarını ve veriler görüntülenmeden önce kimlik doğrulamasını gerektirecek şekilde güncelleştirilir.

1. **TodoActivity** sınıfına aşağıdaki kodu ekleyin:
   
        // Define an authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    Bu, bir kullanıcının kimliğini doğrulamak için yeni bir yöntem ve yeni bir **Oturum Açma** düğmesi için bir yöntem işleyicisi oluşturur. Yukarıdaki örnek koddaki kullanıcı, Facebook girişi kullanılarak kimlik doğrulanır. Kimlik doğrulaması alındıktan sonra kullanıcı kimliğini görüntülemek için bir iletişim kutusu kullanılır.
   
   > [!NOTE]
   > Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, **loginAsync'e** aktarılan değeri aşağıdakilerden biriyle değiştirin: *MicrosoftAccount*, *Twitter*, *Google*, veya *WindowsAzureActiveDirectory*.
   > 
   > 
2. **OnCreate** yönteminde, aşağıdaki kod satırını silin veya yorumlayın:
   
        OnRefreshItemsSelected ();
3. Activity_To_Do.axml dosyasında, varolan *AddItem* düğmesinden önce aşağıdaki *LoginUser* düğmesi tanımını ekleyin:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Strings.xml kaynakları dosyasına aşağıdaki öğeyi ekleyin:
   
        <string name="login_button_text">Sign in</string>
5. AndroidManifest.xml dosyasını açın, XML `<application>` öğesinin içine aşağıdaki kodu ekleyin:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. Visual Studio veya Xamarin Studio'da, istemci projesini bir aygıt veya emülatör üzerinde çalıştırın ve seçtiğiniz kimlik sağlayıcıyla oturum açın. Başarılı bir şekilde oturum açtığınızda, uygulama giriş kimliğinizi ve todo öğelerinin listesini görüntüler ve verilerde güncellemeler yapabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

**Uygulama ile çöktü`Java.Lang.NoSuchMethodError: No static method startActivity`**

Bazı durumlarda, destek paketlerindeki çakışmalar Visual stüdyosunda sadece bir uyarı olarak görüntülenir, ancak uygulama çalışma zamanında bu özel durumla birlikte çöker. Bu durumda, projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olduğundan emin olmanız gerekir. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

<!-- URLs. -->
[Bir Xamarin.Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
