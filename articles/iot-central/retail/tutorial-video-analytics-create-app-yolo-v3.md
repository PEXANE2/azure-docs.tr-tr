---
title: Öğretici-Azure IoT Central bir video analizi oluşturma-nesne ve hareket algılama uygulaması (YOLO v3)
description: Bu öğreticide, IoT Central bir video analizi uygulamasının nasıl oluşturulacağı gösterilmektedir. Oluşturun, özelleştirin ve diğer Azure hizmetlerine bağlayın. Bu öğretici, YOLO v3 gerçek zamanlı nesne algılama sistemini kullanır.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 10/06/2020
ms.openlocfilehash: ecc32908aea2fb474d2ebe5bd94f556527eda814
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763455"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central-yolo-v3"></a>Öğretici: Azure IoT Central bir video Analytics-nesne ve hareket algılama uygulaması oluşturma (YOLO v3)

Bir çözüm Oluşturucusu olarak, IoT Central *video analizi-nesne ve hareket algılama* uygulama şablonu, Azure IoT Edge cihazları, Azure Media Services ve Yolo v3 gerçek zamanlı nesne ve hareket algılama sistemi ile bir video analizi uygulaması oluşturmayı öğrenin. Çözüm, güvenlik kameralarını izlemek için ortak iş ihtiyacını nasıl karşılakullanacağınızı göstermek için bir perakende mağaza kullanır. Çözüm, ilginç olayları hızlı bir şekilde tanımlamak ve bulmak için bir video akışında otomatik nesne algılamayı kullanır.

> [!TIP]
> &trade;Bir hareket algılaması nesnesi IÇIN YOLO v3 yerine OpenVINO kullanmak için bkz. [öğretici: Azure IoT Central (openvino &trade; ) bir video analizi oluşturma-nesne ve hareket algılama uygulaması](tutorial-video-analytics-create-app-openvino.md).

[!INCLUDE [iot-central-video-analytics-part1](../../../includes/iot-central-video-analytics-part1.md)]

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt) -bu dosya, bu öğreticilerde çalışırken ihtiyacınız olan çeşitli yapılandırma seçeneklerini kaydetmenize yardımcı olur.
- [ Üzerindedeployment.amd64.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.amd64.json)
- [ ÜzerindeLvaEdgeGatewayDcm.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [state.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json) , ikinci öğreticide yalnızca Intel nuc cihazını kullanmayı planlıyorsanız bu dosyayı indirmeniz gerekir.

> [!NOTE]
> GitHub deposu, **Lvaedgegatewaymodule** ve **lvaYolov3** IoT Edge modüllerinin kaynak kodunu da içerir. Kaynak kodla çalışma hakkında daha fazla bilgi için bkz. [LVA Gateway modüllerini oluşturma](tutorial-video-analytics-build-module.md).

[!INCLUDE [iot-central-video-analytics-part2](../../../includes/iot-central-video-analytics-part2.md)]

## <a name="edit-the-deployment-manifest"></a>Dağıtım bildirimini düzenleme

Dağıtım bildirimi kullanarak bir IoT Edge modülünü dağıtırsınız. IoT Central bildirimi bir cihaz şablonu olarak içeri aktarabilir ve ardından modül dağıtımını IoT Central yönetmenizi sağlayabilirsiniz.

Dağıtım bildirimini hazırlamak için:

1. Bir metin düzenleyicisi kullanarak *LVA-yapılandırma* klasörüne kaydettiğiniz *deployment.amd64.jsdosya üzerinde* açın.

1. Ayarları bulun `LvaEdgeGatewayModule` ve görüntü adının aşağıdaki kod parçacığında gösterildiği gibi olduğundan emin olun:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Bölümündeki düğümüne Media Services hesabınızın adını ekleyin `env` `LvaEdgeGatewayModule` . *scratchpad.txt* dosyasında Media Services hesap adı ' nı bir yere yaptınız:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_SERVICES_ACCOUNT_NAME>"
        }
    }
    ```

1. Şablon bu özellikleri IoT Central kullanıma sunmaz, bu nedenle Media Services yapılandırma değerlerini dağıtım bildirimine eklemeniz gerekir. Modülün yerini bulun `lvaEdge` ve Media Services hesabınızı oluştururken *scratchpad.txt* dosyasında bir Note yaptığınız değerlerle birlikte yer tutucuları değiştirin.

    , `azureMediaServicesArmId` Media Services hesabını oluştururken *scratchpad.txt* dosyasında bir Note oluşturdunuz **kaynak kimliğidir** .

    Aşağıdaki tabloda, dağıtım bildiriminde kullanmanız gereken *scratchpad.txt* dosyasında **Media Services API 'sine (JSON) Bağlan** değeri gösterilmektedir:

    | Dağıtım bildirimi       | Karalama çubuğu  |
    | ------------------------- | ----------- |
    | Aadtenantıd               | Aadtenantıd |
    | Aadserviceprincıpalappıd  | Aadclientıd |
    | Aadservicesprincipalsecret | AadSecret   |

    > [!CAUTION]
    > Dağıtım bildiriminde doğru değerleri eklediğinizden emin olmak için önceki tabloyu kullanın, aksi takdirde cihaz çalışmaz.

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Değişiklikleri kaydedin.

Bu öğretici, çözümünüzü nesne ve hareket algılama için YOLO v3 modülünü kullanacak şekilde yapılandırır. Aşağıdaki kod parçacığında modülün yapılandırması gösterilmektedir:

```json
"lvaYolov3": {
    "settings": {
        "image": "mcr.microsoft.com/lva-utilities/yolov3-onnx:1.0"
    },
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "version": "1.0"
}
```

[!INCLUDE [iot-central-video-analytics-part3](../../../includes/iot-central-video-analytics-part3.md)]

### <a name="replace-the-manifest"></a>Bildirimi değiştirme

**LVA Edge Gateway v2** sayfasında **+ bildirimi Değiştir**' i seçin.

:::image type="content" source="./media/tutorial-video-analytics-create-app-yolo-v3/replace-manifest.png" alt-text="Bildirimi Değiştir":::

*LVA-yapılandırma* klasörüne gidin ve daha önce düzenlediğiniz bildirim dosyasında *deployment.amd64.js* seçin. **Karşıya Yükle**’yi seçin. Doğrulama tamamlandığında **Değiştir**' i seçin.

[!INCLUDE [iot-central-video-analytics-part4](../../../includes/iot-central-video-analytics-part4.md)]
