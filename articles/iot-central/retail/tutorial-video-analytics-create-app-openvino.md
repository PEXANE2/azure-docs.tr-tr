---
title: Öğretici-Azure IoT Central (OpenVINO) bir video analizi oluşturma-nesne ve hareket algılama uygulaması
description: Bu öğreticide, IoT Central bir video analizi uygulamasının nasıl oluşturulacağı gösterilmektedir. Oluşturun, özelleştirin ve diğer Azure hizmetlerine bağlayın. Bu öğretici, gerçek zamanlı nesne algılama için Intel OpenVINO &trade; araç setini kullanır.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 10/06/2020
ms.openlocfilehash: e16342b4b46a189a5a5c4c0554fb2d870c9cdcac
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877235"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central-openvinotrade"></a>Öğretici: Azure IoT Central (OpenVINO) üzerinde bir video analizi oluşturma-nesne ve hareket algılama uygulaması &trade;

Bir çözüm Oluşturucusu olarak, IoT Central *video analizi-nesne ve hareket algılama* uygulama şablonu, Azure IoT Edge cihazları, Azure Media Services ve Intel 'in, &trade; nesne ve hareket algılaması Için donanımla Iyileştirilmiş openvino ile bir video analizi uygulaması oluşturmayı öğrenin. Çözüm, güvenlik kameralarını izlemek için ortak iş ihtiyacını nasıl karşılakullanacağınızı göstermek için bir perakende mağaza kullanır. Çözüm, ilginç olayları hızlı bir şekilde tanımlamak ve bulmak için bir video akışında otomatik nesne algılamayı kullanır.

> [!TIP]
> Bir hareket algılaması nesnesi için OpenVINO yerine YOLO v3 'yi kullanmak için &trade; bkz. [öğretici: Azure IoT Central video analizi oluşturma-nesne ve hareket algılama uygulaması (Yolo v3)](tutorial-video-analytics-create-app-yolo-v3.md).

[!INCLUDE [iot-central-video-analytics-part1](../../../includes/iot-central-video-analytics-part1.md)]

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt)
- [ Üzerindedeployment.openvino.amd64.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.openvino.amd64.json)
- [ ÜzerindeLvaEdgeGatewayDcm.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [ Üzerindestate.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json)

[!INCLUDE [iot-central-video-analytics-part2](../../../includes/iot-central-video-analytics-part2.md)]

## <a name="edit-the-deployment-manifest"></a>Dağıtım bildirimini düzenleme

Dağıtım bildirimi kullanarak bir IoT Edge modülünü dağıtırsınız. IoT Central bildirimi bir cihaz şablonu olarak içeri aktarabilir ve ardından modül dağıtımını IoT Central yönetmenizi sağlayabilirsiniz.

Dağıtım bildirimini hazırlamak için:

1. Bir metin düzenleyicisi kullanarak *LVA-yapılandırma* klasörüne kaydettiğiniz *deployment.openvino.amd64.jsdosya üzerinde* açın.

1. `LvaEdgeGatewayModule`Aşağıdaki kod parçacığında gösterildiği gibi ayarları bulun ve görüntü adını değiştirin:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Bölümündeki düğümüne Media Services hesabınızın adını ekleyin `env` `LvaEdgeGatewayModule` . *scratchpad.txt* dosyasında bu hesap adının bir örneğini yaptınız:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_ACCOUNT_NAME>"
        }
    }
    ```

1. Şablon bu özellikleri IoT Central kullanıma sunmaz, bu nedenle Media Services yapılandırma değerlerini dağıtım bildirimine eklemeniz gerekir. Modülün yerini bulun `lvaEdge` ve Media Services hesabınızı oluştururken *scratchpad.txt* dosyasında bir Note yaptığınız değerlerle birlikte yer tutucuları değiştirin.

    , `azureMediaServicesArmId` Media Services hesabını oluştururken *scratchpad.txt* dosyasında bir Note oluşturdunuz **kaynak kimliğidir** .

    `aadTenantId` `aadServicePrincipalAppId` `aadServicePrincipalSecret` Media Services hesabınız için hizmet sorumlusu oluştururken *scratchpad.txt* dosyasında,, ve ' yi bir yere görürsünüz:

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

Bu öğretici, çözümünüzü &trade; nesne ve hareket algılama Için OpenVINO modülünü kullanacak şekilde yapılandırır. Aşağıdaki kod parçacığında modülün yapılandırması gösterilmektedir:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

[!INCLUDE [iot-central-video-analytics-part3](../../../includes/iot-central-video-analytics-part3.md)]

### <a name="replace-the-manifest"></a>Bildirimi değiştirme

**LVA Edge Gateway** sayfasında **+ bildirimi Değiştir**' i seçin.

:::image type="content" source="./media/tutorial-video-analytics-create-app-openvino/replace-manifest.png" alt-text="Bildirimi Değiştir":::

*LVA-yapılandırma* klasörüne gidin ve daha önce düzenlediğiniz bildirim dosyasında *deployment.openvino.amd64.js* seçin. **Karşıya Yükle**’yi seçin. Doğrulama tamamlandığında **Değiştir**' i seçin.

[!INCLUDE [iot-central-video-analytics-part4](../../../includes/iot-central-video-analytics-part4.md)]
