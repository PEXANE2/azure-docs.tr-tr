---
title: 'Quickstart: DirectX ile HoloLens uygulaması oluşturun'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak bir HoloLens uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: virivera
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 01a9a8239e2997335f5123d63cfc664027cffa02
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75376401"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Hızlı başlangıç: C++/WinRT ve DirectX'te Azure Uzamsal Çapaları ile bir HoloLens uygulaması oluşturun

Bu hızlı başlangıç, C++/WinRT ve DirectX'te [Azure Uzamsal Çapaları](../overview.md) kullanarak bir HoloLens uygulamasının nasıl oluşturulacak olduğunu kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, uzamsal bir çapayı kaydedip geri çağırabilen bir HoloLens uygulamasına sahip olacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * HoloLens aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- Visual Studio <a href="https://www.visualstudio.com/downloads/" target="_blank">2019</a> ile bir Windows makinesi **Evrensel Windows Platformu geliştirme** iş yükü ve Windows **10 SDK (10.0.18362.0 veya daha yeni)** bileşeni ile yüklü. Ayrıca Windows ve Git <a href="https://git-lfs.github.com/">LFS</a> <a href="https://git-scm.com/download/win" target="_blank">için Git</a> yüklemeniz gerekir.
- Visual Studio için [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Marketplace'ten](https://marketplace.visualstudio.com/)yüklenmelidir.
- [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin bir HoloLens cihazı. Bu makale, [Windows 10 Ekim 2018 Güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir) içeren bir HoloLens aygıtı gerektirir. HoloLens'teki en son sürümde güncelleme yapmak için **Ayarlar** uygulamasını açın, **& Güvenliği Güncelleştir'e**gidin ve ardından **güncellemeler için Kontrol** düğmesini seçin.
- Uygulamanız, AppX manifestosunda **mekansal Algılama** özelliğini belirlemelidir.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açma

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Visual `HoloLens\DirectX\SampleHoloLens.sln` Studio'da aç.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

Bir sonraki adım, uygulamanın hesap tanımlayıcınızı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`HoloLens\DirectX\SampleHoloLens\ViewController.cpp` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

## <a name="deploy-the-app-to-your-hololens"></a>Uygulamayı HoloLens'inize dağıtın

Çözüm **Yapılandırmasını** **Sürüm**olarak değiştirin, **Çözüm Platformu'nu** **x86**olarak değiştirin ve dağıtım hedef seçeneklerinden **Aygıt'ı** seçin.

HoloLens 2 kullanıyorsanız, **x86**yerine **ÇÖZÜM Platformu**olarak **ARM64** kullanın.

![Görsel Stüdyo Yapılandırması](./media/get-started-hololens/visual-studio-configuration.png)

HoloLens aygıtında güç, oturum açın ve usb kablosu kullanarak PC'ye bağlayın.

Uygulamanızı dağıtmak ve hata ayıklamaya başlamak için **Hata** > **Ayıklama Başlat'ı** seçin.

Bir çapa yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Visual Studio'da Hata **Ayıklamayı Durdur'u** seçerek veya **Shift + F5 tuşuna**basarak uygulamayı durdurun.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
