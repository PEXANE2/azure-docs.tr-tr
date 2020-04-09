---
title: '& Microsoft Graph (Android) - Microsoft kimlik platformu | Azure'
description: Microsoft kimlik platformundan (Android) erişim belirteçleri gerektiren bir erişim jetonu alın ve Microsoft Graph veya API'leri arayın
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 11/26/2019
ms.author: hahamil
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 5c8bd5accefceee042601c3cf7d71f5e9131e04e
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80880831"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-from-an-android-application"></a>Öğretici: Oturum açın ve bir Android uygulamasından Microsoft Graph'ı arayın 

>[!NOTE]
>Bu öğretici, Android için MSAL ile nasıl çalışılabilen basitleştirilmiş örnekler gösterir. Basitlik için, bu öğretici yalnızca Tek Hesap Modu kullanır. Ayrıca repo'yu görüntüleyebilir ve daha karmaşık senaryoları keşfetmek için [önceden yapılandırılmış örnek uygulamayı](https://github.com/Azure-Samples/ms-identity-android-java/) klonlayabilirsiniz. Örnek uygulama, yapılandırma ve kayıt hakkında daha fazla bilgi için [Quickstart'ı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-android) görüntüleyin. 

Bu eğitimde, Android için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak android uygulamanızı Microsoft kimlik platformuyla nasıl entegre acağınızı öğreneceksiniz. Nasıl oturum açacağını ve bir kullanıcıyı oturum açmayı, Microsoft Graph API'yi aramak için bir erişim jetonunu almayı ve Grafik API'sine bir istekte bulunmayı öğreneceksiniz. 

> [!div class="checklist"]
> * Android Uygulamanızı Microsoft Kimlik Platformu ile tümleştirin 
> * Kullanıcıda oturum açma 
> * Microsoft Graph API'yi aramak için bir erişim belirteci alın 
> * Microsoft Graph API'yi arayın 
> * Bir kullanıcıyı oturum aç 

Bu öğreticiyi tamamladığınızda, uygulamanız Azure Active Directory kullanan herhangi bir şirket veya kuruluşun iş veya okul hesaplarının yanı sıra kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) oturum açmalarını kabul eder.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="how-this-tutorial-works"></a>Bu öğretici nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

Bu eğitimdeki uygulama, kullanıcılar oturum açacak ve onların adına veri alacak. Bu verilere, yetkilendirme gerektiren ve Microsoft kimlik platformu tarafından korunan korumalı api (Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanız kullanıcıda bir tarayıcı veya Microsoft Authenticator ve Intune Company Portal üzerinden oturum açar.
* Son kullanıcı, uygulamanızın istediği izinleri kabul eder.
* Uygulamanız Microsoft Graph API için bir erişim belirteci verilir.
* Erişim belirteci, web API'ye http isteğine eklenecektir.
* Microsoft Graph yanıtını işleme.

Bu örnek, Kimlik Doğrulama'yı uygulamak için Android için Microsoft Kimlik Doğrulama kitaplığını (MSAL) kullanır: [com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal).

 MSAL belirteçleri otomatik olarak yeniler, cihazdaki diğer uygulamalar arasında tek oturum açma (SSO) sağlar ve Hesabı(lar) yönetir.

### <a name="prerequisites"></a>Ön koşullar

* Bu öğretici Android Studio sürümü 3.5 + gerektirir

## <a name="create-a-project"></a>Proje Oluşturma
Zaten bir Android uygulamanız yoksa, yeni bir proje ayarlamak için aşağıdaki adımları izleyin. 

1. Android Studio'u açın ve **yeni bir Android Studio projesi başlat'ı**seçin.
2. **Temel Etkinlik'i** seçin ve **İleri'yi**seçin.
3. Uygulamanızı adlandırın.
4. Paket adını kaydedin. Daha sonra Azure portalına girersiniz.
5. **Dili Kotlin'den** **Java'ya**değiştirin.
6. Minimum **API düzeyini** **API 19** veya daha yüksek olarak ayarlayın ve **Finish'i**tıklatın.
7. Proje görünümünde, kaynak ve kaynak olmayan proje dosyalarını görüntülemek, **app/build.gradle'yi** açmak ve ayarlamak `targetSdkVersion` için açılır açılır `28` **durumdaProject'i** seçin.

## <a name="integrate-with-microsoft-authentication-library"></a>Microsoft Kimlik Doğrulama Kitaplığı ile tümleştirme 

### <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure portalına](https://aka.ms/MobileAppReg)gidin.
2. Uygulama [kayıtları bıçak](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) açın ve **+Yeni kayıt**tıklayın.
3. Uygulamanız için bir **Ad** girin ve ardından, Bir Yeniden Yönlendirme URI **ayarlamadan** **Kaydol'u**tıklatın.
4. Bölmenin görünen **Yönet** bölümünde Kimlik **Doğrulama** > + Platform > **Android****ekle'yi**seçin. (Bu bölümü görmek için bıçağın üst kısmına yakın "Yeni deneyime geçiş" seçeneğini seçmeniz gerekebilir)
5. Projenizin Paket Adını girin. Kodu indirdiyseniz, bu değer `com.azuresamples.msalandroidapp`.
6. **Android uygulama sayfanızı Yapılandır'ın** **İmza karma** bölümünde, geliştirme **İmza Hash oluşturma'yı tıklatın.** ve platformiçin kullanmak üzere KeyTool komutunu kopyalayın.

   > [!Note]
   > KeyTool.exe Java Geliştirme Kiti (JDK) bir parçası olarak yüklenir. KeyTool komutunu çalıştırmak için OpenSSL aracını da yüklemeniz gerekir. Daha fazla bilgi için [bir anahtar oluşturma yla ilgili Android belgelerine](https://developer.android.com/studio/publish/app-signing#generate-key) bakın. 

7. KeyTool tarafından oluşturulan **İmza karmasını** girin.
8. Android `Configure` **yapılandırma** sayfasında görünen **MSAL Yapılandırmasını** tıklatın ve kaydedin, böylece uygulamanızı daha sonra yapılandırdığınızda bu yapılandırmayı girebilirsiniz.  **Bitti**’ye tıklayın.

### <a name="configure-your-application"></a>Uygulamanızı yapılandırma 

1. Android Studio'nun proje **bölmesinde, app\src\main\res'e**gidin.
2. Sağ tıklayın **res** ve **Yeni** > **Dizin**seçin. Yeni `raw` dizin adı olarak girin ve **Tamam'ı**tıklatın.
3. **App** > **src** > **main** > **res** > **raw'da,** yeni `auth_configbn_single_account.json` bir JSON dosyası oluşturun ve daha önce kaydettiğiniz MSAL Yapılandırmasını yapıştırın. 

    Uri yönlendirme altında, yapıştırın: 
    ```json
      "account_mode" : "SINGLE",
    ```
    Config dosyanız bu örneğe benzemelidir: 
    ```json   
    {
      "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
      "authorization_user_agent" : "DEFAULT",
      "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
      "account_mode" : "SINGLE",
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
    
   >[!NOTE]
   >Bu öğretici yalnızca Tek Hesap modunda bir uygulamanın nasıl yapılandırılabildiğini gösterir. Tek ve [çoklu hesap modu](https://docs.microsoft.com/azure/active-directory/develop/single-multi-account) ve uygulamanızı yapılandırma hakkında daha fazla bilgi için belgeleri [görüntüleyin](https://docs.microsoft.com/azure/active-directory/develop/msal-configuration)
   
4. **Uygulama** > **src** > **ana** > **AndroidManifest.xml** `BrowserTabActivity` , uygulama gövdesine aşağıdaki etkinliği ekleyin. Bu giriş, Microsoft'un kimlik doğrulaması tamamlandıktan sonra uygulamanızı geri aramasına olanak tanır:

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

    Azure portalına kaydettiğiniz paket adını `android:host=` değer için değiştirin.
    Azure portalına kaydettiğiniz anahtar karmasını `android:path=` değer için değiştirin. İmza Hash URL kodlanmış **olmamalıdır.** İmza Karmanızın başında `/` bir lider olduğundan emin olun. 
    >[!NOTE]
    >`android:host` "Paket Adı" ile değiştireceğiniz değer benzer görünmelidir: "com.azuresamples.msalandroidapp" "Signature Hash" değerinizi değiştireceğiniz `android:path` "İmza Hash" benzer görünmelidir: "/1wIqXSqBj7w+h11ZifsnqwgyKrY=" Ayrıca bu değerleri uygulama kaydının Kimlik Doğrulama bıçağında bulabilirsiniz. Yeniden yönlendirme URI'nizin şuna benzeyeceğini unutmayın: "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D". İmza Hash bu değerin sonunda kodlanmış URL olsa da, İmza Hash değerinizde `android:path` kodlanmış URL olmamalıdır. **not** 

## <a name="use-msal"></a>MSAL kullanın 

### <a name="add-msal-to-your-project"></a>Projenize MSAL Ekleyin

1. Android Studio proje penceresinde, **uygulama** > **src** > **build.gradle** gidin ve aşağıdaki ekleyin: 

    ```gradle
    repositories{
        jcenter()
    }  
    dependencies{
        implementation 'com.microsoft.identity.client:msal:1.+'
        implementation 'com.microsoft.graph:microsoft-graph:1.5.+'
    }
    packagingOptions{
        exclude("META-INF/jersey-module-version") 
    }
    ```
    [Microsoft Graph SDK hakkında daha fazla](https://github.com/microsoftgraph/msgraph-sdk-java/)

### <a name="required-imports"></a>Gerekli İçeri Aktarmalar 

**App** > **src** > **ana**> **java** > **com.example(yourapp)** > **MainActivity.java** üstüne aşağıdaki ekleyin 

```java
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import com.google.gson.JsonObject;
import com.microsoft.graph.authentication.IAuthenticationProvider; //Imports the Graph sdk Auth interface
import com.microsoft.graph.concurrency.ICallback;
import com.microsoft.graph.core.ClientException;
import com.microsoft.graph.http.IHttpRequest;
import com.microsoft.graph.models.extensions.*;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import com.microsoft.identity.client.AuthenticationCallback; // Imports MSAL auth methods
import com.microsoft.identity.client.*;
import com.microsoft.identity.client.exception.*;
```

## <a name="instantiate-publicclientapplication"></a>Anında KamuMüşteri Uygulaması
#### <a name="initialize-variables"></a>Değişkenleri Başlatma 
```java
private final static String[] SCOPES = {"File.Read"};
/* Azure AD v2 Configs */
final static String AUTHORITY = "https://login.microsoftonline.com/common";
private ISingleAccountPublicClientApplication mSingleAccountApp;

private static final String TAG = MainActivity.class.getSimpleName();

/* UI & Debugging Variables */
Button signInButton;
Button signOutButton;
Button callGraphApiInteractiveButton;
Button callGraphApiSilentButton;
TextView logTextView;
TextView currentUserTextView;
```

### <a name="oncreate"></a>Oncreate
`MainActivity` Sınıf içinde, aşağıdaki onCreate() yöntemini kullanarak MSAL instantiate `SingleAccountPublicClientApplication`bakın.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    initializeUI();

    PublicClientApplication.createSingleAccountPublicClientApplication(getApplicationContext(),
            R.raw.auth_config_single_account, new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
                @Override
                public void onCreated(ISingleAccountPublicClientApplication application) {
                    mSingleAccountApp = application;
                    loadAccount();
                }
                @Override
                public void onError(MsalException exception) {
                    displayError(exception);
                }
            });
}
```

### <a name="loadaccount"></a>loadAccount 

```java
//When app comes to the foreground, load existing account to determine if user is signed in 
private void loadAccount() {
    if (mSingleAccountApp == null) {
        return;
    }

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
}
```

### <a name="initializeui"></a>initializeUI
Düğmeleri dinleyin ve arama yöntemlerini veya günlük hatalarını buna göre kaydedin. 
```java
private void initializeUI(){
        signInButton = findViewById(R.id.signIn);
        callGraphApiSilentButton = findViewById(R.id.callGraphSilent);
        callGraphApiInteractiveButton = findViewById(R.id.callGraphInteractive);
        signOutButton = findViewById(R.id.clearCache);
        logTextView = findViewById(R.id.txt_log);
        currentUserTextView = findViewById(R.id.current_user);
        
        //Sign in user 
        signInButton.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v) {
                if (mSingleAccountApp == null) {
                    return;
                }
                mSingleAccountApp.signIn(MainActivity.this, null, SCOPES, getAuthInteractiveCallback());
            }
        });
        
        //Sign out user
        signOutButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mSingleAccountApp == null){
                    return;
                }
                mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
                    @Override
                    public void onSignOut() {
                        updateUI(null);
                        performOperationOnSignOut();
                    }
                    @Override
                    public void onError(@NonNull MsalException exception){
                        displayError(exception);
                    }
                });
            }
        });
        
        //Interactive 
        callGraphApiInteractiveButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mSingleAccountApp == null) {
                    return;
                }
                mSingleAccountApp.acquireToken(MainActivity.this, SCOPES, getAuthInteractiveCallback());
            }
        });

        //Silent
        callGraphApiSilentButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mSingleAccountApp == null){
                    return;
                }
                mSingleAccountApp.acquireTokenSilentAsync(SCOPES, AUTHORITY, getAuthSilentCallback());
            }
        });
    }
```

> [!Important]
> MSAL ile oturum alabilme, kullanıcı hakkında bilinen tüm bilgileri uygulamadan kaldırır, ancak kullanıcının aygıtında etkin bir oturumu vardır. Kullanıcı yeniden oturum açmaya çalışırsa oturum açma ui'sini görebilir, ancak aygıt oturumu hala etkin olduğundan kimlik bilgilerini yeniden girmeleri gerekmeyebilir. 

### <a name="getauthinteractivecallback"></a>getAuthInteractiveCallback
Etkileşimli istekler için kullanılan geri arama.

```java 
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, use it to call a protected resource - MSGraph */
            Log.d(TAG, "Successfully authenticated");
            /* Update UI */
            updateUI(authenticationResult.getAccount());
            /* call graph */
            callGraphAPI(authenticationResult);
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());
            displayError(exception);
        }
        @Override
        public void onCancel() {
            /* User canceled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

### <a name="getauthsilentcallback"></a>getAuthSilentCallback
Sessiz istekler için kullanılan geri arama 
```java 
private SilentAuthenticationCallback getAuthSilentCallback() {
    return new SilentAuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            Log.d(TAG, "Successfully authenticated");
            callGraphAPI(authenticationResult);
        }
        @Override
        public void onError(MsalException exception) {
            Log.d(TAG, "Authentication failed: " + exception.toString());
            displayError(exception);
        }
    };
}
```

## <a name="call-microsoft-graph-api"></a>Microsoft Graph API'si çağırma 

Aşağıdaki kod, Graph SDK'yı kullanarak GraphAPI'nin nasıl çağrıldığını gösterir. 

### <a name="callgraphapi"></a>callGraphAPI 

```java
private void callGraphAPI(IAuthenticationResult authenticationResult) {

    final String accessToken = authenticationResult.getAccessToken();

    IGraphServiceClient graphClient =
            GraphServiceClient
                    .builder()
                    .authenticationProvider(new IAuthenticationProvider() {
                        @Override
                        public void authenticateRequest(IHttpRequest request) {
                            Log.d(TAG, "Authenticating request," + request.getRequestUrl());
                            request.addHeader("Authorization", "Bearer " + accessToken);
                        }
                    })
                    .buildClient();
    graphClient
            .me()
            .drive()
            .buildRequest()
            .get(new ICallback<Drive>() {
                @Override
                public void success(final Drive drive) {
                    Log.d(TAG, "Found Drive " + drive.id);
                    displayGraphResult(drive.getRawObject());
                }

                @Override
                public void failure(ClientException ex) {
                    displayError(ex);
                }
            });
}
```

## <a name="add-ui"></a>UI Ekle
### <a name="activity"></a>Etkinlik 
Kullanıcı arabiriminizi bu öğreticiden modellemek isterseniz, aşağıdaki yöntemler metni güncelleştirmek ve düğmeleri dinlemek için bir kılavuz sağlar.

#### <a name="updateui"></a>updateUI
Oturum açma durumuna ve metni ayarla'ya göre düğmeleri etkinleştirme/devre dışı bırak.  
```java 
private void updateUI(@Nullable final IAccount account) {
    if (account != null) {
        signInButton.setEnabled(false);
        signOutButton.setEnabled(true);
        callGraphApiInteractiveButton.setEnabled(true);
        callGraphApiSilentButton.setEnabled(true);
        currentUserTextView.setText(account.getUsername());
    } else {
        signInButton.setEnabled(true);
        signOutButton.setEnabled(false);
        callGraphApiInteractiveButton.setEnabled(false);
        callGraphApiSilentButton.setEnabled(false);
        currentUserTextView.setText("");
        logTextView.setText("");
    }
}
```
#### <a name="displayerror"></a>displayError
```java 
private void displayError(@NonNull final Exception exception) {
       logTextView.setText(exception.toString());
   }
```

#### <a name="displaygraphresult"></a>displayGraphResult

```java
private void displayGraphResult(@NonNull final JsonObject graphResponse) {
      logTextView.setText(graphResponse.toString());
  }
```
#### <a name="performoperationonsignout"></a>performOperationOnSignOut
Oturum açmayı yansıtacak şekilde Kullanıcı-ı ÖTü'deki metni güncelleştirme yöntemi. 

```java
private void performOperationOnSignOut() {
    final String signOutText = "Signed Out.";
    currentUserTextView.setText("");
    Toast.makeText(getApplicationContext(), signOutText, Toast.LENGTH_SHORT)
            .show();
}
```
### <a name="layout"></a>Düzen 

Düğmeleri ve metin kutularını görüntülemek için örnek `activity_main.xml` dosya. 

```xml 
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:paddingTop="5dp"
        android:paddingBottom="5dp"
        android:weightSum="10">

        <Button
            android:id="@+id/signIn"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="5"
            android:gravity="center"
            android:text="Sign In"/>

        <Button
            android:id="@+id/clearCache"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="5"
            android:gravity="center"
            android:text="Sign Out"
            android:enabled="false"/>

    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal">

        <Button
            android:id="@+id/callGraphInteractive"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="5"
            android:text="Get Graph Data Interactively"
            android:enabled="false"/>

        <Button
            android:id="@+id/callGraphSilent"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="5"
            android:text="Get Graph Data Silently"
            android:enabled="false"/>
    </LinearLayout>

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <TextView
        android:id="@+id/current_user"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="20dp"
        android:layout_weight="0.8"
        android:text="Account info goes here..." />

    <TextView
        android:id="@+id/txt_log"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="20dp"
        android:layout_weight="0.8"
        android:text="Output goes here..." />
</LinearLayout>
```

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Uygulamayı bir test aygıtına veya emülatöre oluşturun ve dağıtın. Azure AD veya kişisel Microsoft hesapları için oturum açabilmeli ve jeton alabilmelisiniz.

Oturum açmanızdan sonra, uygulama Microsoft Graph `/me` bitiş noktasından döndürülen verileri görüntüler.

### <a name="consent"></a>Izni

Herhangi bir kullanıcı uygulamanıza ilk kez giriş yaptıklarında, talep edilen izinleri kabul etmeleri için Microsoft kimliği tarafından istenir. Bazı Azure AD kiracıları, yöneticilerin tüm kullanıcılar adına onay ını gerektiren kullanıcı onayını devre dışı bıraktı. Bu senaryoyu desteklemek için, kendi kiracınızı oluşturmanız veya yönetici onayı almanız gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, [uygulama adımınızı kaydedin'de](#register-your-application) oluşturduğunuz uygulama nesnesini silin.

## <a name="get-help"></a>Yardım alın

Bu [öğreticiyle](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) veya Microsoft kimlik platformunda sorun yaşıyorsanız Yardım ve desteği ziyaret edin.

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
