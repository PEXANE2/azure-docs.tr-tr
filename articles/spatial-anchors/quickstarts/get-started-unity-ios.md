---
title: 'Quickstart: Unity iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak Unity ile bir iOS uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 37a82c4001dd42a4cfbbb9dabec29f28359afd75
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240593"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure Uzamsal Çapalarla Bir Birlik iOS uygulaması oluşturun

Bu hızlı başlangıç, [Azure Uzamsal Bağlantılarını](../overview.md)kullanarak bir Unity iOS uygulamasının nasıl oluşturulabildiğini kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, bir uzamsal çapayı kaydedip geri çağırabilen Birbirliğe sahip bir ARKit iOS uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Unity yapı ayarlarını hazırlama
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Xcode projesini dışa aktarma
> * iOS aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.1 veya 2019.2</a>ile bir macOS makinesi , <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a>en son sürümü , ve <a href="https://cocoapods.org" target="_blank">CocoaPods</a> yüklü.
- Git HomeBrew üzerinden yüklü. Aşağıdaki komutu Terminalin tek bir `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`satırına girin: . Sonra, `brew install git` koş `brew install git-lfs`ve.
- Bir geliştirici <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> iOS aygıtını etkinleştirdim.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity örnek projesini indirin ve açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

**Proje** bölmesinde, sahne `Assets/AzureSpatialAnchors.Examples/Scenes` dosyasına `AzureSpatialAnchorsBasicDemo.unity` gidin ve açın.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

**Dosya** -> Kaydet'i seçerek sahneyi**kaydedin.**

## <a name="export-the-xcode-project"></a>Xcode projesini dışa aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Bir çapa yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Bittiğinde, Xcode'da **Durdur** tuşuna basarak uygulamayı durdurun.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="rendering-issues"></a>Oluşturma sorunları

Uygulamayı çalıştırırken, kamerayı arka plan olarak görmüyorsanız (örneğin boş, mavi veya diğer dokular görürsünüz) büyük olasılıkla Varlıkları Unity'de yeniden içe aktarmanız gerekir. Uygulamayı durdurun. Unity'deki üst menüden **Varlıklar -> Yeniden tümlerini içe aktar'ı**seçin. Ardından uygulamayı yeniden çalıştırın.

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>macOS Catalina cocoaPods sorunları (10.15)

Yakın zamanda macOS Catalina 'ya (10.15) güncellediyseniz ve CocoaPods önceden yüklendiyse, CocoaPods bozuk durumda olabilir `.xcworkspace` ve bölmelerinizi ve proje dosyalarınızı düzgün bir şekilde yapılandıramayabilir. Bu sorunu gidermek için, aşağıdaki komutları çalıştırarak CocoaPods yeniden yüklemeniz gerekir:

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

### <a name="unity-20193"></a>Birlik 2019.3

Kırılan değişiklikler nedeniyle, Unity 2019.3 şu anda desteklenmiyor. Lütfen Unity 2019.1 veya 2019.2'yi kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
