---
title: Hızlı başlangıç-Azure Iletişim hizmetlerini kullanarak bir Android uygulamasına çağıran VOıP ekleme
description: Bu öğreticide, Android için istemci kitaplığı 'nı çağıran Azure Iletişim hizmetlerini kullanmayı öğreneceksiniz.
author: matthewrobertson
ms.author: marobert
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 658aaf018dd4c231b9bc9fc8c6dda78b2a6f05c0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948333"
---
Bu hızlı başlangıçta, Android için istemci kitaplığı 'nı çağıran Azure Iletişim Hizmetleri 'ni kullanarak nasıl çağrı başlayacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Android uygulamanızı oluşturmak için [Android Studio](https://developer.android.com/studio).
- Azure Iletişim hizmetiniz için bir [Kullanıcı erişim belirteci](../../access-tokens.md) .
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).


## <a name="setting-up"></a>Ayarlanıyor


### <a name="create-an-android-app-with-an-empty-activity"></a>Boş bir etkinliğe sahip bir Android uygulaması oluşturma

Android Studio yeni bir Android Studio projesi Başlat ' ı seçin.

:::image type="content" source="../media/android/studio-new-project.png" alt-text="Android Studio ' de seçili ' yeni Android Studio projesi Başlat ' düğmesini gösteren ekran görüntüsü.":::

"Telefon ve Tablet" altında "boş etkinlik" proje şablonunu seçin.

:::image type="content" source="../media/android/studio-blank-activity.png" alt-text="Proje şablonu ekranında ' boş etkinlik ' seçeneğinin seçili olduğunu gösteren ekran görüntüsü.":::

En düşük istemci kitaplığı "API 26: Android 8,0 (Oreo)" veya daha büyük bir değer seçin.

:::image type="content" source="../media/android/studio-calling-min-api.png" alt-text="Proje şablonu ekranı 2 ' de seçili ' boş etkinlik ' seçeneğinin gösterildiği ekran görüntüsü.":::


### <a name="install-the-package"></a>Paketi yükler

<!-- TODO: update with instructions on how to download, install and add package to project -->
Proje seviyesi Build. Gradle ' i bulun ve `mavenCentral()` altındaki depolar listesine eklediğinizden emin olun. `buildscript``allprojects`
```groovy
buildscript {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```

```groovy
allprojects {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```
Ardından, modül düzeyi derlemenize. Gradle aşağıdaki satırları bağımlılıklar ve Android bölümlerine ekleyin

```groovy
android {
    ...
    packagingOptions {
        pickFirst  'META-INF/*'
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    ...
    implementation 'com.azure.android:azure-communication-calling:1.0.0-beta.1'
    ...
}
```

### <a name="add-permissions-to-application-manifest"></a>Uygulama bildirimine izinler ekleme

Çağrı yapmak için gerekli izinleri istemek amacıyla, önce uygulama bildiriminde ( `app/src/main/AndroidManifest.xml` ) bildirilmelidir. Dosyanın içeriğini aşağıdaki kodla değiştirin:

```xml
    <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.contoso.acsquickstart">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <!--Our calling client library depends on the Apache HTTP client library.
When targeting Android client library 28+, this library needs to be explicitly referenced.
See https://developer.android.com/about/versions/pie/android-9.0-changes-28#apache-p-->
        <uses-library android:name="org.apache.http.legacy" android:required="false"/>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
    
```

### <a name="setup-the-layout-for-the-app"></a>Uygulamanın yerleşimini ayarlama

İki giriş gereklidir: aranan kimliği için bir metin girişi ve çağrıyı yerleştirmek için bir düğme. Bunlar tasarımcı aracılığıyla veya Düzen XML 'sini düzenleyerek eklenebilir. Kimliği ve metin girişi olan bir düğme oluşturun `call_button` `callee_id` . () Öğesine gidin `app/src/main/res/layout/activity_main.xml` ve dosyanın içeriğini aşağıdakiler ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/call_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:text="Call"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <EditText
        android:id="@+id/callee_id"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="Callee Id"
        android:inputType="textPersonName"
        app:layout_constraintBottom_toTopOf="@+id/call_button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### <a name="create-the-main-activity-scaffolding-and-bindings"></a>Ana Etkinlik yapı iskelesi ve bağlamaları oluşturma

Düzen oluşturulduktan sonra bağlamalar, etkinliğin temel yapı iskelesi ile eklenebilir. Etkinlik, istenen çalışma zamanı izinlerini, çağrı aracısını oluşturmayı ve düğmeye basıldığında çağrıyı yerleştirmeyi işleymeyecektir. Her biri kendi bölümünde ele alınacaktır. `onCreate`Yöntemi çağırmak için geçersiz kılınır `getAllPermissions` ve `createAgent` Ayrıca çağrı düğmesi için bağlamaları ekler. Bu, etkinlik oluşturulduğunda yalnızca bir kez gerçekleşir. Hakkında daha fazla bilgi için `onCreate` , [etkinlik yaşam döngüsünü anlama](https://developer.android.com/guide/components/activities/activity-lifecycle)kılavuzuna bakın.

**MainActivity. Java** ' ya gidin ve içeriği şu kodla değiştirin:

```java
package com.contoso.acsquickstart;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.azure.android.communication.common.CommunicationUser;
import com.azure.android.communication.common.CommunicationUserCredential;
import com.azure.communication.calling.CallAgent;
import com.azure.communication.calling.CallClient;
import com.azure.communication.calling.StartCallOptions;


import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    
    private CallAgent callAgent;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        getAllPermissions();
        createAgent();
        
        // Bind call button to call `startCall`
        Button callButton = findViewById(R.id.call_button);
        callButton.setOnClickListener(l -> startCall());
    }

    /**
     * Request each required permission if the app doesn't already have it.
     */
    private void getAllPermissions() {
        // See section on requesting permissions
    }

    /**
      * Create the call agent for placing calls
      */
    private void createAgent() {
        // See section on creating the call agent
    }

    /**
     * Place a call to the callee id provided in `callee_id` text input.
     */
    private void startCall() {
        // See section on starting the call
    }
}

```

### <a name="request-permissions-at-runtime"></a>Çalışma zamanında izin iste

Android 6,0 ve üzeri (API düzeyi 23) ve `targetSdkVersion` 23 veya üzeri için, uygulama yüklendiği zaman yerine çalışma zamanında izin verilir. Bunu desteklemek için, `getAllPermissions` `ActivityCompat.checkSelfPermission` `ActivityCompat.requestPermissions` gerekli her izin için ve çağırmak üzere uygulanabilir.

```java
/**
 * Request each required permission if the app doesn't already have it.
 */
private void getAllPermissions() {
    String[] requiredPermissions = new String[]{Manifest.permission.RECORD_AUDIO, Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.READ_PHONE_STATE};
    ArrayList<String> permissionsToAskFor = new ArrayList<>();
    for (String permission : requiredPermissions) {
        if (ActivityCompat.checkSelfPermission(this, permission) != PackageManager.PERMISSION_GRANTED) {
            permissionsToAskFor.add(permission);
        }
    }
    if (!permissionsToAskFor.isEmpty()) {
        ActivityCompat.requestPermissions(this, permissionsToAskFor.toArray(new String[0]), 1);
    }
}
```

> [!NOTE]
> Uygulamanızı tasarlarken, bu izinlerin ne zaman istendiğine göz önünde bulundurun. İzinler, gerekli oldukları sırada değil, istenerek istenir. Daha fazla bilgi için bkz [. Android Izinleri Kılavuzu.](https://developer.android.com/training/permissions/requesting)

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler:

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| CallClient| CallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.|
| CallAgent | CallAgent, çağrıları başlatmak ve yönetmek için kullanılır. |
| CommunicationUserCredential | CommunicationUserCredential, CallAgent örneğini oluşturmak için belirteç kimlik bilgileri olarak kullanılır.|

## <a name="create-an-agent-from-the-user-access-token"></a>Kullanıcı erişim belirtecinden bir aracı oluşturma

Kullanıcı belirteciyle, kimliği doğrulanmış bir çağrı Aracısı oluşturulabilir. Genellikle bu belirteç uygulamaya özgü kimlik doğrulaması olan bir hizmetten oluşturulacaktır. Kullanıcı belirteçleri hakkında daha fazla bilgi için [Kullanıcı erişim belirteçleri](../../access-tokens.md) Kılavuzu ' na bakın. Hızlı başlangıç için, `<User_Access_Token>` Azure Iletişim hizmeti kaynağınız için oluşturulmuş bir Kullanıcı erişim belirteci ile değiştirin.

```java

/**
 * Create the call agent for placing calls
 */
private void createAgent() {
    String userToken = "<User_Access_Token>";

    try {
        CommunicationUserCredential credential = new CommunicationUserCredential(userToken);
        callAgent = new CallClient().createCallAgent(getApplicationContext(), credential).get();
    } catch (Exception ex) {
        Toast.makeText(getApplicationContext(), "Failed to create call agent.", Toast.LENGTH_SHORT).show();
    }
}

```

## <a name="start-a-call-using-the-call-agent"></a>Çağrı Aracısı 'nı kullanarak çağrı başlatma

Çağrının yerleştirilmesi çağrı Aracısı aracılığıyla yapılabilir ve yalnızca aranan kimliklerin ve çağrı seçeneklerinin bir listesini sağlamayı gerektirir. Hızlı başlangıç için, video olmadan varsayılan çağrı seçenekleri ve metin girişinden tek bir aranan kimliği kullanılır.

```java
/**
 * Place a call to the callee id provided in `callee_id` text input.
 */
private void startCall() {
    EditText calleeIdView = findViewById(R.id.callee_id);
    String calleeId = calleeIdView.getText().toString();
    
    StartCallOptions options = new StartCallOptions();

    callAgent.call(
        getApplicationContext(),
        new CommunicationUser[] {new CommunicationUser(calleeId)},
        options);
}
```


## <a name="launch-the-app-and-call-the-echo-bot"></a>Uygulamayı başlatın ve yankı bot 'ı çağırın

Uygulama artık araç çubuğunda "uygulama Çalıştır" düğmesi kullanılarak başlatılabilir (SHIFT + F10). Çağırarak çağrı yerleştirebildiğinizi doğrulayın `8:echo123` . Önceden kaydedilmiş bir ileti oynatılır ve sonra iletinizi size geri tekrarlayacak.

:::image type="content" source="../media/android/quickstart-android-call-echobot.png" alt-text="Tamamlanmış uygulamayı gösteren ekran görüntüsü.":::
