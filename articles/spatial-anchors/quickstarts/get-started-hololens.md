---
title: 'Hızlı başlangıç: DirectX ile bir HoloLens uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak bir HoloLens uygulaması oluşturmayı öğreneceksiniz.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c96c45869ee1c9c96cd77d0b3eb10c733199666e
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95993525"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Hızlı başlangıç: C++/Wınrt ve DirectX 'te Azure uzamsal bağlayıcılarla bir HoloLens uygulaması oluşturma

Bu hızlı başlangıçta, C++/Wınrt ve DirectX 'teki [Azure uzamsal bağlayıcılarını](../overview.md) kullanarak bir HoloLens uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedip geri çekebilmeniz için bir HoloLens uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * HoloLens cihazında dağıtın ve çalıştırın

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.18362.0 veya daha yeni)** bileşeniyle birlikte <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> yüklü bir Windows makinesi. Ayrıca, Windows ve <a href="https://git-lfs.github.com/">GIT LFS</a> <a href="https://git-scm.com/download/win" target="_blank">için git</a> 'i de yüklemelisiniz.
- Visual Studio için [C++/Wınrt Visual Studio uzantısı (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Market](https://marketplace.visualstudio.com/)yüklenmelidir.
- [Geliştirici modu](/windows/mixed-reality/using-visual-studio) etkin bir HoloLens cihazı. Bu makalede, [Windows 10 mayıs 2020 güncelleştirmesi](/windows/mixed-reality/whats-new/release-notes-may-2020)Ile bir HoloLens cihazı gerekir. HoloLens 'te en son sürüme güncelleştirmek için **Ayarlar** uygulamasını açın, **güncelleştirme & güvenlik**' e gidin ve **Güncelleştirmeleri denetle** düğmesini seçin.
- Uygulamanızın, AppX bildiriminde **Spatıalperception** özelliğini ayarlaması gerekir.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

`HoloLens\DirectX\SampleHoloLens.sln`Visual Studio 'da açın.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`HoloLens\DirectX\SampleHoloLens\ViewController.cpp` dosyasını açın.

Alanı bulun `SpatialAnchorsAccountKey` ve `Set me` Hesap anahtarıyla değiştirin.

Alanı bulun `SpatialAnchorsAccountId` ve `Set me` Hesap tanımlayıcısıyla değiştirin.

Alanı bulun `SpatialAnchorsAccountDomain` ve `Set me` hesap etki alanı ile değiştirin.

## <a name="deploy-the-app-to-your-hololens"></a>Uygulamayı HoloLens 'e dağıtın

**Çözüm yapılandırmasını** **yayın** olarak değiştirin, **çözüm platformunu** **x86** olarak değiştirin ve dağıtım hedefi seçeneklerinden **cihaz** ' ı seçin.

HoloLens 2 kullanıyorsanız, **x86** yerine **çözüm platformu** olarak **ARM64** kullanın.

![Visual Studio yapılandırması](./media/get-started-hololens/visual-studio-configuration.png)

HoloLens cihazında güç açın, oturum açın ve USB kablosu kullanarak BILGISAYARA bağlayın.

**Debug**  >  Uygulamanızı dağıtmak ve hata ayıklamayı başlatmak için hata **ayıklamayı Başlat** ' ı seçin.

Bir bağlayıcıyı yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

Visual Studio 'da, **hata ayıklamayı Durdur** ' u seçerek veya **SHIFT + F5** tuşlarına basarak uygulamayı durdurun.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)