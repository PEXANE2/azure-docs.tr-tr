---
title: Azure IoT Tak ve Kullan cihaz telemetrinizi depolamak ve analiz etmek için Time Series Insights kullanma | Microsoft Docs
description: IoT Tak ve Kullan cihazlarınızdan Telemetriyi görüntülemek ve analiz etmek için bir Time Series Insights ortamı ayarlayın ve IoT Hub 'ınızı bağlayın.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: e1eeb85980430e33446329b5f25ff963ab1c8027
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96600852"
---
# <a name="preview-tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Önizleme öğreticisi: IoT Tak ve Kullan cihaz telemetrisini depolamak, görselleştirmek ve çözümlemek için Time Series Insights Gen2 oluşturma ve bağlama

Bu öğreticide, IoT Tak ve Kullan çözümünüz ile tümleştirme için bir [Azure Time Series Insights Gen2](../time-series-insights/overview-what-is-tsi.md) (TSI) ortamı oluşturmayı ve doğru yapılandırmayı öğreneceksiniz. Nesnelerin İnterneti (IoT) ölçeğinde zaman serisi verilerini toplamak, işlemek, depolamak, sorgulamak ve görselleştirmek için TSI 'ı kullanın.

İlk olarak, bir TSI ortamı sağladığınızda IoT Hub 'ınızı bir akış olay kaynağı olarak bağlayabilirsiniz. Daha sonra, sıcaklık denetleyicisi ve termostat cihazları için kullandığınız [dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) örnek modeli dosyalarını temel alarak [zaman serisi modelinizi](../time-series-insights/concepts-model-overview.md) yazmak üzere model eşitleme üzerinden çalışırsınız.

> [!NOTE]
> Bu tümleştirme önizlemededir. DTDL cihaz modellerinin zaman serisi modeliyle eşleşmesi değişebilir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu noktada şunları yapabilirsiniz:

* Azure IoT Hub 'ı.
* IoT Hub 'ınıza bağlı bir DPS örneği, IoT Tak ve Kullan cihazınız için tek bir cihaz kaydı ile.
* Tek veya çok katmanlı bir cihazdan IoT Hub 'ınıza bir bağlantı, veri akışı benzetimi.

Azure CLı 'yı yerel olarak kurma gereksinimini ortadan kaldırmak için Azure Cloud Shell kullanarak bulut hizmetlerini ayarlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-your-event-source"></a>Olay kaynağınızı hazırlama

Daha önce oluşturduğunuz IoT Hub 'ı, TSI ortamınızın [olay kaynağıdır](../time-series-insights/concepts-streaming-ingestion-event-sources.md).

> [!IMPORTANT]
> Varolan tüm IoT Hub yollarını devre dışı bırakın. Bir IoT Hub 'ı, [yönlendirme](../iot-hub/iot-hub-devguide-messages-d2c.md#routing-endpoints) YAPıLANDıRıLMıŞ bir TSI olay kaynağı olarak kullanırken bilinen bir sorun vardır. Tüm yönlendirme uç noktalarını geçici olarak devre dışı bırakın ve IoT Hub 'ı TSI 'a bağlıyken yeniden etkinleştirebilirsiniz.

TSI 'ın tüketmesi için IoT Hub 'ınızda benzersiz bir tüketici grubu oluşturun. `my-pnp-hub`Daha önce kullandığınız IoT Hub 'ının adıyla değiştirin:

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group
```

## <a name="choose-your-time-series-id"></a>Zaman serisi KIMLIĞINIZI seçin

TSI ortamınızı sağladığınızda bir *zaman SERISI kimliği* seçmeniz gerekir. Bu özellik sabit olduğundan ve ayarlandıktan sonra değiştirilemediğinden, uygun zaman serisi KIMLIĞINI seçmeniz önemlidir. Bir zaman serisi KIMLIĞI, bir veritabanı bölüm anahtarı gibidir. Zaman serisi KIMLIĞI, zaman serisi modelinize yönelik birincil anahtar olarak davranır. Daha fazla bilgi edinmek için bkz. [bir zaman SERISI kimliği seçmek Için en iyi uygulamalar](../time-series-insights/how-to-select-tsid.md).

IoT Tak ve Kullan bir kullanıcı olarak, zaman serisi KIMLIĞINIZ dışında ve ' den oluşan bir _bileşik anahtar_ belirtin `iothub-connection-device-id` `dt-subject` . IoT Hub, IoT Tak ve Kullan cihaz KIMLIĞINIZI ve cihaz bileşeni adlarınızı sırasıyla içeren bu sistem özelliklerini ekler.

IoT Tak ve Kullan cihaz modelleriniz Şu anda bileşenleri kullanmasa bile, `dt-subject` gelecekte kullanabilmeniz için bileşik anahtarın bir parçası olarak dahil etmelisiniz. Zaman serisi KIMLIĞINIZ sabit olduğundan, Microsoft bu seçeneğin gelecekte gerekli olması durumunda bu seçeneği etkinleştirmeyi önerir.

> [!NOTE]
> Aşağıdaki örnekler çok bileşen **TemperatureController** cihazına yöneliktir, ancak kavramlar bileşen olmayan bir **termostat** cihazı için aynıdır.

## <a name="provision-your-tsi-environment"></a>TSI ortamınızı sağlama

Bu bölümde, Azure Time Series Insights Gen2 ortamınızın nasıl sağlanacağı açıklanmaktadır.

Aşağıdaki komut:

* Ortamınızın [soğuk deposu](../time-series-insights/concepts-storage.md#cold-store)için, uzun süreli saklama ve geçmiş veriler üzerinde analiz için tasarlanan bir Azure depolama hesabı oluşturur.
  * `mytsicoldstore`Soğuk depolama hesabınız için benzersiz bir adla değiştirin.
* Yedi günlük bir saklama süresi ve sonsuz saklama için soğuk depolama dahil olmak üzere, bir Azure Time Series Insights Gen2 ortamı oluşturur.
  * `my-tsi-env`TSI ortamınız için benzersiz bir adla değiştirin.
  * `my-pnp-resourcegroup`Ayarlama sırasında kullandığınız kaynak grubunun adıyla değiştirin.
  * `iothub-connection-device-id, dt-subject` Zaman serisi KIMLIĞINIZ özelliği.

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties iothub-connection-device-id, dt-subject --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

IoT Hub olay kaynağınızı bağlayın. `my-pnp-resourcegroup`, `my-pnp-hub` Ve `my-tsi-env` değerlerini seçtiğiniz değerlerle değiştirin. Aşağıdaki komut, daha önce oluşturduğunuz TSI için tüketici grubuna başvurur:

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```

[Azure Portal](https://portal.azure.com) kaynak grubunuza gidin ve yeni Time Series Insights ortamınızı seçin. Örneğe genel bakış bölümünde gösterilen *Time Series Insights gezgin URL 'sini* ziyaret edin:

![Portala genel bakış sayfası](./media/tutorial-configure-tsi/view-environment.png)

Gezgin 'de üç örnek görürsünüz:

* &lt;PNP cihaz KIMLIĞINIZ &gt; , thermostat1
* &lt;PNP cihaz KIMLIĞINIZ &gt; , thermostat2
* &lt;PNP cihaz KIMLIĞINIZ &gt; , `null`

> [!NOTE]
> Üçüncü etiket, **TemperatureController** kendisinden oluşan, cihaz belleği çalışma kümesi gibi Telemetriyi temsil eder. Bu bir üst düzey özellik olduğundan, bileşen adı değeri null olur. Daha sonraki bir adımda bunu daha kolay bir Kullanıcı adı ile güncelleştirin.

![Gezgin görünüm 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="configure-model-translation"></a>Model çevirisini yapılandırma

Ardından, DTDL cihaz modelinizi Azure Time Series Insights (TSI) içinde varlık modeline çevirmenizi sağlar. TSI 'ın zaman serisi modeli, TSI içindeki veri bağlamı için bir semantik modelleme aracıdır. Zaman serisi modelinin üç çekirdek bileşeni vardır:

* [Zaman serisi model örnekleri](../time-series-insights/concepts-model-overview.md#time-series-model-instances). Örnekler, zaman serisinin kendi sanal temsilleridir. Örnekler, zaman serisi KIMLIĞINIZ tarafından benzersiz şekilde tanımlanır.
* [Zaman serisi model hiyerarşileri](../time-series-insights/concepts-model-overview.md#time-series-model-hierarchies). Hiyerarşiler, özellik adlarını ve bunların ilişkilerini belirterek örnekleri düzenler.
* [Zaman serisi model türleri](../time-series-insights/concepts-model-overview.md#time-series-model-types). Türler hesaplamalar için [değişkenler](../time-series-insights/concepts-variables.md) veya formüller tanımlamanıza yardımcı olur. Türler belirli bir örnekle ilişkilendirilir.

### <a name="define-your-types"></a>Türlerinizi tanımlama

Önceden tanımlanmış bir modele sahip olmadan Azure Time Series Insights Gen2 'e veri almaya başlayabilirsiniz. Telemetri geldiğinde, TSI zaman serisi KIMLIĞI özellik değer (ler) i temelinde zaman serisi örneklerini oto çözümlemeye çalışır. Tüm örneklere *Varsayılan tür* atanır. Örneklerinizi doğru şekilde kategorilere ayırmak için el ile yeni bir tür oluşturmanız gerekir. Aşağıdaki ayrıntılar, cihaz DTDL modellerinizi zaman serisi model türleriniz ile eşitlemeye yönelik en basit yöntemi gösterir:

* Dijital ikizi modeli tanımlarınız tür KIMLIĞINIZ olur.
* Tür adı, model adı ya da görünen ad olabilir.
* Model açıklaması türün açıklaması olur.
* Sayısal bir şemaya sahip her telemetri için en az bir tür değişkeni oluşturulur.
  * Değişkenler için yalnızca sayısal veri türleri kullanılabilir, ancak bir değer dönüştürülebileceği başka bir tür olarak gönderilmişse, örneğin gibi bir `"0"` [dönüştürme](/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions) işlevi kullanabilirsiniz `toDouble` .
* Değişken adı telemetri adı ya da görünen ad olabilir.
* Değişken zaman serisi Ifadesini tanımladığınızda, telemetride telemetri adına ve veri türüne bakın.

| DTDL JSON | Zaman serisi model türü JSON | Örnek değer |
|-----------|------------------|-------------|
| `@id` | `id` | `dtmi:com:example:TemperatureController;1` |
| `displayName`    | `name`   |   `Temperature Controller`  |
| `description`  |  `description`  |  `Device with two thermostats and remote reboot.` |  
|`contents` dizide| `variables` nesne  | Aşağıdaki örneği görüntüleyin

![DTDL-zaman serisi model türü](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

> [!NOTE]
> Bu örnekte üç değişken gösterilmektedir, ancak her tür en fazla 100 olabilir. Farklı değişkenler gerektiğinde farklı hesaplamalar gerçekleştirmek için aynı telemetri değerine başvurabilir. Filtrelerin, toplamların ve skaler işlevlerin tam listesi için bkz. [Time Series Insights Gen2 zaman serisi ifade sözdizimi](/rest/api/time-series-insights/reference-time-series-expression-syntax).

Bir metin Düzenleyicisi açın ve aşağıdaki JSON 'ı yerel sürücünüze kaydedin:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:TemperatureController;1",
      "name": "Temperature Controller",
      "description": "Device with two thermostats and remote reboot.",
      "variables": {
        "workingSet": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.workingSet.Long, toLong($event.workingSet.Double))"
          }, 
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.temperature.Long, toLong($event.temperature.Double))"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "eventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        }
      }
    }
  ]
}
```

Time Series Insights Gezgini ' nde, sol taraftaki model simgesini seçerek **model** sekmesine gidin. **Türler** ' i seçin ve ardından **JSON yükle**' yi seçin:

![Karşıya Yükleme](./media/tutorial-configure-tsi/upload-type.png)

**Dosya Seç**' i seçin, daha önce kaydettiğiniz JSON 'ı seçip **karşıya yükle** ' yi seçin.

Yeni tanımlanan **sıcaklık denetleyicisi** türünü görürsünüz.

### <a name="create-a-hierarchy"></a>Hiyerarşi oluşturma

**TemperatureController** üst öğesi altındaki etiketleri düzenlemek için bir hiyerarşi oluşturun. Aşağıdaki basit örnekte tek bir düzey vardır, ancak IoT çözümlerinin genellikle bir kuruluştaki fiziksel ve anlam konumlarına göre etiketleri bağlamak için birçok iç içe geçme düzeyi vardır.

**Hiyerarşiler** ' ı seçin ve **hiyerarşi Ekle**' yi seçin. Ad olarak *cihaz Fleet* girin ve *Cihaz adı* adlı bir düzey oluşturun. Sonra **Kaydet**'i seçin.

![Hiyerarşi ekleme](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Örneklerinizi doğru türe atayın

Ardından, örneklerinizin türünü değiştirirsiniz ve bunları hiyerarşiye yerleştirebilirsiniz.

**Örnekler** sekmesini seçin, cihazın çalışma kümesini temsil eden örneği bulun ve en sağdaki **düzenleme** simgesini seçin:

![Örnekleri düzenle](./media/tutorial-configure-tsi/edit-instance.png)

**Tür** açılan listesini seçin ve **sıcaklık denetleyicisi**' ni seçin. Aygıtınızla ilişkili tüm üst düzey etiketleri temsil eden örneğin adını güncelleştirmek için *Defaultcomponent <your device name>* girin.

![Örnek türünü değiştir](./media/tutorial-configure-tsi/change-type.png)

Kaydet ' i seçmeden önce **örnek alanları** sekmesini seçin ve **cihaz Fleet** kutusunu işaretleyin. `<your device name> - Temp Controller`Telemetrisini birlikte gruplandırmak için yazın ve ardından **Kaydet**' i seçin.

![Hiyerarşiye ata](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

Termostat etiketlerinizi doğru tür ve hiyerarşiye atamak için önceki adımları tekrarlayın.

## <a name="view-your-data"></a>Verilerinizi görüntüleme

Grafik bölmesine geri gidin ve cihazınızı **> cihaz Fleet**' i genişletin. **Thermostat1** öğesini seçin, **sıcaklık** değişkenini seçin ve ardından değeri grafiğe **Ekle** ' yi seçin. **Thermostat2** ve **defaultcomponent** **WorkingSet** değeri için aynısını yapın.

![Thermostat2 için örnek türünü değiştir](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Sonraki adımlar

* Aralık boyutlandırma ve Y ekseni denetimleri de dahil olmak üzere çeşitli grafik seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Azure Time Series Insights Explorer](../time-series-insights/concepts-ux-panels.md).

* Ortamınızın zaman serisi modeline derinlemesine bir genel bakış için, [Azure Time Series Insights Gen2 makalesinde zaman serisi modeli](../time-series-insights/concepts-model-overview.md) konusuna bakın.

* Sorgu API 'Lerini ve zaman serisi Ifade sözdizimini incelemek için bkz. [Azure Time Series Insights Gen2 Query API 'leri](/rest/api/time-series-insights/reference-query-apis).
