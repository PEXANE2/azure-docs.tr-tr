---
title: Android ile çalışmaya başlama-Microsoft Identity platform | Mavisi
description: Android uygulaması, bir erişim belirteci alabilir ve Microsoft Identity platform 'dan erişim belirteçleri gerektiren API 'Leri veya API 'Leri çağırabilir Microsoft Graph.
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/09/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: a53a0d5ea8405c116d0286d3b67b1640f98ed96d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852454"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Kullanıcıları oturum açın ve Android uygulamasından Microsoft Graph çağırın

Bu öğreticide, bir Android uygulamasını Microsoft Identity platformu ile tümleştirmeyi öğreneceksiniz. Uygulamanız bir kullanıcıya oturum açacaktır, Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır ve Microsoft Graph API 'sine bir istek yapar.  

Kılavuzu tamamladığınızda, uygulamanız kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) ve Azure Active Directory kullanan herhangi bir şirketten veya kuruluştan iş veya okul hesapları için oturum açma işlemlerini kabul eder.

## <a name="how-this-tutorial-works"></a>Bu öğreticinin nasıl çalıştığı

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

Bu öğreticideki uygulama, kullanıcıların oturum açmasını ve adına veri almasını sağlayacaktır.  Bu verilere, yetkilendirme gerektiren ve Microsoft Identity platform tarafından korunan korumalı bir API (Microsoft Graph API) üzerinden erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanız kullanıcı tarafından bir tarayıcı veya Microsoft Authenticator ile Intune Şirket Portalı oturum açacaktır.
* Son Kullanıcı, uygulamanızın istediği izinleri kabul eder.
* Uygulamanıza Microsoft Graph API 'SI için bir erişim belirteci verilmeyecektir.
* Erişim belirteci, Web API 'sine HTTP isteğine dahil edilir.
* Microsoft Graph yanıtını işleyin.

Bu örnek, kimlik doğrulamasını uygulamak için Android için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanır: [com. Microsoft. Identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal).

 MSAL, belirteçleri otomatik olarak yenileyecek, cihazdaki diğer uygulamalar arasında çoklu oturum açma (SSO) sunacaktır ve hesapları yönetir.

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticide Android Studio 16 veya üzeri bir sürüm gerekir (19 + önerilir).

## <a name="create-a-project"></a>Proje oluşturma

Bu öğretici, yeni bir proje oluşturur. Bunun yerine tamamlanan öğreticiyi indirmek isterseniz [kodu indirin](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip).

1. Android Studio açın ve **Yeni bir Android Studio projesi Başlat**' ı seçin.
2. **Temel etkinlik** ' i seçin ve **İleri ' yi**seçin.
3. Uygulamanızı adlandırın.
4. Paket adını kaydedin. Daha sonra Azure portal buraya girersiniz.
5. **En düşük API düzeyini** **api 19** veya üzeri olarak ayarlayın ve **son**' a tıklayın.
6. Proje görünümünde, açılan listeden **Proje** ' yi seçerek kaynak ve kaynak olmayan proje dosyalarını görüntüleyin, **App/Build. Gradle** dosyasını açın ve olarak `targetSdkVersion` `27`ayarlayın.

## <a name="register-your-application"></a>Uygulamanızı kaydedin

1. [Azure Portal](https://aka.ms/MobileAppReg) gidin.
2. [Uygulama kayıtları dikey penceresini](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) açın ve **+ Yeni kayıt**' ye tıklayın.
3. Uygulamanız için bir **ad** girin ve yeniden yönlendirme URI 'Sini ayarlamadan **Kaydet**' e tıklayın.
4. Görüntülenen bölmenin **Yönet** bölümünde **kimlik doğrulama** >  **+ Platform** > **Android**Ekle ' yi seçin.
5. Projenizin paket adını girin. Kodu indirdiyseniz, bu değer olur `com.azuresamples.msalandroidapp`.
6. **Android uygulamanızı yapılandırma** sayfanızın **imza karması** bölümünde, **bir geliştirme imza karması oluşturma** ' ya tıklayın. ve platformunuz için kullanmak üzere KeyTool komutunu kopyalayın.

   > [!Note]
   > KeyTool. exe, Java Development Kit 'in (JDK) bir parçası olarak yüklenir. Ayrıca, KeyTool komutunu yürütmek için OpenSSL aracını da yüklemelisiniz.

7. KeyTool tarafından oluşturulan **imza karmasını** girin.
8. Uygulamanızı `Configure` daha sonra yapılandırırken girebilmek için **Android yapılandırma** sayfasında görüntülenen **msal yapılandırmasını** tıklatın ve kaydedin.  **Bitti**’ye tıklayın.

## <a name="build-your-app"></a>Uygulamanızı oluşturun

### <a name="add-your-app-registration"></a>Uygulama kaydınızı ekleyin

1. Android Studio projesi bölmesinde **app\src\mainres dizinine**gidin.
2. **Kay** ' a sağ tıklayın ve **Yeni** > **Dizin**' i seçin. Yeni `raw` dizin adı olarak girin ve **Tamam**' a tıklayın.
3. **App**src res RAW bölümünde, adlı`auth_config.json` yeni bir JSON dosyası oluşturun ve daha önce kaydettiğiniz msal yapılandırmasını yapıştırın. >  >  >  [Daha fazla bilgi için bkz. msal Configuration](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
4. **App**src Main AndroidManifest. xml dosyasında aşağıdaki etkinliği`BrowserTabActivity` ekleyin. >  >  >  Bu giriş, Microsoft 'un, kimlik doğrulamasını tamamladıktan sonra uygulamanıza geri çağırmasını sağlar:

    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
        </intent-filter>
    </activity>
    ```

    `android:host=` Değer için Azure Portal kaydettiğiniz paket adını değiştirin.
    `android:path=` Değer için Azure Portal kaydettiğiniz anahtar karmasını değiştirin. Imza karması URL kodlamalı olmamalıdır.

5. **AndroidManifest. xml**içinde, `<application>` etiketinin hemen üzerinde aşağıdaki izinleri ekleyin:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

### <a name="create-the-apps-ui"></a>Uygulamanın kullanıcı arabirimini oluşturma

1. Android Studio projesi penceresinde, **App** > **src** > **ana** > **res** düzeni ' ne gidin ve activity_main. xml ' i açın ve metni açın > görüntüleyin.
2. Etkinlik yerleşimini değiştirin, örneğin: `<androidx.coordinatorlayout.widget.CoordinatorLayout`. `<androidx.coordinatorlayout.widget.LinearLayout`
3. `android:orientation="vertical"` Özelliği`LinearLayout` düğümüne ekleyin.
4. Aşağıdaki kodu `LinearLayout` düğüme yapıştırın ve geçerli içeriği değiştirin:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

### <a name="add-msal-to-your-project"></a>Projenize MSAL ekleyin

1. Android Studio projesi penceresinde, **App** > **src** > **Build. Gradle**öğesine gidin.
2. **Bağımlılıklar**altında aşağıdakileri yapıştırın:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3.+'
    ```

### <a name="use-msal"></a>MSAL kullanma

Şimdi uygulamanızda msal eklemek `MainActivity.java` ve kullanmak için değişiklikler yapın.
Android Studio projesi penceresinde, **App** > **src** >  `MainActivity.java`**Main** > Javacom >  **. example. msal**' a gidin ve açın.

#### <a name="required-imports"></a>Gerekli içeri aktarmalar

Aşağıdaki içeri aktarmaları en üst `MainActivity.java`tarafına ekleyin:

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
import com.microsoft.identity.client.exception.*;
```

#### <a name="instantiate-msal"></a>MSAL örneği oluştur

`MainActivity` Sınıfının içinde, erişmek istediğimiz kapsamları ve Web API 'sini de içerecek şekilde uygulamanın ne yapacaklarıyla ilgili birkaç yapılandırmaya sahip msal örneği oluşturmanız gerekir.

Aşağıdaki değişkenleri `MainActivity` sınıfının içine kopyalayın:

```java
final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

/* UI & Debugging Variables */
private static final String TAG = MainActivity.class.getSimpleName();
Button callGraphButton;
Button signOutButton;

/* Azure AD Variables */
private PublicClientApplication sampleApp;
private IAuthenticationResult authResult;
```

MSAL örneğini oluşturmak için `onCreate()` içeriğini aşağıdaki kodla değiştirin:

```java
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

callGraphButton = (Button) findViewById(R.id.callGraph);
signOutButton = (Button) findViewById(R.id.clearCache);

callGraphButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onCallGraphClicked();
    }
});

signOutButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onSignOutClicked();
    }
});

/* Configure your sample app and save state for this activity */
sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);

/* Attempt to get a user and acquireTokenSilent */
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {
        if (!accounts.isEmpty()) {
            /* This sample doesn't support multi-account scenarios, use the first account */
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts */
        }
    }
});
```

Yukarıdaki kod, uygulamanızda uygulamanızı `getAccounts()` açtıklarında sessizce oturum açmaya çalışır ve `acquireTokenSilentAsync()`başarılı olursa.  Sonraki birkaç bölümde, oturum açmış bir hesap olmadığı için geri çağırma işleyicisini uygulayacağız.

#### <a name="use-msal-to-get-tokens"></a>Belirteçleri almak için MSAL kullanma

Şimdi, MSAL aracılığıyla uygulamanın kullanıcı arabirimi işleme mantığını uygulayabilir ve belirteçleri etkileşimli olarak elde edebilirsiniz.

MSAL belirteçleri almak için iki birincil yöntem sunar: `acquireTokenSilentAsync()` ve `acquireToken()`.  

`acquireTokenSilentAsync()`bir hesap mevcutsa Kullanıcı etkileşimi olmadan bir kullanıcıya oturum açar ve belirteçleri alın. Başarılı olursa, MSAL belirteçleri uygulamanıza iletilecektir. Bu işlem başarısız olursa, bir `MsalUiRequiredException`oluşturacaktır.  Bu özel durum oluşturulursa veya kullanıcının etkileşimli oturum açma deneyimine sahip olmasını istiyorsanız (kimlik bilgileri, MFA veya diğer koşullu erişim ilkeleri olabilir veya gerekli olmayabilir), ardından kullanın `acquireToken()`.  

`acquireToken()`Kullanıcı oturum açmaya çalışırken ve belirteçleri alırken Kullanıcı ARABIRIMINI görüntüler. Ancak etkileşimli-SSO deneyimi sağlamak için tarayıcıda oturum tanımlama bilgilerini veya Microsoft Authenticator içindeki bir hesabı kullanabilir.

`MainActivity` Sınıfının içinde aşağıdaki üç UI yöntemini oluşturun:

```java
/* Set the UI for successful token acquisition data */
private void updateSuccessUI() {
    callGraphButton.setVisibility(View.INVISIBLE);
    signOutButton.setVisibility(View.VISIBLE);
    findViewById(R.id.welcome).setVisibility(View.VISIBLE);
    ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
            authResult.getAccount().getUsername());
    findViewById(R.id.graphData).setVisibility(View.VISIBLE);
}

/* Set the UI for signed out account */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");

    Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
            .show();
}

/* Use MSAL to acquireToken for the end-user
 * Callback will call Graph api w/ access token & update UI
 */
private void onCallGraphClicked() {
    sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
}
```

Geçerli etkinliği almak ve sessiz & etkileşimli geri çağırmaları işlemek için aşağıdaki yöntemleri ekleyin:

```java
public Activity getActivity() {
    return this;
}

/* Callback used in for silent acquireToken calls.
 * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
 * else errors that we need to do an interactive request.
 */
private AuthenticationCallback getAuthSilentCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}

/* Callback used for interactive request.  If succeeds we use the access
 * token to call the Microsoft Graph. Does not check cache
 */
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");
            Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

#### <a name="use-msal-for-sign-out"></a>Oturumu kapatma için MSAL kullanma

Ardından, oturum kapatma desteği ekleyin.

> [!Important]
> MSAL ile oturum açmak, bir kullanıcıyla ilgili tüm bilinen bilgileri uygulamadan kaldırır, ancak kullanıcının cihazında etkin bir oturumu olmaya devam edecektir. Kullanıcı yeniden oturum açmayı denerse, oturum açma kullanıcı arabirimini görebilir, ancak cihaz oturumu hala etkin olduğundan kimlik bilgilerini yeniden girmeniz gerekebilir.

Oturum kapatma özelliği eklemek için aşağıdaki yöntemi `MainActivity` sınıfının içine ekleyin. Bu yöntem tüm hesaplar boyunca geçiş yapar ve bunları kaldırır:

```java
/* Clears an account's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 * User will get interactive SSO if trying to sign back-in.
 */
private void onSignOutClicked() {
    /* Attempt to get a user and acquireTokenSilent
     * If this fails we do an interactive request
     */
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (accounts.isEmpty()) {
                /* No accounts to remove */

            } else {
                for (final IAccount account : accounts) {
                    sampleApp.removeAccount(
                            account,
                            new PublicClientApplication.AccountsRemovedCallback() {
                        @Override
                        public void onAccountsRemoved(Boolean isSuccess) {
                            if (isSuccess) {
                                /* successfully removed account */
                            } else {
                                /* failed to remove account */
                            }
                        }
                    });
                }
            }

            updateSignedOutUI();
        }
    });
}
```

#### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API 'sini çağırma

Bir belirteç aldıktan sonra, [Microsoft Graph API](https://graph.microsoft.com) 'sine bir istek yapabiliriz. erişim belirteci `AuthenticationResult` , kimlik doğrulama geri çağırma `onSuccess()` yönteminin içinde olacaktır. Yetkilendirilmiş bir istek oluşturmak için, uygulamanızın erişim belirtecini HTTP üstbilgisine eklemesi gerekir:

| üst bilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Authorization | Taşıyıcı \<erişim-belirteç > |

Grafiği çağırmak ve Kullanıcı arabirimini güncelleştirmek için `MainActivity` aşağıdaki iki yöntemi sınıfına ekleyin:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```

#### <a name="multi-account-applications"></a>Çoklu hesap uygulamaları

Bu uygulama, tek bir hesap senaryosu için oluşturulmuştur. MSAL ayrıca çoklu hesap senaryolarını destekler, ancak uygulamalardan bazı ek işler gerektirir. Kullanıcının belirteç gerektiren her eylem için hangi hesabı kullanmak istediğini belirlemek için kullanıcı ARABIRIMI oluşturmanız gerekir. Alternatif olarak, uygulamanız `getAccounts()` yöntemi aracılığıyla kullanılacak hesabı seçmek için bir buluşsal yöntem uygulayabilir.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Uygulamayı derleyin ve bir test cihazında veya öykünücüsünde dağıtın. Azure AD veya kişisel Microsoft hesapları için oturum açabiliyor ve belirteçleri alabilmesi gerekir.

Oturum açtıktan sonra, uygulama Microsoft Graph `/me` uç noktasından döndürülen verileri görüntüler.

### <a name="consent"></a>Onayla

Herhangi bir Kullanıcı uygulamanızda ilk kez oturum açtığında, bu kullanıcılara istenen izinleri onaylaması için Microsoft kimliği sorulur.  Birçok kullanıcı kabul etme yeteneğine sahip olsa da, bazı Azure AD kiracılar, yöneticilerin tüm kullanıcılar adına onay vermesini gerektiren Kullanıcı onayını devre dışı bırakmış olur. Bu senaryoyu desteklemek için, uygulamanızın kapsamlarını Azure portal kaydedin.

## <a name="get-help"></a>Yardım alın

Bu öğreticiyle veya Microsoft Identity platformu ile ilgili sorun yaşıyorsanız [Yardım ve destek](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) bölümünü ziyaret edin.

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)