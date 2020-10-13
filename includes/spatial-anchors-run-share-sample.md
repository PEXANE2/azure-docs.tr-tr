---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 8/27/2020
ms.author: rgarcia
ms.openlocfilehash: 50e4799f09322eab05b4f8ddf7004c2e0078fdab
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971498"
---
## <a name="android"></a>[Android](#tab/Android)

Java Android örneği cihazlar arasında paylaşmayı destekler.

Android Studio ' de, Samples klasöründen *Sharedactivity. Java* dosyasını açın. 

Önceki adımda kopyaladığınız URL 'YI (ASP.NET Web uygulaması Azure dağıtımından) `SharingAnchorsServiceUrl` *sharedactivity. Java* dosyasındaki değeri olarak girin. 

`index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

[!INCLUDE [Run shared sample](spatial-anchors-deploy-sample.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="ios"></a>[iOS](#tab/iOS)

Amaç-C iOS örneği, cihazlar arasında paylaşmayı destekler.

Samples klasöründeki *Shareddemoviewcontroller. d* dosyasını açın. 

Önceki adımda edindiğiniz URL 'YI (ASP.NET Web uygulaması Azure dağıtımından) `SharingAnchorsServiceUrl` *Shareddemoviewcontroller. d* dosyasında için değer olarak girin. 

`index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

Uygulamayı cihazınıza dağıtın. 

Uygulama başladıktan sonra, **paylaşılan tanıtım ' i başlatmak Için dokunun** seçeneğini belirleyin ve ardından uygulamadaki yönergeleri izleyin. Bağlantı noktası **numarasını bulmak Için dokunun '** **ı veya bağlayıcı oluşturmak ve hizmete kaydetmek için dokunun**seçeneğini belirleyebilirsiniz.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="xamarin"></a>[Xamarin](#tab/Xamarin)

Hem Xamarin Android hem de iOS örnekleri cihazlar arasında paylaşmayı destekler.

Samples klasöründe *AccountDetails.cs* dosyasını açın. 

Önceki adımda elde ettiğiniz URL 'YI (ASP.NET Web uygulaması Azure dağıtımından) `AnchorSharingServiceUrl` *AccountDetails.cs* dosyasındaki değeri olarak girin. 

`index.html`URL içindeki öğesini ile değiştirin `api/anchors` . Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

[!INCLUDE [Run shared sample](spatial-anchors-deploy-sample.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="unity"></a>[Unity](#tab/Unity)

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>İOS cihazı ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

**Proje** bölmesinde öğesine gidin `Assets\AzureSpatialAnchors.Examples\Resources` . 

**SpatialAnchorSamplesConfig**öğesini seçin. Ardından, **Inspector** bölmesinde, `Sharing Anchors Service` için değer olarak URL 'yi (ASP.NET Web App Azure dağıtımından) girin `Base Sharing Url` . `index.html` yerine `api/anchors` yazın. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors` .

**Dosya**Kaydet ' i seçerek sahneyi kaydedin  >  **Save**.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-an-android-device"></a>Android cihazına dağıtma

Android cihazınızda oturum açın ve USB kablosu kullanarak bilgisayarınıza bağlayın.

**Dosya**derleme ayarları ' nı seçerek **derleme ayarlarını** açın  >  **Build Settings**.

**Derlemede sahne**' ın altında, her sahnenin yanında bir onay işareti olduğundan emin olun.

**Dışarı aktarma projesinin** onay işareti olmadığından emin olun. **Oluştur ve Çalıştır ' ı**seçin. *. Apk* dosyanızı kaydetmeniz istenecektir. Bunun için herhangi bir ad seçebilirsiniz.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

### <a name="deploy-to-an-ios-device"></a>İOS cihazına dağıtma

**Dosya**derleme ayarları ' nı seçerek **derleme ayarlarını** açın  >  **Build Settings**.

**Derlemede sahne**' ın altında, her sahnenin yanında bir onay işareti olduğundan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

Xcode 'da **Durdur**' u seçerek uygulamayı durdurun.
