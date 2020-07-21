---
title: 'Öğretici: Azure IoT Hub aracılığıyla cihaz verileri alma'
description: Bu öğreticide, IoT Bağlayıcısı üzerinden FHıR için Azure API 'ye IoT Hub cihaz veri yönlendirmeyi nasıl etkinleştireceğinizi öğreneceksiniz.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: tutorial
ms.date: 06/27/2020
ms.author: punagpal
ms.openlocfilehash: 95e3e2be175fa810b1b966a7dda5a0e53e23d780
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536740"
---
# <a name="tutorial-receive-device-data-through-azure-iot-hub"></a>Öğretici: Azure IoT Hub aracılığıyla cihaz verileri alma

IoT Bağlayıcısı, tıp 'leri Internet 'ten, FHAR için Azure API 'ye veri alma özelliği sağlar. Azure portal hızlı başlangıç [kullanan IoT bağlayıcısını dağıtma (Önizleme)](iot-fhir-portal-quickstart.md) , Azure tarafından yönetilen bir cihaz örneği olduğunu gösterdi IoT Central IoT bağlayıcısına [telemetri gönderiyor](iot-fhir-portal-quickstart.md#connect-your-devices-to-iot) . IoT Bağlayıcısı, Azure IoT Hub aracılığıyla sağlanan ve yönetilen cihazlarla da çalışabilir. Bu öğreticide, cihaz verilerini Azure IoT Hub IoT Bağlayıcısı 'na bağlama ve yönlendirme yordamı sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- En az bir IoT Bağlayıcısı ile FHıR kaynağı için Azure API- [Azure Portal kullanarak IoT bağlayıcısını dağıtma (Önizleme)](iot-fhir-portal-quickstart.md)
- Azure IoT Hub kaynak gerçek veya sanal cihazla bağlandı- [Azure Portal kullanarak IoT Hub 'ı oluşturma](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)

> [!TIP]
> Azure IoT Hub sanal cihaz uygulaması kullanıyorsanız, tercih ettiğiniz uygulamayı farklı desteklenen diller ve sistemler arasında seçebilirsiniz.

## <a name="get-connection-string-for-iot-connector-preview"></a>IoT Bağlayıcısı için bağlantı dizesi al (Önizleme)

Azure IoT Hub, IoT bağlayıcınıza güvenli bir şekilde bağlanmak için bir bağlantı dizesi gerektirir. IoT Bağlayıcınız için [bağlantı dizesi](iot-fhir-portal-quickstart.md#generate-a-connection-string)oluşturma bölümünde açıklandığı gibi yeni bir bağlantı dizesi oluşturun. Sonraki adımda kullanılmak üzere bu bağlantı dizesini koru.

IoT Bağlayıcısı, cihaz iletilerini almak için kullanılan bir Azure Event hub örneğini kullanır. Yukarıda oluşturulan bağlantı dizesi temelde bu temel olay hub 'ına yönelik bağlantı dizesidir.

## <a name="connect-azure-iot-hub-with-the-iot-connector-preview"></a>Azure IoT Hub IoT Bağlayıcısı (Önizleme) ile bağlama

Azure IoT Hub, cihaz verilerini Olay Hub 'ı, depolama hesabı ve Service Bus gibi çeşitli Azure hizmetlerine gönderme yeteneği sağlayan [ileti yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) adlı bir özelliği destekler. IoT Bağlayıcısı, Azure IoT Hub 'den Olay Hub 'ı uç noktasına bağlanmak ve cihaz verilerini göndermek için bu özellikten yararlanır.

> [!NOTE] 
> Şu anda, IoT bağlayıcısının Olay Hub 'ı müşteri aboneliğinde barındırmadığı için, yalnızca PowerShell veya CLı komutunu, [ileti yönlendirme oluşturmak](https://docs.microsoft.com/azure/iot-hub/tutorial-routing) için kullanabilirsiniz. bu nedenle, Azure Portal tarafından görülemez. Ancak, ileti yönlendirme nesneleri PowerShell veya CLı kullanılarak eklendikten sonra, Azure portal görünür ve buradan yönetilebilir.

İleti yönlendirmeyi ayarlama iki adımdan oluşur.

### <a name="add-an-endpoint"></a>Bir uç nokta ekleme
Bu adım IoT Hub verileri yönlendiren bir uç nokta tanımlar. Bu uç noktayı, tercihlerinize göre [Add-AzIotHubRoutingEndpoint](https://docs.microsoft.com/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint) PowerShell komutunu ya da [az IoT Hub Routing-ENDPOINT Create](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?#az-iot-hub-routing-endpoint-create) CLI komutunu kullanarak oluşturun.

Aşağıda, bir uç nokta oluşturmak için komutuyla birlikte kullanılacak parametrelerin listesi verilmiştir:

|PowerShell parametresi|CLı parametresi|Description|
|---|---|---|
|ResourceGroupName|resource-group|IoT Hub kaynağınızın kaynak grubu adı.|
|Name|Hub-adı|IoT Hub kaynağınızın adı.|
|Uçnoktaadı|uç nokta adı|Oluşturulan uç noktaya atamak istediğiniz bir ad kullanın.|
|EndpointType|uç nokta türü|IoT Hub bağlantı kurmak için gereken uç nokta türü. PowerShell için "EventHub" ve CLı için "eventhub" değerlerini kullanın.|
|EndpointResourceGroup|uç nokta-kaynak grubu|IoT bağlayıcısının FHıR kaynağı için Azure API 'sinin kaynak grubu adı. Bu değeri, FHıR için Azure API 'nin Genel Bakış sayfasından edinebilirsiniz.|
|Endpointsubscriptionıd|uç nokta-abonelik kimliği|IoT bağlayıcısının FHıR kaynağı için Azure API 'SI için abonelik kimliği. Bu değeri, FHıR için Azure API 'nin Genel Bakış sayfasından edinebilirsiniz.|
|Dizisi|bağlantı dizesi|IoT bağlayıcınıza bağlantı dizesi. Önceki adımda edindiğiniz değeri kullanın.|

### <a name="add-a-message-route"></a>İleti yolu ekleme
Bu adım, yukarıda oluşturulan uç noktayı kullanarak bir ileti yolunu tanımlar. Tercihinize göre [Add-AzIotHubRoute](https://docs.microsoft.com/powershell/module/az.iothub/Add-AzIoTHubRoute) PowerShell komutunu veya [az IoT Hub Route Create](https://docs.microsoft.com/cli/azure/iot/hub/route#az-iot-hub-route-create) CLI komutunu kullanarak bir yol oluşturun.

Aşağıda, bir uç nokta oluşturmak için komutuyla birlikte kullanılacak parametrelerin listesi verilmiştir:

|PowerShell parametresi|CLı parametresi|Description|
|---|---|---|
|ResourceGroupName|g|IoT Hub kaynağınızın kaynak grubu adı.|
|Name|Hub-adı|IoT Hub kaynağınızın adı.|
|Uçnoktaadı|uç nokta adı|Yukarıda oluşturduğunuz bitiş noktasının adı.|
|Routetablename|yol adı|Oluşturulan ileti yoluna atamak istediğiniz ad.|
|Kaynak|Kaynak türü|Uç noktaya gönderilen veri türü. PowerShell için "DeviceMessages" değeri ve CLı için "devicemessages" değerlerini kullanın.|

## <a name="send-device-message-to-iot-hub"></a>IoT Hub cihaz iletisi gönder

Aşağıda gösterilen örnek kalp oranı iletisini Azure IoT Hub göndermek için cihazınızı (gerçek veya sanal) kullanın. Bu ileti, iletinin bir FHıR gözlem kaynağına dönüştürülebileceği ve FHıR için Azure API 'sinde depolanacak IoT bağlayıcısına yönlendirilir.

```json
{
  "HeartRate": 80,
  "RespiratoryRate": 12,
  "HeartRateVariability": 64,
  "BodyTemperature": 99.08839032397609,
  "BloodPressure": {
    "Systolic": 23,
    "Diastolic": 34
  },
  "Activity": "walking"
}
```
> [!IMPORTANT]
> IoT Bağlayıcınız ile yapılandırılmış [eşleme şablonlarına](iot-mapping-templates.md) uyan cihaz iletisini gönderdiğinizden emin olun.

## <a name="view-device-data-in-azure-api-for-fhir"></a>FHıR için Azure API 'de cihaz verilerini görüntüleme

IoT Bağlayıcısı tarafından oluşturulan FHıR izleme kaynakları 'nı Postman kullanarak FHıR için Azure API üzerinde görüntüleyebilirsiniz. [Fhır Için Azure API 'sine erişmek üzere Postman](access-fhir-postman-tutorial.md) 'nizi ayarlayın ve `GET` `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4` Yukarıdaki örnek iletide sinyal oranı değeri gönderilen gözehar kaynaklarını görüntülemek için bir istek yapın.

> [!TIP]
> Usx'lerinizin FHıR veri düzlemi için Azure API 'sine uygun erişimi olduğundan emin olun. Gerekli veri düzlemi rollerini atamak için [Azure rol tabanlı Access Control](configure-azure-rbac.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, cihaz verilerini IoT bağlayıcısına yönlendirmek için Azure IoT Hub ayarlarsınız. IoT Bağlayıcısı hakkında daha fazla bilgi edinmek için aşağıdaki adımları aşağıdan seçin:

IoT Bağlayıcısı içindeki farklı veri akışı aşamalarını anlayın.

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı veri akışı](iot-data-flow.md)

Cihaz ve FHıR eşleme şablonlarını kullanarak IoT bağlayıcısını nasıl yapılandıracağınızı öğrenin.

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı eşleme şablonları](iot-mapping-templates.md)

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.

