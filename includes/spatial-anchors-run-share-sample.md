---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 8/27/2020
ms.author: rgarcia
ms.openlocfilehash: 8e3f58a5a88495263396c4e915a6363ad065c202
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89570181"
---
## <a name="android"></a>[Android](#tab/Android)

Java Android örneği cihazlar arasında paylaşmayı destekler.
Dosyayı `SharedActivity.java` Android Studio içindeki Samples klasöründen açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) dosyadaki değeri olarak girin `SharingAnchorsServiceUrl` `SharedActivity.java` . `index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="ios"></a>[iOS](#tab/iOS)

Amaç-C iOS örneği, cihazlar arasında paylaşmayı destekler.
Dosyayı `SharedDemoViewController.m` Samples klasöründe açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) dosyadaki değeri olarak girin `SharingAnchorsServiceUrl` `SharedDemoViewController.m` . `index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="xamarin"></a>[Xamarin](#tab/Xamarin)

Hem Xamarin Android hem de iOS örnekleri cihazlar arasında paylaşmayı destekler.
Dosyayı `AccountDetails.cs` Samples klasöründe açın. Önceki adımda elde ettiğiniz URL 'yi (ASP.NET Web uygulaması Azure dağıtımından) dosyadaki değeri olarak girin `AnchorSharingServiceUrl` `AccountDetails.cs` . `index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="unity"></a>[Unity](#tab/Unity)

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>İOS cihazı ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

**Proje** bölmesinde öğesine gidin `Assets\AzureSpatialAnchors.Examples\Resources` . `SpatialAnchorSamplesConfig` öğesini seçin. Ardından, **Inspector** bölmesinde, `Sharing Anchors Service url` ile değiştirme için değeri olarak (ASP.NET Web App Azure dağıtımından) değerini girin `Base Sharing Url` `index.html` `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

**Dosya**Kaydet ' i seçerek sahneyi kaydedin  >  **Save**.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-android-device"></a>Android cihazına dağıtma

Android cihazınızda oturum açın ve USB kablosu kullanarak bilgisayarınıza bağlayın.

**Dosya**derleme ayarları ' nı seçerek **derleme ayarlarını** açın  >  **Build Settings**.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

**Dışarı aktarma projesinin** onay işareti olmadığından emin olun. **Oluştur ve Çalıştır ' ı**seçin. Dosyanızı kaydetmeniz istenecektir `.apk` . Bunun için herhangi bir ad seçebilirsiniz.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

### <a name="deploy-to-an-ios-device"></a>İOS cihazına dağıtma

**Dosya**derleme ayarları ' nı seçerek **derleme ayarlarını** açın  >  **Build Settings**.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

Xcode 'da **Durdur**' u seçerek uygulamayı durdurun.
