---
title: Azure Veri Gezgini'nde zaman serisi anomali algılama & tahmin
description: Azure Veri Gezgini'ni kullanarak anormallik tespiti ve tahmini için zaman serisi verilerini nasıl analiz edebilirsiniz öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: a482fef93d43f92257608b65c9c0e2ade535bcca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194166"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Azure Veri Gezgini'nde anormallik algılama ve tahmin

Azure Veri Gezgini, bulut hizmetlerinden veya IoT aygıtlarından telemetri verilerinin toplanmasını devam ediyor. Bu veriler, hizmet durumunun izlenmesi, fiziksel üretim süreçleri, kullanım eğilimleri ve yük tahmini gibi çeşitli öngörüler için analiz edilir. Analiz, metrik normal taban çizgisi desenine göre bir sapma deseni bulmak için seçilen ölçümlerin zaman serilerinde yapılır. Azure Veri Gezgini, birden çok zaman serisinin oluşturulması, manipülasyonu ve çözümlemesi için yerel destek içerir. Binlerce zaman serisini saniyeler içinde oluşturabilir ve analiz edebilir ve neredeyse gerçek zamanlı izleme çözümleri ve iş akışları sağlar.

Bu makalede, Azure Veri Gezgini zaman serisi anomali algılama ve tahmin yetenekleri ayrıntılı olarak anlatılıyor. Geçerli zaman serisi işlevleri, her orijinal zaman serisinin mevsimsel, trend ve artık bileşenlere ayrıştırıldığı sağlam ve iyi bilinen ayrıştırma modeline dayanır. Anomaliler artık bileşen üzerinde aykırı lar tarafından tespit edilirken, tahmin mevsimsel ve trend bileşenleri nin tahmin edilerek yapılır. Azure Veri Gezgini uygulaması, otomatik mevsimsellik algılama, sağlam aykırı çözümleme ve binlerce zaman serisini saniyeler içinde işlemek için vektörel uygulama ile temel ayrışma modelini önemli ölçüde geliştirir.

## <a name="prerequisites"></a>Ön koşullar

Zaman serisi özelliklerine genel bir bakış için [Azure Veri Gezgini'nde Zaman serisi çözümlemesi'ni](/azure/data-explorer/time-series-analysis) okuyun.

## <a name="time-series-decomposition-model"></a>Zaman serisi ayrışma modeli

Zaman serisi tahmini ve anomali algılama için Azure Veri Gezgini yerel uygulaması, iyi bilinen bir ayrışma modeli kullanır. Bu model, gelecekteki metrik değerleri tahmin etmek ve anormal olanları tespit etmek için servis trafiği, bileşen kalp atışları ve IoT periyodik ölçümleri gibi periyodik ve eğilim davranışı göstermesi beklenen zaman ölçümleri serisine uygulanır. Bu regresyon sürecinin varsayımı, daha önce bilinen mevsimsel ve eğilim davranışı dışında, zaman serilerinin rasgele dağıtıldığıdır. Daha sonra, toplu olarak adlandırılmış taban çizgisi olan mevsimsel ve eğilim bileşenlerinden gelecekteki metrik değerleri tahmin edebilir ve kalan kısmı yoksayabilirsiniz. Ayrıca, yalnızca kalan kısmı kullanarak aykırı çözümlemesi dayalı anormal değerleri algılayabilirsiniz.
Ayrıştırma modeli oluşturmak için [`series_decompose()`](/azure/kusto/query/series-decomposefunction)işlevi kullanın. İşlev `series_decompose()` bir dizi zaman serisi alır ve her zaman serisini mevsimsel, eğilim, artık ve temel bileşenleriyle otomatik olarak ayrıştırır. 

Örneğin, aşağıdaki sorguyu kullanarak dahili bir web hizmetinin trafiğini ayrıştırabilirsiniz:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA3WQ3WrDMAyF7/sUukvCnDXJGIOVPEULuwxqoixm/gm2+jf28JObFjbYrmyho3M+yRCD1a5jaGFAJtaW8qaqX8qqLqvnYrMySYHnvxRNWT1B07xW1U03JFEzbVYDWd9Z/KAuUtAUm9UXpLJcSnAH2+LxPZe3AO9gJ6ZbRjvDGLy9EbG/BUemOXnvLxD1AOJ1mijQtWhbyHbbOgOA9RogkqGeAaXn3g1BooVb6OiDNHpD6CjAUccDGv2JrL0TSzozuQHyPYqHdqRkDKN3aBRwkJaCQJIoQ4VsuXh2A/Xezj5SWkVBWSvI0vSoOSsWpLtEpyDwY4KTW8nnJ5ws+2+eAhSyOxjkd+HDVVcIfHplp2TYTxgYTpqnnDUbarM32gPO86PY4jjqfmGw3vGkftNlCi5xNprbWW5kYvENQQnqDh8CAAA=)**\]**

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (baseline, seasonal, trend, residual) = series_decompose(num, -1, 'linefit')  //  decomposition of a set of time series to seasonal, trend, residual, and baseline (seasonal+trend)
| render timechart with(title='Web app. traffic of a month, decomposition', ysplit=panels)
```

![Time series decomposition](media/anomaly-detection/series-decompose-timechart.png)

* Orijinal zaman serisi **num** (kırmızı) olarak etiketlenir. 
* İşlem, işlevi [`series_periods_detect()`](/azure/kusto/query/series-periods-detectfunction) kullanarak mevsimselliğin otomatik olarak algılanmasıyla başlar ve **mevsimsel** deseni (mor renkte) ayıklar.
* Mevsimsel desen orijinal zaman serisinden çıkarılır ve **eğilim** bileşenini (açık [`series_fit_line()`](/azure/kusto/query/series-fit-linefunction) mavi) bulmak için işlev kullanılarak doğrusal bir regresyon çalıştırılır.
* İşlev eğilimi çıkarır ve geri kalan **kalan kalan** bileşen (yeşil).
* Son olarak, işlev **taban çizgisi** (mavi) oluşturmak için mevsimsel ve eğilim bileşenleri ekler.

## <a name="time-series-anomaly-detection"></a>Zaman serisi anomali tespiti

İşlev, [`series_decompose_anomalies()`](/azure/kusto/query/series-decompose-anomaliesfunction) zaman serileri kümesinde anormal noktalar bulur. Bu işlev `series_decompose()` ayrışma modelini [`series_outliers()`](/azure/kusto/query/series-outliersfunction) oluşturmak için çağırır ve sonra artık bileşen üzerinde çalışır. `series_outliers()`Tukey'nin çit testini kullanarak artık bileşenin her noktası için anomali skorlarını hesaplar. -1.5'in üzerindeki anomali skorları sırasıyla hafif bir anomali artış veya düşüş gösterir. -3.0'ın üzerindeki anomali skorları güçlü bir anomali olduğunu gösterir. 

Aşağıdaki sorgu, dahili web hizmeti trafiğindeki anormallikleri algılamanızı sağlar:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA3WR3W7CMAyF73mKI25KpRbaTmjSUJ8CpF1WoXVptPxUifmb9vBLoGO7GFeR7ePv2I4ihpamYdToBBNLTYuqKF/zosyLdbqZqagQl/8UVV68oKreimLSdVFUDZtZR9o2WnxQ48lJ8tXsCzHM7yHMUdfidFiEN4U12AXoloUe0Turp4nYTsaeaYzs/RVedgis80CObkFdI9ltywTAagV4UtQyRKiZgyLEaTGZ9taFQqtIGHI4SX8USn4KltYEJF2YTIeFMFaHPPkMvrWOMuxFoEpDaVjujmo6aq0erafmIY+7ZCiX6wx5mSGJHb3kJA1sF8jB8q69toNwjLPkYfGTseqoja//eLNkRXXyTnuIcVyCneh72cL2YQdtDQ8ZHvIkDcsfPWH+3AvPvObx0FMXD/RLhfDYW9VhtNKwj/8U69M1b2S//AbRUQMWQQIAAA==)**\]**

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (anomalies, score, baseline) = series_decompose_anomalies(num, 1.5, -1, 'linefit')
| render anomalychart with(anomalycolumns=anomalies, title='Web app. traffic of a month, anomalies') //use "| render anomalychart with anomalycolumns=anomalies" to render the anomalies as bold points on the series charts.
```

![Zaman serisi anomali tespiti](media/anomaly-detection/series-anomaly-detection.png)

* Orijinal zaman serisi (kırmızı). 
* Taban çizgisi (mevsimsel + trend) bileşeni (mavi).
* Orijinal zaman serisinin üstündeki anormal noktalar (mor). Anormal noktalar beklenen temel değerlerden önemli ölçüde sapma gösterir.

## <a name="time-series-forecasting"></a>Zaman serisi tahmini

İşlev, [`series_decompose_forecast()`](/azure/kusto/query/series-decompose-forecastfunction) bir zaman serisi kümesinin gelecekteki değerlerini öngörür. Bu işlev `series_decompose()` ayrışma modelini oluşturmak için çağırır ve sonra, her zaman serisi için, gelecek için temel bileşeni tahmin eder.

Aşağıdaki sorgu, gelecek haftanın web hizmeti trafiğini tahmin etmenizi sağlar:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA22QzW6DMBCE73mKuQFqKISqitSIW98gkXpEDl5iK9hG9uanUR++dqE99YRGO8x845EYRtuO0UIKJtaG8qbebMt6U9avxW41Joe4/+doyvoFTfNW14tPJlOjZqGc1w9n263crSQZ1xlxpi6Q1xSa1ReSLGcJezGtuJ7y+C3gLA6xZM/CTBi8MwshuxnkaUlGYJpS5/ETQUvEzJsiTz+ibZEd9psMQFUBgUbqGSLe7GkkpBVYygfn46EfSVjyuOpwEaN+CNbOxki6M1mZTNSLkAbOv3WSemcmF6j7vSX8dcTUlvOFsZJcFDHFx4wYnmp7JTzjplnlrHmkNvugI8Q0PYO9GAbdww0RyDjLav1XHLnBimAjEG5E5zQ7vRP284x36hOOTtxZ8Q3The8P2QEAAA==)**\]**

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week by Time Series Decomposition')
```

![Zaman serisi tahmini](media/anomaly-detection/series-forecasting.png)

* Orijinal metrik (kırmızı). Gelecekteki değerler eksik ve varsayılan olarak 0 olarak ayarlanır.
* Gelecek haftanın değerlerini tahmin etmek için temel bileşeni (mavi) tahmin edin.

## <a name="scalability"></a>Ölçeklenebilirlik

Azure Veri Gezgini sorgu dili sözdizimi, birden çok zaman serisini işlemek için tek bir çağrı sağlar. Benzersiz optimize edilmiş uygulaması, neredeyse gerçek zamanlı senaryolarda binlerce sayacı izlerken etkili anomali tespiti ve tahmini için kritik öneme sahip hızlı performanssağlar.

Aşağıdaki sorgu aynı anda üç zaman serisinin işlenmesini gösterir:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA21Qy26DMBC85yvmFlChcUirSI34ikTqETl4KVawjfDmqX587UCaHuqLtePxPLYjhtG2YpRQkom1oaQQy3Uulrl4TzezLjLk5T9GkYsViuJDiImnIqlox6F1g745W67VZqbIuMrIA1WeBk2+mH0jjvk4wh5NKU9fSbhTOItdMNmyND2awZkpIbsxyMukDM/UR8/9FV6rIEkXJqvgmsYTl7X0lISHspzvtqt5hjdxPxkeYBHA4gGKFMBiAUilIAfWja617CY1NG4ASX/FSfuj7PRNsg4ZXANz7Fj3HSGuBmOjZ5hYbcSqIBwbZpNk+iQFcQpx4/omrqLamd55qh5v41d22nIybWChOI0qQ9Cg4e5ftyE6zprbhDV3VM4/aQ/Z96/gQTahU4wsYZzlNvs11vYL3BJsCIQz0eHed/W30jz9AUEBI0ktAgAA)**\]**

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid
| extend offset=case(sid=='TS3', 4000000, sid=='TS2', 2000000, 0)   //  add artificial offset for easy visualization of multiple time series
| extend num=series_add(num, offset)
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week for 3 time series')
```

![Zaman serileri ölçeklenebilirlik](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Özet

Bu belge, zaman serisi anomali algılama ve tahmini için yerel Azure Veri Gezgini işlevlerini ayrıntılarıyla anlatır. Her orijinal zaman serisi, anormallikleri ve/veya tahminleri tespit etmek için mevsimsel, trend ve artık bileşenlere ayrıştırılır. Bu işlevler, hata algılama, tahmine dayalı bakım ve talep ve yük tahmini gibi neredeyse gerçek zamanlı izleme senaryoları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Gezgini'nde [Makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) hakkında bilgi edinin.
