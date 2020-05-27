---
title: 'Hızlı başlangıç: iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak bir iOS uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.custom: has-adal-ref
ms.openlocfilehash: 542b5168bb70d876d91f8a747a230f2b754568e9
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873116"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Hızlı başlangıç: Azure uzamsal bağlayıcılarla, Swift veya amaç-C ' d a iOS uygulaması oluşturma

Bu hızlı başlangıçta, Swift veya amaç-C ' d e [Azure uzamsal bağlayıcılarını](../overview.md) kullanarak bir iOS uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedip geri çekebilmeniz için bir ARKit iOS uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * İOS cihazında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- En son <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> ve <a href="https://cocoapods.org" target="_blank">Cocoapods</a> sürümü yüklü olan MacOS makinesi özellikli bir geliştirici.
- HomeBrew aracılığıyla Git yüklendi:
  1. Terminalde aşağıdaki komutu tek bir satır olarak girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` . 
  1. `brew install git` ve `brew install git-lfs` komutlarını çalıştırın.
  1. Git yapılandırmasını `git lfs install` (geçerli kullanıcı için) veya `git lfs install --system` (tüm sistem için) ile güncelleştirin.
- Bir geliştirici, <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> bir iOS cihazını etkinleştirdi.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

Aşağıdaki eylemleri gerçekleştirmek için terminali kullanın.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Cocoapods kullanarak gerekli Pod 'yi yükler:

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

`pod install --repo-update`Proje Için Cocoapods yüklemek üzere çalıştırın.

Şimdi `.xcworkspace` Xcode 'da açın.

> [!NOTE]
> MacOS Catalina 'e yükselttikten sonra CocoaPod sorunları [yaşıyorsanız, sorun giderme adımlarına bakın](#cocoapods-issues-on-macos-catalina-1015) (10,15).

# <a name="swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

# <a name="swift"></a>[Swift](#tab/openproject-swift)

`iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift` dosyasını açın.

Alanı bulun `spatialAnchorsAccountKey` ve `Set me` Hesap anahtarıyla değiştirin.

Alanı bulun `spatialAnchorsAccountId` ve `Set me` Hesap tanımlayıcısıyla değiştirin.

`startSession()`Hesap etki alanında yer alan aşağıdaki satırı bulun ve daha önce ekleyin: `cloudSession!.configuration.accountDomain = "MyAccountDomain";` .

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

`iOS/Objective-C/SampleObjC/BaseViewController.m` dosyasını açın.

Alanı bulun `SpatialAnchorsAccountKey` ve `Set me` Hesap anahtarıyla değiştirin.

Alanı bulun `SpatialAnchorsAccountId` ve `Set me` Hesap tanımlayıcısıyla değiştirin.

`startSession()`Hesap etki alanında yer alan aşağıdaki satırı bulun ve daha önce ekleyin: `_cloudSession.configuration.accountDomain = @"MyAccountDomain";` .

---

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazını Mac 'e bağlayın ve **etkin düzeni** iOS cihazınıza ayarlayın.

![Cihazı seçin](./media/get-started-ios/select-device.png)

**Oluştur ' u seçin ve ardından geçerli düzeni çalıştırın**.

![Dağıt ve Çalıştır](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Bir hata görürseniz, `library not found for -lPods-SampleObjC` büyük olasılıkla `.xcodeproj` dosyası yerine dosyayı açtınız `.xcworkspace` . Öğesini açın `.xcworkspace` ve yeniden deneyin.

Xcode 'da **Durdur**' a basarak uygulamayı durdurun.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>MacOS Catalina CocoaPods sorunlar (10,15)

Yakın zamanda MacOS Catalina (10,15) olarak güncelleştirdiyseniz ve önceden Cocoapods yüklüyse, Cocoapods bozuk bir durumda olabilir ve pod ve proje dosyalarınızı düzgün şekilde yapılandıramadı `.xcworkspace` . Bu sorunu çözmek için aşağıdaki komutları çalıştırarak CocoaPods öğesini yeniden yüklemeniz gerekir:

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

### <a name="app-crashes-when-deploying-to-ios-1031-from-a-personal-provisioning-profiledeveloper-account"></a>Bir kişisel sağlama profili/geliştirici hesabından iOS 10.3.1 'a dağıtım yaparken uygulama kilitleniyor 

İOS uygulamanızı bir kişisel sağlama profili/geliştirici hesabından iOS 10.3.1 üzerinde dağıtırsanız şu hatayı görebilirsiniz: `Library not loaded: @rpath/ADAL...` . 

Sorunu çözmek için:

- Kişisel bir ekip profili (ücretli geliştirici hesabı) olmayan bir sağlama profili kullanın.
- Uygulamanızı iOS 13,3 veya önceki bir sürümü çalıştıran bir iOS cihazına veya iOS 13,4 Beta ya da yayın sürümünü çalıştıran bir iOS cihazına dağıtın.
- Bu sorun hakkında daha fazla bilgi için [Stack Overflow](https://stackoverflow.com/questions/60015309/running-ios-apps-causes-runtime-error-for-frameworks-code-signature-invalid).


[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)
