---
title: Hava durumu iş ortağı tümleştirmesi
description: Bu makalede, hava durumu sağlayıcısı 'nın Farmtempts ile nasıl tümleştirileceği açıklanmaktadır
author: sunasing
ms.topic: article
ms.date: 07/09/2020
ms.author: sunasing
ms.openlocfilehash: a2677b5343b2d65a39e7c9f6d5006db599c1ac73
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86497004"
---
# <a name="weather-partner-integration"></a>Hava durumu iş ortağı tümleştirmesi

Bu makalede, veri sağlayıcılarının API 'Lerinden yararlanarak ve bu verileri farmker 'Lara ekleyerek Farmtempts ile tümleşecek Azure Farmker **Connector** Docker bileşeni hakkında bilgi sağlanır. Veriler Farmkörler 'de kullanılabilir olduktan sonra, veri Fusion ve Machine Learning/yapay zeka modelleri oluşturmak için kullanılabilir.

 > [!NOTE]
 > Bu belgelerin amacına yönelik olarak, Azure açık veri kümelerinde NOAA kullanılarak oluşturulan bir başvuru uygulamasını kullanacağız ve ' de kullanılabilir [https://github.com/azurefarmbeats/noaa_docker](https://github.com/azurefarmbeats/noaa_docker) .
 > Karşılık gelen Docker görüntüsü şurada bulunabilir:[https://hub.docker.com/r/azurefarmbeats/farmbeats-noaa](https://hub.docker.com/r/azurefarmbeats/farmbeats-noaa)

Hava durumu ortağının bir Docker görüntüsünü/programını sağlaması gerekir (aşağıda belirtilen belirtimlerle) ve Docker görüntüsünü müşteriler tarafından erişilebilen bir kapsayıcı kayıt defterinde barındırın. Hava durumu ortağının müşterilerine aşağıdaki bilgileri sağlaması gerekir:

- Docker görüntü URL 'SI
- Docker resim etiketi
- Docker görüntüsüne erişim için anahtarlar/kimlik bilgileri
- Hava durumu ortağının sistemindeki verilere erişmek için müşteriye özgü API anahtarları/kimlik bilgileri
- VM SKU 'SU ayrıntıları (Iş ortakları, Docker 'ın belirli VM gereksinimlerine sahip olması durumunda bunu sağlayabilir, aksi takdirde müşteriler Azure 'da desteklenen VM SKU 'Ları arasından seçim yapabilir)

Yukarıdaki Docker bilgilerini kullanarak müşteri, bir hava durumu ortağını, bu kişilerin Farmker örneğine kaydeder. Müşterilerin, veri Hava durumu verilerini almak için Docker 'ı nasıl kullanabileceği hakkında daha fazla bilgi edinmek için [Hava durumu verilerini alma](https://docs.microsoft.com/azure/industry/agriculture/get-weather-data-from-weather-partner) kılavuzuna bakın

## <a name="connector-docker-development"></a>Bağlayıcı Docker geliştirme

**REST API tabanlı tümleştirme**

Farmtts API 'Leri Swagger teknik belgelerini içerir. Tüm API 'Ler ve bunların karşılık gelen istekleri veya yanıtları hakkında bilgi için bkz. [Farmtts Swagger](https://aka.ms/farmbeatsswagger). 

Farmtts yüklediyseniz, şu adreste bulunan Farmtts Swagger verilerinize erişebilirsiniz:`https://yourfarmbeatswebsitename-api.azurewebsites.net/swagger`

"-Api" nin, Farmtts Web sitenizin adının sonuna ekleneceğini unutmayın.
API uç noktası şu şekilde olacaktır:`https://yourfarmbeatswebsitename-api.azurewebsites.net`

### <a name="datahub-lib"></a>Datahub kitaplığı

Farmtler, hava durumu ortağı tarafından kullanılabilecek bir LIB sağlar. LIB Şu anda başvuru uygulamasının bir parçası olarak [kullanılabilir.](https://github.com/azurefarmbeats/noaa_docker/tree/master/datahub_lib) Daha sonra, aynı bir SDK olarak birden çok dil için de kullanılabilir olacaktır.

### <a name="authentication"></a>Kimlik Doğrulaması

**Farmtempts API 'Leri ile kimlik doğrulama**

Farmtts, taşıyıcı kimlik doğrulaması kullanır ve API 'Ler, isteğin üstbilgi bölümünde aşağıdaki gibi bir erişim belirteci sunarak erişilebilir:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Erişim belirteci, müşterinin Farmtts örneğinde çalışan bir Azure Işlevinden istenebilir. Azure Işlevi URL 'SI Docker programına bir bağımsız değişken olarak sağlanacak ve erişim belirteci URL üzerinde bir GET isteği yapılarak elde edilebilir. URL 'den gelen yanıt erişim belirtecini içerir. Datahub kitaplığı, iş ortaklarının erişim belirtecini almasını sağlamak için yardımcı işlevler sağlar. Diğer ayrıntıları [burada](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_auth_helper.py) bulabilirsiniz.

Erişim belirteci yalnızca birkaç saat için geçerlidir ve süresi dolduğunda yeniden istenmesi gerekir.

**İş ortağı tarafı API 'Leri ile kimlik doğrulama**

Müşterilerin Docker yürütmesi sırasında iş ortağı tarafı API 'Lerinde kimlik doğrulamasından geçmesini sağlamak için, müşterilerin Iş ortağı kaydı sırasında aşağıdaki gibi kimlik bilgilerini sağlaması gerekir:

```json
{
 "partnerCredentials": {
   "key1": "value1",
   "key2": "value2"
   }
}
```
API hizmeti bu dict 'yi seri hale getirir ve bunu bir [Keykasasında](https://docs.microsoft.com/azure/key-vault/basic-concepts)depolar.

[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) , hava durumu işlerini düzenlemek ve Docker kodunu yürütmek üzere kaynakları almak için kullanılır. Ayrıca, Docker işinin çalıştırıldığı sanal makineye güvenli bir şekilde veri gönderme mekanizması sağlar. Artık Keykasasında güvenli şekilde depolanan API kimlik bilgileri, anahtar kasasından güvenli dizeler olarak okunabilir ve Docker kapsayıcısının çalışma dizininde activity.jsüzerinde genişletilmiş özellikler olarak kullanılabilir hale getirilir (yol dosya "/mnt/working_dir/activity.json") Docker kodu, çalışma zamanında bu dosyadaki kimlik bilgilerini, müşteri adına iş ortağı tarafı API 'Lerine erişmek için okuyabilir. Kimlik bilgileri dosyada aşağıdaki gibi kullanılabilir olacaktır:

```json
{ 
   "typeProperties" : {
      "extendedProperties" : { 
         "partnerCredentials": "" } 
   } 
}
```
"PartnerCredentials" ın Iş ortağı kaydı sırasında müşteri tarafından sağlanan tam şekilde kullanılabilir olacağını unutmayın.

Farmtts kitaplığı, iş ortaklarının etkinlik özelliklerinden kimlik bilgilerini okumasına olanak tanımak için yardımcı işlevler sağlar. Diğer ayrıntıları [burada](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_adf_helper.py) bulabilirsiniz.

Dosyanın ömrü yalnızca Docker kod yürütme sırasında olduğundan, Docker çalıştırıldıktan sonra silinir.

ADF işlem hatları ve etkinliklerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz [https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping](https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping) ..

**HTTP istek üstbilgileri**

Aşağıda, Farmtts 'ye bir API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri verilmiştir.

**Üst bilgi** | **Açıklama ve örnek**
--- | ---
İçerik Türü | İstek biçimi (Content-Type: Application/ <format> ). Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Content-Type: Application/JSON
Yetkilendirme | API çağrısı yapmak için gereken erişim belirtecini belirtir. Yetkilendirme: taşıyıcı <erişim-belirteç>
Kabul Et | Yanıt biçimi. Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Kabul et: uygulama/JSON

## <a name="data-format"></a>Veri biçimi

JSON, rastgele veri yapılarının basit bir metin gösterimini sağlayan, dilden bağımsız ortak bir veri biçimidir. Daha fazla bilgi için bkz. [JSON.org](http://json.org).

## <a name="docker-specifications"></a>Docker belirtimleri

Docker programının iki bileşeni olması gerekir: **önyükleme** ve **işler**. Birden çok Iş olabilir.

### <a name="bootstrap"></a>Bootstrap

Bu bileşen, müşteri, Farmtts 'de Docker kaydını başlattığında yürütülür. Bu programa geçirilecek bağımsız değişkenler (arg1, arg2) şu şekilde olur:

- Farmtts API uç noktası: API istekleri için Farmtts API uç noktası: Bu, Farmtts dağıtımına API çağrıları yapmak için uç noktasıdır.
- Azure Işlevi URL 'SI: Bu, şirketinizin API 'Leri için erişim belirtecinizi sağlayacak olan kendi kişisel uç noktanıza aittir. Bu URL 'de bir GET çağırmak, yanıt olarak erişim belirtecini getirir.

Önyüklemeden sorumlu olması, kullanıcıların Hava durumu verilerini almak üzere işlerinizi çalıştırabilmeleri için gerekli meta verileri oluşturmaktır. Başvuru uygulamasına [buradan](https://github.com/azurefarmbeats/noaa_docker)bakın. Dosyadaki bootstrap_manifest.jsgereksinimlerinize göre güncelleştirebilir ve başvuru önyükleme programı sizin için gerekli meta verileri oluşturur.

Aşağıdaki meta veriler bu işlemin bir parçası olarak oluşturulur. 

 > [!NOTE]
 > **Lütfen** dosyadaki bootstrap_manifest.js[başvuru uygulamasında](https://github.com/azurefarmbeats/noaa_docker)bahsedildiği gibi güncelleştirirseniz, önyükleme, bildirim dosyanıza göre aynısını oluştururken aşağıdaki meta verileri oluşturmanız gerekmez.

- /**Dalgalı veri modeli**: bir hava kaynağı modeli, hava durumu verilerini temsil eden ve kaynak tarafından sunulan farklı veri kümelerine karşılık gelen bir modeldir. Örneğin, bir Dailyforebir Simplemodel günde bir kez ortalama sıcaklık, nem ve yağış bilgilerini sağlayabilir, bu durumda bir DailyForecastAdvancedModel saatlik ayrıntı düzeyinde daha fazla bilgi sağlayabiliriz. İstediğiniz sayıda dalgalı Therdatamodel oluşturabilirsiniz.
- /**JobType**: farmtts, genişletilebilir bir iş yönetim sistemine sahiptir. Hava durumu veri sağlayıcısı olarak, farklı veri kümelerine/API 'Lere sahip olursunuz (örneğin, Getgünlüktahminleri); onları JobType olarak Farmpts 'de etkinleştirebilirsiniz. Bir JobType oluşturulduktan sonra bir müşteri, kendi konumu/grubu için hava durumu verilerini almak üzere bu türdeki Işleri tetikleyebilir (bkz. [Farmtts Swagger](https://aka.ms/farmbeatsswagger)Içindeki JobType ve Job API 'leri).

### <a name="jobs"></a>İşler

Bu bileşen, bir Farmher Kullanıcı, önyükleme işleminin bir parçası olarak oluşturduğunuz/JobType ' ın bir işini her çalıştırdığında çağrılacaktır. İş için Docker Run komutu, oluşturduğunuz **/JobType** 'ın bir parçası olarak tanımlanmıştır.
- İşin sorumluluğu, kaynaktan veri alıp onları Farmto 'a gönderecektir. Verileri almak için gereken parametreler, önyükleme işleminde/JobType öğesinin bir parçası olarak tanımlanmalıdır.
- İşin bir parçası olarak programın, önyükleme sürecinin bir parçası olarak oluşturulan/dalgalı therdatamodel temelinde bir **/dalgalı iş datalocation** oluşturması gerekir. **/Dalgalı iş datalocation** , Kullanıcı tarafından iş için bir parametre olarak belirtilen bir konuma (Enlem/Long) karşılık gelir.

### <a name="details-of-the-objects"></a>Nesnelerin ayrıntıları

  Dalgalı veri modeli | Açıklama |
  --- | ---
  Ad  | Hava durumu veri modelinin adı |
  Description  | Modelin anlamlı bir açıklamasını sağlayın. |
  Özellikler  | Veri sağlayıcısı tarafından tanımlanan ek özellikler. |
  Hava ölçüleri > adı  | Hava durumu ölçüsünün adı. Örneğin humidity_max |
  Dalgalı ölçüler > veri türü  | Double veya Enum. Enum ise, measureEnumDefinition gerekir |
  >, measureEnumDefinition  | Yalnızca veri türü enum ise gereklidir. Örneğin, {"NoRain": 0, "kar": 1, "Drizzle": 2, "yağmur": 3} |
  Hava ölçüleri > türü  | Hava durumu Telemetri verilerinin türü. Örneğin, "Relativenem". Sistem tanımlı türler şunlardır: AmbientTemperature, NoUnit, CO2, Depth, Elektriicaliletkenctivity, Leafwetitesi, length, LiquidLevel, Nitrate, O2, PH, Fospnefret, PointInTime, Potassium, basınç, RainGauge, Relativenem, Salinity, Soılmoisture, Soalıngıl, Soalınş Daha fazla bilgi için,/ExtendedType API 'sine veya aşağıdaki [tür ve birimler Ekle bölümüne](weather-partner-integration-in-azure-farmbeats.md#add-extendedtype) bakın
  Dalgalı ölçüler > birimi | Hava durumu telemetri verileri birimi. Sistem tarafından tanımlanan birimler şunlardır: NoUnit, santigrat, Fahrenhayvan, Kelvin, Rankine, Pascal, CIO, Ukrayna, mil ölçüm, Santimeter, ölçüm, Inç, fit, mil, kilometre, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, derece, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, Partspermilyon, MicroMol, mikro Molesperlitre, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Santibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, litre, MilliLiter, saniyeler, UnixTimestamp, MicroMolPerMeterSquaredPerSecond ve InchesPerHour. Daha fazla bilgi için,/ExtendedType API 'sine veya aşağıdaki [tür ve birimler ekleme bölümüne](weather-partner-integration-in-azure-farmbeats.md#add-extendedtype) bakın.
  > AggregationType  | Hiçbiri, ortalama, maksimum, en az, Standardsapma, toplam, toplam
  Hava ölçüleri > derinliği  | Algılayıcının santimetre cinsinden derinliği. Örneğin, zemin altındaki nemi 10 cm ölçümü.
  Hava ölçüleri > açıklaması  | Ölçümün anlamlı bir açıklamasını sağlayın. |
  **JobType** | **Açıklama** |
  Name  | Işin adı; örneğin Get_Daily_Forecast; müşterinin Hava durumu verilerini almak için çalışacağı iş|
  pipelineDetails > parametreler > adı  | parametrenin adı |
  pipelineDetails > parametreler > tür | String, INT, float, bool, Array seçeneklerinden biri |
  pipelineDetails > parametreler > IsRequired | Boolean gerekli parametre ise true, değilse false; Varsayılan değer true 'dur |
  pipelineDetails > parametreler > defaultValue | Parametrenin varsayılan değeri |
  pipelineDetails > parametreler > açıklama | Parametrenin açıklaması |
  Özellikler  | Üreticiden ek özellikler.
  > **Programruncommand** özellikleri | Docker Run komutu-bu komut, müşteri Hava durumu işini çalıştırdığında yürütülür. |
  **Dalgalı bir Datalocation** | **Açıklama** |
  Dalgalı veri ModelId  | Önyükleme sırasında oluşturulan karşılık gelen dalgalı veri modelinin KIMLIĞI|
  location  | Enlem, boylam ve yükseltmeyi temsil eder |
  Name | Nesnenin adı |
  Description | Description |
  farmId | **isteğe bağlı** İş parametresinin bir parçası olarak müşteri tarafından sunulan grubun KIMLIĞI |
  Özellikler  | Üreticiden ek özellikler.

 Nesnelerin ve özelliklerinin her biri hakkında bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsSwagger).

 > [!NOTE]
 > API 'Ler oluşturulan her örnek için benzersiz kimlikler döndürür. Bu KIMLIğIN cihaz yönetimi ve meta veri eşitleme çevirmeni tarafından korunması gerekir.

**Meta veri eşitleme**

Bağlayıcı Docker 'ın meta verilerde güncelleştirme gönderebilme özelliği olmalıdır. Güncelleştirme senaryolarına örnek olarak, hava durumu sağlayıcısının veri kümesindeki yeni hava durumu parametrelerinin eklenmesi, işlevselliğin eklenmesi (örn. 30 günlük tahmine ekleme)

> [!NOTE]
> Meta veriler için silme desteklenmiyor. Hava durumu veri modeli.
>
> Meta verileri güncelleştirmek için, hava durumu veri modelinde/Get/{ID} ' ı çağırmanız, değiştirilen özellikleri güncelleştirmeniz ve ardından, Kullanıcı tarafından ayarlanan tüm özellikler kaybolmaması için/Put/{ID} yapmanız zorunludur.

## <a name="weather-data-telemetry-specifications"></a>Hava durumu verileri (telemetri) belirtimleri

Hava durumu verileri, işlenmek üzere bir Azure Olay Hub 'ına gönderilen kurallı bir iletiyle eşleştirilir. Azure EventHub, bağlı cihazlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alımı sağlayan bir hizmettir. Hava durumu verilerini Farmtts 'e göndermek için, bir olay hub 'ına Farmtts 'de ileti gönderen bir istemci oluşturmanız gerekir. Telemetri gönderme hakkında daha fazla bilgi için bkz. [bir olay hub 'ına telemetri gönderme](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send)

Aşağıda, belirtilen bir olay hub 'ına bir istemci olarak telemetri gönderen örnek bir Python kodu verilmiştir.

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

Kurallı ileti biçimi aşağıdaki gibidir:

```json
{
   "weatherstations": [
   {
   "id": "id of the WeatherDataLocation",
   "weatherdata": [
   {
     "timestamp": "timestamp of the data. For historical, this is the time for which the observations are sent. For forecast this is the time for which data is forecasted. Format is ISO 8601. Default time-zone is UTC",
     "predictiontimestamp": "timestamp on which the forecast data is predicted i.e time of prediction. Required only for forecast data. Format is ISO 8601. Default timezone is UTC ",
     "weathermeasurename1": <value>,
     "weathermeasurename2": <value>
     }
     ]
    }
   ]
}
```

Örneğin, aşağıda bir telemetri iletisi verilmiştir:

```json
{
   "weatherstations": [
   {
     "id": "5e4b34e7-bf9e-4f39-xxxx-f3c06d0366ea",
     "weatherdata": [
     {
      "timestamp": "2019-07-10T00:00:00Z",
      "predictiontimestamp": "2019-07-05T00:00:00Z",
      "PrecipitationTotalLiquidEquivalent": 0,
      "AvgPressure": 0,
      "AvgRelativeHumidity": 72
     }
    ] 
  }
 ]
}

```

## <a name="troubleshooting-and-error-management"></a>Sorun giderme ve hata yönetimi

**Hata günlüğü**

İş ortağı işi mevcut iş çerçevesinde çalışmaya başladıktan sonra, bu hatalar, daha önceden var olan diğer işlere yönelik hatalarla aynı şekilde günlüğe kaydedilir (GetFarmData, Sensoryerleştirmesini vb. gibi). ADF işlem hattı içinde çalışan ADF etkinliği hem STDERR hem de STDOUT 'ı günlüğe kaydeder. Her iki dosya da Farmrets kaynak grubu içindeki "datahublogs-xxx" depolama hesabında kullanılabilir.

Datahub kitaplığı, genel veri hub 'ı günlüklerinin bir parçası olarak günlüğe kaydetmeyi etkinleştirmek için yardımcı işlevler sağlar. Diğer ayrıntıları [burada](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/framework/logger.py) bulabilirsiniz.

**Sorun giderme seçeneği veya destek**

Müşterinin, belirtilen Farmtts örneğinde Hava durumu verileri alamamasından dolayı, hava durumu ortağı destek ve aynı sorunu gidermek için bir mekanizma sağlamalıdır.

## <a name="add-extendedtype"></a>ExtendedType Ekle

Farmtts, yeni algılayıcı ölçü türleri ve birimleri eklemeyi destekler. Bir hava durumu ortağının, [burada](https://github.com/azurefarmbeats/noaa_docker) başvuru uygulamasındaki dosyadaki bootstrap_manifest.jsgüncelleştirerek yeni birim/tür ekleyebileceğini unutmayın

Yeni bir dalgalı ölçü türü eklemek için, örneğin "PrecipitationDepth", aşağıdaki adımları izleyin.

1. /ExtendedType üzerinde sorgu filtresiyle bir GET isteği yapın-anahtar = dalgalı iş Ölçümtürü
2. Döndürülen nesnenin KIMLIĞINI aklınızda edin.
3. Döndürülen nesnedeki listeye yeni türü ekleyin ve/ExtendedType{ID} üzerinde aşağıdaki yeni liste ile bir PUT isteği yapın. Giriş yükü, yukarıda alınan Yanıtla aynı olmalıdır ve değer listesinin sonuna eklenen yeni birim.

/ExtendedType API 'SI hakkında daha fazla bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsSwagger).

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, Farmker ile tümleşen bir bağlayıcı Docker 'a sahipsiniz. Daha sonra, Docker ' i kullanarak hava durumu verilerinin nasıl olacağını aşağıda bulabilirsiniz. Bkz. [Hava durumu verileri al](get-weather-data-from-weather-partner.md).
