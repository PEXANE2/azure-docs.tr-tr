---
title: 'Hızlı başlangıç: Unity Android uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak Unity ile bir Android uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 213cf9b64f2c43274192c22efa2fa5a7dfbce5e5
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277072"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure uzamsal bağlayıcılarla Unity Android uygulaması oluşturma

Bu hızlı başlangıçta, [Azure uzamsal bağlayıcılarını](../overview.md)kullanarak Unity Android uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedip geri çekebilmeniz için Unity ile oluşturulmuş bir ARCore Android uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Unity derleme ayarlarını hazırlama
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Android Studio projeyi dışarı aktarma
> * Android cihazında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Android derleme desteği ve Android SDK & NDK araçları modülleri de dahil olmak üzere <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.1 +</a> Içeren bir Windows veya MacOS makinesi.
  - Windows üzerinde çalışıyorsa, <a href="https://git-scm.com/download/win" target="_blank">Windows Için git</a>de gerekecektir.
  - MacOS üzerinde çalışıyorsa, HomeBrew aracılığıyla Git ' i alın. Terminalin tek bir satırına şu komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Ardından `brew install git`çalıştırın.
- <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">Geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">korumalı</a> bir Android cihaz.
  - Bilgisayarınızın Android cihazından iletişim kurması için ek cihaz sürücüleri gerekebilir. Ek bilgi ve yönergeler için [buraya](https://developer.android.com/studio/run/device.html) bakın.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity örnek projesini indir ve aç

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

**Proje** bölmesinde `Assets/AzureSpatialAnchors.Examples/Scenes` ' a gidin ve `AzureSpatialAnchorsBasicDemo.unity` sahne dosyasını açın.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

**Dosya** -> **Kaydet**' i seçerek sahneyi kaydedin.

## <a name="export-the-android-studio-project"></a>Android Studio projeyi dışarı aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

**Çalıştır cihaz** ' de cihazınızı seçin ve **Oluştur ve Çalıştır**' a tıklayın. İçin herhangi bir ad seçebileceğiniz bir `.apk` dosyasını kaydetmeniz istenecektir.

Bir bağlayıcıyı yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

## <a name="troubleshooting"></a>Sorun giderme

Uygulamayı çalıştırırken, kamerayı arka plan olarak görmüyorsanız (örneğin, boş, mavi veya diğer dokular görürseniz), muhtemelen varlıkları Unity 'de yeniden içeri aktarmanız gerekir. Uygulamayı durdurun. Unity 'deki üst menüden **varlıklar-> tümünü yeniden al**' ı seçin. Sonra, uygulamayı yeniden çalıştırın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)
