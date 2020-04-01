---
title: Geçmiş telemetri verilerini alma
description: Bu makalede, geçmiş telemetri verilerinin nasıl yutuluruluyorum.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: e0a5e89f256b562ce5f702e9ff1388cb4d021bf5
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437685"
---
# <a name="ingest-historical-telemetry-data"></a>Geçmiş telemetri verilerini alma

Bu makalede, azure farmbeats içine geçmiş sensör verileri almak için nasıl açıklanmaktadır.

Cihazlar ve sensörler gibi Nesnelerin İnterneti (IoT) kaynaklarından geçmiş verileri yutmak FarmBeats'te yaygın bir senaryodur. Aygıtlar ve sensörler için meta veriler oluşturur sunuz ve tarihi verileri farmbeats'e kanonik biçimde sindirirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleye devam etmeden önce FarmBeats yüklü ve IoT aygıtlarınızdan geçmiş verileri toplanan emin olun. Ayrıca, aşağıdaki adımlarda belirtildiği gibi iş ortağı erişimini etkinleştirmeniz gerekir.

## <a name="enable-partner-access"></a>İş ortağı erişimini etkinleştirme

Azure FarmBeats örneğinize iş ortağı tümleştirmesini etkinleştirmeniz gerekir. Bu adım, aygıt ortağınız olarak Azure FarmBeats örneğinize erişimi olan bir istemci oluşturur ve sonraki adımlarda gereken aşağıdaki değerleri sağlar:

- API bitiş noktası: Bu Datahub URL'sidir, örneğin, https://\<datahub>.azurewebsites.net
- Kiracı Kimliği
- İstemci Kimliği
- Gizli anahtar
- EventHub bağlantı dizesi

Şu adımları uygulayın:

> [!NOTE]
> Aşağıdaki adımları yapmak için bir yönetici olmalısınız.

1. https://portal.azure.com/ adresinde oturum açın.

2. **FarmBeats sürüm 1.2.7 veya sonraki sürümdeyseniz, a, b ve c adımlarını atlayın ve adım 3'e gidin.** FarmBeats UI'nin sağ üst köşesindeki **Ayarlar** simgesini seçerek FarmBeats sürümünü kontrol edebilirsiniz.

      a.  Azure **Active Directory** > **Uygulama Kayıtları'na** gidin

      b. FarmBeats dağıtımınızın bir parçası olarak oluşturulan **Uygulama Kaydı'nı** seçin. FarmBeats datahub'ınızla aynı ada sahip olacaktır.

      c. **Bir API'yi ortaya >** seçin bir istemci uygulaması **ekleyin** ve **04b07795-8ddb-461a-bbee-02f9e1bf7b46** girin ve Yetki Kapsamı'nı kontrol **edin.** Bu, aşağıdaki adımları gerçekleştirmek için Azure CLI'ye (Bulut Kabuğu) erişim sağlayacaktır:

3. Cloud Shell'i açın. Bu seçenek, Azure portalının sağ üst köşesindeki araç çubuğunda kullanılabilir.

    ![Azure portal araç çubuğu](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

4. Ortamın **PowerShell**olarak ayarlandığından emin olun. Varsayılan olarak, Bash olarak ayarlanır.

    ![PowerShell araç çubuğu ayarı](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

5. Ev dizine git.

    ```azurepowershell-interactive 
    cd  
    ```

6. Şu komutu çalıştırın. Bu, ev dizininize bir komut dosyası indirir.

    ```azurepowershell-interactive 

    wget –q https://aka.ms/farmbeatspartnerscriptv3 -O ./generatePartnerCredentials.ps1

    ```

7. Aşağıdaki komut dosyasını çalıştırın. Komut dosyası, **Azure Etkin Dizin** > **Genel Bakış** sayfasından elde edilebilen Kiracı Kimliğini sorar.

    ```azurepowershell-interactive 

    ./generatePartnerCredentials.ps1   

    ```

8. **API Endpoint,** **Kiracı Kimliği,** **İstemci Kimliği, İstemci** **Gizli**ve **EventHub Bağlantı Dizesi**değerlerini yakalamak için ekrandaki yönergeleri izleyin.


## <a name="create-device-or-sensor-metadata"></a>Aygıt veya sensör meta verileri oluşturma

 Artık gerekli kimlik bilgilerine sahip olduğunuza göre, aygıtı ve sensörleri tanımlayabilirsiniz. Bunu yapmak için FarmBeats API'lerini arayarak meta verileri oluşturun. Yukarıdaki bölümde oluşturduğunuz istemci uygulaması olarak API'leri aradığından emin olun.

 FarmBeats Datahub, aygıt veya sensör meta verilerinin oluşturulmasını ve yönetilmesini sağlayan aşağıdaki API'lere sahiptir.

 > [!NOTE]
 > Bir iş ortağı olarak yalnızca meta verileri okumak, oluşturmak ve güncelleştirmek için erişebilirsiniz; **silme seçeneği ortakla sınırlıdır.**

- /**DeviceModel : DeviceModel,** üretici ve ağ geçidi veya düğüm olan aygıt türü gibi aygıtın meta verilerine karşılık gelir.
- /**Cihaz**: Cihaz çiftlikte bulunan fiziksel bir cihaza karşılık gelir.
- /**SensorModel : SensorModel,** üretici, analog veya dijital sensör tipi ve ortam sıcaklığı ve basınç gibi sensör ölçümü gibi sensörün meta verilerine karşılık gelir.
- /**Sensör**: Sensör, değerleri kaydeden fiziksel bir sensöre karşılık gelir. Sensör genellikle aygıt kimliğine sahip bir aygıta bağlanır.  


|        DeviceModel   |  Öneriler   |
| ------- | -------             |
|     Türü (düğüm, ağ geçidi)        |          Cihazın Türü - Düğüm veya Ağ Geçidi      |
|          Üretici            |         Üreticinin adı    |
|  Productcode                    |  Cihaz ürün kodu veya model adı veya numarası. Örneğin, EnviroMonitor#6800.  |
|            Bağlantı Noktaları          |     Dijital veya analog olan bağlantı noktası adı ve türü.
|     Adı                 |  Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.
      Açıklama     | Modelin anlamlı bir açıklamasını sağlayın.
|    Özellikler          |    Üreticiden ek özellikler.   |
|    **Cihaz**             |                      |
|   DeviceModelId     |     İlişkili aygıt modelinin kimliği.  |
|  Hırdavat          | MAC adresi gibi aygıt için benzersiz kimlik.
|  RaporlamaInterval        |   Saniye cinsinden raporlama aralığı.
|  Konum            |  Cihaz enlemi (-90 ila +90), boylam (-180-180) ve yükseklik (metre olarak).   
|ParentDeviceId       |    Bu aygıtın bağlı olduğu ana aygıtın kimliği. Örneğin, bir ağ geçidine bağlı bir düğüm. Bir düğümün ağ geçidi olarak parentDeviceId vardır.  |
|    Adı            | Kaynağı tanımlamak için bir ad. Aygıt ortakları, ortak tarafındaki aygıt adıyla tutarlı bir ad göndermelidir. İş ortağı aygıtı adı kullanıcı tanımlıysa, aynı kullanıcı tanımlı ad FarmBeats'e yayılmalıdır.|
|     Açıklama       |      Anlamlı bir açıklama sağlayın. |
|     Özellikler    |  Üreticiden ek özellikler.
|     **Sensör Modeli**        |          |
|       Türü (analog, dijital)          |      Sensör türü, analog veya dijital olsun.       |
|          Üretici            |       Sensörün üreticisi.     |
|     Productcode| Ürün kodu veya model adı veya numarası. Örneğin, RS-CO2-N01. |
|       SensorMeasures> Adı       | Sensör ölçüsünün adı. Yalnızca küçük harf desteklenir. Farklı derinliklerden ölçümler için derinliği belirtin. Örneğin, soil_moisture_15cm. Bu ad telemetri verileriyle tutarlı olmalıdır.  |
|          SensorMeasures > DataType       |Telemetri veri türü. Şu anda, çift desteklenir.|
|    SensorMeasures > Tipi    |Sensör telemetri verilerinin ölçüm türü. Sistem tanımlı tipler AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Fosfat, PointInTime, Potasyum, Basınç, RainGauge, RelativeMoisturey, Tuzluluk, SoilMoisture, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, WindSpeed, WindSpeed, WindSpeed PARSIlasyon paritesidir. Daha fazla eklemek için /ExtendedType API'sine bakın.|
|        SensorMeasures > Ünitesi              | Sensör telemetri veri birimi. Sistem tanımlı birimler NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, Milimetre, Santimetre, Metre, Inç, Feet, Mile, Kilometre, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Derece, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetrikWaterContent, Yüzde, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquarePerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Saniye, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchPerHour daha fazla eklemek için , / Genişletilmiş Type API bakın.|
|    SensorMeasures> Agregasyon Tipi    |  Değerler hiçbir, ortalama, maksimum, minimum veya StandardDeviation olabilir.  |
|          Adı            | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.  |
|    Açıklama        | Modelin anlamlı bir açıklamasını sağlayın.|
|   Özellikler       |  Üreticiden ek özellikler.|
|    **Sensör**      |          |
| Hırdavat          |   Üretici tarafından ayarlanan sensör için benzersiz kimlik.|
|  SensorModelId     |    İlişkili sensör modelinin kimliği.|
| Konum          |  Sensör enlemi (-90 ila +90), boylam (-180-180) ve yükseklik (metre olarak).|
|   Port > Adı        |  Sensörün cihazda bağlı olduğu bağlantı noktasının adı ve türü. Bu, aygıt modelinde tanımlanan adla aynı olması gerekir.|
|    Deviceıd  |    Sensörün bağlı olduğu cihazın kimliği. |
| Adı            |   Kaynağı tanımlamak için ad. Örneğin, sensör adı veya ürün adı ve model numarası veya ürün kodu.|
|    Açıklama      | Anlamlı bir açıklama sağlayın.|
|    Özellikler        |Üreticiden ek özellikler.|

Nesneler hakkında daha fazla bilgi için Bkz. [Swagger.](https://aka.ms/FarmBeatsDatahubSwagger)

### <a name="api-request-to-create-metadata"></a>Meta veri oluşturmak için API isteği

API isteğinde bulunmak için HTTP (POST) yöntemini, API hizmetinin URL'sini ve URI'yi sorgulanacak, veri göndermek, oluşturmak veya silmek için bir kaynağa birleştirirsiniz. Sonra bir veya daha fazla HTTP istek üstbilgiekleyin. API hizmetinin URL'si API bitiş noktasıdır, diğer bir\<şey de Datahub URL'sidir (https:// yourdatahub>.azurewebsites.net).  

### <a name="authentication"></a>Kimlik Doğrulaması

FarmBeats Datahub, önceki bölümde oluşturulan aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:

- İstemci Kimliği
- Gizli anahtar
- Kiracı Kimliği

Arayan bu kimlik bilgilerini kullanarak bir erişim belirteci isteyebilir. Belirteç sonraki API isteklerinde, üstbilgi bölümünde aşağıdaki gibi gönderilmelidir:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Aşağıdaki örnek Python kodu FarmBeats sonraki API aramaları için kullanılabilecek erişim belirteci verir: 

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

**HTTP istek üstbilgi**

FarmBeats Datahub'a API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri şunlardır:

- **İçerik Türü**: uygulama/json
- **Yetkilendirme**: Taşıyıcı <Erişim-Belirteç>
- **Kabul et**: uygulama/json

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

Sensör Modeli

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
Sensör

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

Aşağıdaki örnek istek bir aygıt oluşturur. Bu istek, JSON'u istek gövdesine yük olarak girdi.

```bash
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H  
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  
\"reportingInterval\": 900,  \"name\": \"Device123\",  
\"description\": \"Test Device 123\"}" *
```

Aşağıda Python bir örnek koddur. Bu örnekte kullanılan erişim belirteci, kimlik doğrulaması sırasında alınanla aynıdır.

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
> API'ler oluşturulan her örnek için benzersiz adlar döndürer. İlgili telemetri iletilerini göndermek için renkli leri saklamanız gerekir.

### <a name="send-telemetry"></a>Telemetri gönderme

FarmBeats'teki cihazları ve sensörleri oluşturduğunuza göre, ilişkili telemetri iletilerini gönderebilirsiniz.

### <a name="create-a-telemetry-client"></a>Bir telemetri istemcisi oluşturma

İşlem için telemetriyi Azure Etkinlik Hub'larına göndermeniz gerekir. Azure Etkinlik Hub'ları, bağlı aygıtlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alınmasını sağlayan bir hizmettir. FarmBeats'e telemetri verileri göndermek için FarmBeats'teki bir etkinlik merkezine ileti gönderen bir istemci oluşturun. Telemetri gönderme hakkında daha fazla bilgi için [Azure Etkinlik Hub'larına](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send)bakın.

### <a name="send-a-telemetry-message-as-the-client"></a>İstemci olarak bir telemetri iletisi gönderme

Olay Hub'ları istemcisi olarak kurulan bir bağlantınız olduktan sonra, olay merkezine JSON olarak ileti gönderebilirsiniz.

Burada, belirli bir olay hub'ına istemci olarak telemetri gönderen örnek Python kodu vermiştir:

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

Geçmiş sensör veri biçimini Azure FarmBeats'in anladığı bir kanonik biçime dönüştürün. Kanonik ileti biçimi aşağıdaki gibidir:

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
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

İlgili aygıtları ve sensörleri ekledikten sonra, önceki bölümde açıklandığı gibi, telemetri iletisinde aygıt kimliğini ve sensör kimliğini edinin.

Bir telemetri iletisi örneği aşağıda verilmiştir:


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

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sensörlerinizden geçmiş/akışverilerini sindirdikten sonra telemetri verilerini görüntüleyemiyorum

**Belirti**: Cihazlar veya sensörler dağıtılır ve FarmBeats'teki cihazları/sensörleri oluşturdunuz ve EventHub'a telemetri aldınız, ancak FarmBeats'teki telemetri verilerini alamıyor veya görüntüleyemezsiniz.

**Düzeltici eylem**:

1. Uygun iş ortağı kaydını yaptığınızdan emin olun - datahub swagger'ınıza giderek bunu kontrol edebilir, /İş Ortağı API'sine gidin, Ortak kayıtlı olup olmadığını alın ve kontrol edin. Değilse, iş ortağı eklemek için [aşağıdaki adımları](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats) izleyin.

2. Ortak istemci kimlik bilgilerini kullanarak meta verileri (DeviceModel, Device, SensorModel, Sensor) oluşturduğunuzdan emin olun.

3. Doğru Telemetri ileti biçimini kullandığınızdan emin olun (aşağıda belirtildiği gibi):

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
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

REST API tabanlı tümleştirme ayrıntıları hakkında daha fazla bilgi için [REST API'ye](rest-api-in-azure-farmbeats.md)bakın.
