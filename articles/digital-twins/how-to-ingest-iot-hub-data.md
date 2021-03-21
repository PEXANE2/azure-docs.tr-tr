---
title: IoT Hub’dan telemetri alma
titleSuffix: Azure Digital Twins
description: IoT Hub cihaz telemetri iletilerini alma bölümüne bakın.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 9/15/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 3223a1c8e20d8b0caced5d940132c32fa0aba97c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103149101"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Azure dijital TWINS 'e alma IoT Hub telemetrisi

Azure dijital TWINS, IoT cihazlarından ve diğer kaynaklardaki verilerle birlikte çalıştırılır. Azure dijital TWINS 'te kullanılacak cihaz verileri için ortak bir kaynak [IoT Hub](../iot-hub/about-iot-hub.md).

Verileri Azure dijital TWINS 'e geri alma işlemi, [Azure işlevleri](../azure-functions/functions-overview.md)kullanılarak yapılan bir işlev gibi bir dış işlem kaynağı kurmındır. İşlevi, verileri alır ve [dijital TWINS](concepts-twins-graph.md) 'lerde buna uygun olarak özellikleri ayarlamak veya telemetri olaylarını başlatmak Için [Digitaltwins API 'lerini](/rest/api/digital-twins/dataplane/twins) kullanır. 

Bu nasıl yapılır belgesi, IoT Hub telemetri alabilen bir işlev yazma işlemini adım adım göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu örneğe devam etmeden önce aşağıdaki kaynakları önkoşul olarak ayarlamanız gerekir:
* **IoT Hub 'ı**. Yönergeler için, [bu IoT Hub hızlı başlangıç](../iot-hub/quickstart-send-telemetry-cli.md)konusunun *IoT Hub oluşturma* bölümüne bakın.
* Cihaz telemetrinizi alacak **bir Azure dijital TWINS örneği** . Yönergeler için bkz. [*nasıl yapılır: Azure dijital TWINS örneği ve kimlik doğrulaması ayarlama*](./how-to-set-up-instance-portal.md).

### <a name="example-telemetry-scenario"></a>Örnek telemetri senaryosu

Bu nasıl yapılır, Azure 'da bir işlev kullanarak IoT Hub 'den Azure dijital TWINS 'e ileti gönderme hakkında özetlenmektedir. İleti göndermek için kullanabileceğiniz birçok olası yapılandırma ve eşleşen strateji vardır, ancak bu makaleye yönelik örnek aşağıdaki bölümleri içerir:
* Bilinen bir cihaz KIMLIĞIYLE IoT Hub bir termostat cihazı
* Eşleşen bir KIMLIK ile cihazı temsil eden dijital ikizi

> [!NOTE]
> Bu örnek, cihaz KIMLIĞI ile karşılık gelen dijital ikizi KIMLIĞI arasında basit bir KIMLIK eşleşmesi kullanır, ancak cihazdan ikizi 'e daha karmaşık eşlemeler sağlamak mümkündür (örneğin, bir eşleme tablosu ile).

Termostat cihazı tarafından bir sıcaklık telemetri olayı gönderildiğinde, bir işlev Telemetriyi işler ve Digital ikizi 'ın *sıcaklık* özelliği güncellenir. Bu senaryo aşağıdaki diyagramda özetlenmiştir:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Akış grafiğini gösteren diyagram. Grafikte, bir IoT Hub cihaz, Azure dijital TWINS 'teki bir ikizi üzerinde sıcaklık özelliğini güncelleştiren Azure 'daki bir işleve sıcaklık IoT Hub telemetri gönderir." border="false":::

## <a name="add-a-model-and-twin"></a>Model ve ikiz ekleme

Bu bölümde, Azure dijital TWINS 'de, termostat cihazını temsil edecek bir [dijital ikizi](concepts-twins-graph.md) ayarlayacaksınız ve IoT Hub bilgiler ile güncelleştirilecektir.

Bir termostat türü ikizi oluşturmak için önce, bir termostat 'nın özelliklerini açıklayan ve daha sonra ikizi oluşturmak için kullanılacak olan termostat [modelini](concepts-models.md) örneğinizi yüklemeniz gerekir. 

Model şuna benzer:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

**Bu modeli TWINS örneğinizi karşıya yüklemek** Için AŞAĞıDAKI Azure CLI komutunu çalıştırın, bu, yukarıdaki modeli satır içi JSON olarak yükler. Komutunu tarayıcınızda [Azure Cloud Shell](/cloud-shell/overview.md) veya CLI 'niz [yerel olarak yüklüyse](/cli/azure/install-azure-cli.md)makinenizde çalıştırabilirsiniz.

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

Daha sonra **Bu modeli kullanarak bir ikizi oluşturmanız** gerekecektir. **Thermostat67** adlı bir termostat ikizi oluşturmak ve ilk sıcaklık değeri olarak 0,0 ayarlamak için aşağıdaki komutu kullanın.

```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

>[!NOTE]
> PowerShell ortamında Cloud Shell kullanıyorsanız, değerlerinin doğru ayrıştırılabilmesi için satır içi JSON alanlarındaki tırnak işareti karakterlerini atlamanız gerekebilir. Bu değişiklik ile modeli karşıya yükleme ve ikizi oluşturma komutları aşağıda verilmiştir:
>
> Modeli karşıya yükle:
> ```azurecli-interactive
> az dt model create --models '{  \"@id\": \"dtmi:contosocom:DigitalTwins:Thermostat;1\",  \"@type\": \"Interface\",  \"@context\": \"dtmi:dtdl:context;2\",  \"contents\": [    {      \"@type\": \"Property\",      \"name\": \"Temperature\",      \"schema\": \"double\"    }  ]}' -n {digital_twins_instance_name}
> ```
>
> İkizi oluştur:
> ```azurecli-interactive
> az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{\"Temperature\": 0.0,}' --dt-name {digital_twins_instance_name}
> ```

İkizi başarıyla oluşturulduğunda, komuttan gelen CLı çıktısı şuna benzer görünmelidir:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-a-function"></a>İşlev oluşturma

Bu bölümde, Azure dijital TWINS 'e erişmek ve aldığı IoT telemetri olaylarına göre TWINS 'yi güncelleştirmek için bir Azure işlevi oluşturacaksınız. İşlevi oluşturmak ve yayımlamak için aşağıdaki adımları izleyin.

#### <a name="step-1-create-a-function-app-project"></a>1. Adım: işlev uygulaması projesi oluşturma

İlk olarak, Visual Studio 'da yeni bir işlev uygulama projesi oluşturun. Bunun nasıl yapılacağı hakkında yönergeler için bkz. *nasıl yapılır: verileri işlemek için bir Işlev ayarlama* makalesine yönelik [**Visual Studio 'Da işlev uygulaması oluşturma**](how-to-create-azure-function.md#create-a-function-app-in-visual-studio) başlıklı Bölüm.

#### <a name="step-2-fill-in-function-code"></a>2. Adım: işlev kodunu doldur

Aşağıdaki paketleri projenize ekleyin:
* [Azure. DigitalTwins. Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core/)
* [Azure. Identity](https://www.nuget.org/packages/Azure.Identity/)
* [Microsoft. Azure. WebJobs. Extensions. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid/)

Visual Studio 'Nun *IoTHubtoTwins. cs*' ye yeni projeyle oluşturduğu *işlev1. cs* örnek işlevini yeniden adlandırın. Dosyadaki kodu aşağıdaki kodla değiştirin:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs":::

İşlev kodunuzu kaydedin.

#### <a name="step-3-publish-the-function-app-to-azure"></a>3. Adım: işlev uygulamasını Azure 'da yayımlama

Projeyi Azure 'da bir işlev uygulamasında yayımlayın.

Bunun nasıl yapılacağı hakkında yönergeler için bkz. *nasıl yapılır: verileri işlemek için bir Işlev ayarlama* makalesini [**Azure 'Da işlev uygulamasını yayımlama**](how-to-create-azure-function.md#publish-the-function-app-to-azure)

#### <a name="step-4-configure-the-function-app"></a>4. Adım: işlev uygulamasını yapılandırma

Sonra, işlev için **bir erişim rolü atayın** ve **uygulama ayarlarını** Azure dijital TWINS örneğinizi erişebilecek şekilde yapılandırın. Bunun nasıl yapılacağı hakkında yönergeler için bkz. *nasıl yapılır: verileri işlemek için bir Işlev ayarlama* makalesine [**yönelik güvenlik erişimini ayarlama**](how-to-create-azure-function.md#set-up-security-access-for-the-function-app) bölümüne bakın.

## <a name="connect-your-function-to-iot-hub"></a>İşlevinizi IoT Hub bağlama

Bu bölümde, IoT Hub cihaz verileri için işlevinizi bir olay hedefi olarak ayarlayacaksınız. Bu, IoT Hub 'daki termostat cihazındaki verilerin işlenmek üzere Azure işlevine gönderilmesini sağlar.

[Azure Portal](https://portal.azure.com/), [*önkoşullar*](#prerequisites) bölümünde oluşturduğunuz IoT Hub örneğine gidin. **Olaylar**' ın altında, işleviniz için bir abonelik oluşturun.

:::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Olay aboneliği eklemeyi gösteren Azure portal ekran görüntüsü.":::

**Olay aboneliği oluştur** sayfasında, alanları aşağıdaki gibi girin:
  1. **Ad** için, olay aboneliği için istediğiniz adı seçin.
  2. **Olay şeması** Için _Event Grid şeması_' nı seçin.
  3. **Sistem konu adı** için istediğiniz adı seçin.
  1. **Olay türlerine filtre uygulamak** Için _cihaz telemetri_ onay kutusunu seçin ve diğer olay türlerinin işaretini kaldırın.
  1. **Uç nokta türü** Için _Azure işlevi_' ni seçin.
  1. Uç **nokta için, uç** nokta Için kullanılacak Azure işlevini seçmek üzere uç nokta _Seç_ bağlantısını kullanın.
    
:::image type="content" source="media/how-to-ingest-iot-hub-data/create-event-subscription.png" alt-text="Olay aboneliği ayrıntılarını oluşturmak için Azure portal ekran görüntüsü":::

Açılan _Azure Işlevi Seç_ sayfasında, aşağıdaki ayrıntıları doğrulayın veya girin.
 1. **Abonelik**: Azure aboneliğiniz.
 2. **Kaynak grubu**: kaynak grubunuz.
 3. **İşlev uygulaması**: işlev uygulamanızın adı.
 4. **Yuva**: _Üretim_.
 5. **İşlev**: açılan listeden daha önce, *IoTHubtoTwins* öğesinden işlevi seçin.

_Seçimi Onayla_ düğmesini kullanarak ayrıntılarınızı kaydedin.            
      
:::image type="content" source="media/how-to-ingest-iot-hub-data/select-azure-function.png" alt-text="İşlevi seçmek için Azure portal ekran görüntüsü.":::

Olay aboneliği oluşturmak için _Oluştur_ düğmesini seçin.

## <a name="send-simulated-iot-data"></a>Sanal IoT verisi gönder

Yeni giriş işlevinizi test etmek için öğreticideki cihaz simülatörünü kullanın [*: uçtan uca bir çözümü bağlama*](./tutorial-end-to-end.md). Bu öğretici, C# dilinde yazılmış örnek bir proje tarafından çalıştırılır. Örnek kod şurada bulunur: [Azure Digital TWINS uçtan uca örnekler](/samples/azure-samples/digital-twins-samples/digital-twins-samples). Bu depoda **Devicesimülatör** projesini kullanıyorsunuz.

Uçtan uca öğreticide, aşağıdaki adımları izleyin:
1. [*Sanal cihazı IoT Hub Kaydet*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*Simülasyonu yapılandırma ve çalıştırma*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Sonuçlarınızı doğrulama

Yukarıdaki cihaz simülatörünü çalıştırırken, dijital ikizi 'ın sıcaklık değeri değişecek. Azure CLı 'de, sıcaklık değerini görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

Çıktılarınız şöyle bir sıcaklık değeri içermelidir:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Değer değişikliğini görmek için yukarıdaki sorgu komutunu tekrar tekrar çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS ile veri giriş ve çıkış hakkında bilgi edinin:
* [*Kavramlar: diğer hizmetlerle tümleştirme*](concepts-integration.md)
