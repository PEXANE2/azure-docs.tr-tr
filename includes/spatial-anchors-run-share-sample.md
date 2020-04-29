---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: ec8fb6efab126dcf5556a9abfdf58d1fd69d4212
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72882294"
---
## <a name="android"></a>[Android](#tab/Android)

Java Android örneği cihazlar arasında paylaşmayı destekler.
Dosyayı `SharedActivity.java` Android Studio içindeki Samples klasöründen açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) `SharingAnchorsServiceUrl` `SharedActivity.java` dosyadaki değeri olarak girin. URL `index.html` içindeki öğesini ile `api/anchors`değiştirin. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="ios"></a>[iOS](#tab/iOS)

Amaç-C iOS örneği, cihazlar arasında paylaşmayı destekler.
Dosyayı `SharedDemoViewController.m` Samples klasöründe açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) `SharingAnchorsServiceUrl` `SharedActivity.java` dosyadaki değeri olarak girin. URL `index.html` içindeki öğesini ile `api/anchors`değiştirin. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="xamarin"></a>[Xamarin](#tab/Xamarin)

Hem Xamarin Android hem de iOS örnekleri cihazlar arasında paylaşmayı destekler.
Dosyayı `AccountDetails.cs` Samples klasöründe açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) `AnchorSharingServiceUrl` `SharedActivity.java` dosyadaki değeri olarak girin. URL `index.html` içindeki öğesini ile `api/anchors`değiştirin. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="unity"></a>[Unity](#tab/Unity)

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>İOS cihazı ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

**Proje** bölmesinde, öğesine `Assets/AzureSpatialAnchorsPlugin/Examples` gidin ve `AzureSpatialAnchorsLocalSharedDemo.unity` sahne dosyasını açın.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

**Proje** bölmesinde öğesine `Assets\AzureSpatialAnchors.Examples\Resources`gidin. `SpatialAnchorSamplesConfig` öğesini seçin. Ardından, **Inspector** bölmesinde `Sharing Anchors Service url` , ile `Base Sharing Url` `index.html` `api/anchors`değiştirme için değeri olarak (ASP.NET Web App Azure dağıtımından) değerini girin. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

**Dosya** > **Kaydet**' i seçerek sahneyi kaydedin.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-android-device"></a>Android cihazına dağıtma

Android cihazınızda oturum açın ve USB kablosu kullanarak bilgisayarınıza bağlayın.

**File**Dosya > **derleme ayarları**' nı seçerek **derleme ayarlarını** açın.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

**Dışarı aktarma projesinin** onay işareti olmadığından emin olun. **Oluştur ve Çalıştır ' ı**seçin. `.apk` Dosyanızı kaydetmeniz istenecektir. Bunun için herhangi bir ad seçebilirsiniz.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

### <a name="deploy-to-an-ios-device"></a>İOS cihazına dağıtma

**File**Dosya > **derleme ayarları**' nı seçerek **derleme ayarlarını** açın.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

Xcode 'da **Durdur**' u seçerek uygulamayı durdurun.
