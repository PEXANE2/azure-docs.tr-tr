---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: deb94cab97bd9a402676cdc5c0239da8d07ed8b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67189059"
---
Önceki örnekte, istemcinin uygulama her başlatınca hem kimlik sağlayıcısına hem de arka uçAzure hizmetiyle iletişim kurmasını gerektiren standart bir oturum açma gösterilmektedir. Bu yöntem verimsizdir ve birçok müşteri uygulamanızı aynı anda başlatmaya çalışırsa kullanımla ilgili sorunlarla karşılayabilirsiniz. Daha iyi bir yaklaşım, Azure hizmeti tarafından döndürülen yetkilendirme belirteci önbelleğe almak ve sağlayıcı tabanlı oturum açma kullanmadan önce bunu kullanmaya çalışmaktır.

> [!NOTE]
> İstemci tarafından yönetilen veya hizmet tarafından yönetilen kimlik doğrulaması kullanıp kullanmadığınıza bakılmaksızın, arka uç Azure hizmeti tarafından verilen belirteci önbelleğe alabilirsiniz. Bu öğretici, hizmet tarafından yönetilen kimlik doğrulaması kullanır.
>
>

1. ToDoActivity.java dosyasını açın ve aşağıdaki alma ekstrelerini ekleyin:

    ```java
    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;
    ```

2. Aşağıdaki üyeleri sınıfa `ToDoActivity` ekleyin.

    ```java
    public static final String SHAREDPREFFILE = "temp";
    public static final String USERIDPREF = "uid";
    public static final String TOKENPREF = "tkn";
    ```

3. ToDoActivity.java dosyasında, `cacheUserToken` yöntem için aşağıdaki tanımı ekleyin.

    ```java
    private void cacheUserToken(MobileServiceUser user)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        Editor editor = prefs.edit();
        editor.putString(USERIDPREF, user.getUserId());
        editor.putString(TOKENPREF, user.getAuthenticationToken());
        editor.commit();
    }
    ```

    Bu yöntem, kullanıcı kimliğini ve belirteci özel olarak işaretlenmiş bir tercih dosyasında depolar. Bu, aygıttaki diğer uygulamaların belirtemeye erişimi olmaması için önbelleğe erişimi korumalıdır. Uygulama için tercih sandboxed. Ancak, birisi aygıta erişirse, belirteç önbelleğine başka yollarla erişebilmek mümkündür.

   > [!NOTE]
   > Belirteci şifreleme yle daha da koruyabilirsiniz, verilerinize giriş belirteç erişimi son derece hassas kabul edilirse ve birisi aygıta erişebilirse. Ancak tamamen güvenli bir çözüm bu öğreticinin kapsamı dışındadır ve güvenlik gereksinimlerinize bağlıdır.
   >
   >

4. ToDoActivity.java dosyasında, `loadUserTokenCache` yöntem için aşağıdaki tanımı ekleyin.

    ```java
    private boolean loadUserTokenCache(MobileServiceClient client)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        String userId = prefs.getString(USERIDPREF, null);
        if (userId == null)
            return false;
        String token = prefs.getString(TOKENPREF, null);
        if (token == null)
            return false;

        MobileServiceUser user = new MobileServiceUser(userId);
        user.setAuthenticationToken(token);
        client.setCurrentUser(user);

        return true;
    }
    ```

5. *ToDoActivity.java* dosyasında, belirteç önbelleği kullanan aşağıdakilerle `authenticate` ve `onActivityResult` yöntemleri değiştirin. Google dışında bir hesap kullanmak istiyorsanız oturum açma sağlayıcısını değiştirin.

    ```java
    private void authenticate() {
        // We first try to load a token cache if one exists.
        if (loadUserTokenCache(mClient))
        {
            createTable();
        }
        // If we failed to load a token cache, sign in and create a token cache
        else
        {
            // Sign in using the Google provider.
            mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the sign-in request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // sign-in succeeded
                    createAndShowDialog(String.format("You are now signed in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    cacheUserToken(mClient.getCurrentUser());
                    createTable();
                } else {
                    // sign-in failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

6. Geçerli bir hesap kullanarak uygulamayı oluşturun ve kimlik doğrulamasını test edin. En az iki kez çalıştırın. İlk çalıştırma sırasında, oturum açma ve belirteç önbelleği oluşturma istemleri almanız gerekir. Bundan sonra, her çalıştırma kimlik doğrulaması için belirteç önbelleğini yüklemeye çalışır. Oturum açmanız gerekmez.
