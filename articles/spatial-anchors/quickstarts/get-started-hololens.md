---
title: Hızlı başlangıç-Azure uzamsal bağlayıcılarla HoloLens uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak bir HoloLens uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 94cae186cee099618772f53d2b820e12f20cad64
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562405"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Hızlı Başlangıç: /Wınrt ve DirectX içinde C++Azure uzamsal bağlayıcılarla bir HoloLens uygulaması oluşturma

Bu hızlı başlangıçta,/Wınrt ve DirectX 'teki C++ [Azure uzamsal bağlayıcılarını](../overview.md) kullanarak bir HoloLens uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedip geri çekebilmeniz için bir HoloLens uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * HoloLens cihazında dağıtın ve çalıştırın

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.18362.0 veya üzeri)** bileşeni ve <a href="https://git-scm.com/download/win" target="_blank">Windows için git</a>Ile <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> yüklü bir Windows makinesi.
- Visual Studio için [ C++/wınrt Visual Studio uzantısı (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Market](https://marketplace.visualstudio.com/)yüklenmelidir.
- [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin bir HoloLens cihazı. Bu makalede, [Windows 10 ekim 2018 güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir) Ile bir HoloLens cihazı gerekir. HoloLens 'te en son sürüme güncelleştirmek için **Ayarlar** uygulamasını açın, **güncelleştirme & güvenlik**' e gidin ve **Güncelleştirmeleri denetle** düğmesini seçin.
- Uygulamanızın, AppX bildiriminde **Spatıalperception** özelliğini ayarlaması gerekir.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Visual `HoloLens\DirectX\SampleHoloLens.sln` Studio 'da açın.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

Açık `HoloLens\DirectX\SampleHoloLens\ViewController.cpp`.

Alanı bulun ve hesap anahtarıyla `Set me`değiştirin. `SpatialAnchorsAccountKey`

Alanı bulun ve hesap tanımlayıcısıyla `Set me`değiştirin. `SpatialAnchorsAccountId`

## <a name="deploy-the-app-to-your-hololens"></a>Uygulamayı HoloLens 'e dağıtın

**Çözüm yapılandırmasını** **yayın**olarak değiştirin, **çözüm platformunu** **x86**olarak değiştirin ve dağıtım hedefi seçeneklerinden **cihaz** ' ı seçin.

HoloLens 2 kullanıyorsanız, **x86**yerine **ARM** 'yi **çözüm platformu**olarak kullanın.

![Visual Studio yapılandırması](./media/get-started-hololens/visual-studio-configuration.png)

HoloLens cihazında güç açın, oturum açın ve USB kablosu kullanarak BILGISAYARA bağlayın.

Uygulamanızı dağıtmak ve hata ayıklamayı başlatmak için hata**ayıklamayı Başlat** ' **ı seçin.**  > 

Bir bağlayıcıyı yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Visual Studio 'da, **hata ayıklamayı Durdur** ' u seçerek veya **SHIFT + F5**tuşlarına basarak uygulamayı durdurun.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal bağlayıcıları cihazlarda paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)
