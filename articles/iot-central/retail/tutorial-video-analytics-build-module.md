---
title: Öğretici-IoT Edge canlı video analizi modüllerini değiştirme
description: Bu öğreticide, video analizi-nesnesi ve hareket algılama uygulama şablonunun kullandığı canlı video analizi ağ geçidi modüllerinin nasıl değiştirileceği ve oluşturulacağı gösterilmektedir.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: d21eb8d8d79ec04f0f7e766b4eeb370811553e64
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88038463"
---
# <a name="tutorial-modify-and-build-the-live-video-analytics-gateway-modules"></a>Öğretici: canlı video analizi ağ geçidi modüllerini değiştirme ve derleme

Bu öğreticide, canlı video analizi (LVA) modülleri için IoT Edge modülü kodunun nasıl değiştirileceği gösterilmektedir.

Önceki öğreticiler, modüllerin önceden oluşturulmuş görüntülerini kullanır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımları tamamlayabilmeniz için şunlar gerekir:

* [Node.js](https://nodejs.org/en/download/) ile v10 arasındaki veya üzeri
* [Tslınt](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-tslint-plugin) uzantısı yüklü [Visual Studio Code](https://code.visualstudio.com/Download)
* [Docker](https://www.docker.com/products/docker-desktop) altyapısı
* Modül sürümlerinizi barındırmak için bir [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) .
* [Azure Media Services](https://docs.microsoft.com/azure/media-services/) hesabı. Önceki öğreticileri tamamladıysanız, daha önce oluşturduğunuz birini yeniden kullanabilirsiniz.

## <a name="clone-the-repository"></a>Depoyu kopyalama

Depoyu henüz klonlamadıysanız, yerel makinenizde uygun bir konuma kopyalamak için aşağıdaki komutu kullanın:

```cmd
git clone https://github.com/Azure/live-video-analytics
```

VS Code yerel *canlı video analizi* deposu klasörünü açın.

## <a name="edit-the-deploymentamd64json-file"></a>Dosyadaki deployment.amd64.jsdüzenleme

1. Daha önce yapmadıysanız, **LVA-Gateway** deposunun yerel kopyasında *ref-Apps/LVA-Edge-IoT-merkezi-Gateway/Storage* adlı bir klasör oluşturun. Bu klasör, yanlışlıkla herhangi bir gizli bilgiyi denetlemeyi engellemek için git tarafından yok sayılır.
1. Dosya *deployment.amd64.js* , *Kurulum* klasöründen *depolama* klasörüne kopyalayın.
1. VS Code, *depolama/deployment.amd64.js* dosyasını açın.
1. `registryCredentials`Azure Container Registry kimlik bilgilerinizi eklemek için bölümü düzenleyin.
1. `LvaEdgeGatewayModule`İçindeki görüntü adını ve AMS hesabınızın adını eklemek için modül bölümünü düzenleyin `env:amsAccountName:value` .
1. `lvaYolov3`Modül bölümünü düzenleyin ve resminizin adını ekleyin.
1. `lvaEdge`Modül bölümünü düzenleyin ve resminizin adını ekleyin.
1. Yapılandırmanın nasıl tamamlanacağı hakkında daha fazla bilgi için bkz. [Azure 'da bir video Analytics uygulaması oluşturma IoT Central](tutorial-video-analytics-create-app.md) .

## <a name="build-the-code"></a>Kodu oluşturma

1. Kodu ilk kez derlemeyi denemeden önce, komutu çalıştırmak için VS Code terminalini kullanın `npm install` . Bu komut gerekli paketleri yükleyerek kurulum betikleri çalıştırır.

    > [!TIP]
    > Depo GitHub deposunun daha yeni bir sürümünü çekmeniz durumunda *node_modules* klasörünü silip `npm install` yeniden çalıştırın.

1. Kapsayıcı kayıt defteriniz adına göre adlı görüntüyü güncelleştirmek için *./Setup/imageConfig.js* dosyasını düzenleyin:

    ```json
    {
        "arch": "amd64",
        "imageName": "[Server].azurecr.io/lva-edge-gateway"
    }
    ```

1. Komutu çalıştırmak için VS Code terminalini kullanın `docker login [your server].azurecr.io` . Modüller için dağıtım bildiriminde verdiğiniz kimlik bilgilerini kullanın.

1. **NPM Sürüm Patch** komutunu çalıştırmak için vs Code terminalini kullanın. Bu derleme betiği, görüntüleri kapsayıcı Kayıt defterinize dağıtır. VS Code Terminal penceresindeki çıktı, oluşturma işleminin başarılı olup olmadığını gösterir.

1. **Lvaedgegatewaymodule** görüntüsünün sürümü, yapı her tamamlandığında artar. Dağıtım bildirimi dosyasında bu sürümü kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Artık video analizi-nesne ve hareket algılama uygulama şablonu ve LVA IoT Edge modülleriyle ilgili daha fazla bilgi edindiğinize göre, önerilen sonraki adım hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [Azure IoT Central ile perakende çözümleri oluşturma](overview-iot-central-retail.md)
