---
title: Geçmiş telemetri verilerini alma
description: Bu makalede, geçmiş Telemetri verilerinin nasıl alınacağını açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: d47fdb9461786d80d65ee2448cc983a7a8348ff2
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79298776"
---
# <a name="ingest-historical-telemetry-data"></a>Geçmiş telemetri verilerini alma

Bu makalede, geçmiş algılayıcı verilerinin Azure Farmınts 'e alınması açıklanmaktadır.

Cihazlar ve algılayıcılar gibi Nesnelerin İnterneti (IoT) kaynaklarından geçmiş verileri almak, Farmınts 'de yaygın bir senaryodur. Cihazlar ve sensörler için meta veriler oluşturur ve ardından geçmiş verileri, kurallı bir biçimde Farmto 'Lar için kullanırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleye devam etmeden önce, IoT cihazlarınızdan Farmtts 'yi yüklediğinizden ve geçmiş verileri topladığınızdan emin olun. Ayrıca, aşağıdaki adımlarda belirtildiği gibi iş ortağı erişimini de etkinleştirmeniz gerekir.

## <a name="enable-partner-access"></a>İş ortağı erişimini etkinleştir

Azure Farmtts örneğiniz için iş ortağı tümleştirmesini etkinleştirmeniz gerekir. Bu adım, cihaz iş ortağınız olarak Azure Farmtts örneğine erişimi olan bir istemci oluşturur ve sonraki adımlarda gerekli olan aşağıdaki değerleri sağlar:

- API uç noktası: Bu veri hub URL 'sidir, örneğin, https://\<Datahub >. azurewebsites. net
- Kiracı Kimliği
- İstemci Kimliği
- Gizli anahtar
- EventHub bağlantı dizesi

Şu adımları uygulayın:

>[!NOTE]
> Aşağıdaki adımları uygulamak için yönetici olmanız gerekir.

1. [ZIP dosyasını](https://aka.ms/farmbeatspartnerscriptv2)indirin ve yerel sürücünüze ayıklayın. ZIP dosyasının içinde bir dosya olacaktır.
2. https://portal.azure.com/ oturum açın ve **Azure Active Directory** > **uygulama kayıtlarına**gidin.

3. Farmtts dağıtımınızın bir parçası olarak oluşturulan **uygulama kaydını** seçin. Bu, Farmtts veri hub 'ınızla aynı ada sahip olacaktır.

4. Bir **API 'Yi kullanıma** sunma ' yı seçin > **istemci uygulaması Ekle** ' yi seçin ve **04b07795-8ddb-461A-bbee-02f9e1bf7b46** girin ve **Yetkilendirme kapsamını**denetleyin. Bu işlem, aşağıdaki adımları gerçekleştirmek için Azure CLı (Cloud Shell) erişimi sağlar:

5. Cloud Shell'i açın. Bu seçenek, Azure portal sağ üst köşesindeki araç çubuğunda bulunur.

    ![Azure portal araç çubuğu](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

6. Ortamın **PowerShell**olarak ayarlandığından emin olun. Varsayılan olarak, Bash olarak ayarlanır.

    ![PowerShell araç çubuğu ayarı](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

7. Cloud Shell örneğindeki 1. adımdan dosyayı karşıya yükleyin.

    ![Araç çubuğu düğmesini karşıya yükle](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

8. Dosyanın karşıya yüklendiği dizine gidin. Varsayılan olarak, dosyalar Kullanıcı adı altında ana dizine yüklenir.

9. Aşağıdaki betiği çalıştırın. Betik, **Azure Active Directory** > **Genel Bakış sayfasından**elde edilen Kiracı kimliğini ister.

    ```azurepowershell-interactive

    ./generatePartnerCredentials.ps1   

    ```

10. **API uç noktası**, **kiracı KIMLIĞI**, **Istemci kimliği**, **istemci gizli anahtarı**ve **EventHub bağlantı dizesi**için değerleri yakalamak üzere ekran yönergelerini izleyin.
## <a name="create-device-or-sensor-metadata"></a>Cihaz veya algılayıcı meta verileri oluşturma

 Artık gerekli kimlik bilgilerine sahip olduğunuza göre, cihazı ve algılayıcıları tanımlayabilirsiniz. Bunu yapmak için, Farmpts API 'Lerini çağırarak meta verileri oluşturun. API 'Leri yukarıdaki bölümde oluşturduğunuz istemci uygulaması olarak çağırdığınızdan emin olun.

 Farmrets veri hub 'ı, cihaz veya algılayıcı meta verilerinin oluşturulmasını ve yönetilmesini sağlayan aşağıdaki API 'Lere sahiptir.

 > [!NOTE]
 > Bir iş ortağı olarak yalnızca okuma, meta verileri oluşturma ve güncelleştirme için erişim sahibi olursunuz; **Sil seçeneği iş ortağıyla kısıtlıdır.**

- /**devicemodel**: devicemodel, Cihazın üreticisi ve bir ağ geçidi ya da düğüm olan cihaz türü gibi cihazın meta verilerine karşılık gelir.
- **cihaz**/: cihaz, grupta bulunan bir fiziksel cihaza karşılık gelir.
- **sensormodel**/: sensormodel, üretici, analog veya dijital olan algılayıcı türü ve çevresel sıcaklık ve basınç gibi algılayıcı ölçümü gibi algılayıcı meta verilerine karşılık gelir.
- /**algılayıcı**: algılayıcı, değerleri kaydeden bir fiziksel sensöre karşılık gelir. Bir algılayıcı genellikle cihaz KIMLIĞI olan bir cihaza bağlanır.  


|        DeviceModel   |  Öneriler   |
| ------- | -------             |
|     Tür (düğüm, ağ geçidi)        |          Cihaz düğümü veya ağ geçidi türü      |
|          Üretici            |         Üreticinin adı    |
|  ProductCode                    |  Cihaz ürün kodu veya model adı veya numarası. Örneğin, EnviroMonitor # 6800.  |
|            Bağlantı Noktaları          |     Bağlantı noktası adı ve türü, dijital veya analog.
|     Adı                 |  Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.
      Açıklama     | Modelin anlamlı bir açıklamasını sağlayın.
|    Özellikler          |    Üreticiden ek özellikler.   |
|    **Aygıtların**             |                      |
|   Devicemodelıd     |     İlişkili cihaz modelinin KIMLIĞI.  |
|  Donanım kimliği          | Cihazın MAC adresi gibi benzersiz KIMLIĞI.
|  Reportingınterval        |   Saniye cinsinden raporlama aralığı.
|  Konum            |  Cihaz Latitude (-90 ile + 90), Boylam (-180-180) ve yükseltme (ölçü cinsinden).   
|Parentdeviceıd       |    Bu cihazın bağlı olduğu üst cihazın KIMLIĞI. Örneğin, bir ağ geçidine bağlı bir düğüm. Bir düğümde ağ geçidi olarak Parentdeviceıd vardır.  |
|    Adı            | Kaynağı tanımlamak için bir ad. Cihaz iş ortakları, iş ortağı tarafında cihaz adı ile tutarlı bir ad göndermelidir. İş ortağı cihaz adı Kullanıcı tanımlı ise, aynı kullanıcı tanımlı ad, Farmtts 'ye yayılmalıdır.|
|     Açıklama       |      Anlamlı bir açıklama sağlayın. |
|     Özellikler    |  Üreticiden ek özellikler.
|     **SensorModel**        |          |
|       Tür (analog, dijital)          |      Onun analog veya dijital olup olmadığı algılayıcı türü.       |
|          Üretici            |       Algılayıcısı üreticisi.     |
|     ProductCode| Ürün kodu veya model adı veya numarası. Örneğin, RS-CO2-N01. |
|       Sensorölçüleri > adı       | Algılayıcı ölçüsünün adı. Yalnızca küçük harf desteklenir. Farklı derinlikleri olan ölçümler için derinliği belirtin. Örneğin, soil_moisture_15cm. Bu ad telemetri verileriyle tutarlı olmalıdır.  |
|          Sensorölçüleri > veri türü       |Telemetri veri türü. Şu anda, Double destekleniyor.|
|    Sensorölçüleri > türü    |Algılayıcı Telemetri verilerinin ölçüm türü. Sistem tanımlı türler şunlardır AmbientTemperature, CO2, Depth, Elektriicalttivity, Leafwetity, length, LiquidLevel, Nitrate, O2, PH, Fospnefret, PointInTime, Potassium, basınç, RainGauge, Relativenem, Salınity, SoilMoisture, Soilsıcaklık, SolarRadiation, durum, TimeDuration, UVRadiation, Uıvindex, hacim, WIN Direction, WIN Run, Wıngıı, Evapotranspiration, PAR. Daha fazla eklemek için/ExtendedType API 'sine bakın.|
|        Sensorölçüleri > birimi              | Algılayıcı telemetri verileri birimi. Sistem tarafından tanımlanan birimler NoUnit, santigrat, Fahrenhayt, Kelvin, Rankine, Pascal, CIO, PSI, milimetre, santimetre, metre, Inç, fit, mil, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, derece, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, Partspermilyon, MicroMol, mikro Molesperlitre, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Santibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, litre, MilliLiter, saniyeler, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchesPerHour daha fazla bilgi eklemek Için/ExtendedType API 'sine bakın.|
|    Sensorölçüleri > AggregationType    |  Değerler None, Average, Maximum, minimum veya Standardsapması olabilir.  |
|          Adı            | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.  |
|    Açıklama        | Modelin anlamlı bir açıklamasını sağlayın.  |
|   Özellikler       |  Üreticiden ek özellikler.  |
|    **Algılayıcısı**      |          |
| Donanım kimliği          |   Üretici tarafından ayarlanan algılayıcı için benzersiz KIMLIK. |
|  Sensormodelıd     |    İlişkili algılayıcı modelinin KIMLIĞI.   |
| Konum          |  Enlem (-90 ile + 90), Boylam (-180-180) ve yükseltme (ölçü cinsinden).|
|   Bağlantı noktası > adı        |  Algılayıcıdan cihazda bağlı olduğu bağlantı noktasının adı ve türü. Bunun, cihaz modelinde tanımlananla aynı ada sahip olması gerekir. |
|    DeviceID  |    Algılayıcıın bağlı olduğu cihazın KIMLIĞI.     |
| Adı            |   Kaynağı tanımlamak için ad. Örneğin, algılayıcı adı veya ürün adı ve model numarası ya da ürün kodu.|
|    Açıklama      | Anlamlı bir açıklama sağlayın. |
|    Özellikler        |Üreticiden ek özellikler. |

Nesneler hakkında daha fazla bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

### <a name="api-request-to-create-metadata"></a>Meta veri oluşturmak için API isteği

Bir API isteği oluşturmak için, HTTP (POST) yöntemini, API hizmeti URL 'sini ve sorgulanacak bir kaynağa yönelik URI 'yi birleştiren, verileri gönderecek, oluşturacak veya silebileceğiniz bir kaynağa birleştirmelisiniz. Daha sonra bir veya daha fazla HTTP istek üst bilgisi eklersiniz. API hizmetinin URL 'si, API uç noktasıdır, diğer bir deyişle, Datahub URL 'SI (https://\<yourdatahub >. azurewebsites. net).  

### <a name="authentication"></a>Kimlik Doğrulaması

Farmrets veri hub 'ı, önceki bölümde oluşturulan aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:

- İstemci Kimliği
- Gizli anahtar
- Kiracı Kimliği

Bu kimlik bilgilerini kullanarak, çağıran bir erişim belirteci isteyebilir. Belirtecin sonraki API isteklerinde, üst bilgi bölümünde aşağıdaki gibi gönderilmesi gerekir:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Aşağıdaki örnek Python kodu, Farmtts 'e yönelik sonraki API çağrıları için kullanılabilen erişim belirtecini verir: 

```python
import azure 

from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net" [Azure website](https://<yourdatahub>.azurewebsites.net)
CLIENT_ID = "<Your Client ID>"   
CLIENT_SECRET = "<Your Client Secret>"   
TENANT_ID = "<Your Tenant ID>" 
AUTHORITY_HOST = 'https://login.microsoftonline.com' 
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID 
#Authenticating with the credentials 
context = adal.AuthenticationContext(AUTHORITY) 
token_response = context.acquire_token_with_client_credentials(ENDPOINT, CLIENT_ID, CLIENT_SECRET) 
#Should get an access token here 
access_token = token_response.get('accessToken') 
```


**HTTP istek üstbilgileri**

Aşağıda, Farmrets veri hub 'ına bir API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri verilmiştir:

- **Content-Type**: Application/JSON
- **Yetkilendirme**: taşıyıcı < erişim-belirteç >
- **Kabul et**: uygulama/JSON

### <a name="input-payload-to-create-metadata"></a>Meta veri oluşturmak için giriş yükü

DeviceModel


```json
{
  "type": "Node",
  "manufacturer": "string",
  "productCode": "string",
  "ports": [
    {
      "name": "string",
      "type": "Analog"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

Cihaz

```json
{
  "deviceModelId": "string",
  "hardwareId": "string",
  "farmId": "string",
  "reportingInterval": 0,
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "parentDeviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

SensorModel

```json
{
  "type": "Analog",
  "manufacturer": "string",
  "productCode": "string",
  "sensorMeasures": [
    {
      "name": "string",
      "dataType": "Double",
      "type": "string",
      "unit": "string",
      "aggregationType": "None",
      "depth": 0,
      "description": "string"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Algılayıcısı

```json
{
  "hardwareId": "string",
  "sensorModelId": "string",
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "depth": 0,
  "port": {
    "name": "string",
    "type": "Analog"
  },
  "deviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
Aşağıdaki örnek istek bir cihaz oluşturur. Bu istek, istek gövdesiyle birlikte JSON girişi olarak giriş içeriyor.

```bash
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H  
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  
\"reportingInterval\": 900,  \"name\": \"Device123\",  
\"description\": \"Test Device 123\"}" *
```

Python 'da örnek bir kod aşağıda verilmiştir. Bu örnekte kullanılan erişim belirteci, kimlik doğrulaması sırasında alınan aynıdır.

```python
import requests
import json

# Got access token - Calling the Device Model API
headers = {
    "Authorization": "Bearer " + access_token,
    "Content-Type" : "application/json"
    }
payload = '{"type" : "Node", "productCode" : "TestCode", "ports": [{"name": "port1","type": "Analog"}], "name" : "DummyDevice"}'
response = requests.post(ENDPOINT + "/DeviceModel", data=payload, headers=headers)
```


> [!NOTE]
> API 'Ler oluşturulan her örnek için benzersiz kimlikler döndürür. Karşılık gelen telemetri iletilerini göndermek için kimlikleri tutmanız gerekir.

### <a name="send-telemetry"></a>Telemetri gönderme

Artık, Farmtts 'de cihazları ve algılayıcıları oluşturduğunuza göre, ilişkili telemetri iletilerini gönderebilirsiniz.

### <a name="create-a-telemetry-client"></a>Telemetri istemcisi oluşturma

İşlenmek üzere Azure Event Hubs telemetri göndermeniz gerekir. Azure Event Hubs, bağlı cihazlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alımı sağlayan bir hizmettir. Telemetriye telemetri verileri göndermek için, Farmtts 'deki bir olay hub 'ına ileti gönderen bir istemci oluşturun. Telemetri gönderme hakkında daha fazla bilgi için bkz. [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

### <a name="send-a-telemetry-message-as-the-client"></a>İstemci olarak bir telemetri iletisi gönder

Event Hubs istemcisi olarak bir bağlantı kurulduktan sonra, Olay Hub 'ına JSON olarak iletiler gönderebilirsiniz.

Aşağıda, belirtilen bir olay hub 'ına bir istemci olarak telemetri gönderen örnek Python kodu verilmiştir:

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

Geçmiş algılayıcı veri biçimini Azure Farmtarafından anladığı kurallı bir biçime dönüştürün. Kurallı ileti biçimi aşağıdaki gibidir:

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": "<values>"
        }
      ]
    }
 ]
}
```

İlgili cihazları ve algılayıcıları ekledikten sonra, önceki bölümde açıklandığı gibi telemetri iletisinde cihaz KIMLIĞINI ve algılayıcı KIMLIĞINI alın.

Telemetri iletisine bir örnek aşağıda verilmiştir:


 ```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version": "1",
  "sensors": [
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 15
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 42
        }
      ]
    },
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 20
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 89
        }
      ]
    }
  ]
}
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerden geçmiş/akış verileri alındıktan sonra telemetri verileri görüntülenemiyor

**Belirti**: cihazlar veya algılayıcılar dağıtılır ve bu cihaz/sensörler ve EventHub üzerinde telemetri/algılayıcılar oluşturdunuz, ancak bu verileri, farmınts üzerinde telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. Uygun iş ortağı kaydını bitirdiğinizden emin olun. bunu, veri hub 'ının Swagger 'ınızla gidip/partner API 'sine giderek bir get yapın ve iş ortağının kayıtlı olup olmadığını kontrol edebilirsiniz. Aksi takdirde, iş ortağı eklemek için [buradaki adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.

2. İş ortağı istemci kimlik bilgilerini kullanarak meta verileri (DeviceModel, cihaz, Sensormodeli, algılayıcı) oluşturduğunuzdan emin olun.

3. Doğru telemetri ileti biçimini kullandığınızdan emin olun (aşağıda belirtildiği gibi):

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": "<value>"
        }
      ]
    }
 ]
}
```


## <a name="next-steps"></a>Sonraki adımlar

REST API tabanlı tümleştirme ayrıntıları hakkında daha fazla bilgi için bkz. [REST API](rest-api-in-azure-farmbeats.md).
