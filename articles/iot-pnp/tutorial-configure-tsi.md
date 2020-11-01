---
title: IoT Tak ve Kullan cihaz telemetrinizi depolamak ve analiz etmek için Time Series Insights kullanma | Microsoft Docs
description: IoT Tak ve Kullan cihazlarınızdan Telemetriyi görüntülemek ve analiz etmek için bir Time Series Insights ortamı ayarlayın ve IoT Hub bağlayın.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 0d4c24ed771b4d112a448ceec0e9fc2f9e8ac359
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146436"
---
# <a name="tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Öğretici: IoT Tak ve Kullan cihaz telemetrisini depolamak, görselleştirmek ve çözümlemek için Time Series Insights Gen2 oluşturma ve bağlanma

Bu öğreticide, IoT Tak ve Kullan çözümünüz ile tümleştirme için bir [Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) ortamı oluşturmayı ve doğru yapılandırmayı öğreneceksiniz. TSI ile, Nesnelerin İnterneti (IoT) ölçeğinde zaman serisi verilerini toplayabilir, işleyebilir, saklayabilir, sorgulayabilir ve görselleştirebilirsiniz.

İlk olarak, bir TSI ortamı sağlayacak ve IoT Hub bir akış olay kaynağı olarak bağlayacaksınız. Daha sonra, sıcaklık denetleyicisi ve termostat cihazları için kullandığınız [dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) örnek model dosyalarını temel alarak, TSI ortamınızın zaman serisi modelini kullanarak bir model eşitleme üzerinden çalışacaksınız.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Azure CLı 'yı yerel olarak kurma gereksinimini ortadan kaldırmak için Azure Cloud Shell kullanarak bulut hizmetlerini ayarlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="time-series-id-selection"></a>Zaman serisi KIMLIĞI seçimi

TSI ortamınızı sağlarken, bir zaman serisi KIMLIĞI seçmeniz gerekir. Özellik sabit olduğu ve ayarlandıktan sonra değiştirilemeyen uygun zaman serisi KIMLIĞINI seçmek kritik öneme sahiptir. Bir zaman serisi KIMLIĞI seçilmesi, bir veritabanı için bölüm anahtarı seçmeye benzer. Genellikle, TS KIMLIĞINIZ varlık modelinizin yaprak düğümü olmalıdır. Diğer bir deyişle, genellikle Telemetriyi yayan en ayrıntılı varlık veya algılayıcının ID özelliğini seçmek istersiniz.

IoT Tak ve Kullan bir kullanıcı olarak, TS KIMLIĞINIZI seçmeye yönelik ilgili soru, cihaz modellerinizde bulunan [bileşenlerin](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#component) varlığına yöneliktir. 

![TS KIMLIK seçimi](./media/tutorial-configure-tsi/ts-id-selection-pnp.png)

* Hızlı başlangıç yaptıysanız ve IoT Hub cihazınız [termostat](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/Thermostat.json)'yi temsil EDIYORSA, `iot-hub-connection-device-id` TS kimliğiniz olarak kullanın.

* Çoklu bileşen [TemperatureController](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/TemperatureController.json)öğreticilerden birini yaptıysanız, aşağıdaki bölümde gösterildiği gibi bir bileşik anahtar kullanın.  `iot-hub-connection-device-id, dt-subject`

## <a name="provision-your-azure-time-series-insights-gen2-environment"></a>Azure Time Series Insights Gen2 ortamınızı sağlama

Aşağıdaki komut şunları yapar:

* Ortamınızın [soğuk deposu](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store)için, uzun süreli saklama ve geçmiş veriler üzerinde analiz için tasarlanan bir Azure depolama hesabı oluşturur.
  * `mytsicoldstore`Hesabınız için benzersiz bir adla değiştirin.
* 7 günlük bir saklama süresi ve sonsuz saklama için soğuk bir depo dahil olmak üzere, bir Azure Time Series Insights Gen2 ortamı oluşturur. 
  * `my-tsi-env`TSI ortamınız için benzersiz bir adla değiştirin 
  * Kurulum `my-pnp-resourcegroup` sırasında kullandığınız kaynak grubunun adıyla değiştirin
  * `my-ts-id-property`Yukarıdaki seçim ölçütlerine göre TS ID özellik değeri ile değiştirin

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties my-ts-id-property --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

Şimdi, daha önce oluşturduğunuz IoT Hub ortamınız [olay kaynağı](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources)olarak yapılandıracaksınız. Olay kaynağınız bağlı olduğunda, TSI kuyruktaki en erken olaydan başlayarak hub 'ınızdaki olayların dizinini oluşturmaya başlar.

İlk olarak, IoT Hub TSI ortamınız için benzersiz bir tüketici grubu oluşturun. `my-pnp-hub`Daha önce kullandığınız IoT Hub adıyla değiştirin.

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group 
```

IoT Hub bağlayın. `my-pnp-resourcegroup`, `my-pnp-hub` Ve `my-tsi-env` değerlerini kendi değerlerinizle değiştirin.

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```
[Azure Portal](https://portal.azure.com) kaynak grubunuza gidin ve yeni oluşturduğunuz Time Series Insights ortamınızı seçin. Örneğe genel bakış bölümünde gösterilen *Time Series Insights gezgin URL 'sini* ziyaret edin.

![Portala genel bakış sayfası](./media/tutorial-configure-tsi/view-environment.png)

Gezgin 'de, "tüm hiyerarşiler" bölümünde iki termoterzinizi görmeniz gerekir. Daha sonra, zaman serisi modelinizi cihaz modelinize göre seçeceksiniz.

![Gezgin görünüm 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="model-synchronization-between-digital-twins-definition-language-and-time-series-insights-gen2"></a>Dijital TWINS tanım dili ve Time Series Insights Gen2 arasında model eşitleme

Daha sonra DTDL cihaz modelinizi Azure Time Series Insights (TSI) ' deki varlık modeline çevireceğiz. TSI 'ın zaman serisi modeli, TSI içindeki veri bağlamı için bir semantik modelleme aracıdır. Zaman serisi modelinin üç çekirdek bileşeni vardır:

* [Zaman serisi model örnekleri](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-instances). Örnekler, zaman serisinin kendi sanal temsilleridir. Örnekler, TS KIMLIĞINIZ tarafından benzersiz şekilde tanımlanır.
* [Zaman serisi model hiyerarşileri](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-hierarchies). Hiyerarşiler, özellik adlarını ve bunların ilişkilerini belirterek örnekleri düzenler.
* [Zaman serisi model türleri](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-types). Türler, hesaplamalar yapmak için [değişkenler](https://docs.microsoft.com/azure/time-series-insights/concepts-variables) veya formüller tanımlamanıza yardımcı olur. Türler belirli bir örnekle ilişkilendirilir.

> [!NOTE]
> Aşağıdaki örnekler Çoklu bileşen TemperatureController içindir.

### <a name="define-your-types"></a>Türlerinizi tanımlama

Önceden tanımlı bir model olmadan verileri Azure Time Series Insights Gen2 'e aktarabilirsiniz. Telemetri geldiğinde, TSI, TH KIMLIĞI özellik değerine göre zaman serisi örneklerini otomatik olarak çözümlemeye çalışır. Tüm örneklere *Varsayılan tür* atanır. Modellerinizi göstermek için el ile yeni bir tür oluşturmanız gerekir. Aşağıdaki görüntüde, bir DTDL modelinin ve TSD türünün eşitlenmesi için basit bir yöntem gösterilmektedir:

![DTDL-TSD türü](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

* Dijital ikizi modeli tanımlayıclarınız (DTMı) tür KIMLIĞINIZ olacak
* Tür adı modelin adı ya da görünen adı olabilir
* Model açıklaması türün açıklaması olur
* Sayısal bir şemaya sahip her telemetri bileşeni için en az bir tür değişkeni oluşturulur. 
  * Değişkenler için yalnızca sayısal veri türleri kullanılabilir, ancak örneğin bir değer, ayrıştırılabilir bir dize olarak gönderilirse, bir `"0"` [dönüştürme](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions) işlevi kullanabilirsiniz `toDouble`
* Değişken adı telemetri adı ya da görünen ad olabilir
* Değişken zaman serisi Ifadesi (TSX) tanımlarken, telemetride telemetri adına ve veri türüne bakın.

> [!NOTE]
> Bu örnekte yalnızca iki değişken gösterilmektedir--bir toplama ve sayısal, ancak her tür en fazla 100 olabilir. Farklı değişkenler, gerektiğinde farklı durgun işlemleri gerçekleştirmek için aynı telemetri değerine başvurabilir. Filtrelerin, toplamaların ve skaler işlevlerin tam listesi için [Time Series Insights Gen2 zaman serisi ifade sözdizimi](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax) belgelerini görüntüler.

İstediğiniz metin düzenleyicinizi açın ve aşağıdaki JSON 'yi yerel sürücünüze kaydedin:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:Thermostat;1",
      "name": "Thermostat",
      "description": "Reports current temperature and provides desired temperature control.",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event.temperature.Double"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

Time Series Insights Gezgini ' nde, sol taraftaki model simgesine tıklayarak model sekmesine gidin. **Türler** ' e tıklayın ve **JSON karşıya yükle** ' ye tıklayın:

![Karşıya Yükleme](./media/tutorial-configure-tsi/upload-type.png)

**Dosya Seç** ' i seçin, daha önce kaydettiğiniz JSON 'ı seçip **karşıya yükle** ' ye tıklayın.

Yeni tanımlanan termostat türünü görmeniz gerekir.

### <a name="optional---create-a-hierarchy"></a>İsteğe bağlı-hiyerarşi oluşturma

İsteğe bağlı olarak, iki termostat bileşenini kendi TemeraptureController üst kısmında düzenlemek için bir hiyerarşi oluşturabilirsiniz.

*Hiyerarşiler* ' e tıklayın ve *hiyerarşi Ekle* ' yi seçin. `Device Fleet`Ad olarak yazın ve adlı bir düzey oluşturun `Device Name` ve *Kaydet* ' e tıklayın.

![Hiyerarşi ekleme](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Örneklerinizi doğru türe atayın

Ardından, örneklerinizin türünü değiştireceksiniz ve isteğe bağlı olarak bunları hiyerarşi içinde situate

*Örnekler* sekmesini seçin ve en sağdaki *Düzenle* simgesine tıklayın.

![Örnekleri düzenle](./media/tutorial-configure-tsi/edit-instance.png)

Tür açılan listesine tıklayın ve öğesini seçin `Thermostat` . 

![Örnek türünü değiştir](./media/tutorial-configure-tsi/change-type.png)

Bir hiyerarşi oluşturduysanız, *örnek alanlar* ' ı seçin ve kutuyu işaretleyin `Device Fleet` . `Temperature Controller`Üst cihazının değeri olarak girin ve ardından *Kaydet* ' e tıklayın.

![Hiyerarşiye ata](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

İkinci termostat için yukarıdaki adımları tekrarlayın.

### <a name="view-your-data"></a>Verilerinizi görüntüleme

Grafik bölmesine geri gidin ve cihaz Fleve TemperatureController ' ı genişletin. Thermostat1 ' a tıklayın, `Temperature` değişkeni seçin ve ardından değeri grafiğe *eklemek için Ekle* ' ye tıklayın. Thermostat2 için aynısını yapın.

![Örnek türünü değiştir](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Sonraki adımlar

* Aralık boyutlandırma ve Y ekseni denetimleri de dahil olmak üzere çeşitli grafik seçenekleri hakkında daha fazla bilgi edinmek için [Azure Time Series Insights gezgin ](https://docs.microsoft.com/azure/time-series-insights/concepts-ux-panels) belgelerini görüntüleyin.

* Ortamınızın zaman serisi modeline derinlemesine bir genel bakış için [Bu](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview) makaleye bakın.

* Sorgu API 'Lerini ve zaman serisi Ifadesi sözdizimini incelemek için öğesini [seçin](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis).




