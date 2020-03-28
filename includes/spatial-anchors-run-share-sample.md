---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: ec8fb6efab126dcf5556a9abfdf58d1fd69d4212
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72882294"
---
## <a name="android"></a>[Android](#tab/Android)

Java android örneği cihazlar arasında paylaşımı destekler.
Dosyayı `SharedActivity.java` Android Studio'daki örnekler klasöründen açın. Önceki adımda (ASP.NET web uygulamanızdan Azure dağıtımından) elde ettiğiniz `SharingAnchorsServiceUrl` url'yi `SharedActivity.java` dosyadaki değer olarak girin. `index.html` Url'yi `api/anchors`' ile değiştirin. Bu gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="ios"></a>[iOS](#tab/iOS)

Objective-C iOS örneği aygıtlar arasında paylaşımı destekler.
Örnekler klasöründeki dosyayı `SharedDemoViewController.m` açın. Önceki adımda (ASP.NET web uygulamanızdan Azure dağıtımından) elde ettiğiniz `SharingAnchorsServiceUrl` url'yi `SharedActivity.java` dosyadaki değer olarak girin. `index.html` Url'yi `api/anchors`' ile değiştirin. Bu gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="xamarin"></a>[Xamarin](#tab/Xamarin)

Hem Xamarin Android hem de iOS örnekleri cihazlar arasında paylaşımı destekler.
Örnekler klasöründeki dosyayı `AccountDetails.cs` açın. Önceki adımda (ASP.NET web uygulamanızdan Azure dağıtımından) elde ettiğiniz `AnchorSharingServiceUrl` url'yi `SharedActivity.java` dosyadaki değer olarak girin. `index.html` Url'yi `api/anchors`' ile değiştirin. Bu gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="unity"></a>[Unity](#tab/Unity)

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Android Cihazı Ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>iOS Aygıtı Ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Hesap tanımlayıcısını ve anahtarı nı yapılandırma

**Proje** bölmesinde, sahne `Assets/AzureSpatialAnchorsPlugin/Examples` dosyasına `AzureSpatialAnchorsLocalSharedDemo.unity` gidin ve açın.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

**Proje** bölmesinde, '' `Assets\AzureSpatialAnchors.Examples\Resources`ye gidin. `SpatialAnchorSamplesConfig` öğesini seçin. Daha sonra, **Denetçi** bölmesine, `Sharing Anchors Service url` (ASP.NET web uygulamanızdan Azure dağıtımı) `Base Sharing Url`değeri `index.html` olarak `api/anchors`, değiştirerek girin. Bu gibi görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

**Dosya** > Kaydet'i seçerek sahneyi**kaydedin.**

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-android-device"></a>Android cihaza dağıt

Android cihazınızda oturum açın ve usb kablosu kullanarak bilgisayarınıza bağlayın.

 >  **Dosya**Oluşturma Ayarları'nı seçerek **Yapı Ayarlarını** Açın.**Build Settings**

**Scenes In Build'in**altında, tüm sahnelerin yanlarında bir onay işareti olduğundan emin olun.

**Dışa Aktarma Projesi'nde** onay işareti olmadığından emin olun. **Oluştur ve Çalıştır'ı**seçin. Dosyanızı kaydetmeniz `.apk` istenir. Herhangi bir isim seçebilirsin.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

### <a name="deploy-to-an-ios-device"></a>iOS aygıtına dağıtma

 >  **Dosya**Oluşturma Ayarları'nı seçerek **Yapı Ayarlarını** Açın.**Build Settings**

**Scenes In Build'in**altında, tüm sahnelerin yanlarında bir onay işareti olduğundan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

Xcode'da, **Durdur'u**seçerek uygulamayı durdurun.
