---
title: 'Hızlı başlangıç: Unity iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak Unity ile bir iOS uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 09/29/2020
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 630e90069cd1b9735dbaec4c0d825be71df7e69c
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91539383"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure uzamsal bağlayıcılarla Unity iOS uygulaması oluşturma

Bu hızlı başlangıçta, [Azure uzamsal bağlayıcılarını](../overview.md)kullanarak Unity iOS uygulaması oluşturma ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedeve geri çekebilmeniz için Unity ile oluşturulmuş bir ARKit iOS uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Unity derleme ayarlarını hazırlama
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * Xcode projesini dışa aktarma
> * İOS cihazında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019,4 (LTS)</a>Içeren bir MacOS makinesi, en son <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> sürümü yüklendi.
- Git, HomeBrew aracılığıyla yüklendi. Terminalin tek bir satırına şu komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` . Ardından, ve öğesini çalıştırın `brew install git` `brew install git-lfs` .
- Bir geliştirici, <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit uyumlu</a> bir iOS cihazını etkinleştirdi.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity örnek projesini indir ve aç

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

## <a name="export-the-xcode-project"></a>Xcode projesini dışa aktarma

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Uygulamada, okları kullanarak **Basicdemo** ' i seçin ve git ' e basın **!** gösteriyi çalıştırmak için düğme. Bir bağlayıcıyı yerleştirmek ve geri çağırmak için yönergeleri izleyin.

![Ekran görüntüsü 1 ](./media/get-started-unity-ios/screenshot-1.jpg)
 ![ ekran görüntüsü 2 ](./media/get-started-unity-ios/screenshot-2.jpg)
 ![ ekran görüntüsü 3](./media/get-started-unity-ios/screenshot-3.jpg)

İşiniz bittiğinde, Xcode 'da **Durdur** ' a basarak uygulamayı durdurun.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="rendering-issues"></a>İşleme sorunları

Uygulamayı çalıştırırken, kamerayı arka plan olarak görmüyorsanız (örneğin, boş, mavi veya diğer dokular görürseniz), muhtemelen varlıkları Unity 'de yeniden içeri aktarmanız gerekir. Uygulamayı durdurun. Unity 'deki üst menüden varlıklar ' ı seçin **> tümünü yeniden içeri aktarın**. Sonra, uygulamayı yeniden çalıştırın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)

> [!div class="nextstepaction"]
> [Nasıl yapılır: Unity projesinde Azure uzamsal bağlayıcılarını yapılandırma](../how-tos/setup-unity-project.md)
