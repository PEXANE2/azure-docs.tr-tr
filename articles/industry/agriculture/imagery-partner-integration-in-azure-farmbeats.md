---
title: Görüntüleme iş ortağı tümleştirmesi
description: Bu makalede Imagery iş ortağı tümleştirmesi açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 62e5b363f8008380a61e24c0549573a30ecaeb73
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131861"
---
# <a name="imagery-partner-integration"></a>Görüntüleme iş ortağı tümleştirmesi

Bu makalede, Azure Farmtts Translator bileşeninin, verileri Farmto 'a göndermek için nasıl kullanılacağı açıklanır. Agricultürey verileri, multispectral kameralar, uydu ve Drones gibi çeşitli kaynaklardan üretilebilir. Agricultürel iş ortakları, müşterilere grupları için özel olarak oluşturulan haritalar sağlamak üzere Farmtempts ile tümleştirilebilir.

Veriler, kullanılabilir olduktan sonra, bir şekilde gizlenebilir hızlandırıcı ve potansiyel olarak veri Fusion ve makine öğrenimi/yapay zekası (ML/AI) modeli için, agricultürel işletmelerin veya müşteri sistem tümleştiricilerine göre oluşturmakta kullanılabilir.

Farmtörler şunları sağlar:

- /ExtendedType API 'Lerini kullanarak özel görüntü türlerini, kaynağı ve dosya biçimini tanımlayın.
- Çeşitli kaynaklardan alınan verileri/sahnenin ve/manzara eFile API 'Leri aracılığıyla alma.

Aşağıdaki bilgiler, Farmtts sistemine herhangi bir Imagery formu almaya odaklanır.

**Drone Imagery** bölümünü seçtiğinizde, drone diklik 'in yüksek çözünürlüklü görüntüsünü göstermek için bir açılır pencere açılır. İş ortağı yazılımına erişerek, drone fışıkları planlamaya ve ham veri almanıza yardımcı olur. Yol planlama ve ormozaik görüntü birleştirme için iş ortağının yazılımını kullanmaya devam edersiniz.

Drone iş ortaklarının, müşterilerin müşteri hesaplarını Azure 'daki Farmtts örneğiyle bağlantı kurmasını sağlama ihtiyacı vardır.

Farmtts 'yi bağlamak için drone iş ortağı yazılımında aşağıdaki kimlik bilgilerini kullanmanız gerekir:

- API uç noktası
- Kiracı Kimliği
- İstemci Kimliği
- Gizli anahtar

## <a name="api-development"></a>API Geliştirme

API 'Ler Swagger teknik belgelerini içerir. API 'Ler ve karşılık gelen istekler veya yanıtlar hakkında daha fazla bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

## <a name="authentication"></a>Kimlik Doğrulaması

Farmtts Microsoft Azure [Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) (Azure AD) kullanır. Azure App Service, yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar. 

Azure AD hakkında daha fazla bilgi için bkz. [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).   

Farmrets veri hub 'ı, aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:

- İstemci Kimliği
- Gizli anahtar
- Kiracı Kimliği

Çağıran, önceki kimlik bilgilerini kullanarak, üst bilgi bölümünde aşağıdaki gibi, sonraki API isteklerinde gönderilmesi gereken bir erişim belirteci isteyebilir:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Aşağıdaki Python kod örneği, erişim belirtecini alır. Daha sonra bu belirteci, Farmtts için sonraki API çağrıları için kullanabilirsiniz.

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

## <a name="http-request-headers"></a>HTTP istek üstbilgileri

Farmrets veri hub 'ına bir API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri aşağıda verilmiştir.

**Üst Bilgi** | **Açıklama ve örnek**
--- | ---
Content-Type  | İstek biçimi (Content-Type: Application/<format>). Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Content-Type: Application/JSON
Yetkilendirme | API çağrısı yapmak için gereken erişim belirtecini belirtir. Yetkilendirme: taşıyıcı < erişim-belirteç >
Kabul et  | Yanıt biçimi. Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Kabul et: uygulama/JSON


## <a name="api-requests"></a>API istekleri

REST API bir istek yapmak için şunu birleştirmelisiniz:

- HTTP yöntemi (GET, POST ve PUT).
- API hizmetinin URL 'SI.
- Kaynak URI 'SI (sorgulamak, veri göndermek, güncelleştirmek veya silmek için).
- Bir veya daha fazla HTTP istek üst bilgisi.

İsteğe bağlı olarak, filtrelemeye yönelik çağrıları al, boyut sınırını sınırla ve yanıtlarındaki verileri sıralamak için sorgu parametreleri ekleyebilirsiniz.

Aşağıdaki örnek istek, cihazların listesini almak için kullanılır:

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H
"Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>”
```

Çoğu GET, POST ve PUT çağrısı, bir JSON istek gövdesi gerektirir.

Aşağıdaki örnek istek bir cihaz oluşturmaktır. Bu örnekte, istek gövdesine sahip bir JSON girişi vardır.


```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  
"accept: application/json" -H  
"Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d
"{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  
\"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Veri biçimi

JSON, rastgele veri yapılarının basit bir metin gösterimini sağlayan, dilden bağımsız ortak bir veri biçimidir. Daha fazla bilgi için bkz. [JSON org](https://JSON.org).

## <a name="ingest-imagery-into-farmbeats"></a>Farmınts 'e Alım

İş ortağının Farmrets veri merkezine bağlanmak için kimlik bilgileri olduktan sonra, iş ortağı Translator bileşeninde aşağıdaki adımları gerçekleştirir.

1.  Karşıya yüklenecek olan Imagery türüne uygun olarak, aşağıdaki alanlar için yeni bir genişletilmiş tür oluşturun:

    - **Sahne kaynağı**: örneğin, drone_partner_name
    - **Sahne türü**: Örneğin, drone
    - **Sahne dosyası türü**: Örneğin, Chlorophyll dizini
    - **Sahne dosyası Içerik türü**: Örneğin, görüntü/TIFF

2.  Azure Farmtts sistemi içinden grupların listesini almak için/gruplar API 'sini çağırın.
3.  Müşteriyi, gruplar listesinden tek bir grup seçebilme olanağı sağlar.

    İş ortağı sisteminin yol planlama ve kurutma ve görüntü toplamayı yapması için ortak yazılım içindeki grubu göstermesi gerekir.

4.  /Sahnenin API 'sini çağırın ve benzersiz bir sahne KIMLIĞIYLE yeni bir sahne oluşturmak için gerekli ayrıntıları sağlayın.
5.  Gerekli görüntüleri, seçilen grup bağlamında Farmrets sisteminde bulunan bir blob SAS URL 'SI alın.

API çağrılarında ayrıntılı bir akış aşağıda verilmiştir.

### <a name="step-1-extendedtype"></a>1\. Adım: ExtendedType

Tür ve dosya kaynağının, Farmtts üzerinde kullanılabilir olup olmadığını görmek için/ExtendedType API 'sini iade edin. Bunu yapmak için/ExtendedType API 'sinde bir GET çağırın.

Sistem tarafından tanımlanan değerler aşağıda verilmiştir:

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

Bu adım tek seferlik bir kurulumdır. Bu yeni sahne türünün kapsamı, Azure Farmınts 'nin yüklendiği abonelikle sınırlıdır.

Örneğin, manzara: "SlantRange",/ExtendedType API 'sinin KIMLIĞINI "manzara \ kaynak" giriş yükü ile birlikte bir araya getirin.

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

Yeşil alan, sistem tarafından tanımlanan sahnenin kaynak değerlerinin yeni bir ektir.

### <a name="step-2-get-farm-details"></a>2\. Adım: Grup ayrıntılarını al

Sahneler (. tiff veya. csv dosyaları) bir grup bağlamındadır. /Farm API 'sinde bir GET yaparak Grup ayrıntılarını almanız gerekir. API, Farmtempts 'de kullanılabilir grupların listesini döndürür. Verilerini almak istediğiniz grubu seçebilirsiniz.

/Farm yanıtı al:

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

### <a name="step-3-create-a-scene-id-post-call"></a>3\. Adım: sahne KIMLIĞI oluşturma (çağrı Gönder)

Verilen bilgileri içeren yeni bir sahne (. tiff veya. csv dosyası) oluşturun ve sahnenin ilişkilendirildiği Tarih, sıra ve grup KIMLIĞINI sağlar. Sahneyle ilişkili meta veriler, ölçü süresi ve türü dahil olmak üzere Özellikler altında tanımlanabilir.

Yeni bir sahnenin oluşturulması, grupla ilişkilendirilmiş yeni bir sahne KIMLIĞI oluşturur. Sahne KIMLIĞI oluşturulduktan sonra, Kullanıcı yeni bir dosya (. tiff veya. csv) oluşturmak için aynısını kullanabilir ve dosyanın içeriğini saklayabilir.

/Sahnenin API 'sindeki POST çağrısının örnek giriş yükü:

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

Adım 3 ' te döndürülen sahne KIMLIĞI, sahne dosyasının giriştir. Sahne dosyası, 24 saat için geçerli olan bir SAS URL belirteci döndürür.

Kullanıcı bir görüntü akışını karşıya yüklemeyi programlı bir şekilde gerektiriyorsa, BLOB depolama SDK 'Sı sahne dosya KIMLIĞI, konum ve URL 'YI kullanarak bir yöntemi tanımlamak için kullanılabilir.

/Manzara eFile API 'sindeki POST çağrısının örnek giriş yükü:

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

/Manzara eFile API 'sine yönelik POST çağrısı, Azure Blob depolama istemcisi veya kitaplığı kullanılarak. csv veya. tiff dosyasını karşıya yüklemek için kullanılabilen bir SAS karşıya yükleme URL 'SI döndürür.


## <a name="next-steps"></a>Sonraki adımlar

REST API tabanlı tümleştirme ayrıntıları hakkında daha fazla bilgi için bkz. [REST API](rest-api-in-azure-farmbeats.md).
