---
title: 'Quickstart: Xamarin iOS uygulaması oluşturma'
description: Bu hızlı başlangıçta, Uzamsal Çapalar'ı kullanarak Xamarin ile bir iOS uygulaması oluşturmayı öğrenirsiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c5e217a33c8b461a438e2d0209fe6733850634e6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75465154"
---
# <a name="quickstart-create-a-xamarin-ios-app-with-azure-spatial-anchors"></a>Hızlı başlangıç: Azure Uzamsal Çapalarla bir Xamarin iOS uygulaması oluşturun

Bu hızlı başlangıç, [Azure Uzamsal Bağlantılarını](../overview.md)kullanarak Xamarin ile nasıl bir iOS uygulaması oluşturulacak larını kapsar. Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. İşi bittiğinde, uzamsal bir çapayı kaydedip geri çağırabilen bir iOS uygulamanız olur.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uzamsal Çapalar hesabı oluşturma
> * Uzamsal Çapalar hesap tanımlayıcısını ve hesap anahtarını yapılandırın
> * iOS aygıtında dağıtma ve çalıştırma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:
- Mac çalışan macOS High Sierra (10.13) veya üzeri ile:
  - [App Store'dan](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)yüklenen Xcode ve iOS SDK'nın en son sürümü.
  - <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Mac 8.1+ için Visual Studio'nun</a>güncel bir sürümü.
  - <a href="https://git-scm.com/download/mac" target="_blank">macOS için Git</a>.
  - <a href="https://git-lfs.github.com/">Git LFS</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açma

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Visual `Xamarin/SampleXamarin.sln` Studio'da aç.

## <a name="configure-account-identifier-and-key"></a>Hesap tanımlayıcısı ve anahtarı yapılandırma

Bir sonraki adım, uygulamanın hesap tanımlayıcınızı ve hesap anahtarınızı kullanacak şekilde yapılandırmaktır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

`Xamarin/SampleXamarin.Common/AccountDetails.cs` dosyasını açın.

`SpatialAnchorsAccountKey` Alanı bulun ve `Set me` hesap anahtarıyla değiştirin.

`SpatialAnchorsAccountId` Alanı bulun ve `Set me` hesap tanımlayıcısıyla değiştirin.

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS aygıtınıza dağıtma

iOS aygıtında güç, oturum açın ve bir USB kablosu kullanarak bilgisayara bağlayın.

Başlangıç projesini **SampleXamarin.iOS**olarak ayarlayın, **Çözüm Yapılandırmasını** **Sürüm**olarak değiştirin ve aygıt seçici açılır durumda dağıtmak istediğiniz aygıtı seçin.

![Görsel Stüdyo Yapılandırması](./media/get-started-xamarin-iOS/visual-studio-macos-configuration.jpg)

Uygulamanızı dağıtmak ve başlatmak için > **Hata Ayıklama olmadan** **Çalıştır'ı**seçin.

Uygulamada, demoyu çalıştırmak ve bir çapa yerleştirmek ve geri çağırmak için yönergeleri izlemek için **Temel'i** seçin.

> ![Ekran](./media/get-started-xamarin-ios/screenshot-1.jpg)
> ![Görüntüsü](./media/get-started-xamarin-ios/screenshot-2.jpg)
> ![1 Ekran Görüntüsü 2 Ekran Görüntüsü 3](./media/get-started-xamarin-ios/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Öğretici: Uzamsal Bağlantı larını aygıtlar arasında paylaşın](../tutorials/tutorial-share-anchors-across-devices.md)
