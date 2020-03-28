---
title: 'Öğretici: Azure Not Defterleri(Python) kullanarak hava durumu verileriyle sensör verilerine katılın | Microsoft Azure Haritaları'
description: Bu öğretici, Azure Not Defterleri(Python) kullanarak Microsoft Azure Haritalar Hava Durumu Hizmeti'nden alınan hava durumu verileriyle sensör verilerine nasıl katılacağınızı gösterir.
author: philmea
ms.author: philmea
ms.date: 01/29/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: e5292f5166e739264e9cf969480b70f415fcc75a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333488"
---
# <a name="tutorial-join-sensor-data-with-weather-forecast-data-by-using-azure-notebooks-python"></a>Öğretici: Azure Not Defterleri (Python) kullanarak hava durumu verileriyle sensör verilerine katılın

Rüzgar enerjisi iklim değişikliğine karşı mücadele fosil yakıtlar için alternatif bir enerji kaynağıdır. Rüzgar doğası gereği tutarlı olmadığından, rüzgar enerjisi operatörleri rüzgar gücü kapasitesini tahmin etmek için makine öğrenme (ML) modelleri inşa etmek gerekir. Bu tahmin elektrik talebini karşılamak ve şebeke istikrarını sağlamak için gereklidir. Bu eğitimde, Azure Maps hava tahmini verilerinin hava durumu okumaları için demo verilerle nasıl birleştirildiğinden geçiyoruz. Hava tahmini verileri Azure Haritalar Hava Durumu hizmetini arayarak istenir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Buluttaki Azure [Not Defterleri'ndeki](https://docs.microsoft.com/azure/notebooks) veri dosyalarıyla çalışın.
> * Demo verilerini dosyadan yükleyin.
> * Python'da Azure Haritalar REST API'lerini arayın.
> * Konum verilerini haritada işle.
> * Demo verilerini Azure Haritalar [Günlük Tahmini](https://aka.ms/AzureMapsWeatherDailyForecast) hava durumu verileriyle zenginleştirin.
> * Grafiklerdeki tahmin verilerini çizin.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için öncelikle şunları yapmanız gerekir:

1. Hesap Oluştur'daki yönergeleri izleyerek S0 fiyatlandırma katmanında bir Azure Haritalar hesabı aboneliği [oluşturun.](quick-demo-map-app.md#create-an-account-with-azure-maps)
2. Hesabınız için birincil abonelik anahtarını alın, [birincil anahtarı al'daki](quick-demo-map-app.md#get-the-primary-key-for-your-account)yönergeleri izleyin.


Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

Azure dizüstü bilgisayarları tanımak ve nasıl başlayacağınız hakkında bilgi almak için, [Azure Not Defteri Oluştur](https://docs.microsoft.com/azure/azure-maps/tutorial-ev-routing#create-an-azure-notebook)yönergelerini izleyin.

> [!Note]
> Bu proje için Jupyter not defteri dosyası [Hava Haritaları Jupyter dizüstü deposundan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/tree/master/AzureMapsJupyterSamples/Tutorials/Analyze%20Weather%20Data)indirilebilir.

## <a name="load-the-required-modules-and-frameworks"></a>Gerekli modülleri ve çerçeveleri yükleyin

Gerekli tüm modülleri ve çerçeveleri yüklemek için aşağıdaki komut dosyasını çalıştırın:

```python
import pandas as pd
import datetime
from IPython.display import Image, display
!pip install aiohttp
import aiohttp
```

## <a name="import-weather-data"></a>Hava durumu verilerini içe aktarma

Bu öğretici uğruna, dört farklı rüzgar türbinleri yüklü sensörler hava veri okumaları kullanacağız. Örnek veriler 30 günlük hava durumu okumalarından oluşur. Bu ölçümler her türbin yerinin yakınındaki hava durumu veri merkezlerinden toplanır. Demo verileri sıcaklık, rüzgar hızı ve yön için veri okumaları içerir. Demo verilerini [buradan](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/tree/master/AzureMapsJupyterSamples/Tutorials/Analyze%20Weather%20Data/data)indirebilirsiniz. Aşağıdaki komut dosyası demo verilerini Azure Not Defteri'ne aktarın.

```python
df = pd.read_csv("./data/weather_dataset_demo.csv")
```

## <a name="request-daily-forecast-data"></a>Günlük tahmin verilerini isteme

Senaryomuzda, her sensör konumu için günlük tahmin talep etmek istiyoruz. Aşağıdaki komut dosyasında Azure Haritalar hava durumu hizmetinin [Günlük Tahmin API'si](https://aka.ms/AzureMapsWeatherDailyForecast) çağrılmaktadır. Bu API, geçerli tarihten itibaren gelecek 15 gün boyunca her rüzgar türbini için hava tahminini döndürür.


```python
subscription_key = "Your Azure Maps key"

# Get a lists of unique station IDs and their coordinates 
station_ids = pd.unique(df[['StationID']].values.ravel())
coords = pd.unique(df[['latitude','longitude']].values.ravel())

years,months,days = [],[],[]
dates_check=set()
wind_speeds, wind_direction = [], []

# Call azure maps weather service to get daily forecast data for 15 days from current date
session = aiohttp.ClientSession()
j=-1
for i in range(0, len(coords), 2):
    wind_speeds.append([])
    wind_direction.append([])
    
    query = str(coords[i])+', '+str(coords[i+1])
    forecast_response = await(await session.get("https://atlas.microsoft.com/weather/forecast/daily/json?query={}&api-version=1.0&subscription-key={}&duration=15".format(query, subscription_key))).json()
    j+=1
    for day in range(len(forecast_response['forecasts'])):
            date = forecast_response['forecasts'][day]['date'][:10]
            wind_speeds[j].append(forecast_response['forecasts'][day]['day']['wind']['speed']['value'])
            wind_direction[j].append(forecast_response['forecasts'][day]['day']['windGust']['direction']['degrees'])
            
            if date not in dates_check:
                year,month,day= date.split('-')
                years.append(year)
                months.append(month)
                days.append(day)
                dates_check.add(date)
            
await session.close()
```

Aşağıdaki komut dosyası, Azure Haritalar [Harita Görüntü Al hizmetini](https://docs.microsoft.com/rest/api/maps/render/getmapimage)arayarak haritadaki türbin konumlarını işler.

```python
# Render the turbine locations on the map by calling the Azure Maps Get Map Image service
session = aiohttp.ClientSession()

pins="default|la-25+60|ls12|lc003C62|co9B2F15||'Location A'{} {}|'Location B'{} {}|'Location C'{} {}|'Location D'{} {}".format(coords[1],coords[0],coords[3],coords[2],coords[5],coords[4], coords[7],coords[6])

image_response = "https://atlas.microsoft.com/map/static/png?subscription-key={}&api-version=1.0&layer=basic&style=main&zoom=6&center={},{}&pins={}".format(subscription_key,coords[7],coords[6],pins)

static_map_response = await session.get(image_response)

poi_range_map = await static_map_response.content.read()

await session.close()

display(Image(poi_range_map))
```

![Türbin konumları](./media/weather-service-tutorial/location-map.png)


Tahmin verilerini istasyon kimliğine göre demo verileriyle gruplayacağız. İstasyon kimliği hava durumu veri merkezi içindir. Bu gruplandırma, demo verilerini tahmin verileriyle genişletir.

```python
# Group forecasted data for all locations
df = df.reset_index(drop=True)
forecast_data = pd.DataFrame(columns=['StationID','latitude','longitude','Year','Month','Day','DryBulbCelsius','WetBulbFarenheit','WetBulbCelsius','DewPointFarenheit','DewPointCelsius','RelativeHumidity','WindSpeed','WindDirection'])

for i in range(len(station_ids)):
    loc_forecast = pd.DataFrame({'StationID':station_ids[i], 'latitude':coords[0], 'longitude':coords[1], 'Year':years, 'Month':months, 'Day':days, 'WindSpeed':wind_speeds[i], 'WindDirection':wind_direction[i]})
    forecast_data = pd.concat([forecast_data,loc_forecast], axis=0, sort=False)
    
combined_weather_data = pd.concat([df,forecast_data])
grouped_weather_data = combined_weather_data.groupby(['StationID'])
```

Aşağıdaki tablo, türbin konumlarından birinin birleşik geçmiş ve tahmin verilerini görüntüler.

```python
# Display data for first location
grouped_weather_data.get_group(station_ids[0]).reset_index()
```

<center>

![Gruplanmış veriler](./media/weather-service-tutorial/grouped-data.png)</center>

## <a name="plot-forecast-data"></a>Tahmin verilerini çizin

Tahmin edilen değerleri tahmin edildikleri günlere göre çizeceğiz. Bu arsa bize önümüzdeki 15 gün boyunca rüzgarın hız ve yön değişiklikleri görmenizi sağlar.

```python
# Plot wind speed
curr_date = datetime.datetime.now().date()
windsPlot_df = pd.DataFrame({ 'Location A': wind_speeds[0], 'Location B': wind_speeds[1], 'Location C': wind_speeds[2], 'Location D': wind_speeds[3]}, index=pd.date_range(curr_date,periods=15))
windsPlot = windsPlot_df.plot.line()
windsPlot.set_xlabel("Date")
windsPlot.set_ylabel("Wind speed")
```

```python
#Plot wind direction 
windsPlot_df = pd.DataFrame({ 'Location A': wind_direction[0], 'Location B': wind_direction[1], 'Location C': wind_direction[2], 'Location D': wind_direction[3]}, index=pd.date_range(curr_date,periods=15))
windsPlot = windsPlot_df.plot.line()
windsPlot.set_xlabel("Date")
windsPlot.set_ylabel("Wind direction")
```

Aşağıdaki grafikler tahmin verilerini görselleştirin. Rüzgar hızının değişmesi için sol grafiğe bakın. Rüzgar yönü değişikliği için doğru grafiğe bakın. Bu veriler, verilerin istendiği günden itibaren gelecek 15 gün için tahmin edilir.

<center>

![Rüzgar hızı](./media/weather-service-tutorial/speed-date-plot.png) ![arsa Rüzgar yönü arsa](./media/weather-service-tutorial/direction-date-plot.png)</center>


## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, hava tahmini verilerini almak için Azure Maps REST API'lerini nasıl arayacağınızı öğrendiniz. Ayrıca grafiklerdeki verileri nasıl görselleştirdiğinizi de öğrendiniz.

Azure Dizüstü Bilgisayarlar'da Azure Haritalar REST API'leri hakkında daha fazla bilgi edinmek için [Azure Not Defterleri'ni kullanarak EV yönlendirmesine](https://docs.microsoft.com/azure/azure-maps/tutorial-ev-routing)bakın.

Bu eğitimde kullanılan Azure Haritalar API'larını keşfetmek için bkz:

* [Günlük Tahmin](https://aka.ms/AzureMapsWeatherDailyForecast)
* [Render - Harita Görüntüsü Al](https://docs.microsoft.com/rest/api/maps/render/getmapimage)

Azure Haritalar REST API'lerinin tam listesi için Azure [Haritalar REST API'leri'ne](https://docs.microsoft.com/azure/azure-maps/consumption-model)bakın.

Azure Not Defterleri hakkında daha fazla bilgi edinmek için [Azure Not Defterleri'ne](https://docs.microsoft.com/azure/notebooks)bakın.
