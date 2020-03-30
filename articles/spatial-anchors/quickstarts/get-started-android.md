---
title: 'Quickstart: Android uygulaması oluşturma'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak bir Android uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 0501c8bb1d71c6cff6033fc937cda019c8890056
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75376469"
---
# <a name="quickstart-create-an-android-app-with-azure-spatial-anchors"></a>Quickstart: Azure Uzamsal Çapalar ile bir Android uygulaması oluşturun

Bu hızlı başlangıç, Java veya C++/NDK'da [Azure Uzamsal Çapaları](../overview.md) kullanarak bir Android uygulamasının nasıl oluşturulacağını kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, uzamsal bir çapayı kaydedip geri çağırabilen bir ARCore Android uygulamanız olur.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Android aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.4+</a>ile bir Windows veya macOS makinesi.
  - Windows'da çalışıyorsanız, Windows <a href="https://git-scm.com/download/win" target="_blank">için Git</a> ve <a href="https://git-lfs.github.com/">Git LFS'ye</a>de ihtiyacınız vardır.
  - macOS'ta çalışıyorsanız, Git'in HomeBrew üzerinden yüklenmesini sorun. Aşağıdaki komutu Terminalin tek bir `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`satırına girin: . Sonra, `brew install git` koş `brew install git-lfs`ve.
  - NDK örneğini oluşturmak için, Android Studio'da NDK ve CMake 3.6 veya daha büyük SDK Araçlarını da yüklemeniz gerekir.
- Bir <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore yetenekli</a> Android cihaz.
  - Bilgisayarınızın Android cihazınızla iletişim kurması için ek aygıt sürücüleri gerekebilir. Ek bilgi ve talimatlar için [buraya](https://developer.android.com/studio/run/device.html) bakın.
- Uygulamanız ARCore **1.11.0**hedef olmalıdır.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açma

# <a name="java"></a>[Java](#tab/openproject-java)

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

# <a name="ndk"></a>[Ndk](#tab/openproject-ndk)

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Buradan `arcore_c_api.h` [here](https://raw.githubusercontent.com/google-ar/arcore-android-sdk/v1.11.0/libraries/include/arcore_c_api.h) indirin ve `Android\NDK\libraries\include`yerleştirin .

Yeni klonlanan deponun içinden, aşağıdaki komutu çalıştırarak alt modülleri başlatma:

```console
git submodule update --init --recursive
```

---

Android Studio’yu açın.

# <a name="java"></a>[Java](#tab/openproject-java)

**Varolan bir Android Studio projesini** aç'ı `Android/Java/`seçin ve 'de bulunan projeyi seçin.

# <a name="ndk"></a>[Ndk](#tab/openproject-ndk)

**Varolan bir Android Studio projesini** aç'ı `Android/NDK/`seçin ve 'de bulunan projeyi seçin.

---

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

Bir sonraki adım, uygulamanın hesap tanımlayıcınızı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

# <a name="java"></a>[Java](#tab/openproject-java)

`Android/Java/app/src/main/java/com/microsoft/sampleandroid/AzureSpatialAnchorsManager.java` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

# <a name="ndk"></a>[Ndk](#tab/openproject-ndk)

`Android/NDK/app/src/main/cpp/AzureSpatialAnchorsApplication.cpp` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

---

## <a name="deploy-the-app-to-your-android-device"></a>Uygulamayı Android cihazınıza dağıtın

Android aygıtında güç, oturum açın ve usb kablosu kullanarak pc'ye bağlayın.

Android Studio araç çubuğundan **Çalıştır'ı** seçin.

![Android Studio Dağıt ve Çalıştır](./media/get-started-android/android-studio-deploy-run.png)

**Dağıtım Hedefini Seç** iletişim kutusundaAndroid aygıtını seçin ve uygulamayı Android aygıtında çalıştırmak için **Tamam'ı** seçin.

Bir çapa yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Android Studio araç çubuğundan **Durdur'u** seçerek uygulamayı durdurun.

![Android Studio Stop](./media/get-started-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
