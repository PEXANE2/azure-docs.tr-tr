---
title: Kaba Relocalization
description: Size yakın çapabulmak için Kaba yeniden yerelleştirme kullanma hakkında bilgi edinin.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/18/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 4c1604eaad1ebdedf6a360a647fe5b9f95c829c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844403"
---
# <a name="coarse-relocalization"></a>Kaba yeniden yerelleştirme

Kaba relocalization soruya ilk cevap sağlayan bir *özelliktir: Nerede benim cihaz şimdi / Hangi içeriği gözlemleme olmalıdır?* Yanıt kesin değildir, ancak bunun yerine şeklindedir: *Bu çapalara yakınsınız; bunlardan birini bulmaya çalışın.*

Kaba yeniden yerelleştirme, çeşitli cihaz algılayıcı okumalarını hem bağlantı noktası oluşturma hem de sorgulama ile ilişkilendirerek çalışır. Dış ortam senaryoları için sensör verileri genellikle cihazın GPS (Küresel Konumlandırma Sistemi) konumudur. GPS kullanılamadığında veya (iç mekanlar gibi) algılayıcı verileri WiFi erişim noktalarından ve aralıktaki Bluetooth işaretlerinden oluşur. Toplanan tüm sensör verileri, Azure Uzamsal Çapaları tarafından cihazınızın yaklaşık 100 metre içindeki çapalarını hızlı bir şekilde belirlemek için kullanılan uzamsal dizinin korunmasına katkıda bulunur.

Kaba yeniden yerelleştirme nin sağladığı çapaların hızlı bir şekilde görünmesi, dünya ölçeğinde milyonlarca çapa (örneğin, milyonlarca coğrafi dağıtılmış) çapa koleksiyonu yla desteklenen uygulamaların geliştirilmesini kolaylaştırır. Çapa yönetiminin karmaşıklığı, harika uygulama mantığınıza daha fazla odaklanmanızı sağlayarak gizlenmiştir. Tüm çapa ağır kaldırma azure uzamsal çapalar tarafından sahne arkasında sizin için yapılır.

## <a name="collected-sensor-data"></a>Toplanan sensör verileri

Bağlantı hizmetine gönderebileceğiniz sensör verileri aşağıdakilerden biridir:

* GPS konumu: enlem, boylam, yükseklik.
* WiFi erişim noktalarının sinyal gücü aralıkta.
* Bluetooth işaretlerinin menzildeki sinyal gücü.

Genel olarak, uygulamanızın GPS, WiFi veya BLE verilerine erişmek için cihaza özel izinler alması gerekir. Ayrıca, yukarıdaki sensör verilerinin bazıları belirli platformlarda tasarım tarafından kullanılamaz. Bu durumları hesaba katmak için, sensör verilerinin toplanması isteğe bağlıdır ve varsayılan olarak kapatılır.

## <a name="set-up-the-sensor-data-collection"></a>Sensör veri toplamayı ayarlama

Bir sensör parmak izi sağlayıcısı oluşturarak ve oturumun bundan haberdar olmasını sağlayarak başlayalım:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
// Create the sensor fingerprint provider
sensorProvider = new PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = new CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.LocationProvider = sensorProvider;
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
// Create the sensor fingerprint provider
ASAPlatformLocationProvider *sensorProvider;
sensorProvider = [[ASAPlatformLocationProvider alloc] init];

// Create and configure the session
cloudSpatialAnchorSession = [[ASACloudSpatialAnchorSession alloc] init];

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.locationProvider = sensorProvider;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
// Create the sensor fingerprint provider
var sensorProvider: ASAPlatformLocationProvider?
sensorProvider = ASAPlatformLocationProvider()

// Create and configure the session
cloudSpatialAnchorSession = ASACloudSpatialAnchorSession()

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession!.locationProvider = sensorProvider
```

# <a name="java"></a>[Java](#tab/java)

```java
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = new PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = new CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.setLocationProvider(sensorProvider);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
// Create the sensor fingerprint provider
std::shared_ptr<PlatformLocationProvider> sensorProvider;
sensorProvider = std::make_shared<PlatformLocationProvider>();

// Create and configure the session
cloudSpatialAnchorSession = std::make_shared<CloudSpatialAnchorSession>();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession->LocationProvider(sensorProvider);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)
```cpp
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.LocationProvider(sensorProvider);
```
---

Ardından, kaba yeniden yerelleştirme için hangi sensörleri kullanmak istediğinize karar vermeniz gerekir. Bu karar, geliştirmekte olduğunuz uygulamaya özgüdür, ancak aşağıdaki tablodaki öneriler size iyi bir başlangıç noktası vermelidir:


|             | Kapalı | Açık havada |
|-------------|---------|----------|
| Gps         | Kapalı | Açık |
| Wifi        | Açık | A)'ye göre |
| BLE işaretleri | On (uyarılar ile isteğe bağlı, aşağıya bakın) | Kapalı |


### <a name="enabling-gps"></a>GPS'i etkinleştirme

Uygulamanızın aygıtın GPS konumuna erişmek için zaten izni olduğunu varsayarsak, Azure Uzamsal Bağlantı Larını kullanmak üzere yapılandırabilirsiniz:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
sensorProvider.Sensors.GeoLocationEnabled = true;
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.geoLocationEnabled = true;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.geoLocationEnabled = true
```

# <a name="java"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setGeoLocationEnabled(true);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->GeoLocationEnabled(true);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.GeoLocationEnabled(true);
```

---

Uygulamanızda GPS kullanırken, donanım tarafından sağlanan okumaların genellikle şunlardır:

* asenkron ve düşük frekans (1 Hz'den az).
* güvenilmez / gürültülü (ortalama 7-m standart sapma).

Genel olarak, hem aygıt işletim sistemi hem de Azure Uzamsal Çapaları, bu sorunları azaltmak amacıyla ham GPS sinyaliüzerinde bazı filtreleme ve ekstrapolasyon lar yapar. Bu ekstra işleme yakınsama için ek zaman gerektirir, bu nedenle en iyi sonuçlar için denemelisiniz:

* uygulamanızda mümkün olduğunca erken bir sensör parmak izi sağlayıcısı oluşturun
* birden fazla oturum arasında sensör parmak izi sağlayıcısını canlı tutmak
* sensör parmak izi sağlayıcısını birden çok oturum arasında paylaşma

Sensör parmak izi sağlayıcısını bir bağlantı oturumu dışında kullanmayı planlıyorsanız, sensör tahminleri istemeden önce başlatdığınızdan emin olun. Örneğin, aşağıdaki kod cihazınızın haritadaki konumunu gerçek zamanlı olarak güncelleştirmeyi sağlar:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
// Game about to start, start tracking the sensors
sensorProvider.Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.GetLocationEstimate();

    // Paint it on the map
    drawCircle(
        x: geoPose.Longitude,
        y: geoPose.Latitude,
        radius: geoPose.HorizontalError);
}

// Game ended, no need to track the sensors anymore
sensorProvider.Stop();
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
// Game about to start, start tracking the sensors
[sensorProvider start];

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    ASAGeoLocation *geoPose = [sensorProvider getLocationEstimate];

    // Paint it on the map
    drawCircle(geoPose.longitude, geoPose.latitude, geoPose.horizontalError);
}

// Game ended, no need to track the sensors anymore
[sensorProvider stop];
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
// Game about to start, start tracking the sensors
sensorProvider?.start()

// Game loop
while m_isRunning
{
    // Get the GPS estimate
    var geoPose: ASAGeoLocation?
    geoPose = sensorProvider?.getLocationEstimate()

    // Paint it on the map
    drawCircle(geoPose.longitude, geoPose.latitude, geoPose.horizontalError)
}

// Game ended, no need to track the sensors anymore
sensorProvider?.stop()
```

# <a name="java"></a>[Java](#tab/java)

```java
// Game about to start, start tracking the sensors
sensorProvider.start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.getLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose.getLongitude(), geoPose.getLatitude(), geoPose.getHorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider.stop();
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
// Game about to start, start tracking the sensors
sensorProvider->Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    std::shared_ptr<GeoLocation> geoPose = sensorProvider->GetLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose->Longitude(), geoPose->Latitude(), geoPose->HorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider->Stop();
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
// Game about to start, start tracking the sensors
sensorProvider.Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.GetLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose.Longitude(), geoPose.Latitude(), geoPose.HorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider.Stop();
```

---

### <a name="enabling-wifi"></a>WiFi'ı etkinleştirme

Uygulamanızın aygıtın Wi-Fi durumuna erişmek için zaten izni olduğunu varsayarsak, Azure Uzamsal Bağlantı Larını kullanmak üzere yapılandırabilirsiniz:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
sensorProvider.Sensors.WifiEnabled = true;
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.wifiEnabled = true;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.wifiEnabled = true
```

# <a name="java"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setWifiEnabled(true);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->WifiEnabled(true);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.WifiEnabled(true);
```

---

Uygulamanızda WiFi kullanırken, donanım tarafından sağlanan okumaların genellikle şunlardır:

* asenkron ve düşük frekans (0.1 Hz'den az).
* potansiyel olarak işletim sistemi düzeyinde daraltılır.
* güvenilmez / gürültülü (ortalama 3-dBm standart sapma).

Azure Uzamsal Çapalar, bu sorunları azaltmak amacıyla oturum sırasında filtreuygulanmış bir Wi-Fi sinyal gücü eşlemi oluşturmaya çalışır. En iyi sonuçlar için şunları denemelisiniz:

* ilk çapa yerleştirmeden önce oturumu iyi oluşturun.
* oturumu mümkün olduğunca uzun süre canlı tutun (diğer bir deyişle, tüm çapaları ve sorguyu tek bir oturumda oluşturun).

### <a name="enabling-bluetooth-beacons"></a>Bluetooth işaretçilerini etkinleştirme

Uygulamanızın aygıtın Bluetooth durumuna erişmek için zaten izni olduğunu varsayarsak, Azure Uzamsal Bağlantı Larını kullanmak üzere yapılandırabilirsiniz:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
sensorProvider.Sensors.BluetoothEnabled = true;
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.bluetoothEnabled = true;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.bluetoothEnabled = true
```

# <a name="java"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setBluetoothEnabled(true);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->BluetoothEnabled(true);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors();
sensors.BluetoothEnabled(true);
```

---

İşaretçiler, UUID'ler ve MAC adresleri de dahil olmak üzere her şeyin yapılandırılabildiği çok yönlü aygıtlardır. Bu esneklik, işaretçilerin UUID'leri tarafından benzersiz bir şekilde tanımlanacağını düşündüğü için Azure Uzamsal Çapaları için sorunlu olabilir. Bu eşsizliği sağlayamamak büyük olasılıkla uzaysal solucan deliklerine neden olacaktır. En iyi sonuçlar için şunları göstermelisiniz:

* işaretçilerinize benzersiz UUID'ler atayın.
* genellikle ızgara gibi normal bir desende dağıtın.
* benzersiz işaret UUIDs listesini sensör parmak izi sağlayıcısına iletin:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
sensorProvider.Sensors.KnownBeaconProximityUuids = new[]
{
    "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1",
    "a63819b9-8b7b-436d-88ec-ea5d8db2acb0",
    . . .
};
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
NSArray *uuids = @[@"22e38f1a-c1b3-452b-b5ce-fdb0f39535c1", @"a63819b9-8b7b-436d-88ec-ea5d8db2acb0"];

ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.knownBeaconProximityUuids = uuids;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let uuids = [String]()
uuids.append("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1")
uuids.append("a63819b9-8b7b-436d-88ec-ea5d8db2acb0")

let sensors = locationProvider?.sensors
sensors.knownBeaconProximityUuids = uuids
```

# <a name="java"></a>[Java](#tab/java)

```java
String uuids[] = new String[2];
uuids[0] = "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1";
uuids[1] = "a63819b9-8b7b-436d-88ec-ea5d8db2acb0";

SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setKnownBeaconProximityUuids(uuids);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
std::vector<std::string> uuids;
uuids.push_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.push_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->KnownBeaconProximityUuids(uuids);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
std::vector<winrt::hstring> uuids;
uuids.emplace_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.emplace_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

SensorCapabilities sensors = sensorProvider.Sensors();
sensors.KnownBeaconProximityUuids(uuids);
```

---

Azure Uzamsal Bağlantı çapaları yalnızca bilinen işaret yakını UUID'ler listesinde bulunan Bluetooth işaretçilerini izler. İzin veren UUID'lere sahip olmak üzere programlanmış kötü amaçlı işaretçiler yine de hizmetkalitesini olumsuz etkileyebilir. Bu nedenle, işaretçileri yalnızca dağıtımlarını kontrol edebilirsiniz küratörlük alanlarda kullanmalısınız.

## <a name="querying-with-sensor-data"></a>Sensör verileriyle sorgulama

İlişkili sensör verileriyle bağlantı oluşturduktan sonra, cihazınız tarafından bildirilen sensör okumalarını kullanarak bunları almaya başlayabilirsiniz. Artık hizmeti bulmayı beklediğiniz bilinen çapaların bir listesini sağlamanız gerekmez , bunun yerine servise cihazınızın yerleşik sensörleri tarafından bildirilen konumunu bildirmeniz gerekir. Azure Uzamsal Çapalar daha sonra cihazınıza yakın olan çapa kümesini çözer ve bunları görsel olarak eşleştirmeye çalışır.

Sorguların sensör verilerini kullanmasını sağlamak için "cihaza yakın" ölçütleri oluşturarak başlayın:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.DistanceInMeters = 5;

// Cap the number of anchors returned
nearDeviceCriteria.MaxResultCount = 25;

anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.NearDevice = nearDeviceCriteria;
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
ASANearDeviceCriteria *nearDeviceCriteria = [[ASANearDeviceCriteria alloc] init];

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.distanceInMeters = 5.0f;

// Cap the number of anchors returned
nearDeviceCriteria.maxResultCount = 25;

ASAAnchorLocateCriteria *anchorLocateCriteria = [[ASAAnchorLocateCriteria alloc] init];
anchorLocateCriteria.nearDevice = nearDeviceCriteria;
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let nearDeviceCriteria = ASANearDeviceCriteria()!

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.distanceInMeters = 5.0

// Cap the number of anchors returned
nearDeviceCriteria.maxResultCount = 25

let anchorLocateCriteria = ASAAnchorLocateCriteria()!
anchorLocateCriteria.nearDevice = nearDeviceCriteria
```

# <a name="java"></a>[Java](#tab/java)

```java
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.setDistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria.setMaxResultCount(25);

AnchorLocateCriteria anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.setNearDevice(nearDeviceCriteria);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto nearDeviceCriteria = std::make_shared<NearDeviceCriteria>();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria->DistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria->MaxResultCount(25);

auto anchorLocateCriteria = std::make_shared<AnchorLocateCriteria>();
anchorLocateCriteria->NearDevice(nearDeviceCriteria);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
NearDeviceCriteria nearDeviceCriteria = NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.DistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria.MaxResultCount(25);

// Set the session's locate criteria
anchorLocateCriteria = AnchorLocateCriteria();
anchorLocateCriteria.NearDevice(nearDeviceCriteria);
```

---

Parametre, `DistanceInMeters` içeriği almak için bağlantı grafiğini ne kadar araştırabileceğimizi denetler. Örneğin, her metrede sabit yoğunlukta çapalarla bir miktar boşluk doldurmuş olduğunuzu varsayalım. Ayrıca, cihazınızdaki kamera tek bir çapayı gözlemliyor ve hizmet bunu başarıyla tespit etti. Büyük olasılıkla, şu anda gözlemlediğiniz tek çapa yerine yakına yerleştirdiğiniz tüm çapaları almakla ilgileniyorsunuz. Yerleştirdiğiniz çapaların bir grafiğe bağlı olduğunu varsayarsak, hizmet grafikteki kenarları izleyerek yakındaki tüm çapaları sizin için alabilir. Yapılan grafik traversi miktarı; `DistanceInMeters` bulunduğunuz çapaya bağlı tüm çapalar size `DistanceInMeters`verilecektir.

Büyük değerlerin performansı olumsuz `MaxResultCount` etkileyebileceğini unutmayın. Uygulamanız için mantıklı bir değere ayarlayın.

Son olarak, sensör tabanlı görünümü kullanmak için oturuma şunları söylemeniz gerekir:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
[cloudSpatialAnchorSession createWatcher:anchorLocateCriteria];
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
cloudSpatialAnchorSession!.createWatcher(anchorLocateCriteria)
```

# <a name="java"></a>[Java](#tab/java)

```java
cloudSpatialAnchorSession.createWatcher(anchorLocateCriteria);
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
cloudSpatialAnchorSession->CreateWatcher(anchorLocateCriteria);
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

---

## <a name="expected-results"></a>Beklenen sonuçlar

Tüketici sınıfı GPS cihazları genellikle oldukça kesin değildir. [Zandenbergen ve Barbeau tarafından bir çalışma (2011)][6] destekli GPS ile cep telefonlarının ortanca doğruluğu raporları (A-GPS) etrafında 7 metre - göz ardı edilecek oldukça büyük bir değer! Bu ölçüm hatalarını hesaba katmak için, hizmet çapaları GPS alanında olasılık dağılımları olarak ele verir. Bu nedenle, bir çapa şimdi uzayın en olası (yani% 95'ten fazla güven ile) gerçek, bilinmeyen GPS konumunu içeren bölgedir.

GPS ile sorgulanırken de aynı mantık uygulanır. Cihaz, gerçek, bilinmeyen GPS konumu etrafında başka bir mekansal güven bölgesi olarak temsil edilir. Yakındaki çapaları keşfetmek, aşağıdaki resimde gösterildiği gibi, cihazın güven bölgesine *yeterince yakın* güven bölgeleri olan çapaları bulmak anlamına gelir:

![GPS ile çapa adaylarının seçimi](media/coarse-reloc-gps-separation-distance.png)

GPS sinyalinin doğruluğu, hem çapa oluşturma hem de sorgular sırasında, döndürülen çapa kümesi üzerinde büyük bir etkiye sahiptir. Buna karşılık, WiFi / işaretçileri dayalı sorgular sorgu ile ortak en az bir erişim noktası / işaretçi tüm çapa dikkate alacaktır. Bu anlamda, WiFi / işaretleri dayalı bir sorgu sonucu çoğunlukla erişim noktaları / işaretleri fiziksel aralığı ve çevresel engeller tarafından belirlenir.

Aşağıdaki tabloda her sensör türü için beklenen arama alanı tahmin edilmektedir:

| Sensör      | Arama alanı yarıçapı (yaklaşık) | Ayrıntılar |
|-------------|:-------:|---------|
| Gps         | 20 m - 30 m | Diğer faktörler arasında GPS belirsizlik tarafından belirlenir. Bildirilen numaraları A-GPS ile cep telefonlarının medyan GPS doğruluğu için tahmin edilmektedir, yani 7 metre. |
| Wifi        | 50 m - 100 m | Kablosuz erişim noktalarının aralığına göre belirlenir. Frekansına, verici gücüne, fiziksel engellere, parazite ve benzeri ne rekata bağlı. |
| BLE işaretleri |  70 m | İşaretin menziline göre belirlenir. Frekans, iletim gücü, fiziksel engeller, girişim, ve benzeri bağlıdır. |

## <a name="per-platform-support"></a>Platform başına destek

Aşağıdaki tablo, desteklenen platformların her birinde toplanan sensör verilerini ve platforma özgü uyarıları özetler:


|             | HoloLens | Android | iOS |
|-------------|----------|---------|-----|
| Gps         | Yok | [LocationManager][3] API'leri (hem GPS hem de NETWORK) aracılığıyla desteklenir | [CLLocationManager][4] API'leri ile desteklenir |
| Wifi        | Her 3 saniyede bir yaklaşık bir tazyik hızında desteklenir | Destekleniyor. API düzeyi 28 ile başlayarak, WiFi taramaları her 2 dakikada 4 arama ile daraltılır. Android 10'dan itibaren, azaltma Geliştirici ayarları menüsünden devre dışı bilebilir. Daha fazla bilgi için [Android belgelerine][5]bakın. | N/A - genel API yok |
| BLE işaretleri | [Eddystone][1] ve [iBeacon][2] ile sınırlıdır | [Eddystone][1] ve [iBeacon][2] ile sınırlıdır | [Eddystone][1] ve [iBeacon][2] ile sınırlıdır |

## <a name="next-steps"></a>Sonraki adımlar

Bir uygulamada kaba yeniden yerelleştirme kullanın.

> [!div class="nextstepaction"]
> [Unity](../how-tos/set-up-coarse-reloc-unity.md)

> [!div class="nextstepaction"]
> [Objective-C](../how-tos/set-up-coarse-reloc-objc.md)

> [!div class="nextstepaction"]
> [Swift](../how-tos/set-up-coarse-reloc-swift.md)

> [!div class="nextstepaction"]
> [Java](../how-tos/set-up-coarse-reloc-java.md)

> [!div class="nextstepaction"]
> [C++/NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md)

> [!div class="nextstepaction"]
> [C++/WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
