---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: eded2d6a9f2c270a2b3ccca296277b0a016733fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67189058"
---
1. Android Studio'da projeyi açın.

2. Android Studio'daki **Project Explorer'da** dosyayı `ToDoActivity.java` açın ve aşağıdaki alma bildirimlerini ekleyin:

    ```java
    import java.util.concurrent.ExecutionException;
    import java.util.concurrent.atomic.AtomicBoolean;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;

    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
    ```

3. **ToDoActivity** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
    public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

    private void authenticate() {
        // Sign in using the Google provider.
        mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the login request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // sign-in succeeded
                    createAndShowDialog(String.format("You are now signed in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
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

    Bu kod, Google kimlik doğrulama işlemini işlemek için bir yöntem oluşturur. İletişim kutusu, kimliği doğrulanan kullanıcının kimliğini görüntüler. Yalnızca başarılı bir kimlik doğrulama üzerinde devam edebilirsiniz.

    > [!NOTE]
    > Google dışında bir kimlik sağlayıcısı kullanıyorsanız, **oturum açma** yöntemine geçen değeri aşağıdaki değerlerden biriyle değiştirin: _MicrosoftAccount_, _Facebook_, _Twitter_, veya _windowsazureactivedirectory_.

4. **onCreate** yönteminde, `MobileServiceClient` nesneyi anında etkileyen koddan sonra aşağıdaki kod satırını ekleyin.

    ```java
    authenticate();
    ```

    Bu çağrı kimlik doğrulama işlemini başlatır.

5. `authenticate();` **onCreate** yönteminde kalan kodu yeni bir createTable yöntemine **taşıyın:**

    ```java
    private void createTable() {

        // Get the table instance to use.
        mToDoTable = mClient.getTable(ToDoItem.class);

        mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

        // Create an adapter to bind the items with the view.
        mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
        ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
        listViewToDo.setAdapter(mAdapter);

        // Load the items from Azure.
        refreshItemsFromTable();
    }
    ```

6. Yeniden yönlendirmenin beklendiği gibi çalıştığından `RedirectUrlActivity` emin olmak için `AndroidManifest.xml`aşağıdaki bölümü ekleyin:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}"
                android:host="easyauth.callback"/>
        </intent-filter>
    </activity>
    ```

7. Android `redirectUriScheme` `build.gradle` uygulamanıza ekleyin.

    ```gradle
    android {
        buildTypes {
            release {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
            debug {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
        }
    }
    ```

8. Sizin `com.android.support:customtabs:23.0.1` bağımlılıkları `build.gradle`ekleyin:

    ```gradle
    dependencies {
        // ...
        compile 'com.android.support:customtabs:23.0.1'
    }
    ```

9. **Çalıştır** menüsünden, uygulamayı başlatmak ve seçtiğiniz kimlik sağlayıcıyla oturum kurmak için **Çalıştır uygulamasını** tıklatın.

> [!WARNING]
> Belirtilen URL Düzeni büyük/küçük harf duyarlıdır. Tüm `{url_scheme_of_you_app}` kullanım durum larının aynı durumda olduğundan emin olun.

Başarılı bir şekilde oturum aştığınızda, uygulama hatasız çalışmalı ve arka uç hizmetini sorgulayıp verilerde güncellemeler yapabilmelisin.
