---
title: Azure Notebooks kullanarak elektrik araç yönlendirmesi (Python) | Microsoft Docs
description: Azure haritalar yönlendirme API 'Leri ve Azure Notebooks kullanarak EV yönlendirme.
author: walsehgal
ms.author: v-musehg
ms.date: 10/01/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: c4b46bc952782fc7c9b56d6f0c049fe17b63d0f2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71836439"
---
# <a name="electric-vehicle-routing-using-azure-notebooks-python"></a>Azure Notebooks kullanarak elektrik araç yönlendirmesi (Python)

Azure haritalar, geliştiricilerin, kuruluşların ve ISV 'lerin konuma duyarlı uygulamalar ve IoT, Mobility, lojistik ve varlık izleme çözümleri oluşturmasına olanak sağlayan, Azure ile yerel olarak tümleştirilmiş Jeo-uzamsal hizmet API 'Lerinin bir portföyüdür. Azure haritalar REST API 'Leri, Jeo-uzamsal veri analizi ve makine öğrenimi senaryolarını etkinleştirmek için Python ve R gibi dillerden çağrılabilir. Azure Maps, kullanıcıların araç türü veya ulaşılabilir alan gibi çeşitli koşullara göre çeşitli veri noktaları arasındaki yolları hesaplamasını sağlayan sağlam bir [yönlendirme API](https://docs.microsoft.com/rest/api/maps/route) 'si kümesi sunar. Bu öğreticide, aracı pil ücreti düşük olan elektrik araç sürücüsüne yardımcı olmak için bir senaryoya kılavuzluk edecek ve sürücü süresine göre olası en iyi doldurma istasyonunu bulacağız.

Bu öğreticide şunları yapmanız gerekir:

> [!div class="checklist"]
> * Bulutta [Azure Notebooks](https://docs.microsoft.com/azure/notebooks) Jupyter Notebook oluşturma ve çalıştırma
> * Python 'da Azure haritalar REST API 'Lerini çağırma
> * Elektrik araç tüketiminin tüketim modeline göre erişilebilir bir Aralık arayın.
> * Erişilebilir Aralık (veya ısovaone) içinde elektrik araç doldurma istasyonlarını arayın.
> * Bir haritada erişilebilir Aralık sınırı ve ücretlendirme istasyonlarını işleme.
> * Zamana göre en yakın elektrik araç doldurma istasyonuna yönelik rotayı bulun ve görselleştirin.


## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticideki adımları tamamlayabilmeniz için öncelikle bir Azure Maps hesabı oluşturmanız ve birincil anahtarınızı (abonelik anahtarı) almanız gerekir. S1 Fiyatlandırma Katmanı içeren bir Azure Maps hesabı aboneliği oluşturmak için [Hesabı Yönet](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account) bölümündeki yönergeleri izleyin ve hesabınız için birincil abonelik anahtarını almak üzere [birincil anahtar al](./tutorial-search-location.md#getkey) bölümündeki adımları izleyin.

## <a name="create-an-azure-notebook"></a>Azure Not defteri oluşturma

Bu öğreticiyle birlikte takip edebilmek için bir Azure Not defteri projesi oluşturmanız ve Jupyter Not defteri dosyasını indirmeniz ve çalıştırmanız gerekecektir. Not defteri dosyası, bu öğreticide senaryoyu uygulayan Python kodunu içerir. Aşağıdaki adımları izleyerek bir Azure Not defteri projesi oluşturun ve Jupyter Not Defteri belgesini buna yükleyin.

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. Daha fazla bilgi için bkz. [hızlı başlangıç](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks).
2. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin.

    ![projem](./media/tutorial-ev-routing/myproject.png)

3. **Projelerim** sayfasında **Yeni proje**' yi seçin.
 
   ![Yeni proje](./media/tutorial-ev-routing/create-project.png)

4. Görüntülenen **Yeni proje oluştur** açılır penceresinde, aşağıdaki bilgileri girin ve **Oluştur**' a tıklayın:
    * Proje Adı
    * Proje Kimliği
 
    ![Proje oluştur](./media/tutorial-ev-routing/create-project-window.png)

5. Projeniz oluşturulduktan sonra, [Azure Maps Jupyter Notebook deposundan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) [Jupyter Not defteri belge dosyasını](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/EVrouting.ipynb) indirin. 

6. **Projelerim** sayfasındaki projeler listesinden projenizi seçin ve Jupyter Not defteri belge dosyasını karşıya yüklemek Için **karşıya yükle** ' ye tıklayın. Dosyayı bilgisayarınızdan karşıya yükleyin ve **bitti**' ye tıklayın.

    ![Not defterini karşıya yükle](./media/tutorial-ev-routing/upload-notebook.png)

7. Başarılı bir karşıya yükleme işlemi sonrasında dosyanızı proje sayfanızda görürsünüz. Not Defteri dosyasına tıklayarak Jupyter Notebook olarak açın.

Not Defteri dosyasında uygulanan işlevselliği daha iyi anlamak için, kodu Not defterinde bir hücrede tek seferde çalıştırmanızı öneririz. Not defteri uygulamasının en üstündeki **Çalıştır** düğmesine tıklayarak her hücrede kodu çalıştırabilirsiniz.

  ![Çalışmaz](./media/tutorial-ev-routing/run.png)

## <a name="install-project-level-packages"></a>Proje düzeyi paketleri 'ni yükler

Kodu Not defterinde çalıştırmak için, proje düzeyinde paketler kurmanız gerekir. Gerekli paketleri yüklemek için aşağıdaki adımları izleyin:

1. [Azure haritalar Jupyter Notebook deposundan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) ["Requirements. txt"](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/requirements.txt) dosyasını indirin ve projenize yükleyin.
2. Proje panosunda **proje ayarları**' nı seçin. 
3. Görüntülenen açılan pencerede **ortam sekmesini**seçin ve ardından **Ekle**' yi seçin.
4. **Ortam kurulum adımları**altında, 
    * İlk açılan denetimde, **requirements. txt**' yi seçin.
    * İkinci açılan denetimde "Requirements. txt" dosyanızı seçin.
    * Üçüncü açılan denetimde Python sürümü olarak Python sürüm 3,6 ' i seçin.
7. **Kaydet**’i seçin.

    ![Paketleri yükler](./media/tutorial-ev-routing/install-packages.png)

## <a name="load-required-modules-and-frameworks"></a>Gerekli modülleri ve çerçeveleri yükleme

Gerekli tüm modülleri ve çerçeveleri yüklemek için aşağıdaki betiği çalıştırın.

```python
import time
import aiohttp
import urllib.parse
from IPython.display import Image, display
```

## <a name="request-for-reachable-range-boundary"></a>Erişilebilir Aralık sınırı isteği

Senaryolarımızda, bir paket teslimi şirketinin, Fleet 'de bazı elektrik paketleri vardır. Gün boyunca, elektrik taşıtlarının ambara geri dönmek zorunda kalmadan yeniden ücretlendirilmelidir. Elektrik Aracı için kalan geçerli ücret bir saatten daha az olduğunda (elektrik aracı düşük ücretlendirilir), erişilebilir aralıktaki bir dizi doldurma İstasyonu aradık ve bu aralığa ait sınır bilgilerini alır. Şirket ekonomisi ve hız tarafından dengelenen rotalar kullanmayı tercih ettiğinden, istenen routeType ' ekonomik ' olur. Aşağıdaki betik, Azure Maps yönlendirme hizmeti 'nin [yönlendirme aralığı API 'sini](https://docs.microsoft.com/rest/api/maps/route/getrouterange) , aracın tüketim modelinin parametreleriyle çağırır ve arabasının en fazla erişilebilir aralığını temsil eden geojson biçiminin Çokgen bir nesnesini oluşturma yanıtını ayrıştırır .

Elektrikün erişilebilir aralığının sınırlarını almak için betiği aşağıdaki hücrede çalıştırın.

```python
subscriptionKey = "Your Azure Maps primary subscription key"
currentLocation = [34.028115,-118.5184279]
session = aiohttp.ClientSession()

# Parameters for the vehicle consumption model 
travelMode = "car"
vehicleEngineType = "electric"
currentChargeInkWh=45
maxChargeInkWh=80
timeBudgetInSec=550
routeType="eco"
constantSpeedConsumptionInkWhPerHundredkm="50,8.2:130,21.3"


# Get bounds for the electric vehicle's reachable range.
routeRangeResponse = await (await session.get("https://atlas.microsoft.com/route/range/json?subscription-key={}&api-version=1.0&query={}&travelMode={}&vehicleEngineType={}&currentChargeInkWh={}&maxChargeInkWh={}&timeBudgetInSec={}&routeType={}&constantSpeedConsumptionInkWhPerHundredkm={}"
                                              .format(subscriptionKey,str(currentLocation[0])+","+str(currentLocation[1]),travelMode, vehicleEngineType, currentChargeInkWh, maxChargeInkWh, timeBudgetInSec, routeType, constantSpeedConsumptionInkWhPerHundredkm))).json()

polyBounds = routeRangeResponse["reachableRange"]["boundary"]

for i in range(len(polyBounds)):
    coordList = list(polyBounds[i].values())
    coordList[0], coordList[1] = coordList[1], coordList[0]
    polyBounds[i] = coordList

polyBounds.pop()
polyBounds.append(polyBounds[0])

boundsData = {
               "geometry": {
                 "type": "Polygon",
                 "coordinates": 
                   [
                      polyBounds
                   ]
                }
             }
```

## <a name="search-electric-vehicle-charging-stations-within-reachable-range"></a>Erişilebilir Aralık içinde elektrik araç doldurma istasyonlarında ara

Elektrik Aracı için erişilebilir aralığa (ısovaone) sahip olduktan sonra bu aralıktaki ücretlendirme istasyonlarını arayacağız. Aşağıdaki betik, oto 'nın en fazla erişilebilir aralığının sınırları içindeki elektrik araç doldurma [istasyonlarını aramak ve](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) ardından bir erişilebilir konumlar dizisine Yanıt ayrıştırır.

Erişilebilir Aralık içinde elektrik araç doldurma istasyonlarını aramak için aşağıdaki betiği çalıştırın.

```python
# Search for EV stations within reachable range.
searchPolyResponse = await (await session.post(url = "https://atlas.microsoft.com/search/geometry/json?subscription-key={}&api-version=1.0&query=electric vehicle station&idxSet=POI&limit=50".format(subscriptionKey), json = boundsData)).json() 

reachableLocations = []
for loc in range(len(searchPolyResponse["results"])):
                location = list(searchPolyResponse["results"][loc]["position"].values())
                location[0], location[1] = location[1], location[0]
                reachableLocations.append(location)
```

## <a name="upload-reachable-range-and-charging-points-to-azure-maps-data-service"></a>Azure Maps veri hizmeti 'ne erişilebilir Aralık ve ücretlendirme noktalarını yükleme

Haritadaki maksimum erişilebilir Aralık aralığının miktarını ve sınırlarını görselleştirmek için, [veri yükleme API 'sini kullanarak sınır verilerini ve istasyon verilerini Azure Maps veri hizmeti 'ne coğrafi JSON nesneleri olarak şarj etmemiz gerekir ](https://docs.microsoft.com/rest/api/maps/data/uploadpreview). 

Sınır ve ücretlendirme noktası verilerini Azure Maps veri hizmetine yüklemek için aşağıdaki iki hücreyi çalıştırın.

```python
rangeData = {
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          polyBounds
        ]
      }
    }
  ]
}

# Upload range data to Azure Maps data service.
uploadRangeResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = rangeData)

rangeUdidRequest = uploadRangeResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getRangeUdid = await (await session.get(rangeUdidRequest)).json()
    if 'udid' in getRangeUdid:
        break
    else:
        time.sleep(0.2)
rangeUdid = getRangeUdid["udid"]
```

```python
poiData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "properties": {},
        "geometry": {
            "type": "MultiPoint",
            "coordinates": reachableLocations
        }
    }
  ]
}

# Upload EV charging stations data to Azure Maps data service.
uploadPOIsResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = poiData)

poiUdidRequest = uploadPOIsResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getPoiUdid = await (await session.get(poiUdidRequest)).json()
    if 'udid' in getPoiUdid:
        break
    else:
        time.sleep(0.2)
poiUdid = getPoiUdid["udid"]
```

## <a name="render-charging-stations-and-reachable-range-on-map"></a>İşleme istasyonlarını ve eşleme üzerinde erişilebilir aralığı işleme

Veriler veri hizmetine yüklendikten sonra, bir sonraki komut dosyasını çalıştırarak, sabit eşleme görüntüsünde doldurma noktalarını ve en fazla erişilebilir sınırı işlemeye yönelik [harita görüntü hizmeti al](https://docs.microsoft.com/rest/api/maps/render/getmapimage) ' ı çağırın.

```python
# Get bounds for bounding box.
def getBounds(polyBounds):
    maxLon = max(map(lambda x: x[0], polyBounds))
    minLon = min(map(lambda x: x[0], polyBounds))

    maxLat = max(map(lambda x: x[1], polyBounds))
    minLat = min(map(lambda x: x[1], polyBounds))
    
    # Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
    lonBuffer = (maxLon-minLon)*0.1
    minLon -= lonBuffer
    maxLon += lonBuffer

    latBuffer = (maxLat-minLat)*0.1
    minLat -= latBuffer
    maxLat += latBuffer
    
    return [minLon, maxLon, minLat, maxLat]

minLon, maxLon, minLat, maxLat = getBounds(polyBounds)

path = "lcff3333|lw3|la0.80|fa0.35||udid-{}".format(rangeUdid)
pins = "custom|an15 53||udid-{}||https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/images/icons/ev_pin.png".format(poiUdid)

encodedPins = urllib.parse.quote(pins, safe='')

# Render range and EV charging points on the map.
staticMapResponse =  await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&pins={}&path={}&bbox={}&zoom=12".format(subscriptionKey,encodedPins,path,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

poiRangeMap = await staticMapResponse.content.read()

display(Image(poiRangeMap))
```

![Konum aralığı](./media/tutorial-ev-routing/location-range.png)


## <a name="find-the-optimal-charging-station-to-stop"></a>Durdurulacak en iyi doldurma istasyonunu bulun

Erişilebilir Aralık dahilinde tüm olası ücretlendirme istasyonlarınızı aldıktan sonra, istasyonlardan birine en az sürede ulaşılmayı bildirmek istiyoruz. Aşağıdaki betik, verilen araç konumu için döndürülen Azure haritalar [matris YÖNLENDIRME API](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) 'sini, verilen her bir doldurma İstasyonu konumuna seyahat süresi ve mesafesini çağırır. Sonraki hücrede betiği, en yakın erişilebilir doldurma İstasyonu için zamana göre konum almaya yönelik yanıtı ayrıştırır.

En az sürede ulaşılabilen en yakın erişilebilir doldurma istasyonunu bulmak için aşağıdaki hücreyi çalıştırın.

```python
locationData = {
            "origins": {
              "type": "MultiPoint",
              "coordinates": [[currentLocation[1],currentLocation[0]]]
            },
            "destinations": {
              "type": "MultiPoint",
              "coordinates": reachableLocations
            }
         }

# Get the travel time and distance to every given charging station location.
searchPolyRes = await (await session.post(url = "https://atlas.microsoft.com/route/matrix/json?subscription-key={}&api-version=1.0&routeType=shortest&waitForResults=true".format(subscriptionKey), json = locationData)).json()

distances = []
for dist in range(len(reachableLocations)):
    distances.append(searchPolyRes["matrix"][0][dist]["response"]["routeSummary"]["travelTimeInSeconds"])

minDistLoc = []
minDistIndex = distances.index(min(distances))
minDistLoc.extend([reachableLocations[minDistIndex][1], reachableLocations[minDistIndex][0]])
closestChargeLoc = ",".join(str(i) for i in minDistLoc)
```

## <a name="calculate-route-to-the-closest-charging-station"></a>En yakın ücretlendirme istasyonuna olan rotayı hesapla

En yakın ücretlendirme istasyonunu buldığımıza göre, daha sonra elektrik aracı 'nın geçerli konumundan ücretlendirme istasyonuna ayrıntılı rota istemek için [yol yönleri al API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) 'sini çağıracağız.

Yolu almak için aşağıdaki hücrede betiği çalıştırın ve rotayı temsil eden bir geojson nesnesi oluşturmak için yanıtı ayrıştırın.

```python
# Get route from current location to the closest charging station. 
routeResponse = await (await session.get("https://atlas.microsoft.com/route/directions/json?subscription-key={}&api-version=1.0&query={}:{}".format(subscriptionKey, str(currentLocation[0])+","+str(currentLocation[1]), closestChargeLoc))).json()

route = []
for loc in range(len(routeResponse["routes"][0]["legs"][0]["points"])):
                location = list(routeResponse["routes"][0]["legs"][0]["points"][loc].values())
                location[0], location[1] = location[1], location[0]
                route.append(location)

routeData = {
         "type": "LineString",
         "coordinates": route
     }
```

## <a name="visualize-the-route"></a>Rotayı görselleştirin

Rotayı görselleştirmek için ilk olarak Azure haritalar [veri yükleme API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)'Sini kullanarak Azure Maps veri hizmeti 'ne yol verilerini geojson nesnesi olarak karşıya yükleyeceğiz. Ardından Işleme hizmeti ' ni çağırın, Haritayı haritada işlemek ve görselleştirmek için [harita görüntüsü API 'Sini alın](https://docs.microsoft.com/rest/api/maps/render/getmapimage) .

Haritada işlenen yolun bir görüntüsünü almak için aşağıdaki betiği çalıştırın.

```python
# Upload route data to Azure data service.
routeUploadRequest = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = routeData)

udidRequestURI = routeUploadRequest.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    udidRequest = await (await session.get(udidRequestURI)).json()
    if 'udid' in udidRequest:
        break
    else:
        time.sleep(0.2)

udid = udidRequest["udid"]

destination = route[-1]

destination[1], destination[0] = destination[0], destination[1]

path = "lc0f6dd9|lw6||udid-{}".format(udid)
pins = "default|codb1818||{} {}|{} {}".format(str(currentLocation[1]),str(currentLocation[0]),destination[1],destination[0])


# Get bounds for bounding box.
minLat, maxLat = (float(destination[0]),currentLocation[0]) if float(destination[0])<currentLocation[0] else (currentLocation[0], float(destination[0]))
minLon, maxLon = (float(destination[1]),currentLocation[1]) if float(destination[1])<currentLocation[1] else (currentLocation[1], float(destination[1]))

#Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
lonBuffer = (maxLon-minLon)*0.1
minLon -= lonBuffer
maxLon += lonBuffer

latBuffer = (maxLat-minLat)*0.1
minLat -= latBuffer
maxLat += latBuffer

# Render route on the map.
staticMapResponse = await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&&path={}&pins={}&bbox={}&zoom=16".format(subscriptionKey,path,pins,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

staticMapImage = await staticMapResponse.content.read()

await session.close()
display(Image(staticMapImage))
```

![yol](./media/tutorial-ev-routing/route.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure haritalar REST API 'Lerinin doğrudan nasıl çağrılacağını ve Python kullanarak Azure Maps verilerini görselleştirmeyi öğrendiniz.

Bu öğreticide kullanılan Azure Haritalar API 'Lerini araştırmak için, bkz.:

* [Rota aralığını al](https://docs.microsoft.com/rest/api/maps/route/getrouterange)
* [Geometri Içinde arama sonrası](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)
* [Karşıya veri yükleme](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)
* [Render-harita görüntüsünü al](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Rota sonrası matrisi](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview)
* [Rota yönlerini al](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)

Azure haritalar REST API 'lerinin tüm listesi için bkz.:

* [Azure haritalar REST API 'Leri](https://docs.microsoft.com/azure/azure-maps/#reference)

Azure Notebooks hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Notebooks](https://docs.microsoft.com/azure/notebooks)