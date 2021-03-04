---
title: 'Hızlı başlangıç: Unity ile bir HoloLens uygulaması oluşturma'
description: Bu hızlı başlangıçta, nesne bağlayıcılarını kullanarak bir HoloLens Unity uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 03/02/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 4f85a258042430d58690ef578db6d21a6c831d50
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102044825"
---
# <a name="quickstart-create-a-hololens-app-with-azure-object-anchors-in-unity"></a>Hızlı başlangıç: Unity 'de Azure nesne bağlayıcıları ile bir HoloLens uygulaması oluşturma

Bu hızlı başlangıçta, [Azure nesne tutturucularını](../overview.md)kullanan bir Unity HoloLens uygulaması oluşturacaksınız. Azure nesne bağlantıları, HoloLens için nesne açısından algılayan karma gerçeklik deneyimlerini etkinleştiren bir yönetilen bulut hizmetidir. İşiniz bittiğinde, fiziksel dünyadaki nesneleri algılayabildiği Unity ile oluşturulmuş bir HoloLens uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Unity derleme ayarlarını hazırlayın.
> * HoloLens Visual Studio projesini dışarı aktarın.
> * Uygulamayı dağıtın ve bir HoloLens 2 cihazında çalıştırın.

[!INCLUDE [Unity quickstart prerequisites](../../../includes/object-anchors-quickstart-unity-prerequisites.md)]

[!INCLUDE [Unity device setup](../../../includes/object-anchors-quickstart-unity-device-setup.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

[!INCLUDE [Download Unity Package](../../../includes/object-anchors-quickstart-unity-download-package.md)]

Unity 'de `quickstarts/apps/unity/basic` projeyi açın.

[!INCLUDE [Import Unity Package](../../../includes/object-anchors-quickstart-unity-import-package.md)]

[!INCLUDE [Unity build sample scene 1](../../../includes/object-anchors-quickstart-unity-build-sample-scene-1.md)]

[!INCLUDE [Unity build sample scene 2](../../../includes/object-anchors-quickstart-unity-build-sample-scene-2.md)]

[!INCLUDE [Unity build and deploy](../../../includes/object-anchors-quickstart-unity-build-deploy.md)]

### <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Cihazı açın, **tüm uygulamalar**' ı seçin, ardından uygulamayı bulun ve başlatın. Unity giriş ekranındayken, nesne gözlemci 'nin başlatıldığını belirten bir ileti görürsünüz. Ancak, modelinizi uygulamaya eklemeniz gerekir.

[!INCLUDE [Unity setup Windows Device Portal](../../../includes/object-anchors-quickstart-unity-setup-device-portal.md)]

[!INCLUDE [Unity upload your model](../../../includes/object-anchors-quickstart-unity-upload-model.md)]

Uygulama, görünümün geçerli alanındaki nesneleri arar ve sonra bunları tespit edildikten sonra izler. Örnek, kullanıcının konumundan 6 ölçüm olduğunda kaldırılır. Hata ayıklama metni, KIMLIK, güncelleştirilmiş zaman damgası ve yüzey kapsamı oranı gibi bir örnek hakkındaki ayrıntıları gösterir.

[!INCLUDE [Unity troubleshooting](../../../includes/object-anchors-quickstart-unity-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MRTK ile Unity HoloLens](get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [Kavramlar: SDK 'ya genel bakış](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [SSS](../faq.md)
