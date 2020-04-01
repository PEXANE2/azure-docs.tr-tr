---
title: Algılayıcı iş ortağı tümleştirmesi
description: Bu makalede, sensör ortağı entegrasyonu açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 3431576acbb01a0cc3a5f372460b28be05bf7ce7
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437475"
---
# <a name="sensor-partner-integration"></a>Algılayıcı iş ortağı tümleştirmesi

Bu makalede, sensör ortağı tümleştirmesini sağlayan Azure FarmBeats **Translator** bileşeni hakkında bilgi verilmektedir.

Bu bileşeni kullanarak, iş ortakları FarmBeats Datahub API'lerini kullanarak FarmBeats ile tümleşebilir ve farmbeats Datahub'a müşteri cihazı verileri ve telemetri gönderebilir. Veriler FarmBeats'te kullanılabilir hale geldikten sonra FarmBeats Hızlandırıcı kullanılarak görselleştirilmiştir ve veri füzyonu ve makine öğrenimi/yapay zeka modelleri oluşturmak için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce

Çevirmen bileşenini geliştirmek için FarmBeats API'lerine erişimi sağlayacak aşağıdaki kimlik bilgilerine ihtiyacınız olacaktır.

- API Uç Noktası
- Kiracı Kimliği
- İstemci Kimliği
- İstemci Gizli Anahtarı
- EventHub Bağlantı Dizesi

Yukarıdaki kimlik bilgilerini almak için şu bölüme bakın: [Aygıt Tümleştirmesini Etkinleştir](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats)

## <a name="translator-development"></a>Çevirmen geliştirme

**REST API tabanlı tümleştirme**

FarmBeats'in sensör veri tümleştirme yetenekleri REST API ile açığa alınır. Yetenekler arasında meta veri tanımı, cihaz ve sensör sağlama, aygıt ve sensör yönetimi yer alır.

**Telemetri yenmesi**

Telemetri verileri, işlenmek üzere Azure Etkinlik Hub'larında yayınlanan bir kanonik iletiyle eşlenir. Azure Etkinlik Hub'ları, bağlı aygıtlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alınmasını sağlayan bir hizmettir.

**API geliştirme**

API'ler Swagger teknik dokümantasyon içerir. API'ler ve ilgili istekler veya yanıtları hakkında daha fazla bilgi [için](https://aka.ms/FarmBeatsSwagger)Bkz.

**Kimlik Doğrulaması**

FarmBeats, Microsoft Azure Active Directory kimlik doğrulamasını kullanır.Azure Uygulama Hizmeti yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar.

Daha fazla bilgi için [Azure Etkin Dizini'ne](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization)bakın.

FarmBeats Datahub, aşağıdaki kimlik bilgilerini gerektiren taşıyıcı kimlik doğrulamasını kullanır:
   - İstemci Kimliği
   - Gizli anahtar
   - Kiracı Kimliği

Arayan bu kimlik bilgilerini kullanarak bir erişim belirteci isteyebilir. Belirteç aşağıdaki gibi, üstbilgi bölümünde, sonraki API isteklerinde gönderilmesi gerekir:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Aşağıdaki örnek Python kodu FarmBeats sonraki API aramaları için kullanılabilecek erişim belirteci verir.

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

FarmBeats Datahub'a API çağrısı yaptığınızda belirtilmesi gereken en yaygın istek üstbilgileri aşağıda veda eder.


**Üst bilgi** | **Açıklama ve örnek**
--- | ---
İçerik Türü | İstek biçimi (İçerik Türü:<format>uygulama/ ). FarmBeats Datahub API'leri için biçim JSON'dur. İçerik Türü: uygulama/json
Yetkilendirme | API araması yapmak için gereken erişim belirteci belirtir. Yetkilendirme: Taşıyıcı <Erişim-Belirteç>
Kabul Et | Yanıt biçimi. FarmBeats Datahub API'leri için biçim JSON'dur. Kabul Et: uygulama/json

**API istekleri**

REST API isteği nde bulunmak için HTTP (GET, POST veya PUT) yöntemini, API hizmetinin URL'sini, Tek düzen Kaynak Tanımlayıcısını (URI) sorgulanacak bir kaynağa, verileri göndermek, güncelleştirmek veya silmek ve bir veya daha fazla HTTP istek üstbilgisini birleştirirsiniz. API hizmetinin URL'si, sağladığınız API bitiş noktasıdır. İşte bir örnek:\<https:// yourdatahub-web sitesi-adı>.azurewebsites.net

İsteğe bağlı olarak, yanıtlarda verileri filtrelemek, boyutunu sınırlamak ve sıralamak için GET çağrılarına sorgu parametrelerini ekleyebilirsiniz.

Aşağıdaki örnek istek, aygıtların listesini almaktır.

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>"
```
Çoğu GET, POST ve PUT aramaları bir JSON istek gövdesi gerektirir.

Aşağıdaki örnek istek bir aygıt oluşturmaktır. (Bu örnek istek gövdesi ile bir giriş JSON vardır.)

```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Veri biçimi

JSON, rasgele veri yapılarının basit bir metin gösterimi sağlayan ortak bir dilden bağımsız veri biçimidir. Daha fazla bilgi için [json.org.](http://json.org)

## <a name="metadata-specifications"></a>Meta veri özellikleri

FarmBeats Datahub, aygıt ortaklarının aygıt veya sensör meta verilerini oluşturmasını ve yönetmesini sağlayan aşağıdaki API'lere sahiptir.

- /**DeviceModel : DeviceModel,** ağ geçidi veya düğüm olan üretici ve aygıt türü gibi aygıtın meta verilerine karşılık gelir.
- /**Cihaz**: Cihaz çiftlikte bulunan fiziksel bir cihaza karşılık gelir.
- /**SensorModel : SensorModel,** üretici, analog veya dijital sensör tipi ve ortam sıcaklığı ve basınç gibi sensör ölçüsü gibi sensörün meta verilerine karşılık gelir.
- /**Sensör**: Sensör, değerleri kaydeden fiziksel bir sensöre karşılık gelir. Sensör genellikle aygıt kimliğine sahip bir aygıta bağlanır.

  **DeviceModel** |  |
  --- | ---
  Türü (düğüm, ağ geçidi)  | Cihazın türü - Düğüm veya Ağ Geçidi |
  Üretici  | Üreticinin adı |
  Productcode  | Cihaz ürün kodu veya model adı veya numarası. Örneğin, EnviroMonitor#6800. |
  Bağlantı Noktaları  | Dijital veya analog olan bağlantı noktası adı ve türü.  |
  Adı  | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı. |
  Açıklama  | Modelin anlamlı bir açıklamasını sağlayın. |
  Özellikler  | Üreticiden ek özellikler. |
  **Cihaz** |  |
  DeviceModelId  |İlişkili aygıt modelinin kimliği. |
  Hırdavat   |Mac adresi gibi aygıt için benzersiz kimlik.  |
  RaporlamaInterval |Saniye cinsinden raporlama aralığı. |
  Konum    |Cihaz enlemi (-90 ila +90), boylam (-180-180) ve yükseklik (metre olarak). |
  ParentDeviceId | Bu aygıtın bağlı olduğu ana aygıtın kimliği. Örneğin, bir düğüm bir ağ geçidine bağlıysa, düğüm ağ geçidi olarak parentDeviceID'ye sahiptir. |
  Adı  | Kaynağı tanımlamak için ad. Aygıt ortaklarının aygıt ortağı tarafındaki aygıt adıyla tutarlı bir ad göndermesi gerekir. Aygıt adı aygıt ortağı tarafında kullanıcı tarafından tanımlanmışsa, aynı kullanıcı tanımlı ad FarmBeats'e yayılmalıdır.  |
  Açıklama  | Anlamlı bir açıklama sağlayın.  |
  Özellikler  |Üreticiden ek özellikler.  |
  **Sensör Modeli** |  |
  Türü (analog, dijital)  |Analog veya dijital sensörden bahsedin.|
  Üretici  | Üreticinin adı. |
  Productcode  | Ürün kodu veya model adı veya numarası. Örneğin, RS-CO2-N01.  |
  SensorMeasures> Adı  | Sensör ölçüsünün adı. Yalnızca küçük harf desteklenir. Farklı derinliklerden ölçümler için derinliği belirtin. Örneğin, soil_moisture_15cm. Bu ad telemetri verileriyle tutarlı olmalıdır. |
  SensorMeasures > DataType  | Telemetri veri türü. Şu anda, çift desteklenir. |
  SensorMeasures > Tipi  | Sensör telemetri verilerinin ölçüm türü. Sistem tanımlı türleri şunlardır: AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Fosfat, PointInTime, Potasyum, Basınç, RainGauge, RelativeMoisturey, Tuzluluk, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun WindSpeed, Evapotranspiration, PARPAR. Daha fazla eklemek için /ExtendedType API'sine bakın.
  SensorMeasures > Ünitesi | Sensör telemetri veri birimi. Sistem tanımlı birimler şunlardır: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, Milimetre, Santimetre, Metre, Inç, Feet, Mile, KilometreMetre, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Derece, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetrikWaterContent, Yüzde, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMetreMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricionContent, MiliLitre, Saniye, UnixTimestamp, MicroMolPerMeterSquareDPerSecond, ve InçPerHour. Daha fazla eklemek için /ExtendedType API'sine bakın.
  SensorMeasures> Agregasyon Tipi  | Ya yok, ortalama, maksimum, minimum veya StandardDeviation.
  SensorMeasures> Derinlik  | Sensörün derinliği santimetre. Örneğin, nem ölçümü 10 cm zemin altında.
  SensorMeasures > Açıklama  | Ölçümün anlamlı bir açıklamasını sağlayın.
  Adı  | Kaynağı tanımlamak için ad. Örneğin, model adı veya ürün adı.
  Açıklama  | Modelin anlamlı bir açıklamasını sağlayın.
  Özellikler  | Üreticiden ek özellikler.
  **Sensör**  |  |
  Hırdavat  | Üretici tarafından ayarlanan sensör için benzersiz kimlik.
  SensorModelId  | İlişkili sensör modelinin kimliği.
  Konum  | Sensör enlemi (-90 ila +90), boylam (-180-180) ve yükseklik (metre olarak).
  Port > Adı  |Sensörün cihazda bağlı olduğu bağlantı noktasının adı ve türü. Bu, aygıt modelinde tanımlanan adla aynı olmalıdır.
  DeviceId  | Sensörün bağlı olduğu cihazın kimliği.
  Adı  | Kaynağı tanımlamak için ad. Örneğin, sensör adı veya ürün adı ve model numarası veya ürün kodu.
  Açıklama  | Anlamlı bir açıklama sağlayın.
  Özellikler  | Üreticiden ek özellikler.

 Nesnelerin her biri ve özellikleri hakkında bilgi [için](https://aka.ms/FarmBeatsDatahubSwagger)Bkz.

 > [!NOTE]
 > API'ler oluşturulan her örnek için benzersiz adlar döndürer. Bu kimliğin aygıt yönetimi ve meta veri eşitlemesi için Çevirmen tarafından saklanması gerekir.


**Meta veri eşitlemi**

Çevirmen meta verilerle ilgili güncelleştirmeler göndermelidir. Örneğin, güncelleştirme senaryoları aygıt veya sensör adı ve aygıt veya sensör konumu değişikliğidir.

Çevirmen FarmBeats kullanıcı sonrası bağlantı tarafından yüklenen yeni cihazlar veya sensörler eklemek için yeteneğine sahip olmalıdır. Benzer şekilde, bir aygıt veya sensör kullanıcı tarafından güncelleştirilmişse, aynı durum ilgili aygıt veya sensör için FarmBeats'te de güncellenmelidir. Aygıt veya sensörün güncelleştirilmesini gerektiren tipik senaryolar, aygıt konumundaki bir değişiklik veya düğümdeki sensörlerin eklenmesidir.


> [!NOTE]
> Silme, aygıt veya sensör meta verileri için desteklenmez.
>
> Meta verileri güncelleştirmek için cihazveya sensörden /Get/{id} aramak, değiştirilen özellikleri güncelleştirmek ve kullanıcı tarafından ayarlanan özelliklerin kaybolmaması için bir /Put/{id} yapmak zorunludur.

### <a name="add-new-types-and-units"></a>Yeni türler ve birimler ekleme

FarmBeats yeni sensör ölçüm türleri ve birimleri eklemeyi destekler. /ExtendedType API hakkında daha fazla bilgi için [Bkz.](https://aka.ms/FarmBeatsSwagger)

## <a name="telemetry-specifications"></a>Telemetri özellikleri

Telemetri verileri, işlenmek üzere Azure Etkinlik Hub'larında yayınlanan bir kanonik iletiyle eşlenir. Azure Etkinlik Hub'ları, bağlı aygıtlardan ve uygulamalardan gerçek zamanlı veri (telemetri) alınmasını sağlayan bir hizmettir.

## <a name="send-telemetry-data-to-farmbeats"></a>FarmBeats'e telemetri verileri gönder

FarmBeats'e telemetri verileri göndermek için FarmBeats'teki bir etkinlik merkezine ileti gönderen bir istemci oluşturun. Telemetri verileri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send)

Burada, belirli bir olay hub'ına istemci olarak telemetri gönderen örnek bir Python kodu vermiştir.

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

Kanonik ileti biçimi aşağıdaki gibidir:

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
Telemetri JSON tüm anahtar isimleri küçük olmalıdır. Örnekler deviceid ve sensordata vardır.

Örneğin, bir telemetri iletisi aşağıda verilmiştir:


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
> Aşağıdaki bölümler diğer değişikliklerle ilgilidir (örneğin. UI, hata yönetimi vb.) sensör ortağının Çevirmen bileşeninin geliştirilmesinde başvurabileceği anlamına gelebilir.


## <a name="link-a-farmbeats-account"></a>FarmBeats hesabını bağla

Müşteriler cihaz veya sensör satın aldıktan ve dağıttıktan sonra, aygıt ortaklarının yazılımlarına hizmet (SaaS) portalı olarak erişebilirler. Aygıt ortakları, aşağıdaki kimlik bilgilerini girdi etmenin bir yolunu sağlayarak müşterilerin hesaplarını Azure'daki FarmBeats örneğine bağlamalarını sağlayabilir:

   - Görüntü adı (kullanıcıların bu tümleştirme için bir ad tanımlaması için isteğe bağlı bir alan)
   - API uç noktası
   - Kiracı Kimliği
   - İstemci Kimliği
   - Gizli anahtar
   - EventHub bağlantı dizesi
   - Başlangıç tarihi

   > [!NOTE]
   > Başlangıç tarihi, geçmiş veri akışını, yani kullanıcı tarafından belirtilen tarihten gelen verileri sağlar.

## <a name="unlink-farmbeats"></a>Unlink FarmBeats

Aygıt ortakları, müşterilerin varolan farmbeats tümleştirmebağlantısını niçin ayrıştırmasını sağlayabilir. FarmBeats bağlantısı nın bağlantısı, FarmBeats Datahub'da oluşturulan herhangi bir aygıt veya sensör meta verilerini silmemelidir. Unlinking aşağıdakileri yapar:

   - Telemetri akışını durdurur.
   - Aygıt ortağındaki tümleştirme kimlik bilgilerini siler ve siler.

## <a name="edit-farmbeats-integration"></a>FarmBeats entegrasyonunu edin

Aygıt ortakları, istemci gizli veya bağlantı dizesi değişirse müşterilerin FarmBeats tümleştirme ayarlarını değiştirmesini sağlayabilir. Bu durumda, yalnızca aşağıdaki alanlar değiştirilebilir:

   - Görüntü adı (varsa)
   - İstemci sırrı (açık metin yerine "2x8************* biçiminde veya Göster/Gizle özelliğinde görüntülenmelidir)
   - Bağlantı dizesi (açık metin yerine "2x8************* biçiminde" biçiminde görüntülenmelidir veya Göster/Gizle özelliği)

## <a name="view-the-last-telemetry-sent"></a>Gönderilen son telemetriyi görüntüle

Cihaz ortakları, müşterilerin gönderilen son telemetrinin zaman damgasını görüntülemesini sağlayabilir, bu da **Telemetri Gönderildi**altında bulunur. Bu, en son telemetrinin FarmBeats'e başarıyla gönderildiği zamandır.

## <a name="troubleshooting-and-error-management"></a>Sorun giderme ve hata yönetimi

**Sorun giderme seçeneği veya desteği**

Müşteri belirtilen FarmBeats örneğinde aygıt verisi veya telemetri alamıyorsa, aygıt ortağı destek ve sorun giderme mekanizması sağlamalıdır.

**Telemetri veri saklama**

Bir hata veya veri kaybı oluşursa telemetri hata ayıklama veya yeniden gönderme yararlı olabilir, böylece telemetri verileri de önceden tanımlanmış bir süre için tutulmalıdır.

**Hata yönetimi veya hata bildirimi**

Aygıt veya sensör meta verilerini veya aygıt ortağı sistemindeki veri tümleştirmeveya telemetri veri akışını bir hata etkiliyorsa, müşteri bir bildirim almalıdır. Hataları gidermek için bir mekanizma da tasarlanıp uygulanmalıdır.

**Bağlantı kontrol listesi**

Aygıt üreticileri veya iş ortakları, müşteri tarafından sağlanan kimlik bilgilerinin doğru olduğundan emin olmak için aşağıdaki denetim listesini kullanabilir:

   - Sağlanan kimlik bilgileriyle birlikte bir erişim belirteci alınıp alınmadığını denetleyin.
   - Alınan erişim belirteciyle bir API çağrısının başarılı olup olmadığını denetleyin.
   - EventHub istemci bağlantısının kurulup kurulmadığını denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

REST API hakkında daha fazla bilgi için [REST API'ye](rest-api-in-azure-farmbeats.md)bakın.
