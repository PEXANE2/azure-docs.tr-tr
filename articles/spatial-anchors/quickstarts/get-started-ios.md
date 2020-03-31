---
title: 'Quickstart: bir iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak bir iOS uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 6304077a26f5c0ecb91e1ec4936bd79b3d839d95
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79471226"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Hızlı başlangıç: Azure Uzamsal Çapaları ile Swift veya Objective-C'de bir iOS uygulaması oluşturun

Bu hızlı başlangıç, Swift veya Objective-C'de [Azure Uzamsal Çapaları](../overview.md) kullanarak bir iOS uygulamasının nasıl oluşturulacak olduğunu kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, uzamsal bir çapayı kaydedip geri çağırabilen bir ARKit iOS uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * iOS aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir geliştirici <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> ve <a href="https://cocoapods.org" target="_blank">CocoaPods</a> en son sürümü yüklü macOS makine etkin.
- Git HomeBrew üzerinden yüklü:
  1. Terminalde tek bir satır olarak aşağıdaki `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`komutu girin: . 
  1. `brew install git` ve `brew install git-lfs` komutlarını çalıştırın.
  1. Git config'inizi `git lfs install` (geçerli kullanıcı için) veya `git lfs install --system` (tüm sistem için) ile güncelleyin.
- Bir geliştirici <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> iOS aygıtını etkinleştirdim.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açma

Aşağıdaki eylemleri gerçekleştirmek için Terminal'i kullanın.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

CocoaPods kullanarak gerekli bölmeleri yükleyin:

# <a name="swift"></a>[Swift](#tab/openproject-swift)

`iOS/Swift/` sayfasına gidin.

```bash
cd ./iOS/Swift/
```

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

`iOS/Objective-C/` sayfasına gidin.

```bash
cd ./iOS/Objective-C/
```

---

Proje `pod install --repo-update` için CocoaPods yüklemek için çalıştırın.

Şimdi Xcode'u `.xcworkspace` açın.

> [!NOTE]
> MacOS Catalina'ya (10.15) yükselttikten sonra CocoaPod sorunları yaşıyorsanız sorun giderme adımlarını [buradan](#cocoapods-issues-on-macos-catalina-1015) görebilirsiniz.

# <a name="swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

Bir sonraki adım, uygulamanın hesap tanımlayıcınızı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

# <a name="swift"></a>[Swift](#tab/openproject-swift)

`iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift` dosyasını açın.

`spatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`spatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

`iOS/Objective-C/SampleObjC/BaseViewController.m` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

---

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS aygıtınıza dağıtma

iOS aygıtını Mac'e bağlayın ve **etkin düzeni** iOS aygıtınıza ayarlayın.

![Cihazı seçin](./media/get-started-ios/select-device.png)

Yapı'yı seçin **ve ardından geçerli düzeni çalıştırın.**

![Dağıt Ve çalıştır](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Bir `library not found for -lPods-SampleObjC` hata görürseniz, büyük `.xcodeproj` olasılıkla dosyayı `.xcworkspace`' nın yerine açmışsınızdır. Açın `.xcworkspace` ve tekrar deneyin.

Xcode'da, **Durdur'a**basarak uygulamayı durdurun.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>macOS Catalina cocoaPods sorunları (10.15)

Yakın zamanda macOS Catalina 'ya (10.15) güncellediyseniz ve CocoaPods önceden yüklendiyse, CocoaPods bozuk durumda olabilir `.xcworkspace` ve bölmelerinizi ve proje dosyalarınızı düzgün bir şekilde yapılandıramayabilir. Bu sorunu gidermek için, aşağıdaki komutları çalıştırarak CocoaPods yeniden yüklemeniz gerekir:

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

### <a name="app-crashes-when-deploying-to-ios-1031-from-a-personal-provisioning-profiledeveloper-account"></a>Kişisel sağlama profili/geliştirici hesabından iOS 10.3.1'e dağıtılırken uygulama çöküyor 

iOS uygulamanızı iOS 10.3.1'de kişisel bir sağlama profilinden/geliştirici hesabından `Library not loaded: @rpath/ADAL...`dağıtıyorsanız, şu hatayı görebilirsiniz: . 

Sorunu gidermek için:

- Kişisel Takım profili olmayan bir sağlama profili (ücretli geliştirici hesabı) kullanın.
- Uygulamanızı iOS 13.3 veya daha önce çalıştıran bir iOS aygıtına veya iOS 13.4 beta veya sürüm sürümünü çalıştıran bir aygıta dağıtın.
- [Stack Overflow](https://stackoverflow.com/questions/60015309/running-ios-apps-causes-runtime-error-for-frameworks-code-signature-invalid)bu konu hakkında daha fazla bilgi edinin.


[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
