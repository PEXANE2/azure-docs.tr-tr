---
title: 'Hızlı başlangıç: Xamarin Android uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak Xamarin ile bir Android uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 94906d645ec38a24d54536ee8aa93e7418c8dc35
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75465185"
---
# <a name="quickstart-create-a-xamarin-android-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure uzamsal bağlayıcılarla bir Xamarin Android uygulaması oluşturma

Bu hızlı başlangıçta, [Azure uzamsal bağlayıcıları](../overview.md)kullanarak Xamarin Ile bir Android uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedip geri çekebilmeniz için bir Android uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Android cihazında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- Bir Windows veya macOS bilgisayarı:
  - Windows kullanıyorsanız:
    - <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019 16.2 +</a>' un güncel bir sürümü.
    - <a href="https://git-scm.com/download/win" target="_blank">Windows Için git</a>.
    - <a href="https://git-lfs.github.com/">GIT LFS</a>.
  - MacOS kullanılıyorsa:
    - <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Mac için Visual Studio 8.1 +</a>' nin güncel sürümü.
    - <a href="https://git-scm.com/download/mac" target="_blank">MacOS Için git</a>.
    - <a href="https://git-lfs.github.com/">GIT LFS</a>.
- Xamarin. Android ' in en son sürümü, tercih ettiğiniz platformda yüklü ve çalışır. Xamarin. Android Yükleme Kılavuzu için [Xamarin. Android yükleme](https://docs.microsoft.com/xamarin/android/get-started/installation/index) kılavuzlarını inceleyin.
- <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">Geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">korumalı</a> bir Android cihaz.
  - Bilgisayarınızın Android cihazından iletişim kurması için ek cihaz sürücüleri gerekebilir. Daha fazla bilgi için [buraya](https://developer.android.com/studio/run/device.html)bakın.
- Uygulamanızın ARCore **1,8**' i hedeflemesi gerekir.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Visual `Xamarin/SampleXamarin.sln` Studio 'da açın.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`Xamarin/SampleXamarin.Common/AccountDetails.cs` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve hesap anahtarıyla `Set me` değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve hesap tanımlayıcısıyla `Set me` değiştirin.

## <a name="deploy-the-app-to-your-android-device"></a>Uygulamayı Android cihazınıza dağıtma

Android cihazda güç açın, oturum açın ve USB kablosu kullanarak bilgisayara bağlayın.

Başlangıç projesini **Samplexamarin. Android**olarak ayarlayın, **çözüm yapılandırmasını** **Yayınla**değiştirin ve cihaz Seçicisi açılır penceresinde dağıtmak istediğiniz cihazı seçin.

# <a name="windows"></a>[Windows](#tab/deploy-windows)

![Visual Studio yapılandırması](./media/get-started-xamarin-android/visual-studio-windows-configuration.png)

Uygulamanızı dağıtmak ve başlatmak için hata**ayıklamayı Başlat** ' **ı seçin.** > 

# <a name="macos"></a>[Mac OS](#tab/deploy-macos)

![Visual Studio yapılandırması](./media/get-started-xamarin-android/visual-studio-macos-configuration.jpg)

Uygulamanızı dağıtmak ve başlatmak için**hata ayıklama olmadan Başlat** ' **ı seçin.** > 

---

Uygulamada, tanıtımı çalıştırmak için **temel** ' yı seçin ve bir tutturucu yerleştirip geri çekmeniz için yönergeleri izleyin.

> ![Ekran görüntüsü](./media/get-started-xamarin-android/screenshot-1.jpg)
> ![1 ekran](./media/get-started-xamarin-android/screenshot-2.jpg)
> ![görüntüsü 2 ekran görüntüsü 3](./media/get-started-xamarin-android/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)
