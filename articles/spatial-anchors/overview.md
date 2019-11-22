---
title: Azure uzamsal Tutturucuların genel bakış
description: Azure uzamsal Tutturucuların platformlar arası karma gerçeklik deneyimlerini geliştirmenize nasıl yardımcı olduğunu öğrenin.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 503004cba0d4109adcfee62f7acd108cbcb73eb6
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277149"
---
# <a name="azure-spatial-anchors-overview"></a>Azure uzamsal Tutturucuların genel bakış

Azure uzamsal tutturucularını hoş geldiniz. Azure uzamsal bağlantıları, geliştiricilere, istenmeyen ve dağınık karma gerçeklik uygulamaları oluşturmak için önemli yetenekler sağlar. Bu uygulamalar, ARKit destekleyen Microsoft HoloLens, iOS tabanlı cihazları ve ARCore destekleyen Android tabanlı cihazları destekleyebilir. Azure Spatial Anchors geliştiricilerin alanları algılamak, ilgi çekici noktaları tam olarak belirlemek ve desteklenen cihazlarda bu ilgi çekici noktaları anımsamak için karma gerçeklik platformlarıyla çalışmasını sağlar.
Bu kesin ilgi noktaları, uzamsal bağlayıcı olarak adlandırılır.

![Platformlar Arası](./media/cross-platform.png)

## <a name="examples"></a>Örnekler

Uzamsal Tutturucuların etkinleştirildiği bazı örnek kullanım durumları şunlardır:

- [Çok kullanıcılı deneyimler](tutorials/tutorial-share-anchors-across-devices.md). Uzamsal bağlantılar, aynı yerde bulunan kişilerin çok kullanıcılı karma gerçeklik uygulamalarına katılmasını kolaylaştırır. Örneğin, iki kişi bir tabloya sanal satranç panosu yerleştirerek karma gerçeklik satranç oyununu başlatabilir. Ardından, cihazlarını tabloya işaret ederek, sanal satranç panosunu birlikte görüntüleyip etkileşime girebilirler.

- [Yol bulma](concepts/anchor-relationships-way-finding.md). Geliştiriciler, uzamsal bağlantıları aralarında ilişkiler oluşturarak da birbirine bağlayabilirler. Örneğin, bir uygulama, bir kullanıcının bir görevi tamamlaması için etkileşimde bulunması gereken iki veya daha fazla ilgi noktasına sahip bir deneyim içerebilir. Bu ilgi çekici noktaları, bağlı bir biçimde oluşturulabilir. Daha sonra, Kullanıcı çok adımlı görevi tamamlarken, uygulama, kullanıcının görevin bir sonraki adımına yaklaşmasını sağlamak için yakın olan bağlantıları ister.

- [Gerçek dünyada kalıcı sanal içerik](how-tos/create-locate-anchors-unity.md#create-a-cloud-spatial-anchor). Bir uygulama, kullanıcının bir telefon uygulaması veya HoloLens cihazı kullanarak görebilmesine olanak tanıyan bir konferans odası duvarından bir sanal takvim yerleştirmesini sağlayabilir. Endüstriyel ortamda kullanıcı desteklenen bir cihaz kamerasını makineye doğrultarak makine hakkında bağlamsal bilgiler alabilir.

Azure Spatial Anchors bir yönetilen hizmetten ve desteklenen cihaz platformları için istemci SDK'larından oluşur. Aşağıdaki bölümlerde, Azure uzamsal bağlayıcıları kullanarak uygulama oluşturmaya başlama hakkında bilgi sağlanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

Uzamsal bağlayıcılarla ilk uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> ['Yi](unity-overview.yml)

> [!div class="nextstepaction"]
> [iOS](quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](quickstarts/get-started-hololens.md)
