---
title: Zaman serisi anomali algılama & tahmin-Azure Veri Gezgini
description: Azure Veri Gezgini kullanarak anomali algılama ve tahmin için zaman serisi verilerini çözümlemeyi öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: 4a7463d6149a921a4a29b43eaebb78a01543323a
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985834"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Azure Veri Gezgini anomali algılama ve tahmin etme

Azure Veri Gezgini, bulut hizmetlerinden veya IoT cihazlarından Telemetri verilerinin üzerinde toplanmasını gerçekleştirir. Bu veriler, Service Health, fiziksel üretim süreçlerini, kullanım eğilimlerini ve yük tahminini izleme gibi çeşitli Öngörüler için çözümlenir. Analiz, ölçüm için tipik normal taban çizgisi düzenine göre bir sapma deseninin yerini bulmak için seçili ölçümlerin zaman serisinde yapılır. Azure Veri Gezgini, birden çok zaman dizisinin oluşturulması, işlenmesi ve çözümlenmesi için yerel destek içerir. Neredeyse gerçek zamanlı izleme çözümlerini ve iş akışlarını etkinleştirerek, saniyeler içinde binlerce zaman serisi oluşturabilir ve analiz edebilir.

Bu makalede Azure Veri Gezgini zaman serisi anomali algılama ve tahmin özelliklerine ilişkin ayrıntılar yer aldığı açıklanır. İlgili zaman serisi işlevleri, her orijinal zaman serisinin mevsimler, eğilim ve kalan bileşenlere dahil edildiği sağlam, iyi bilinen bir ayrıştırma modelini temel alır. Bu durumlar, kalan bileşen üzerinde aykırı durumlar tarafından algılanır, ancak tahmin, mevsimler ve eğilim bileşenleri tahmin edilirken tahmin yapılır. Azure Veri Gezgini uygulama, otomatik mevsimsellik algılama, güçlü aykırı değer analizi ve vektör haline getirilmiş uygulama için, saniyeler içinde binlerce zaman serisini işlemek üzere temel ayrıştırma modelini önemli ölçüde geliştirir.

## <a name="prerequisites"></a>Ön koşullar

Zaman serisi özelliklerine genel bir bakış için [Azure Veri Gezgini zaman serisi analizini](/azure/data-explorer/time-series-analysis) okuyun.

## <a name="time-series-decomposition-model"></a>Zaman serisi ayrıştırma modeli

Zaman serisi tahmini ve anomali algılama için Azure Veri Gezgini yerel uygulama, iyi bilinen bir ayrıştırma modeli kullanır. Bu model, gelecek ölçüm değerlerini tahmin etmek ve anormal olanları algılamak üzere hizmet trafiği, bileşen sinyalleri ve IoT düzenli ölçümleri gibi dönemsel ve eğilim davranışlarının bildirimde bulunması beklenen ölçüm dizisine uygulanır. Bu gerileme işleminin varsayımını, önceden bilinen mevsimlik ve eğilim davranışından başka, zaman serisinin rastgele dağıtıldığı bir işlemdir. Daha sonra, dönemsel olarak adlandırılmış taban çizgisi ve eğilim bileşenlerinden gelecek ölçüm değerlerini tahmin edebilir ve kalan parçayı yoksayabilirsiniz. Ayrıca, yalnızca fazlalık kısmını kullanarak aykırı değerleri temel alan anormal değerleri tespit edebilirsiniz.
Ayrıştırma modeli oluşturmak için [`series_decompose()`](/azure/kusto/query/series-decomposefunction)işlevini kullanın. `series_decompose()` işlevi bir zaman serisi kümesi alır ve her bir zaman serisini dönemsel, eğilim, artıkya ve temel bileşenlere otomatik olarak kaldırır. 

Örneğin, aşağıdaki sorguyu kullanarak bir iç Web hizmetinin trafiğini parçalara ayırmayı seçebilirsiniz:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA3WQ3WrDMAyF7/sUukvCnDXJGIOVPEULuwxqoixm/gm2+jf28JObFjbYrmyho3M+yRCD1a5jaGFAJtaW8qaqX8qqLqvnYrMySYHnvxRNWT1B07xW1U03JFEzbVYDWd9Z/KAuUtAUm9UXpLJcSnAH2+LxPZe3AO9gJ6ZbRjvDGLy9EbG/BUemOXnvLxD1AOJ1mijQtWhbyHbbOgOA9RogkqGeAaXn3g1BooVb6OiDNHpD6CjAUccDGv2JrL0TSzozuQHyPYqHdqRkDKN3aBRwkJaCQJIoQ4VsuXh2A/Xezj5SWkVBWSvI0vSoOSsWpLtEpyDwY4KTW8nnJ5ws+2+eAhSyOxjkd+HDVVcIfHplp2TYTxgYTpqnnDUbarM32gPO86PY4jjqfmGw3vGkftNlCi5xNprbWW5kYvENQQnqDh8CAAA=) **\]**

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

![Zaman serisi ayrıştırma](media/anomaly-detection/series-decompose-timechart.png)

* Özgün zaman serisi **num** (kırmızı) olarak etiketlenir. 
* İşlem, [`series_periods_detect()`](/azure/kusto/query/series-periods-detectfunction) işlevi kullanılarak mevsimsellik otomatik olarak algılanarak başlar ve **mevsimsel** düzende (mor) ayıklar.
* Dönemsel model özgün zaman serisinden çıkarılır ve **eğilim** bileşenini bulmak için [`series_fit_line()`](/azure/kusto/query/series-fit-linefunction) işlevi kullanılarak doğrusal regresyon çalıştırılır (açık mavi).
* İşlev eğilimi çıkarır ve **kalan bileşen (** yeşil renkte) olur.
* Son olarak, işlev, **taban çizgisini** oluşturmak için mevsimlerini ve eğilim bileşenlerini ekler (mavi).

## <a name="time-series-anomaly-detection"></a>Zaman serisi anomali algılama

İşlev [`series_decompose_anomalies()`](/azure/kusto/query/series-decompose-anomaliesfunction) bir zaman serisi kümesi üzerinde anormal noktaları bulur. Bu işlev, ayrıştırma modelini derlemek için `series_decompose()` çağırır ve ardından fazlalık bileşeni üzerinde [`series_outliers()`](/azure/kusto/query/series-outliersfunction) çalıştırır. `series_outliers()`, Tukey 'in çit testini kullanarak fazlalık bileşenin her bir noktası için anomali puanları hesaplar. 1,5 veya altındaki anomali puanları-1,5 sırasıyla hafif bir anomali artış veya reddetme olduğunu gösterir. 3,0 veya altındaki anomali puanları-3,0 güçlü bir anomali olduğunu gösterir. 

Aşağıdaki sorgu, iç Web hizmeti trafiğinden bozuklukları tespit etmenizi sağlar:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA3WR3W7CMAyF73mKI25KpRbaTmjSUJ8CpF1WoXVptPxUifmb9vBLoGO7GFeR7ePv2I4ihpamYdToBBNLTYuqKF/zosyLdbqZqagQl/8UVV68oKreimLSdVFUDZtZR9o2WnxQ48lJ8tXsCzHM7yHMUdfidFiEN4U12AXoloUe0Turp4nYTsaeaYzs/RVedgis80CObkFdI9ltywTAagV4UtQyRKiZgyLEaTGZ9taFQqtIGHI4SX8USn4KltYEJF2YTIeFMFaHPPkMvrWOMuxFoEpDaVjujmo6aq0erafmIY+7ZCiX6wx5mSGJHb3kJA1sF8jB8q69toNwjLPkYfGTseqoja//eLNkRXXyTnuIcVyCneh72cL2YQdtDQ8ZHvIkDcsfPWH+3AvPvObx0FMXD/RLhfDYW9VhtNKwj/8U69M1b2S//AbRUQMWQQIAAA==) **\]**

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

![Zaman serisi anomali algılama](media/anomaly-detection/series-anomaly-detection.png)

* Özgün zaman serisi (kırmızı). 
* Taban çizgisi (mevsimli ve eğilim) bileşeni (mavi).
* Özgün zaman serisinin en üstünde yer alan anormal noktaları (mor renkte). Anormal noktaları beklenen taban çizgisi değerlerinden önemli ölçüde farklılık gösterir.

## <a name="time-series-forecasting"></a>Zaman serisi tahmin

İşlevi [`series_decompose_forecast()`](/azure/kusto/query/series-decompose-forecastfunction) bir zaman serisi kümesinin gelecek değerlerini tahmin eder. Bu işlev, ayrıştırma modelini oluşturmak için `series_decompose()` çağırır ve sonra her zaman serisi için temel bileşeni daha sonra bir şekilde tahmin edebilir.

Aşağıdaki sorgu, sonraki haftaki Web hizmeti trafiğini tahmin etmenizi sağlar:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA22QzW6DMBCE73mKuQFqKISqitSIW98gkXpEDl5iK9hG9uanUR++dqE99YRGO8x845EYRtuO0UIKJtaG8qbebMt6U9avxW41Joe4/+doyvoFTfNW14tPJlOjZqGc1w9n263crSQZ1xlxpi6Q1xSa1ReSLGcJezGtuJ7y+C3gLA6xZM/CTBi8MwshuxnkaUlGYJpS5/ETQUvEzJsiTz+ibZEd9psMQFUBgUbqGSLe7GkkpBVYygfn46EfSVjyuOpwEaN+CNbOxki6M1mZTNSLkAbOv3WSemcmF6j7vSX8dcTUlvOFsZJcFDHFx4wYnmp7JTzjplnlrHmkNvugI8Q0PYO9GAbdww0RyDjLav1XHLnBimAjEG5E5zQ7vRP284x36hOOTtxZ8Q3The8P2QEAAA==) **\]**

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

![Zaman serisi tahmin](media/anomaly-detection/series-forecasting.png)

* Özgün ölçüm (kırmızı). Varsayılan olarak, gelecekteki değerler eksiktir ve 0 olarak ayarlanır.
* Sonraki haftanın değerlerini tahmin etmek için ana hat bileşenini (mavi olarak) extrapogeç.

## <a name="scalability"></a>Ölçeklenebilirlik

Azure Veri Gezgini sorgu dili sözdizimi, birden çok zaman serisini işlemek için tek bir çağrı sağlar. Benzersiz en iyi duruma getirilmiş uygulama, neredeyse gerçek zamanlı senaryolarda binlerce sayacı izlerken etkili anomali algılama ve tahmin için kritik önem taşıyan hızlı performansa izin verir.

Aşağıdaki sorguda, üç zaman serisinin eşzamanlı olarak işlenmesi gösterilmektedir:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA21Qy26DMBC85yvmFlChcUirSI34ikTqETl4KVawjfDmqX587UCaHuqLtePxPLYjhtG2YpRQkom1oaQQy3Uulrl4TzezLjLk5T9GkYsViuJDiImnIqlox6F1g745W67VZqbIuMrIA1WeBk2+mH0jjvk4wh5NKU9fSbhTOItdMNmyND2awZkpIbsxyMukDM/UR8/9FV6rIEkXJqvgmsYTl7X0lISHspzvtqt5hjdxPxkeYBHA4gGKFMBiAUilIAfWja617CY1NG4ASX/FSfuj7PRNsg4ZXANz7Fj3HSGuBmOjZ5hYbcSqIBwbZpNk+iQFcQpx4/omrqLamd55qh5v41d22nIybWChOI0qQ9Cg4e5ftyE6zprbhDV3VM4/aQ/Z96/gQTahU4wsYZzlNvs11vYL3BJsCIQz0eHed/W30jz9AUEBI0ktAgAA) **\]**

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

![Zaman serisi ölçeklenebilirliği](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Özet

Bu belge, zaman serisi anomali algılama ve tahmin için yerel Azure Veri Gezgini işlevlerini ayrıntılarına yöneliktir. Her bir orijinal zaman serisi, anormalileri ve/veya tahmin algılama için mevsimler, eğilim ve kalan bileşenlere göre belirlenir. Bu işlevler, hata algılama, tahmine dayalı bakım ve talep ve yük tahmini gibi neredeyse gerçek zamanlı izleme senaryoları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Gezgini 'de [makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) hakkında bilgi edinin.
