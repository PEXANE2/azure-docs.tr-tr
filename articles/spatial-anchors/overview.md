---
title: Azure Mekansal Çapalara genel bakış
description: Azure Uzamsal Çapaların platformlar arası karma gerçeklik deneyimleri geliştirmenize nasıl yardımcı olduğunu öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 5ebd29b5fb6fdedcdfbc434209b350512c4cd5dc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77657316"
---
# <a name="azure-spatial-anchors-overview"></a>Azure Mekansal Çapalara genel bakış

Azure Uzamsal Çapalar'a hoş geldiniz. Azure Uzamsal Çapalar, geliştiricilere mekansal olarak farkında olan karma gerçeklik uygulamaları oluşturmada temel özelliklere sahip olmalarını sağlıyor. Bu uygulamalar Microsoft HoloLens'i, ARKit'i destekleyen iOS tabanlı cihazları ve ARCore'u destekleyen Android tabanlı cihazları destekleyebilir. Azure Spatial Anchors geliştiricilerin alanları algılamak, ilgi çekici noktaları tam olarak belirlemek ve desteklenen cihazlarda bu ilgi çekici noktaları anımsamak için karma gerçeklik platformlarıyla çalışmasını sağlar.
Bu hassas ilgi noktaları uzamsal çapa olarak adlandırılır.

![Çoklu Platform](./media/cross-platform.png)

## <a name="examples"></a>Örnekler

Uzamsal Çapalar tarafından etkinleştirilen bazı örnek kullanım örnekleri şunlardır:

- [Çok kullanıcılı deneyimler.](tutorials/tutorial-share-anchors-across-devices.md) Uzamsal Çapalar, aynı yerdeki kişilerin çok kullanıcılı karma gerçeklik uygulamalarına katılmalarını kolaylaştırır. Örneğin, iki kişi bir masaya sanal satranç tahtası yerleştirerek karışık gerçeklik satranç oyunu başlatabilirsiniz. Daha sonra, cihazlarını masaya doğrultarak sanal satranç tahtasını birlikte görüntüleyebilir ve etkileşime girebiliyorlar.

- [Yol bulma.](concepts/anchor-relationships-way-finding.md) Geliştiriciler ayrıca Uzamsal Çapaları aralarında ilişkiler oluşturarak birbirine bağlayabilir. Örneğin, bir uygulama, bir kullanıcının görevi tamamlamak için etkileşimde olması gereken iki veya daha fazla ilgi noktası olan bir deneyim içerebilir. Bu ilgi noktaları bağlı bir şekilde oluşturulabilir. Daha sonra, kullanıcı çok adımlı görevi tamamlarken, uygulama, kullanıcıyı görevdeki bir sonraki adıma yönlendirmek için geçerli olanın yakınında bulunan çapaları isteyebilir.

- [Gerçek dünyada kalıcı sanal içerik](how-tos/create-locate-anchors-unity.md#create-a-cloud-spatial-anchor). Bir uygulama, bir kullanıcının bir konferans odası duvarına, insanların bir telefon uygulaması veya HoloLens cihazı kullanarak görebileceği sanal bir takvim yerleştirmesine izin verebilir. Endüstriyel ortamda kullanıcı desteklenen bir cihaz kamerasını makineye doğrultarak makine hakkında bağlamsal bilgiler alabilir.

Azure Spatial Anchors bir yönetilen hizmetten ve desteklenen cihaz platformları için istemci SDK'larından oluşur. Aşağıdaki bölümler, Azure Uzamsal Çapaları kullanarak uygulama oluşturmaya başlama hakkında bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Mekansal Çapalar ile ilk uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [Birlik (HoloLens)](quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Birlik (iOS)](quickstarts/get-started-unity-ios.md)

> [!div class="nextstepaction"]
> [Birlik (Android)](quickstarts/get-started-unity-android.md)

> [!div class="nextstepaction"]
> [iOS](quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](quickstarts/get-started-hololens.md)

> [!div class="nextstepaction"]
> [Xamarin (Android)](quickstarts/get-started-xamarin-android.md)

> [!div class="nextstepaction"]
> [Xamarin (iOS)](quickstarts/get-started-xamarin-ios.md)
