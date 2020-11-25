---
title: 'Hızlı başlangıç: Xamarin iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, uzamsal bağlayıcıları kullanarak Xamarin ile bir iOS uygulaması oluşturmayı öğreneceksiniz.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: b62ca4b02a1bd28d150f233adf1dfb621995e1c4
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96009747"
---
# <a name="quickstart-create-a-xamarin-ios-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure uzamsal bağlayıcılarla bir Xamarin iOS uygulaması oluşturma

Bu hızlı başlangıçta, [Azure uzamsal bağlayıcıları](../overview.md)kullanarak Xamarin Ile bir iOS uygulamasının nasıl oluşturulacağı ele alınmaktadır. Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak sağlayan bir platformlar arası geliştirici hizmetidir. İşiniz bittiğinde, bir uzamsal bağlayıcıyı kaydedeve geri çekebilmeniz için bir iOS uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal bağlayıcı hesabı oluşturma
> * Uzamsal Tutturucuların hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * İOS cihazında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- MacOS High Sierra (10,13) veya üzeri çalıştıran bir Mac:
  - [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)'Dan yüklenen Xcode ve IOS SDK 'sının en son sürümü.
  - <a href="/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Mac için Visual Studio 8.1 +</a>' nin güncel sürümü.
  - <a href="https://git-scm.com/download/mac" target="_blank">MacOS Için git</a>.
  - <a href="https://git-lfs.github.com/">GIT LFS</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

`Xamarin/SampleXamarin.sln`Visual Studio 'da açın.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcıyı ve anahtarı yapılandırın

Bir sonraki adım, uygulamayı hesap tanımlayıcıyı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Tutturucuların kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`Xamarin/SampleXamarin.Common/AccountDetails.cs` dosyasını açın.

Alanı bulun `SpatialAnchorsAccountKey` ve `Set me` Hesap anahtarıyla değiştirin.

Alanı bulun `SpatialAnchorsAccountId` ve `Set me` Hesap tanımlayıcısıyla değiştirin.

Alanı bulun `SpatialAnchorsAccountDomain` ve `Set me` hesap etki alanı ile değiştirin.

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazında güç açın, oturum açın ve USB kablosu kullanarak bilgisayara bağlayın.

Başlangıç projesini **Samplexamarin. iOS** olarak ayarlayın, **çözüm yapılandırmasını** **Yayınla** değiştirin ve cihaz Seçicisi açılır penceresinde dağıtmak istediğiniz cihazı seçin.

![Visual Studio yapılandırması](./media/get-started-xamarin-iOS/visual-studio-macos-configuration.jpg)

**Run**  >  Uygulamanızı dağıtmak ve başlatmak için **hata ayıklama olmadan Başlat** ' ı seçin.

Uygulamada, tanıtımı çalıştırmak için **temel** ' yı seçin ve bir tutturucu yerleştirip geri çekmeniz için yönergeleri izleyin.

> ![Ekran görüntüsü 1 ](./media/get-started-xamarin-ios/screenshot-1.jpg)
>  ![ ekran görüntüsü 2 ](./media/get-started-xamarin-ios/screenshot-2.jpg)
>  ![ ekran görüntüsü 3](./media/get-started-xamarin-ios/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: cihazlarda uzamsal bağlayıcıları paylaşma](../tutorials/tutorial-share-anchors-across-devices.md)