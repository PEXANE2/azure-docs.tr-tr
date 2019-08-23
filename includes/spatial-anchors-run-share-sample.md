---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 1007533df077c58d9e4d57f9e86b035730ea917f
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69903948"
---
## <a name="set-up-your-device-in-unity"></a>Bu cihazınızı Unity 'de ayarlama

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Android cihazı ayarlama

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>İOS cihazı ayarlama

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

**Proje** bölmesinde, öğesine `Assets/AzureSpatialAnchorsPlugin/Examples` gidin ve `AzureSpatialAnchorsLocalSharedDemo.unity` sahne dosyasını açın.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

**Proje** bölmesinde öğesine `Assets\AzureSpatialAnchors.Examples\Resources`gidin. `SpatialAnchorSamplesConfig` öğesini seçin. Ardından, **Inspector** bölmesinde `Sharing Anchors Service url` , ile `Base Sharing Url` `index.html` değiştirme`api/anchors`için değeri olarak (ASP.NET Web App Azure dağıtımından) değerini girin. Şöyle görünmelidir: `https://<app_name>.azurewebsites.net/api/anchors`.

**Dosya** > **Kaydet**' i seçerek sahneyi kaydedin.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

### <a name="deploy-to-android-device"></a>Android cihazına dağıtma

Android cihazınızda oturum açın ve USB kablosu kullanarak bilgisayarınıza bağlayın.

**Dosya** derlemeayarları'nıseçerek > derleme ayarlarını açın.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

**Dışarı aktarma projesinin** onay işareti olmadığından emin olun. **Oluştur ve Çalıştır ' ı**seçin. `.apk` Dosyanızı kaydetmeniz istenecektir. Bunun için herhangi bir ad seçebilirsiniz.

Uygulama başladıktan sonra, **bir demo Seç** iletişim kutusunda, **localshare** seçeneğini belirlemek için sol veya sağ ok tuşlarını kullanın ve **Git!** ' e dokunun. Uygulamadaki yönergeleri izleyin. **Oluşturma & paylaşım bağlantısı** ' nı veya **paylaşılan bağlayıcıyı bul**' u seçebilirsiniz.

İlk senaryo, daha sonra aynı cihazda veya farklı bir aygıtta yer alan bir bağlantı oluşturmanıza olanak sağlar.
İkinci senaryo, aynı cihazda ya da farklı bir uygulamada uygulamayı zaten çalıştırırsanız, daha önce paylaşılan bağlantıları bulmanızı sağlar. Senaryonuzu belirledikten sonra uygulama, ne yapacaklarına ilişkin daha fazla yönerge için size kılavuzluk eder. Örneğin, ortam bilgilerini toplamak için cihazınızı taşımak isteyip istemediğiniz sorulur. Daha sonra, dünyanın içine bir bağlantı yerleştirirsiniz, bu işlemin kaydedilmesini bekler ve bu şekilde devam edersiniz.

### <a name="deploy-to-an-ios-device"></a>İOS cihazına dağıtma

**Dosya** derlemeayarları'nıseçerek > derleme ayarlarını açın.

**Yapı**' ın altında, tüm sahnelerin yanında onay işareti olduğundan emin olun.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Uygulama başladıktan sonra, **bir demo Seç** iletişim kutusunda, **localshare** seçeneğini belirlemek için sol veya sağ ok tuşlarını kullanın ve **Git!** ' e dokunun. Uygulamadaki yönergeleri izleyin. **Oluşturma & paylaşım bağlantısı** ' nı veya **paylaşılan bağlayıcıyı bul**' u seçebilirsiniz.

İlk senaryo, daha sonra aynı cihazda veya farklı bir aygıtta yer alan bir bağlantı oluşturmanıza olanak sağlar.
İkinci senaryo, aynı cihazda ya da farklı bir uygulamada uygulamayı zaten çalıştırırsanız, daha önce paylaşılan bağlantıları bulmanızı sağlar. Senaryonuzu belirledikten sonra uygulama, ne yapacaklarına ilişkin daha fazla yönerge için size kılavuzluk eder. Örneğin, ortam bilgilerini toplamak için cihazınızı taşımak isteyip istemediğiniz sorulur. Daha sonra, dünyanın içine bir bağlantı yerleştirirsiniz, bu işlemin kaydedilmesini bekler ve bu şekilde devam edersiniz.

Xcode 'da **Durdur**' u seçerek uygulamayı durdurun.
