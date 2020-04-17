---
title: 'Quickstart: Unity ile bir HoloLens uygulaması oluşturun'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak Unity ile bir HoloLens uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9e946d7f4616799768711ee8c18584a2a8fcff2a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81482003"
---
# <a name="quickstart-create-a-unity-hololens-app-that-uses-azure-spatial-anchors"></a>Hızlı başlangıç: Azure Uzamsal Çapaları kullanan bir Unity HoloLens uygulaması oluşturun

Bu hızlı başlangıçta, [Azure Uzamsal Çapaları](../overview.md)kullanan bir Unity HoloLens uygulaması oluşturursunuz. Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye devam eden nesnelerle karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, bir uzamsal çapayı kaydedip geri çağırabilen Unity ile oluşturulmuş bir HoloLens uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

- Uzamsal Çapalar hesabı oluşturun.
- Unity yapı ayarlarını hazırlayın.
- Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın.
- HoloLens Visual Studio projesini dışa aktarın.
- Uygulamayı dağıtın ve holoLens aygıtında çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

- <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.1 veya 2019.2</a> ve <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> veya sonraki yüklü bir Windows bilgisayarınıza ihtiyacınız vardır. Visual Studio yüklemeniz **Evrensel Windows Platformu geliştirme** iş yükünü ve **Windows 10 SDK (10.0.18362.0 veya daha yeni)** bileşenini içermelidir. Ayrıca Windows ve Git <a href="https://git-lfs.github.com/">LFS</a> <a href="https://git-scm.com/download/win" target="_blank">için Git</a> yüklemeniz gerekir.
- [Geliştirici modunun](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin olduğu bir HoloLens aygıtına ihtiyacınız vardır. [Windows 10 Ekim 2018 Güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018) (RS5 olarak da bilinir) aygıta yüklenmiş olmalıdır. HoloLens'teki en son sürümde güncelleme yapmak için **Ayarlar** uygulamasını açın, **& Güvenliği Güncelleştir'e**gidin ve ardından **güncelleştirmeleri**denetle'yi seçin.
- Uygulamanızda, **Uzamsal Algılama** yeteneğini etkinleştirmeniz gerekir. Bu ayar **Yapı Ayarları** > **Oyuncu Ayarları** > **Yayımlama Ayarları** > **Özellikleri'ndedir.**
- Uygulamanızda, **Windows Mixed Reality SDK**ile **desteklenen Sanal Gerçeklik'i** etkinleştirmeniz gerekir. Bu ayar **Yapı Ayarları** > **Oyuncu Ayarları** > **XR Ayarları'** ndadır.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity örnek projesini indirin ve açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

 >  **Dosya**Oluşturma Ayarları'nı seçerek **Yapı Ayarlarını** Açın.**Build Settings**

**Platform** bölümünde Evrensel **Windows Platformu'nu**seçin. Hedef **Aygıtı** **HoloLens**olarak değiştirin.

Platformu **Evrensel Windows Platformu**olarak değiştirmek için Platform'u **Değiştir'i** seçin. Unity, eksikse UWP destek bileşenlerini yüklemenizi isteyebilir.

![Unity Yapı Ayarları penceresi](./media/get-started-unity-hololens/unity-build-settings.png)

Yapı **Ayarları** penceresini kapatın.

## <a name="configure-the-account-identifier-and-key"></a>Hesap tanımlayıcısını ve anahtarı nı yapılandırma

**Proje** bölmesinde, sahne `Assets/AzureSpatialAnchors.Examples/Scenes` dosyasına `AzureSpatialAnchorsBasicDemo.unity` gidin ve açın.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

**Dosya** > Kaydet'i seçerek sahneyi**kaydedin.**

## <a name="export-the-hololens-visual-studio-project"></a>HoloLens Visual Studio projesini dışa aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

**Yapı'yı**seçin. İletişim kutusunda, HoloLens Visual Studio projesini dışa aktarmak için bir klasör seçin.

Dışa aktarım tamamlandığında, dışa aktarılan HoloLens projesini içeren bir klasör görüntülenir.

## <a name="deploy-the-hololens-application"></a>HoloLens uygulamasını dağıtın

Klasörde, Visual Studio'da projeyi açmak için **HelloAR U3D.sln'yi** çift tıklatın.

Çözüm **Yapılandırmasını** **Sürüm**olarak değiştirin, **Çözüm Platformu'nu** **x86**olarak değiştirin ve dağıtım hedef seçeneklerinden **Aygıt'ı** seçin.

HoloLens 2 kullanıyorsanız, **x86**yerine **ÇÖZÜM Platformu**olarak **ARM64** kullanın.

   ![Visual Studio yapılandırması](./media/get-started-unity-hololens/visual-studio-configuration.png)

HoloLens cihazını açın, oturum açın ve usb kablosu kullanarak cihazı BILGISAYARa bağlayın.

Uygulamanızı dağıtmak ve hata ayıklamaya başlamak için **Hata** > **Ayıklama Başlat'ı** seçin.

Bir çapa yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Visual Studio'da Hata **Ayıklamayı Durdur** veya Shift+F5'i seçerek uygulamayı durdurun.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unity-20193"></a>Birlik 2019.3

Kırılan değişiklikler nedeniyle, Unity 2019.3 şu anda desteklenmiyor. Lütfen Unity 2019.1 veya 2019.2'yi kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal çapaları aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
