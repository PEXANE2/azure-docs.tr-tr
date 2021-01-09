---
title: IoT Hub’dan telemetri alma
titleSuffix: Azure Digital Twins
description: IoT Hub cihaz telemetri iletilerini alma bölümüne bakın.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 9/15/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9ecc14aa9591d6e62dccd9899a80de44411928a1
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051097"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Azure dijital TWINS 'e alma IoT Hub telemetrisi

Azure dijital TWINS, IoT cihazlarından ve diğer kaynaklardaki verilerle birlikte çalıştırılır. Azure dijital TWINS 'te kullanılacak cihaz verileri için ortak bir kaynak [IoT Hub](../iot-hub/about-iot-hub.md).

Verileri Azure dijital TWINS 'e geri alma işlemi, [Azure işlevleri](../azure-functions/functions-overview.md)kullanılarak yapılan bir işlev gibi bir dış işlem kaynağı kurmındır. İşlevi, verileri alır ve [dijital TWINS](concepts-twins-graph.md) 'lerde buna uygun olarak özellikleri ayarlamak veya telemetri olaylarını başlatmak Için [Digitaltwins API 'lerini](/rest/api/digital-twins/dataplane/twins) kullanır. 

Bu nasıl yapılır belgesi, IoT Hub telemetri alabilen bir işlev yazma işlemini adım adım göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu örneğe devam etmeden önce aşağıdaki kaynakları önkoşul olarak ayarlamanız gerekir:
* **IoT Hub 'ı**. Yönergeler için, [bu IoT Hub hızlı başlangıç](../iot-hub/quickstart-send-telemetry-cli.md)konusunun *IoT Hub oluşturma* bölümüne bakın.
* Dijital ikizi örneğinizi çağırmak için doğru izinlere sahip **bir işlev** . Yönergeler için bkz. [*nasıl yapılır: Azure 'da veri işleme için bir Işlev ayarlama*](how-to-create-azure-function.md). 
* Cihaz telemetrinizi alacak **bir Azure dijital TWINS örneği** . Yönergeler için bkz. [*nasıl yapılır: Azure dijital TWINS örneği ve kimlik doğrulaması ayarlama*](./how-to-set-up-instance-portal.md).

### <a name="example-telemetry-scenario"></a>Örnek telemetri senaryosu

Bu nasıl yapılır, Azure 'da bir işlev kullanarak IoT Hub 'den Azure dijital TWINS 'e ileti gönderme hakkında özetlenmektedir. İleti göndermek için kullanabileceğiniz birçok olası yapılandırma ve eşleşen strateji vardır, ancak bu makaleye yönelik örnek aşağıdaki bölümleri içerir:
* Bilinen bir cihaz KIMLIĞIYLE IoT Hub bir termometre cihazı
* Eşleşen bir KIMLIK ile cihazı temsil eden dijital ikizi

> [!NOTE]
> Bu örnek, cihaz KIMLIĞI ile karşılık gelen dijital ikizi KIMLIĞI arasında basit bir KIMLIK eşleşmesi kullanır, ancak cihazdan ikizi 'e daha karmaşık eşlemeler sağlamak mümkündür (örneğin, bir eşleme tablosu ile).

Termostat cihazı tarafından bir sıcaklık telemetri olayı gönderildiğinde, bir işlev Telemetriyi işler ve Digital ikizi 'ın *sıcaklık* özelliği güncellenir. Bu senaryo aşağıdaki diyagramda özetlenmiştir:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Akış grafiğini gösteren diyagram. Grafikte, bir IoT Hub cihaz, Azure dijital TWINS 'teki bir ikizi üzerinde sıcaklık özelliğini güncelleştiren Azure 'daki bir işleve sıcaklık IoT Hub telemetri gönderir." border="false":::

## <a name="add-a-model-and-twin"></a>Model ve ikizi ekleme

Aşağıdaki CLı komutunu kullanarak bir model ekleyebilir/karşıya yükleyebilir ve sonra bu modeli kullanarak IoT Hub bilgiler ile güncelleştirilecektir.

Model şöyle görünür:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

**Bu modeli TWINS örneğinizle karşıya yüklemek** IÇIN Azure CLI 'yı açın ve şu komutu çalıştırın:

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

Daha sonra **Bu modeli kullanarak bir ikizi oluşturmanız** gerekecektir. Bir ikizi oluşturmak ve ilk sıcaklık değeri olarak 0,0 ayarlamak için aşağıdaki komutu kullanın.

```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

Başarılı bir ikizi Create komutunun çıkışı şöyle görünmelidir:
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

Bu bölüm, [*nasıl yapılır: verileri işlemek için bir Işlev ayarlama*](how-to-create-azure-function.md)Ile aynı Visual Studio başlangıç adımlarını ve işlev iskelet 'i kullanır. Çatı, kimlik doğrulamasını işler ve Azure dijital TWINS API 'Leri yanıt olarak çağırabilmeniz için bir hizmet istemcisi oluşturur. 

İzleyen adımlarda, IoT Hub IoT telemetri olaylarını işlemek için buna özel kod ekleyeceksiniz.  

### <a name="add-telemetry-processing"></a>Telemetri işleme ekleme
    
Telemetri olayları cihazdan iletiler biçiminde gelir. Telemetri işleme kodu eklemenin ilk adımı, bu cihaz iletisinin ilgili bölümünün Event Grid olayından ayıklanarak oluşur. 

Farklı cihazlar, iletilerini farklı şekilde yapılandırabileceği için, **Bu adımın kodu bağlı cihaza bağlıdır.** 

Aşağıdaki kod, telemetri olarak JSON gönderen basit bir cihaz için bir örnek gösterir. Bu örnek, [*öğretici: uçtan uca çözümü bağlama*](./tutorial-end-to-end.md)konusunda tam olarak araştırılmış. Aşağıdaki kod, iletiyi gönderen cihazın ve sıcaklık değerinin cihaz KIMLIĞINI bulur.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Find_device_ID_and_temperature":::

Sonraki kod örneği, KIMLIĞI ve sıcaklık değerini alır ve bu ikizi "Patch" (güncelleştirme yap) için kullanır.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Update_twin_with_device_temperature":::

### <a name="update-your-function-code"></a>İşlev kodunuzu güncelleştirme

Önceki örneklerden kodu anladığınıza göre, Visual Studio 'daki [*Önkoşullar*](#prerequisites) bölümünde işlevinizi açın. (Azure 'da oluşturulmuş bir işleviniz yoksa, şimdi bir tane oluşturmak için önkoşullara ait bağlantıyı ziyaret edin).

İşlevinizin kodunu bu örnek kodla değiştirin.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs":::

İşlev kodunuzu kaydedin ve işlev uygulamasını Azure 'da yayımlayın. Nasıl yapılacağını öğrenmek için bkz. [*Azure 'da veri işlemek için bir işlev ayarlama*](how-to-create-azure-function.md)bölümünde [*işlev uygulamasını yayımlama*](./how-to-create-azure-function.md#publish-the-function-app-to-azure) .

Başarılı bir yayımladıktan sonra çıktıyı aşağıda gösterildiği gibi Visual Studio komut penceresinde görürsünüz:

```cmd
1>------ Build started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
1>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>------ Publish started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\Out\
2>Publishing C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\adtIngestFunctionSample - 20200911112545669.zip to https://adtingestfunctionsample20200818134346.scm.azurewebsites.net/api/zipdeploy...
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
========== Publish: 1 succeeded, 0 failed, 0 skipped ==========
```
[Azure Portal](https://portal.azure.com/)yayımlama işleminin durumunuzu da doğrulayabilirsiniz. _Kaynak grubunuzu_ arayın ve _etkinlik günlüğü_ ' ne gidin ve listede _Web uygulaması yayımlama profili al_ ' a gidin ve durumun başarılı olduğunu doğrulayın.

:::image type="content" source="media/how-to-ingest-iot-hub-data/azure-function-publish-activity-log.png" alt-text="Yayımla işleminin durumunu gösteren Azure portal ekran görüntüsü.":::

## <a name="connect-your-function-to-iot-hub"></a>İşlevinizi IoT Hub bağlama

Hub verileri için bir olay hedefi ayarlayın.
[Azure Portal](https://portal.azure.com/), [*önkoşullar*](#prerequisites) bölümünde oluşturduğunuz IoT Hub örneğine gidin. **Olaylar**' ın altında, işleviniz için bir abonelik oluşturun.

:::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Olay aboneliği eklemeyi gösteren Azure portal ekran görüntüsü.":::

**Olay aboneliği oluştur** sayfasında, alanları aşağıdaki gibi girin:
  1. **Ad**' ın altında, aboneliği istediğiniz şekilde adlandırın.
  2. **Olay şeması** altında _Event Grid şeması_' nı seçin.
  3. **Olay türleri** altında _cihaz telemetri_ onay kutusunu seçin ve diğer olay türlerinin işaretini kaldırın.
  4. **Uç nokta türü** altında _Azure işlevi_' ni seçin.
  5. Uç **nokta ' ın** altında bir uç nokta oluşturmak için _uç nokta bağlantısını seçin_ öğesini seçin.
    
:::image type="content" source="media/how-to-ingest-iot-hub-data/create-event-subscription.png" alt-text="Olay aboneliği ayrıntılarını oluşturmak için Azure portal ekran görüntüsü":::

Açılan _Azure Işlevi Seç_ sayfasında, aşağıdaki ayrıntıları doğrulayın.
 1. **Abonelik**: Azure aboneliğiniz
 2. **Kaynak grubu**: kaynak grubunuz
 3. **İşlev uygulaması**: işlev uygulamanızın adı
 4. **Yuva**: _Üretim_
 5. **İşlev**: açılan listeden işlevinizi seçin.

_Seçim onaylama_ düğmesini seçerek ayrıntılarınızı kaydedin.            
      
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
