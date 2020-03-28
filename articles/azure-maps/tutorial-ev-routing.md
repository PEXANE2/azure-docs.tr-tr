---
title: 'Öğretici: Azure Dizüstü Bilgisayarlar (Python) kullanarak elektrikli araçları yönlendirin | Microsoft Azure Haritaları'
description: Microsoft Azure Haritalar yönlendirme API'lerini ve Azure Not Defterlerini kullanarak elektrikli araçları yönlendirin.
author: philmea
ms.author: philmea
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 3118ca39ec0efd42c9f7b622c91f857034ef4b03
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333827"
---
# <a name="tutorial-route-electric-vehicles-by-using-azure-notebooks-python"></a>Öğretici: Azure Dizüstü Bilgisayarlar (Python) kullanarak elektrikli araçları yönlendirin

Azure Haritalar, azure'a yerel olarak entegre edilmiş bir jeouzamsal hizmet API'leri portföyüdür. Bu API'ler, geliştiricilerin, işletmelerin ve ISV'lerin konum bilincine sahip uygulamalar, IoT, mobilite, lojistik ve varlık izleme çözümleri geliştirmesini sağlar. 

Azure Haritalar REST API'leri, coğrafi veri çözümlemesini ve makine öğrenimi senaryolarını etkinleştirmek için Python ve R gibi dillerden çağrılabilir. Azure Haritalar, kullanıcıların birkaç veri noktası arasındaki yolları hesaplamasına olanak tanıyan sağlam bir [yönlendirme API'leri](https://docs.microsoft.com/rest/api/maps/route) kümesi sunar. Hesaplamalar, araç tipi veya ulaşılabilir alan gibi çeşitli koşullara dayanmaktadır. 

Bu eğitimde, elektrikli araç aküsü düşük olan bir sürücüye yardım edin. Sürücünün aracın bulunduğu yerden mümkün olan en yakın şarj istasyonunu bulması gerekiyor.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Bulutta [Azure Not Defterlerinde](https://docs.microsoft.com/azure/notebooks) bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın.
> * Python'da Azure Haritalar REST API'lerini arayın.
> * Elektrikli aracın tüketim modeline göre ulaşılabilir bir aralık arayın.
> * Ulaşılabilen aralıkta elektrikli araç şarj istasyonları veya izochrone arayın.
> * Ulaşılabilir menzil sınırını ve şarj istasyonlarını harita üzerinde işle.
> * Sürücü süresine bağlı olarak en yakın elektrikli araç şarj istasyonuna giden bir rotayı bulun ve görselleştirin.


## <a name="prerequisites"></a>Ön koşullar 

Bu öğreticiyi tamamlamak için öncelikle bir Azure Haritalar hesabı oluşturmanız ve birincil anahtarınızı (abonelik anahtarı) almanız gerekir. 

Azure Haritalar hesap aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps)yönergeleri izleyin. S1 fiyat katmanına sahip bir Azure Haritalar hesap aboneliğine ihtiyacınız vardır. 

Hesabınız için birincil abonelik anahtarını almak için [birincil anahtarı al'daki](quick-demo-map-app.md#get-the-primary-key-for-your-account)yönergeleri izleyin.

Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

## <a name="create-an-azure-notebook"></a>Azure dizüstü bilgisayar oluşturma

Bu öğreticiyi takip etmek için bir Azure dizüstü proje oluşturmanız ve Jupyter not defteri dosyasını karşıdan yüklemeniz ve çalıştırmanız gerekir. Not defteri dosyası, bu öğreticide senaryoyu uygulayan Python kodunu içerir. Bir Azure dizüstü bilgisayar projesi oluşturmak ve Jupyter not defteri belgesini ona yüklemek için aşağıdaki adımları yapın:

1. Azure [Not Defterleri'ne](https://notebooks.azure.com) gidin ve oturum açın. Daha fazla bilgi için [Quickstart: Oturum açın ve bir kullanıcı kimliği ayarlayın.](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks)
1. Herkese açık profil sayfanızın üst kısmında **Projelerim'i**seçin.

    ![Projelerim düğmesi](./media/tutorial-ev-routing/myproject.png)

1. **Projelerim** sayfasında Yeni **Proje'yi**seçin.
 
   ![Yeni Proje düğmesi](./media/tutorial-ev-routing/create-project.png)

1. Yeni **Proje Oluştur** bölmesine bir proje adı ve proje kimliği girin.
 
    ![Yeni Proje Oluştur bölmesi](./media/tutorial-ev-routing/create-project-window.png)

1. **Oluştur'u**seçin.

1. Projeniz oluşturulduktan sonra, bu [Jupyter not defteri belge dosyasını](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/EVrouting.ipynb) [Azure Haritalar Jupyter dizüstü bilgisayar deposundan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook)indirin.

1. **Projelerim** sayfasındaki projeler listesinde projenizi seçin ve ardından Jupyter not defteri belge dosyasını yüklemek için **Yükle'yi** seçin. 

    ![yükleme not defteri](./media/tutorial-ev-routing/upload-notebook.png)

1. Dosyayı bilgisayarınızdan yükleyin ve **ardından Bitti'yi**seçin.

1. Yükleme başarıyla tamamlandıktan sonra dosyanız proje sayfanızda görüntülenir. Jupyter dizüstü bilgisayar olarak açmak için dosyaya çift tıklayın.

Not defteri dosyasında uygulanan işlevselliği anlamaya çalışın. Kodu, not defteri dosyasında, her seferinde bir hücreçalıştırın. Dizüstü bilgisayar uygulamasının üst kısmındaki **Çalıştır** düğmesini seçerek her hücredeki kodu çalıştırabilirsiniz.

  ![Çalıştır düğmesi](./media/tutorial-ev-routing/run.png)

## <a name="install-project-level-packages"></a>Proje düzeyi paketlerini yükleme

Kodu not defterinde çalıştırmak için aşağıdaki adımları yaparak paketleri proje düzeyinde yükleyin:

1. [*requirements.txt*](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/requirements.txt) dosyasını [Azure Maps Jupyter dizüstü bilgisayar deposundan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook)indirin ve ardından projenize yükleyin.
1. Proje panosunda **Proje Ayarları'nı**seçin. 
1. Proje **Ayarları** bölmesinde **Çevre** sekmesini seçin ve sonra **Ekle'yi**seçin.
1. **Çevre Kurulum Adımları**altında, aşağıdakileri yapın:   
    a. İlk açılan listede **Requirements.txt'yi**seçin.  
    b. İkinci açılan *listede, gereksinimlerinizi seçin.txt* dosyanızı.  
    c. Üçüncü açılır listede, sürüm olarak **Python Sürüm 3.6'yı** seçin.
1. **Kaydet'i**seçin.

    ![Paketleri yükleme](./media/tutorial-ev-routing/install-packages.png)

## <a name="load-the-required-modules-and-frameworks"></a>Gerekli modülleri ve çerçeveleri yükleyin

Gerekli tüm modülleri ve çerçeveleri yüklemek için aşağıdaki komut dosyasını çalıştırın.

```Python
import time
import aiohttp
import urllib.parse
from IPython.display import Image, display
```

## <a name="request-the-reachable-range-boundary"></a>Ulaşılabilir aralık sınırını isteme

Bir paket teslimat şirketinin filosunda bazı elektrikli araçlar vardır. Gün boyunca, elektrikli araçların depoya dönmek zorunda kalmadan şarj edilmesi gerekir. Kalan ücret her bir saatten az düştüğünde, ulaşılabilir bir aralıkta bir dizi şarj istasyonu ararsınız. Esasen, pil in şarj ı düşük olduğunda bir şarj istasyonu ararsınız. Ve, şarj istasyonları bu dizi için sınır bilgileri olsun. 

Şirket ekonomi ve hız dengesi gerektiren rotalar kullanmayı tercih ettiği için, istenen routeType *ekodur.* Aşağıdaki komut dosyası, Azure Haritalar yönlendirme hizmetinin [Rota Aralığı Al API'sını](https://docs.microsoft.com/rest/api/maps/route/getrouterange) çağırır. Aracın tüketim modeli için parametreleri kullanır. Komut dosyası daha sonra geojson formatında, otomobilin maksimum ulaşılabilir aralığını temsil eden çokgen bir nesne oluşturmak için yanıtı parses.

Elektrikli aracın ulaşılabilir aralığının sınırlarını belirlemek için komut dosyasını aşağıdaki hücrede çalıştırın:

```python
subscriptionKey = "Your Azure Maps key"
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


# Get boundaries for the electric vehicle's reachable range.
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

## <a name="search-for-electric-vehicle-charging-stations-within-the-reachable-range"></a>Ulaşılabilir aralıkta elektrikli araç şarj istasyonları arayın

Elektrikli araç için ulaşılabilir aralığı (izochrone) belirledikten sonra, bu aralıktaki şarj istasyonlarını arayabilirsiniz. 

Aşağıdaki komut dosyası, Azure Haritalar [Sonrası Arama Geometri API'sini](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)çağırır. Bu elektrikli araç için şarj istasyonları için arar, aracın maksimum ulaşılabilir aralığısınırları içinde. Ardından, komut dosyası erişilebilir konumlar dizisine yanıtı parses.

Ulaşılabilen aralıktaki elektrikli araç şarj istasyonlarını aramak için aşağıdaki komut dosyasını çalıştırın:

```python
# Search for electric vehicle stations within reachable range.
searchPolyResponse = await (await session.post(url = "https://atlas.microsoft.com/search/geometry/json?subscription-key={}&api-version=1.0&query=electric vehicle station&idxSet=POI&limit=50".format(subscriptionKey), json = boundsData)).json() 

reachableLocations = []
for loc in range(len(searchPolyResponse["results"])):
                location = list(searchPolyResponse["results"][loc]["position"].values())
                location[0], location[1] = location[1], location[0]
                reachableLocations.append(location)
```

## <a name="upload-the-reachable-range-and-charging-points-to-azure-maps-data-service"></a>Kullanılabilir aralığı ve şarj noktalarını Azure Haritalar Veri Hizmeti'ne yükleyin

Haritada, şarj istasyonlarını ve elektrikli aracın maksimum ulaşılabilir aralığı için sınırı görselleştirmek isteyeceksiniz. Bunu yapmak için, geojson nesneleri olarak sınır verilerini ve şarj istasyonverilerini Azure Haritalar Veri Hizmeti'ne yükleyin. Veri [Yükleme API'sini](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)kullanın. 

Sınır ve şarj noktası verilerini Azure Haritalar Veri Hizmeti'ne yüklemek için aşağıdaki iki hücreyi çalıştırın:

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

# Upload the range data to Azure Maps Data Service.
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

# Upload the electric vehicle charging station data to Azure Maps Data Service.
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

## <a name="render-the-charging-stations-and-reachable-range-on-a-map"></a>Şarj istasyonlarını ve ulaşılabilir aralığı nı harita üzerinde işleme

Verileri veri hizmetine yükledikten sonra Azure Haritalar [Harita Görüntü Al hizmetini](https://docs.microsoft.com/rest/api/maps/render/getmapimage)arayın. Bu hizmet, aşağıdaki komut dosyasını çalıştırarak statik harita görüntüsündeki şarj noktalarını ve maksimum ulaşılabilir sınırı işlemek için kullanılır:

```python
# Get boundaries for the bounding box.
def getBounds(polyBounds):
    maxLon = max(map(lambda x: x[0], polyBounds))
    minLon = min(map(lambda x: x[0], polyBounds))

    maxLat = max(map(lambda x: x[1], polyBounds))
    minLat = min(map(lambda x: x[1], polyBounds))
    
    # Buffer the bounding box by 10 percent to account for the pixel size of pins at the ends of the route.
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

# Render the range and electric vehicle charging points on the map.
staticMapResponse =  await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&pins={}&path={}&bbox={}&zoom=12".format(subscriptionKey,encodedPins,path,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

poiRangeMap = await staticMapResponse.content.read()

display(Image(poiRangeMap))
```

![Konum aralığını gösteren bir harita](./media/tutorial-ev-routing/location-range.png)


## <a name="find-the-optimal-charging-station"></a>En uygun şarj istasyonunu bulun

İlk olarak, ulaşılabilir aralıktaki tüm olası şarj istasyonlarını belirlemek istiyorsunuz. Daha sonra, en az bir süre içinde hangisine ulaşılabileceğini bilmek istiyorsunuz. 

Aşağıdaki komut dosyası, Azure Haritalar [Matris Yönlendirme API'sını](https://docs.microsoft.com/rest/api/maps/route/postroutematrix)çağırır. Belirtilen aracın konumunu, seyahat süresini ve her şarj istasyonuna olan mesafeyi döndürür. Bir sonraki hücredeki komut dosyası, zamana göre en yakın erişilebilen şarj istasyonunu bulmak için yanıtı ayrışır.

En az sürede ulaşılabilen en yakın erişilebilir şarj istasyonunu bulmak için komut dosyasını aşağıdaki hücrede çalıştırın:

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

# Get the travel time and distance to each specified charging station.
searchPolyRes = await (await session.post(url = "https://atlas.microsoft.com/route/matrix/json?subscription-key={}&api-version=1.0&routeType=shortest&waitForResults=true".format(subscriptionKey), json = locationData)).json()

distances = []
for dist in range(len(reachableLocations)):
    distances.append(searchPolyRes["matrix"][0][dist]["response"]["routeSummary"]["travelTimeInSeconds"])

minDistLoc = []
minDistIndex = distances.index(min(distances))
minDistLoc.extend([reachableLocations[minDistIndex][1], reachableLocations[minDistIndex][0]])
closestChargeLoc = ",".join(str(i) for i in minDistLoc)
```

## <a name="calculate-the-route-to-the-closest-charging-station"></a>En yakın şarj istasyonuna giden rotayı hesaplayın

Artık en yakın şarj istasyonunu bulduğunuziçin, elektrikli aracın mevcut konumundan şarj istasyonuna giden ayrıntılı rotayı istemek için [Rota Yol Tarifi API'sini](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) arayabilirsiniz.

Rotayı şarj istasyonuna almak ve rotayı temsil eden bir geojson nesnesi oluşturmak için yanıtı ayrıştırmak için komut dosyasını aşağıdaki hücrede çalıştırın:

```python
# Get the route from the electric vehicle's current location to the closest charging station. 
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

Rotayı görselleştirmeye yardımcı olmak için, rota verilerini önce Geojson nesnesi olarak Azure Haritalar Veri Hizmeti'ne yüklersiniz. Bunu yapmak için Azure Haritalar [Veri Yükleme API'sini](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)kullanın. Ardından, görüntüleme hizmetini arayın, [Harita Görüntüsü API'sini alın,](https://docs.microsoft.com/rest/api/maps/render/getmapimage)rotayı haritaüzerinde işlemek ve görselleştirmek için.

Haritada işlenen rota için bir görüntü almak için aşağıdaki komut dosyasını çalıştırın:

```python
# Upload the route data to Azure Maps Data Service.
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


# Get boundaries for the bounding box.
minLat, maxLat = (float(destination[0]),currentLocation[0]) if float(destination[0])<currentLocation[0] else (currentLocation[0], float(destination[0]))
minLon, maxLon = (float(destination[1]),currentLocation[1]) if float(destination[1])<currentLocation[1] else (currentLocation[1], float(destination[1]))

# Buffer the bounding box by 10 percent to account for the pixel size of pins at the ends of the route.
lonBuffer = (maxLon-minLon)*0.1
minLon -= lonBuffer
maxLon += lonBuffer

latBuffer = (maxLat-minLat)*0.1
minLat -= latBuffer
maxLat += latBuffer

# Render the route on the map.
staticMapResponse = await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&&path={}&pins={}&bbox={}&zoom=16".format(subscriptionKey,path,pins,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

staticMapImage = await staticMapResponse.content.read()

await session.close()
display(Image(staticMapImage))
```

![Rotayı gösteren bir harita](./media/tutorial-ev-routing/route.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Haritalar REST API'lerini doğrudan aramayı ve Python'u kullanarak Azure Haritalar verilerini görselleştirmeyi öğrendiniz.

Bu eğitimde kullanılan Azure Haritalar API'larını keşfetmek için bkz:

* [Rota Aralığını Al](https://docs.microsoft.com/rest/api/maps/route/getrouterange)
* [Geometri İçi Arama Sonrası](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)
* [Veri Yükleme](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)
* [Render - Harita Görüntüsü Al](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Rota Sonrası Matrisi](https://docs.microsoft.com/rest/api/maps/route/postroutematrix)
* [Rota Yol Tarifleri alın](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)

Azure Haritalar REST API'lerinin tam listesi için Azure [Haritalar REST API'leri'ne](https://docs.microsoft.com/azure/azure-maps/consumption-model)bakın.

Azure Not Defterleri hakkında daha fazla bilgi edinmek için [Azure Not Defterleri'ne](https://docs.microsoft.com/azure/notebooks)bakın.
