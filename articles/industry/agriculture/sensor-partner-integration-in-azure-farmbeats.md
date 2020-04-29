---
title: Algılayıcı iş ortağı tümleştirmesi
description: Bu makalede algılayıcı iş ortağı tümleştirmesi açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 3431576acbb01a0cc3a5f372460b28be05bf7ce7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80437475"
---
# <a name="sensor-partner-integration"></a>Algılayıcı iş ortağı tümleştirmesi

Bu makalede, algılayıcı iş ortağı tümleştirmesini sağlayan Azure Farmtts **Translator** bileşeni hakkında bilgi sağlanır.

Bu bileşeni kullanarak, iş ortakları Farmrets veri hub 'ı API 'Leri kullanarak ve müşteri cihaz verilerini ve telemetri ' i veri hub 'ına gönderebilirler. Veriler Farmtörler 'de kullanılabilir olduktan sonra, Farmtts Hızlandırıcısı kullanılarak görselleştirilebilen ve veri Fusion ve makine öğrenimi/yapay zeka modelleri oluşturmak için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce

Çevirmen bileşenini geliştirmek için, Farmtts API 'Lerine erişimi etkinleştirecek aşağıdaki kimlik bilgilerine ihtiyacınız olacaktır.

- API Uç Noktası
- Kiracı Kimliği
- İstemci Kimliği
- İstemci Gizli Anahtarı
- EventHub bağlantı dizesi

Yukarıdaki kimlik bilgilerini almak için bu bölüme bakın: [cihaz Tümleştirmesini Etkinleştirme](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats)

## <a name="translator-development"></a>Çevirmen geliştirme

**REST API tabanlı tümleştirme**

Farmtts 'nin algılayıcı verileri tümleştirme özellikleri REST API aracılığıyla sunulur. Yetenekler, meta veri tanımı, cihaz ve algılayıcı sağlama, cihaz ve algılayıcı yönetimi içerir.

**Telemetri alımı**

Telemetri verileri, işlenmek üzere Azure Event Hubs yayımlanmış olan kurallı bir iletiyle eşleştirilir. Azure Event Hubs, bağlı cihazlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alımı sağlayan bir hizmettir.

**API Geliştirme**

API 'Ler Swagger teknik belgelerini içerir. API 'Ler ve bunlara karşılık gelen istekler ya da yanıtları hakkında daha fazla bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsSwagger).

**Kimlik Doğrulaması**

Farmtler Microsoft Azure Active Directory kimlik doğrulamasını kullanır.Azure App Service, yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).

Farmrets veri hub 'ı, aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:
   - İstemci Kimliği
   - Gizli anahtar
   - Kiracı Kimliği

Bu kimlik bilgilerini kullanarak, çağıran bir erişim belirteci isteyebilir. Belirtecin sonraki API isteklerinde gönderilmesi gerekir, üst bilgi bölümünde aşağıdaki gibi:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Aşağıdaki örnek Python kodu, Farmtts 'e yönelik sonraki API çağrıları için kullanılabilen erişim belirtecini verir.

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

Farmrets veri hub 'ına bir API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri aşağıda verilmiştir.


**Üst bilgi** | **Açıklama ve örnek**
--- | ---
İçerik Türü | İstek biçimi (Content-Type: Application/<format>). Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Content-Type: Application/JSON
Yetkilendirme | API çağrısı yapmak için gereken erişim belirtecini belirtir. Yetkilendirme: taşıyıcı <erişim-belirteç>
Kabul Et | Yanıt biçimi. Farmrets veri hub 'ı API 'Leri için, biçim JSON olur. Kabul et: uygulama/JSON

**API istekleri**

REST API bir istek yapmak için, HTTP (GET, POST veya PUT) yöntemini, API hizmetinin URL 'sini, bir kaynağa, veri göndermek, güncelleştirmek veya silmek için bir veya daha fazla HTTP istek üst bilgisi gönderebilirsiniz. API hizmetinin URL 'SI, sağladığınız API uç noktasıdır. Örnek: https://\<yourdatahub-website-Name>. azurewebsites.net

İsteğe bağlı olarak, filtrelemeye yönelik çağrıları al, boyut sınırını sınırla ve yanıtlarındaki verileri sıralamak için sorgu parametreleri ekleyebilirsiniz.

Aşağıdaki örnek istek, cihazların listesini almak için kullanılır.

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>"
```
Çoğu GET, POST ve PUT çağrısı, bir JSON istek gövdesi gerektirir.

Aşağıdaki örnek istek bir cihaz oluşturmaktır. (Bu örnek, istek gövdesine sahip bir JSON girişi içerir.)

```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Veri biçimi

JSON, rastgele veri yapılarının basit bir metin gösterimini sağlayan, dilden bağımsız ortak bir veri biçimidir. Daha fazla bilgi için bkz. [JSON.org](http://json.org).

## <a name="metadata-specifications"></a>Meta veri belirtimleri

Farmrets veri hub 'ı cihaz iş ortaklarının cihaz veya algılayıcı meta verilerini oluşturmalarına ve yönetmesine olanak tanıyan aşağıdaki API 'Lere sahiptir.

- /**Devicemodel**: devicemodel cihaz, ağ geçidi veya düğüm olan üretici ve cihaz türü gibi cihazın meta verilerine karşılık gelir.
- /**Cihaz**: cihaz, grupta bulunan bir fiziksel cihaza karşılık gelir.
- /**Sensormodel**: sensormodel: üretici, analog veya dijital olan algılayıcı türü ve çevresel sıcaklık ve basınç gibi algılayıcı ölçüsü gibi algılayıcı meta verilerine karşılık gelir.
- /**Algılayıcı**: algılayıcı, değerleri kaydeden bir fiziksel sensöre karşılık gelir. Bir algılayıcı genellikle cihaz KIMLIĞI olan bir cihaza bağlanır.

  **DeviceModel** |  |
  --- | ---
  Tür (düğüm, ağ geçidi)  | Cihaz düğümü veya ağ geçidi türü |
  Üretici  | Üreticinin adı |
  ProductCode  | Cihaz ürün kodu veya model adı veya numarası. Örneğin, EnviroMonitor # 6800. |
  Bağlantı noktaları  | Bağlantı noktası adı ve türü, dijital veya analog.  |
  Adı  | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı. |
  Açıklama  | Modelin anlamlı bir açıklamasını sağlayın. |
  Özellikler  | Üreticiden ek özellikler. |
  **Cihaz** |  |
  Devicemodelıd  |İlişkili cihaz modelinin KIMLIĞI. |
  Donanım kimliği   |Cihazın MAC adresi gibi benzersiz KIMLIĞI.  |
  Reportingınterval |Saniye cinsinden raporlama aralığı. |
  Konum    |Cihaz Latitude (-90 ile + 90), Boylam (-180-180) ve yükseltme (ölçü cinsinden). |
  Parentdeviceıd | Bu cihazın bağlı olduğu üst cihazın KIMLIĞI. Örneğin, bir düğüm bir ağ geçidine bağlıysa, düğümde ağ geçidi olarak Parentdeviceıd vardır. |
  Adı  | Kaynağı tanımlamak için ad. Cihaz iş ortaklarının cihaz adı ile tutarlı bir adı cihaz iş ortağı tarafında gönderebilmesi gerekir. Cihaz adı cihaz iş ortağı tarafında Kullanıcı tanımlı ise, aynı kullanıcı tanımlı ad, Farmınts 'e yayılmalıdır.  |
  Açıklama  | Anlamlı bir açıklama sağlayın.  |
  Özellikler  |Üreticiden ek özellikler.  |
  **SensorModel** |  |
  Tür (analog, dijital)  |Analog veya dijital sensörden bahsedin.|
  Üretici  | Üretici adı. |
  ProductCode  | Ürün kodu veya model adı veya numarası. Örneğin, RS-CO2-N01.  |
  Sensorölçüleri > adı  | Algılayıcı ölçüsünün adı. Yalnızca küçük harf desteklenir. Farklı derinlikleri olan ölçümler için derinliği belirtin. Örneğin, soil_moisture_15cm. Bu ad telemetri verileriyle tutarlı olmalıdır. |
  Sensorölçüleri > veri türü  | Telemetri veri türü. Şu anda, Double destekleniyor. |
  Sensorölçüleri > türü  | Algılayıcı Telemetri verilerinin ölçüm türü. Sistem tanımlı türler şunlardır: AmbientTemperature, CO2, Depth, Elektriicaliletkenctivity, Leafwetlik, length, LiquidLevel, Nitrate, O2, PH, Fospnefret, PointInTime, Potassium, basınç, RainGauge, Relativenem, Salinity, Soılmoisture, Soalıngıl, Soalınş Daha fazla eklemek için/ExtendedType API 'sine bakın.
  Sensorölçüleri > birimi | Algılayıcı telemetri verileri birimi. Sistem tarafından tanımlanan birimler şunlardır: NoUnit, santigrat, Fahrenhayvan, Kelvin, Rankine, Pascal, CIO, Ukrayna, mil ölçüm, Santimeter, ölçüm, Inç, fit, mil, kilometre, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, derece, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, Partspermilyon, MicroMol, mikro Molesperlitre, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Santibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, litre, MilliLiter, saniyeler, UnixTimestamp, MicroMolPerMeterSquaredPerSecond ve InchesPerHour. Daha fazla eklemek için/ExtendedType API 'sine bakın.
  Sensorölçüleri > AggregationType  | Hiçbiri, ortalama, maksimum, en az veya Standartsapması.
  Sensorölçüleri > derinliği  | Algılayıcının santimetre cinsinden derinliği. Örneğin, zemin altındaki nemi 10 cm ölçümü.
  Sensorölçüleri > açıklaması  | Ölçümün anlamlı bir açıklamasını sağlayın.
  Adı  | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.
  Açıklama  | Modelin anlamlı bir açıklamasını sağlayın.
  Özellikler  | Üreticiden ek özellikler.
  **Algılayıcısı**  |  |
  Donanım kimliği  | Üretici tarafından ayarlanan algılayıcı için benzersiz KIMLIK.
  Sensormodelıd  | İlişkili algılayıcı modelinin KIMLIĞI.
  Konum  | Enlem (-90 ile + 90), Boylam (-180-180) ve yükseltme (ölçü cinsinden).
  Bağlantı noktası > adı  |Algılayıcıdan cihazda bağlı olduğu bağlantı noktasının adı ve türü. Bu, cihaz modelinde tanımlananla aynı ada sahip olmalıdır.
  DeviceId  | Algılayıcıın bağlı olduğu cihazın KIMLIĞI.
  Adı  | Kaynağı tanımlamak için ad. Örneğin, algılayıcı adı veya ürün adı ve model numarası ya da ürün kodu.
  Açıklama  | Anlamlı bir açıklama sağlayın.
  Özellikler  | Üreticiden ek özellikler.

 Nesnelerin ve özelliklerinin her biri hakkında bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

 > [!NOTE]
 > API 'Ler oluşturulan her örnek için benzersiz kimlikler döndürür. Bu KIMLIğIN cihaz yönetimi ve meta veri eşitleme çevirmeni tarafından korunması gerekir.


**Meta veri eşitleme**

Çevirmen, güncelleştirmeleri meta veriler üzerinde göndermelidir. Örneğin, güncelleştirme senaryolarında cihaz veya algılayıcı adı ve cihaz ya da algılayıcı konumu değişikliği bulunur.

Çevirmen, Kullanıcı tarafından Farmby bağlantısı sonrası tarafından yüklenen yeni cihaz veya sensörler ekleyebilmelidir. Benzer şekilde, bir cihaz veya algılayıcı Kullanıcı tarafından güncellendiyse, buna karşılık gelen cihaz veya algılayıcı için aynı şekilde de aynı şekilde güncelleştirilmeleri gerekir. Bir cihazın veya algılayıcının güncelleştirilmesi gereken tipik senaryolar, bir cihaz konumunda veya bir düğümdeki sensörların eklenmesiyle bir değişiklik niteliğindedir.


> [!NOTE]
> Cihaz veya algılayıcı meta verileri için silme desteklenmiyor.
>
> Meta verileri güncelleştirmek için, cihaz veya algılayıcı üzerinde/Get/{id} çağrısı yapmanız, değiştirilen özellikleri güncelleştirmeniz ve ardından bir/put/{ID} oluşturarak Kullanıcı tarafından ayarlanan tüm özellikler kaybedilmez.

### <a name="add-new-types-and-units"></a>Yeni türler ve birimler Ekle

Farmtts, yeni algılayıcı ölçü türleri ve birimleri eklemeyi destekler. /ExtendedType API 'SI hakkında daha fazla bilgi için bkz. [Swagger](https://aka.ms/FarmBeatsSwagger).

## <a name="telemetry-specifications"></a>Telemetri belirtimleri

Telemetri verileri, işlenmek üzere Azure Event Hubs yayımlanmış olan kurallı bir iletiyle eşleştirilir. Azure Event Hubs, bağlı cihazlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alımı sağlayan bir hizmettir.

## <a name="send-telemetry-data-to-farmbeats"></a>Telemetri verilerini Farmolar 'a gönderme

Telemetriye telemetri verileri göndermek için, Farmtts 'deki bir olay hub 'ına ileti gönderen bir istemci oluşturun. Telemetri verileri hakkında daha fazla bilgi için bkz. [bir olay hub 'ına telemetri gönderme](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

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
Telemetri JSON 'daki tüm anahtar adları küçük harfle yazılmalıdır. Örnek olarak DeviceID ve sensordata verilebilir.

Örneğin, aşağıda bir telemetri iletisi verilmiştir:


```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version" : "1",
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

> [!NOTE]
> Aşağıdaki bölümler diğer değişikliklerle ilgilidir (örn. Kullanıcı arabirimi, hata yönetimi vb.) algılayıcı iş ortağının çevirici bileşenini geliştirme bölümünde başvurabileceği.


## <a name="link-a-farmbeats-account"></a>Bir Farmtts hesabını bağlama

Müşteriler cihazları veya sensörleri satın aldıktan ve dağıttıktan sonra cihaz verilerine ve telemetri ' e hizmet olarak yazılım (SaaS) Portalı ' na erişebilir. Cihaz iş ortakları, müşterilerin hesabını Azure 'daki Farmtts örneğine, aşağıdaki kimlik bilgilerini girişi için bir yol sağlayarak bağlantı kurmasını sağlayabilir:

   - Görünen ad (kullanıcıların bu tümleştirme için bir ad tanımlayabilmeniz için isteğe bağlı bir alan)
   - API uç noktası
   - Kiracı Kimliği
   - İstemci Kimliği
   - Gizli anahtar
   - EventHub bağlantı dizesi
   - Başlangıç tarihi

   > [!NOTE]
   > Başlangıç tarihi geçmiş veri akışını, diğer bir deyişle Kullanıcı tarafından belirtilen tarihten itibaren verileri sunar.

## <a name="unlink-farmbeats"></a>Farmtörler bağlantısını kaldır

Cihaz iş ortakları, müşterilerin mevcut bir Farmtts tümleştirmesinin bağlantısını kaldırmak için izin verebilir. Farmrekelerin bağlantısını kaldırmak, Farmtts veri hub 'ında oluşturulan herhangi bir cihaz veya algılayıcı meta verilerini silmemelidir. Bağlantıyı kaldırma şunları yapar:

   - Telemetri akışını durduruyor.
   - Cihaz ortağındaki tümleştirme kimlik bilgilerini siler ve siler.

## <a name="edit-farmbeats-integration"></a>Farmtts tümleştirmesini Düzenle

İstemci parolası veya bağlantı dizesi değişirse, cihaz iş ortakları müşterilerin Farmtts tümleştirme ayarlarını düzenlemesini sağlayabilir. Bu durumda, yalnızca aşağıdaki alanlar düzenlenebilir:

   - Görünen ad (varsa)
   - İstemci parolası ("2x8 * * * * * * * * * * *" biçiminde veya şifresiz metin yerine göster/gizle özelliğinde görüntülenmelidir)
   - Bağlantı dizesi ("2x8 * * * * * * * * * * *" biçiminde veya şifresiz metin yerine gösterme/gizleme özelliğinde görüntülenmelidir)

## <a name="view-the-last-telemetry-sent"></a>Gönderilen son Telemetriyi görüntüleme

Cihaz iş ortakları, müşterilerin gönderilen **telemetri**altında bulunan son Telemetriyi görüntülemesini sağlayabilir. Bu, en son telemetrinin Farmtts 'ye başarıyla gönderildiği süredir.

## <a name="troubleshooting-and-error-management"></a>Sorun giderme ve hata yönetimi

**Sorun giderme seçeneği veya destek**

Müşteri, belirtilen Farmtts örneğinde cihaz verilerini veya telemetri alamadıysanız, cihaz ortağı sorun giderme için destek ve bir mekanizma sağlamalıdır.

**Telemetri verilerini bekletme**

Telemetri verileri de önceden tanımlanmış bir süre için tutulmalıdır, böylece bir hata veya veri kaybı oluşursa Telemetriyi yeniden gönderebilirsiniz.

**Hata yönetimi veya hata bildirimi**

Bir hata cihaz veya algılayıcı meta verilerini veya cihaz iş ortağı sistemindeki veri tümleştirmesini veya telemetri veri akışını etkiliyorsa, müşteri bir bildirim almalıdır. Hataları çözmeye yönelik bir mekanizma Ayrıca tasarlanmalı ve uygulanmalıdır.

**Bağlantı denetim listesi**

Cihaz üreticileri veya iş ortakları, müşteri tarafından sunulan kimlik bilgilerinin doğru olduğundan emin olmak için aşağıdaki denetim listesini kullanabilir:

   - Girilen kimlik bilgileriyle bir erişim belirtecinin alınıp alınmadığını denetleyin.
   - Bir API çağrısının alınan erişim belirteciyle başarılı olup olmadığını denetleyin.
   - EventHub istemci bağlantısının oluşturulup yüklenmediğini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

REST API hakkında daha fazla bilgi için bkz. [REST API](rest-api-in-azure-farmbeats.md).
