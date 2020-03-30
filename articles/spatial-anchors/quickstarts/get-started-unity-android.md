---
title: 'Quickstart: Unity Android uygulaması oluşturun'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak Unity ile bir Android uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7acff7f0249cdedcebd367fc315be92cafb9ab78
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77615434"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Quickstart: Azure Uzamsal Çapalar ile Bir Birlik Android uygulaması oluşturun

Bu hızlı başlangıç, [Azure Uzamsal Çapaları](../overview.md)kullanarak bir Unity Android uygulamasının nasıl oluşturulabildiğini kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, bir uzamsal çapayı kaydedip geri çağırabilen Unity ile oluşturulmuş bir ARCore Android uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Unity yapı ayarlarını hazırlama
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Android Studio projesini dışa aktarma
> * Android aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Android Build Support ve Android SDK & NDK Tools modüllerini içeren <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.1 veya 2019.2'ye</a> sahip bir Windows veya macOS makinesi.
  - Windows'da çalışıyorsanız, Windows <a href="https://git-scm.com/download/win" target="_blank">için Git</a> ve <a href="https://git-lfs.github.com/">Git LFS'ye</a>de ihtiyacınız vardır.
  - macOS'ta çalışıyorsanız, Git'in HomeBrew üzerinden yüklenmesini sorun. Aşağıdaki komutu Terminalin tek bir `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`satırına girin: . Sonra, `brew install git` koş `brew install git-lfs`ve.
- Bir <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore yetenekli</a> Android cihaz.
  - Bilgisayarınızın Android cihazınızla iletişim kurması için ek aygıt sürücüleri gerekebilir. Ek bilgi ve talimatlar için [buraya](https://developer.android.com/studio/run/device.html) bakın.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity örnek projesini indirin ve açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

**Proje** bölmesinde, sahne `Assets/AzureSpatialAnchors.Examples/Scenes` dosyasına `AzureSpatialAnchorsBasicDemo.unity` gidin ve açın.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

**Dosya** -> Kaydet'i seçerek sahneyi**kaydedin.**

## <a name="export-the-android-studio-project"></a>Android Studio projesini dışa aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

**Cihazı Çalıştır'da** aygıtınızı seçin ve **Yap ve Çalıştır'ı**tıklatın. Herhangi bir ad seçebileceğiniz `.apk` bir dosyayı kaydetmeniz istenir.

Bir çapa yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="rendering-issues"></a>Oluşturma sorunları

Uygulamayı çalıştırırken, kamerayı arka plan olarak görmüyorsanız (örneğin boş, mavi veya diğer dokular görürsünüz) büyük olasılıkla Varlıkları Unity'de yeniden içe aktarmanız gerekir. Uygulamayı durdurun. Unity'deki üst menüden **Varlıklar -> Tümlerini Yeniden Aktar'ı**seçin. Ardından uygulamayı yeniden çalıştırın.

### <a name="unity-20193"></a>Birlik 2019.3

Kırılan değişiklikler nedeniyle, Unity 2019.3 şu anda desteklenmiyor. Lütfen Unity 2019.1 veya 2019.2'yi kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
