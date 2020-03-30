---
title: Görüntüleme iş ortağı tümleştirmesi
description: Bu makalede, görüntü ortağı tümleştirme açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 62e5b363f8008380a61e24c0549573a30ecaeb73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77131861"
---
# <a name="imagery-partner-integration"></a>Görüntüleme iş ortağı tümleştirmesi

Bu makalede, FarmBeats'e görüntü verileri göndermek için Azure FarmBeats Translator bileşeninin nasıl kullanılacağı açıklanmaktadır. Tarımsal görüntü verileri, çoklu spektral kameralar, uydular ve insansız hava araçları gibi çeşitli kaynaklardan oluşturulabilir. Tarımsal görüntü ortakları, müşterilerine çiftlikleri için özel olarak oluşturulan haritalar sunmak için FarmBeats ile entegre olabilir.

Veriler, bir kez kullanılabilir, FarmBeats Hızlandırıcı ile görselleştirilmiş ve potansiyel tarım işletmeleri veya müşteri sistemi entegratörleri tarafından veri füzyon ve makine öğrenme / yapay zeka (ML / AI) model oluşturma için kullanılabilir.

FarmBeats yeteneği sağlar:

- /ExtendedType API'lerini kullanarak özel görüntü türlerini, kaynağı ve dosya biçimini tanımlayın.
- /Scene ve /SceneFile API'leri aracılığıyla çeşitli kaynaklardan alınan görüntü verilerini alın.

Aşağıdaki bilgiler FarmBeats sistemine herhangi bir görüntü türü elde etmek üzerinde duruluyor.

**Drone Görüntüleri** bölümünü seçtiğinizde, drone ortomoziğinin yüksek çözünürlüklü görüntüsünü göstermek için bir açılır pencere açılır. Drone uçuşlarının planlanıp ham veri elde etmeye yardımcı olan ortak yazılıma erişebilirsiniz. Ortağın yazılımını yol planlaması ve ortomoz görüntü dikişi için kullanmaya devam edersiniz.

Drone iş ortaklarının müşterilerin müşteri hesaplarını Azure'daki FarmBeats örnekleriyle ilişkilendirmelerini sağlaması gerekir.

FarmBeats'i bağlamak için drone ortağı yazılımında aşağıdaki kimlik bilgilerini kullanmanız gerekir:

- API uç noktası
- Kiracı Kimliği
- İstemci Kimliği
- Gizli anahtar

## <a name="api-development"></a>API geliştirme

API'ler Swagger teknik dokümantasyon içerir. API'ler ve ilgili istekler veya yanıtlar hakkında bilgi [için](https://aka.ms/FarmBeatsDatahubSwagger)bkz.

## <a name="authentication"></a>Kimlik doğrulaması

FarmBeats, Microsoft Azure [Etkin Dizini 'ni](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) (Azure AD) kullanır. Azure Uygulama Hizmeti yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar. 

Azure AD hakkında daha fazla bilgi için [Azure Etkin Dizini'ne](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization)bakın.   

FarmBeats Datahub, aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:

- İstemci Kimliği
- Gizli anahtar
- Kiracı Kimliği

Arayan, önceki kimlik bilgilerini kullanarak, sonraki API isteklerinde, üstbilgi bölümünde aşağıdaki gibi gönderilmesi gereken bir erişim belirteci isteyebilir:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Aşağıdaki Python kod örneği erişim belirteci alır. Daha sonra FarmBeats sonraki API aramaları için belirteci kullanabilirsiniz.

```python
from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net"   
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

## <a name="http-request-headers"></a>HTTP istek üstbilgi

FarmBeats Datahub'a API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri aşağıda veda eder.

**Üst bilgi** | **Açıklama ve örnek**
--- | ---
İçerik Türü  | İstek biçimi (İçerik Türü:<format>uygulama/ ). FarmBeats Datahub API'leri için biçim JSON'dur. İçerik Türü: uygulama/json
Yetkilendirme | API araması yapmak için gereken erişim belirteci belirtir. Yetkilendirme: Taşıyıcı <Erişim-Belirteç>
Kabul Et  | Yanıt biçimi. FarmBeats Datahub API'leri için biçim JSON'dur. Kabul Et: uygulama/json


## <a name="api-requests"></a>API istekleri

REST API isteği nde bulunmak için şunları birleştirirsiniz:

- HTTP yöntemi (GET, POST ve PUT).
- API hizmetinin URL'si.
- KAYNAK URI (sorgu, veri göndermek, güncelleştirmek veya silmek için).
- Bir veya daha fazla HTTP istek üstbilgi.

İsteğe bağlı olarak, yanıtlarda verileri filtrelemek, boyutunu sınırlamak ve sıralamak için GET çağrılarına sorgu parametrelerini ekleyebilirsiniz.

Aşağıdaki örnek istek cihazların listesini almaktır:

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H
"Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>”
```

Çoğu GET, POST ve PUT aramaları bir JSON istek gövdesi gerektirir.

Aşağıdaki örnek istek bir aygıt oluşturmaktır. Bu örnek istek gövdesi ile bir giriş JSON vardır.


```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  
"accept: application/json" -H  
"Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d
"{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  
\"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Veri biçimi

JSON, rasgele veri yapılarının basit bir metin gösterimi sağlayan ortak bir dilden bağımsız veri biçimidir. Daha fazla bilgi için [JSON org adresine](https://JSON.org)bakın.

## <a name="ingest-imagery-into-farmbeats"></a>FarmBeats içine görüntüleri yutmak

İş ortağının FarmBeats Datahub'a bağlanmak için kimlik bilgileri ne dikten sonra, iş ortağı Çevirmen bileşeninde aşağıdaki adımları atar.

1.  Yüklenecek görüntü türüne uygun olarak, aşağıdaki alanlar için yeni bir genişletilmiş tür oluşturun:

    - **Sahne Kaynağı**: Örneğin, drone_partner_name
    - **Sahne Tipi**: Örneğin, drone
    - **Sahne Dosya Türü**: Örneğin, klorofil indeksi
    - **Sahne Dosyası İçerik Türü**: Örneğin, resim/tiff

2.  Azure FarmBeats sistemindeki çiftliklerin listesini almak için /Farms API'yi arayın.
3.  Müşteriye çiftlikler listesinden tek bir çiftlik seçme olanağı sağlayın.

    Ortak sistem yol planlama ve drone uçuş ve görüntü toplama yapmak için ortak yazılım içinde çiftlik göstermek gerekir.

4.  /Scene API'yi arayın ve benzersiz bir sahne kimliğiyle yeni bir sahne oluşturmak için gerekli ayrıntıları sağlayın.
5.  FarmBeats sisteminde, seçilen çiftlik bağlamında FarmBeats Datahub'a gerekli resimleri yüklemek için bir blob SAS URL'si alın.

İşte API aramaları hakkında ayrıntılı bir akış.

### <a name="step-1-extendedtype"></a>Adım 1: ExtendedType

FarmBeats'te tür ve dosya kaynağının kullanılabilir olup olmadığını görmek için /ExtendedType API'sini iade edin. Bunu yapmak için /ExtendedType API'den GET'i arayın.

Sistem tanımlı değerler şunlardır:

```json
{
  "items": [
    {
      "id": "ae2aa527-7d27-42e2-8fcf-841937c651d7",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:47:42.5910344Z",
      "key": "SceneFileType",
      "value": [
        "evi",
        "ndvi",
        "ndwi",
        "tci",
        "soil-moisture",
        "sensor-placement",
        "sentinel-b01",
        "sentinel-b02",
        "sentinel-b03",
        "sentinel-b04",
        "sentinel-b05",
        "sentinel-b06",
        "sentinel-b07",
        "sentinel-b08",
        "sentinel-b8a",
        "sentinel-b09",
        "sentinel-b10",
        "sentinel-b11",
        "sentinel-b12",
        "cloud-mask",
        "farm-mask",
        "image/tiff",
        "orthomosaic"
      ],
      "description": "List of scene file types available in system. User can add more values."
    },
    {
      "id": "b7824d2e-3e43-46d3-8fd4-ec7ba8918d84",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:44.8914691Z",
      "key": "SceneFileContentType",
      "value": [
        "image/tiff",
        "image/png",
        "image/jpeg",
        "text/csv",
        "text/plain",
        "text/tab-separated-values",
        "application/json",
        "application/octet-stream",
        "orthomosaic"
      ],
      "description": "List of scene file content types available in system. User can add more values."
    },
    {
      "id": "21d2924d-8b3e-4ce4-a7f8-318e85c0f7f2",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:37.5096445Z",
      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P",
        "DJI"
      ],
      "description": "List of scene sources available in system. User can add more values."
    },
    {
      "id": "6e09ca69-cacf-4b53-b63a-53c659aae4a4",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:34.7483899Z",
      "key": "SceneType",
      "value": [
        "base-bands",
        "sensor-placement",
        "soil-moisture",
        "evi",
        "ndwi",
        "ndvi",
        "drone",
        "orthomosaic-drone"
      ],
      "description": "List of scene types available in system. User can add more values."
    },
    {
      "id": "55d8436b-3a86-4bea-87ab-e1b51226525f",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureType",
      "value": [
        "AmbientTemperature",
        "CO2",
        "Depth",
        "ElectricalConductivity",
        "LeafWetness",
        "Length",
        "LiquidLevel",
        "Nitrate",
        "O2",
        "PH",
        "Phosphate",
        "PointInTime",
        "Potassium",
        "Pressure",
        "RainGauge",
        "RelativeHumidity",
        "Salinity",
        "SoilMoisture",
        "SoilTemperature",
        "SolarRadiation",
        "State",
        "TimeDuration",
        "UVRadiation",
        "UVIndex",
        "Volume",
        "WindDirection",
        "WindRun",
        "WindSpeed",
        "Evapotranspiration",
        "PAR"
      ],
      "description": "List of sensor measure types available in system. User can add more values."
    },
    {
      "id": "0dfd6e6b-df58-428f-9ab8-a0674bfdcbe5",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureUnit",
      "value": [
        "NoUnit",
        "Celsius",
        "Fahrenheit",
        "Kelvin",
        "Rankine",
        "Pascal",
        "Mercury",
        "PSI",
        "MilliMeter",
        "CentiMeter",
        "Meter",
        "Inch",
        "Feet",
        "Mile",
        "KiloMeter",
        "MilesPerHour",
        "MilesPerSecond",
        "KMPerHour",
        "KMPerSecond",
        "MetersPerHour",
        "MetersPerSecond",
        "Degree",
        "WattsPerSquareMeter",
        "KiloWattsPerSquareMeter",
        "MilliWattsPerSquareCentiMeter",
        "MilliJoulesPerSquareCentiMeter",
        "VolumetricWaterContent",
        "Percentage",
        "PartsPerMillion",
        "MicroMol",
        "MicroMolesPerLiter",
        "SiemensPerSquareMeterPerMole",
        "MilliSiemensPerCentiMeter",
        "Centibar",
        "DeciSiemensPerMeter",
        "KiloPascal",
        "VolumetricIonContent",
        "Liter",
        "MilliLiter",
        "Seconds",
        "UnixTimestamp",
        "MicroMolPerMeterSquaredPerSecond",
        "InchesPerHour"
      ],
      "description": "List of sensor measure units available in system. User can add more values."
    }
  ]
}
```

Bu adım tek seferlik bir kurulumdur. Bu yeni sahne türünün kapsamı, Azure FarmBeats'in yüklendiği abonelikle sınırlıdır.

Örneğin, SceneSource eklemek için: "SlantRange," anahtar "SceneSource" giriş yükü ile / ExtendedType API kimliği bir PUT yapmak.

```json
{

      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P"
      ],
      "description": "List of scene sources available in system. User can add more values."
}

```

Yeşil alan, sistem tanımlı sahne kaynağı değerlerine yeni ektir.

### <a name="step-2-get-farm-details"></a>Adım 2: Çiftlik ayrıntılarını alın

Sahneler (.tiff veya .csv dosyaları) bir çiftlik bağlamındadır. /Farm API'de GET yaparak çiftlik detaylarını almanız gerekir. API FarmBeats kullanılabilir çiftliklerin listesini döndürür. Verileri almak istediğiniz çiftliği seçebilirsiniz.

GET /Farm yanıtı:

```json
{
  "id": "07f3e09c-89aa-4619-8d50-e57fb083d8f9",
  "createdAt": "2019-11-01T13:55:41.8804663Z",
  "lastModifiedAt": "2019-11-01T13:55:41.8804663Z",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          78.34252251428694,
          17.402556352862675
        ],
        [
          78.34278000635095,
          17.407920852463022
        ],
        [
          78.34883106989963,
          17.40878079576528
        ],
        [
          78.3498181228054,
          17.403539173730607
        ],
        [
          78.34805859369493,
          17.39977166504127
        ],
        [
          78.34252251428694,
          17.402556352862675
        ]
      ]
    ]
  },
  "name": "Samplefarm",
  "properties": {
    "crops": "Corn",
    "address": "Sample street"
  }
}
 ```

### <a name="step-3-create-a-scene-id-post-call"></a>Adım 3: Sahne kimliği oluşturma (POSTA araması)

Verilen bilgilerle birlikte, sahnenin ilişkili olduğu tarih, sıra ve çiftlik kimliğini sağlayan yeni bir sahne (.tiff veya .csv dosyası) oluşturun. Sahneyle ilişkili meta veriler, ölçünün süresini ve türünü içeren özellikler altında tanımlanabilir.

Yeni bir sahne oluşturmak, çiftlikle ilişkili yeni bir sahne kimliği oluşturur. Sahne kimliği oluşturulduktan sonra, kullanıcı yeni bir dosya (.tiff veya .csv) oluşturmak ve dosyanın içeriğini depolamak için aynı şeyi kullanabilir.

/Scene API'daki POST çağrısı için örnek giriş yükü:

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

API yanıtı:

```json
{
  "id": "a0505928-c480-491b-ba31-d38285a28c1d",
  "createdAt": "2019-10-04T16:19:12.4838584Z",
  "lastModifiedAt": "2019-10-04T16:19:12.4838584Z",
  "type": "new type",
  "source": "SlantRange-3P",
  "farmId": "d41a33e7-b73e-480e-9279-0fcb3207332b",
  "date": "2019-10-04T16:13:39.064Z",
  "sequence": 5,
  "name": "test scene",
  "description": "test scene description",
  "properties": {}
}

```

**Sahne dosyası oluşturma**

Adım 3'te döndürülen sahne kimliği, sahne dosyasının girişidir. Sahne dosyası, 24 saat boyunca geçerli olan bir SAS URL belirteci döndürür.

Kullanıcı bir görüntü akışı yükleme programlı bir şekilde gerekiyorsa, blob depolama SDK sahne dosyası kimliği, konumu ve URL kullanarak bir yöntem tanımlamak için kullanılabilir.

/SceneFile API'daki POST çağrısı için örnek giriş yükü:

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
API yanıtı:

```json
{
  "uploadSASUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840?sv=2018-03-28&sr=b&sig=%2F1426JkDcIFE5g3d%2BjOevCVMIn%2FJo9YKwBn3La5zL8Y%3D&se=2019-10-05T16%3A23%3A57Z&sp=w",
  "id": "e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "createdAt": "2019-10-04T16:23:57.1192916Z",
  "lastModifiedAt": "2019-10-04T16:23:57.1192916Z",
  "blobUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {}
}


```

/SceneFile API'ye yapılan POST çağrısı, Azure Blob depolama istemcisini veya kitaplığını kullanarak .csv veya .tiff dosyasını yüklemek için kullanılabilecek bir SAS yükleme URL'si döndürür.


## <a name="next-steps"></a>Sonraki adımlar

REST API tabanlı tümleştirme ayrıntıları hakkında daha fazla bilgi için [REST API'ye](rest-api-in-azure-farmbeats.md)bakın.
