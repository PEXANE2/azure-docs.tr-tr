---
title: 'Quickstart: Xamarin Android uygulaması oluşturun'
description: Bu hızlı başlangıçta, Uzamsal Çapalar kullanarak Xamarin ile bir Android uygulaması oluşturmak için nasıl öğrenirler.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 94906d645ec38a24d54536ee8aa93e7418c8dc35
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75465185"
---
# <a name="quickstart-create-a-xamarin-android-app-with-azure-spatial-anchors"></a>Quickstart: Azure Uzamsal Çapalar ile bir Xamarin Android uygulaması oluşturun

Bu hızlı başlangıç, [Azure Uzamsal Çapaları](../overview.md)kullanarak Xamarin ile bir Android uygulamasının nasıl oluşturulabildiğini kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, uzamsal bir çapayı kaydedip geri çağırabilen bir Android uygulamanız olur.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Android aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- Bir Windows veya macOS bilgisayarı:
  - Windows kullanıyorsanız:
    - <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019 16.2+</a>'nın güncel bir sürümü.
    - <a href="https://git-scm.com/download/win" target="_blank">Windows için Git</a>.
    - <a href="https://git-lfs.github.com/">Git LFS</a>.
  - macOS kullanıyorsanız:
    - <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Mac 8.1+ için Visual Studio'nun</a>güncel bir sürümü.
    - <a href="https://git-scm.com/download/mac" target="_blank">macOS için Git</a>.
    - <a href="https://git-lfs.github.com/">Git LFS</a>.
- Xamarin.Android en son sürümü yüklü ve seçtiğiniz platformda çalışıyor. Xamarin.Android yüklemek için bir rehber için, [Xamarin.Android Yükleme](https://docs.microsoft.com/xamarin/android/get-started/installation/index) kılavuzlarıbakın.
- Bir <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore yetenekli</a> Android cihaz.
  - Bilgisayarınızın Android cihazınızla iletişim kurması için ek aygıt sürücüleri gerekebilir. Daha fazla bilgi için [buraya](https://developer.android.com/studio/run/device.html)bakın.
- Uygulamanız ARCore **1.8**hedef olmalıdır.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açma

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Visual `Xamarin/SampleXamarin.sln` Studio'da aç.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

Bir sonraki adım, uygulamanın hesap tanımlayıcınızı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`Xamarin/SampleXamarin.Common/AccountDetails.cs` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

## <a name="deploy-the-app-to-your-android-device"></a>Uygulamayı Android cihazınıza dağıtın

Android aygıtında güç, oturum açın ve usb kablosu kullanarak bilgisayara bağlayın.

Başlangıç projesini **SampleXamarin.Android**olarak ayarlayın, **Çözüm Yapılandırmasını** **Serbest Bırakmak**için değiştirin ve aygıt seçici açılır durumda dağıtmak istediğiniz aygıtı seçin.

# <a name="windows"></a>[Windows](#tab/deploy-windows)

![Görsel Stüdyo Yapılandırması](./media/get-started-xamarin-android/visual-studio-windows-configuration.png)

Uygulamanızı dağıtmak ve başlatmak için **Hata** > **Ayıklama Başlat'ı** seçin.

# <a name="macos"></a>[Macos](#tab/deploy-macos)

![Görsel Stüdyo Yapılandırması](./media/get-started-xamarin-android/visual-studio-macos-configuration.jpg)

Uygulamanızı dağıtmak ve başlatmak için > **Hata Ayıklama olmadan** **Çalıştır'ı**seçin.

---

Uygulamada, demoyu çalıştırmak ve bir çapa yerleştirmek ve geri çağırmak için yönergeleri izlemek için **Temel'i** seçin.

> ![Ekran](./media/get-started-xamarin-android/screenshot-1.jpg)
> ![Görüntüsü](./media/get-started-xamarin-android/screenshot-2.jpg)
> ![1 Ekran Görüntüsü 2 Ekran Görüntüsü 3](./media/get-started-xamarin-android/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
