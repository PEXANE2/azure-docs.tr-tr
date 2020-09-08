---
title: 'Hızlı başlangıç: Unity iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak Unity ile bir iOS uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 08/14/2020
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 4d5ec2411b7b1a65da9a1f4262822cb85a3b386e
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536287"
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

Bir bağlayıcıyı yerleştirmek ve geri çağırmak için uygulamadaki yönergeleri izleyin.

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
