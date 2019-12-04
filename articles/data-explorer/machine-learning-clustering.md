---
title: Azure Veri Gezgini makine öğrenimi özelliği
description: Azure Veri Gezgini 'de kök neden analizi için makine öğrenimi Kümelemesi kullanın.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: fe72031ef9ade7473dc4d5de7e090e92ef2a6843
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74769940"
---
# <a name="machine-learning-capability-in-azure-data-explorer"></a>Azure Veri Gezgini makine öğrenimi özelliği

Büyük veri analizi platformu olan Azure Veri Gezgini, yerleşik [anomali algılama ve tahmin](/azure/data-explorer/anomaly-detection) işlevlerini kullanarak anormal davranışlar için hizmet durumu, QoS veya arızalı cihazları izlemek üzere kullanılır. Anormal bir model algılandığında, anomali 'i azaltmak veya çözmek için bir kök neden analizi (RCA) gerçekleştirilir.

Tanılama işlemi karmaşıktır ve uzun ve etki alanı uzmanları tarafından yapılır. Bu işlem, aynı zaman çerçevesi için farklı kaynaklardan ek verileri getirme ve birleştirme işlemini içerir. Bu, birden fazla boyutlardaki değerlerin dağılımını, ek değişkenleri ve etki alanı bilgisine göre diğer teknikleri grafik halinde ve ıntuın. Bu tanılama senaryoları Azure Veri Gezgini 'de yaygın olduğundan, tanılama aşamasını daha kolay hale getirmek ve RCA süresini kısaltmak için makine öğrenimi eklentileri kullanılabilir.

Azure Veri Gezgini üç Machine Learning eklenti içerir: [`autocluster`](/azure/kusto/query/autoclusterplugin), [`basket`](/azure/kusto/query/basketplugin)ve [`diffpatterns`](/azure/kusto/query/diffpatternsplugin). Tüm eklentiler kümeleme algoritmaları uygular. `autocluster` ve `basket` eklentileri kümesi tek bir kayıt kümesidir ve `diffpatterns` eklentisi iki kayıt kümesi arasındaki farkları belirler.

## <a name="clustering-a-single-record-set"></a>Tek bir kayıt kümesi kümeleme

Yaygın bir senaryo, anormal davranış, yüksek sıcaklık cihazı okumaları, uzun süre komutları ve en iyi harcama kullanıcıları gösteren zaman penceresi gibi belirli bir ölçüte göre seçilen bir veri kümesini içerir. Verilerde ortak desenleri (segmentleri) bulmanın kolay ve hızlı bir yolunu sunuyoruz. Desenler, kayıtları birden çok boyut (kategorik sütun) üzerinde aynı değerleri paylaşan veri kümesinin bir alt kümesidir. Aşağıdaki sorgu, on dakikalık sepetlerindeki bir hafta içinde hizmet özel durumlarının bir zaman serisini oluşturur ve gösterir:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5XPsaoCQQyF4d6nCFa7oHCtZd9B0F6G8ajByWTJZHS5+PDOgpVgYRn485EkOAnno9NAriWGFKw7QfQYUy0O43zZ0JNKFQnG/5jrbmeIXHBgwd6DjH2/JVqk2QrTL1aYvlifa4tni29YlzaiUK4yRK3Zu54006dBZ1N5/+X6PqpRI23+pFGGfIKRtz5egzk92K+dsycMyz3szhGEKWJ01lxI760O9ABuq0bMcvV2hqFoqnOz7F9BdSHlSgEAAA==) **\]**

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m
| render timechart with(title="Service exceptions over a week, 10 minutes resolution")
```

![Hizmet özel durumları timechart](media/machine-learning-clustering/service-exceptions-timechart.png)

Hizmet özel durum sayısı genel hizmet trafiğiyle ilişkili. Pazartesi 'Den Cuma 'ya kadar süren iş günleri için günlük deseninin yanı sıra, hizmet özel durum sayısını orta günde bir artış ve gece sayısı düşceye sahip olabilirsiniz. Düz düşük sayımlar, hafta sonu üzerinde görünür. Azure Veri Gezgini [zaman serisi anomali algılama](/azure/data-explorer/anomaly-detection?#time-series-anomaly-detection) kullanılarak özel durum artışları algılanabilir.

Verilerdeki ikinci ani artış, öğleden sonra Salı günü oluşur. Bu ani işlemi tanılamak için aşağıdaki sorgu kullanılır. Bir keskin ani artış olup olmadığını doğrulamak ve kenarlıklarını görüntülemek için daha yüksek çözünürlükte (bir dakikalık sepette sekiz saat) bir grafiği yeniden çizmek için sorguyu kullanın.

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAyXNwQrCMBAE0Hu/YvHUooWkghSl/yDoyUsJyWpCk2xJNnjx403pbeYwbzwyBBdnnoxiZBewHYS89GLshzNIeRWiuzUGA83al8yYXPzI5gdBLdjnWjFDLGHSVCK3HVCEe0LtMj4r9mAVVngnCvsLMO3hOFqo2goyVCxhNJhgu9dWJYavY9uyY4/T4UV1XVm2CEM0kFe34AnkBhXGOs7kCzuKh+4P3/XM5M8AAAA=) **\]**

```kusto
let min_t=datetime(2016-08-23 11:00);
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to min_t+8h step 1m
| render timechart with(title="Zoom on the 2nd spike, 1 minute resolution")
```

![Ani süre grafiğe odaklanma](media/machine-learning-clustering/focus-spike-timechart.png)

15:00 ile 15:02 arasındaki dar iki dakikalık bir ani artış görüyoruz. Aşağıdaki sorguda, bu iki dakikalık penceredeki özel durumları say:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA8tJLVHIzcyLL0hNzI4vsU1JLEktycxN1TAyMDTTNbDQNTJWMDS1MjDQtObKASlNrCCk1AioNCU1Nz8+Oae0uCS1KDMv3ZCrRqE8I7UoVSGgKDU5szg1BKgvuCQxt0AhKbWkPDU1TwPhBj09hCWaQI3J+aV5JQACnQoRpwAAAA==) **\]**

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| count
```

|Sayı |
|---------|
|972    |

Aşağıdaki sorguda örnek 20 özel durum 972:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA4XOsQrCMBSF4b1Pccd2aLmJKKL4DoLu4doeNDSJJb1SBx/eOHV0/37OCVCKPrkJMjo9DaJQH1FbNruW963dkNkemJtjFX5U3v+oLXRAfLo+vGZF9uluqg8tD2TQOaP3M66lu6jEiW7QBUj1+qHr1pGmhCojyPIX7QHvzakAAAA=) **\]**

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| take 20
```

| Ön IO zaman damgası            | Bölge | ScaleUnit | DeploymentId                     | İzleme noktası | ServiceHost                          |
|-----------------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 2016-08-23 15:00:08.7302460 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:09.9496584 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 8d257dav1-7a1c-44F5-9ACD-f9e02ff507fd |
| 2016-08-23 15:00:10.5911748 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:12.2957912 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | f855fcef-ebfe-405d-aaf8-9c5e2e43d862 |
| 2016-08-23 15:00:18.5955357 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 9d390e07-417d-42eb-bebd-793965189a28 |
| 2016-08-23 15:00:20.7444854 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 6e54c1c8-42D3-4e4e-8b79-9bb076ca71f1 |
| 2016-08-23 15:00:23.8694999 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 36109      | 19422243-19b9-4d85-9ca6-bc961861d287 |
| 2016-08-23 15:00:26.4271786 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 36109      | 3271bae4-1c5b-4f73-98ef-cc117e9be914 |
| 2016-08-23 15:00:27.8958124 | scUs   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 904498     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:32.9884969 | scUs   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007007   | d5c7c825-9d46-4ab7-a0c1-8e2ac1d83ddb |
| 2016-08-23 15:00:34.5061623 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:37.4490273 | scUs   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | f2ee8254-173c-477d-a1de-4902150ea50d |
| 2016-08-23 15:00:41.2431223 | scUs   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 103200     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:47.2983975 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 423690590  | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:50.5932834 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 2a41b552-aa19-4987-8CDD-410a3af016ac |
| 2016-08-23 15:00:50.8259021 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 0d56b8e3-470d-4213-91da-97405f8d005e |
| 2016-08-23 15:00:53.2490731 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 36109      | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:57.0000946 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 64038      | cb55739e-4afe-46a3-970f-1b49d8ee7564 |
| 2016-08-23 15:00:58.2222707 | scUs   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | 8215dcf6-2de0-42bd-9c90-181c70486c9c |
| 2016-08-23 15:00:59.9382620 | scUs   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | 451e3c4c-0808-4566-A64D-84d85cf30978 |

### <a name="use-autocluster-for-single-record-set-clustering"></a>Tek kayıt kümesi Kümelemesi için oto Cluster () kullanın

Bin ' den az özel durum olsa bile, her sütunda birden çok değer olduğundan ortak kesimleri bulmak hala zordur. Ortak parçaların küçük bir listesini anında ayıklamak için [`autocluster()`](/azure/kusto/query/autoclusterplugin) eklentisini kullanabilir ve aşağıdaki sorguda görüldüğü gibi ani bir şekilde iki dakika içinde ilgi çekici kümeler bulabilirsiniz:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA4WOsQrCMBRF937FG5OhJYkoovQfBN1DbC8aTNqSvlgHP94IQkf3c+65AUzRD3aCe1hue8dgHyGM0rta7WuzIb09KCWPVfii7vUPNQXtEUfbhTwzkh9uunrTckcCnRI6P+NSvDO7ONEVvACDWD80zRqRRcTThVxa5DKPv00hP81KL1+4AAAA) **\]**

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate autocluster()
```

| SegmentID | Sayı | Yüzde | Bölge | ScaleUnit | DeploymentId | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 94 | 9.67078189300411 | scUs | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |
| 2 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 3 | 68 | 6.99588477366255 | scUs | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |
| 4 | 55 | 5.65843621399177 | WEU | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |

Yukarıdaki sonuçlardan, en baskın segmentin toplam özel durum kayıtlarının% 65,74 ' unu içerdiğini ve dört boyutun paylaşılabileceği hakkında bilgi alabilirsiniz. Sonraki segment çok daha az yaygındır, kayıtların yalnızca% 9,67 ' ünü içerir ve üç boyutu paylaşır. Diğer segmentler de daha az yaygındır. 

Oto kümesi, birden çok boyutu İnceleme ve ilginç segmentleri ayıklama için özel bir algoritma kullanır. "İlginç", her segmentin hem kayıt kümesi hem de özellikler kümesi için önemli kapsama sahip olduğu anlamına gelir. Segmentler Ayrıca, her birinin diğerlerinden önemli ölçüde farklı olduğu anlamına gelir. Bu segmentlerin bir veya daha fazlası RCA işlemi için uygun olabilir. Segment incelemesini ve değerlendirmesini en aza indirmek için, oto kümesi yalnızca küçük bir segment listesini ayıklar.

### <a name="use-basket-for-single-record-set-clustering"></a>Tek kayıt kümesi Kümelemesi için sepet () kullanın

Aşağıdaki sorguda görüldüğü gibi [`basket()`](/azure/kusto/query/basketplugin) eklentisini de kullanabilirsiniz:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA4WOsQ6CMBgGd57iH9sB0tZojMZ3MNG9KfBFG1og7Y84+PDWidH9LncBTNGPdoYbLF96x2AfIYzSh1oda7MjvT8pJc9V+KHu/Q81Be0RJ9uFJTOSHx+6+tD6RAJdEzqfcS/ejV2cqQWvwCi2h6bZIrKIeLmwlBa1Lg9gIb9KJv2TswAAAA==) **\]**

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate basket()
```

| SegmentID | Sayı | Yüzde | Bölge | ScaleUnit | DeploymentId | İzleme noktası | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 642 | 66.0493827160494 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |  |
| 2 | 324 | 33.3333333333333 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 0 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 3 | 315 | 32.4074074074074 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 16108 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 4 | 328 | 33.7448559670782 |  |  |  | 0 |  |
| 5 | 94 | 9.67078189300411 | scUs | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |  |
| 6 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |  |
| 7 | 68 | 6.99588477366255 | scUs | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |  |
| 8 | 167 | 17.1810699588477 | scUs |  |  |  |  |
| 9 | 55 | 5.65843621399177 | WEU | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |  |
| 10 | 92 | 9.46502057613169 |  |  |  | 10007007 |  |
| 11 | 90 | 9.25925925925926 |  |  |  | 10007006 |  |
| 12 | 57 | 5.8641975308642 |  |  |  |  | 00000000-0000-0000-0000-000000000000 |

Sepet, öğe kümesi madenciliği için apriori algoritmasını uygular ve kayıt kümesi kapsamı eşiğin üstünde olan tüm kesimleri ayıklar (varsayılan 5%). Benzer parçalarla daha fazla segment ayıklandığını görebilirsiniz (örneğin, segmentler 0, 1 veya 2, 3).

Her iki eklenti de güçlü ve kullanımı kolaydır, ancak önemli sınırlamaları, tek bir kayıt kümesinin denetimsiz bir şekilde (etiket olmadan) kümelemeleridir. Bu nedenle, ayıklanan desenlerin seçili kayıt kümesini (anormal kayıtları) veya genel kayıt kümesini niteleyen belirsiz olup olmadığı anlaşılır değildir.

## <a name="clustering-the-difference-between-two-records-sets"></a>İki kayıt kümesi arasındaki farkı kümeleme

[`diffpatterns()`](/azure/kusto/query/diffpatternsplugin) eklentisi, `autocluster` ve `basket`olan sınırlamanın üzerine gelir. `Diffpatterns` iki kayıt kümesi alır ve bunlar arasında farklı olan ana kesimleri ayıklar. Bir küme genellikle araştırılmakta olan anormal kayıt kümesini içerir (bir tane `autocluster` ve `basket`ile çözümlenir). Diğer küme, başvuru kayıt kümesini (taban çizgisi) içerir. 

Aşağıdaki sorguda, ani kümelerdeki kümelerden farklı olan, ani olarak oluşan iki dakika içinde ilginç kümeler bulmak için `diffpatterns` kullanırız. Ana hat penceresini 15:00 ' dan (ani başladığında) sekiz dakika önce tanımladık. Ayrıca, belirli bir kaydın temele veya anormal bir kümeye ait olup olmadığını belirten bir ikili sütuna (AB) göre Genişlememiz gerekir. `Diffpatterns` denetimli bir öğrenme algoritması uygular, burada iki sınıf etiketi, ana hat (AB) ile anormal bayrak tarafından oluşturulmuştur.

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA42QzU+DQBDF7/wVcwOi5UtrmhJM4OzBRO9kWqbtpssuYacfGv94t0CrxFTd02by5jfvPUkMtVBlQ7gtOauQiUVNXhLFD5NoNknuIJ7Oo8hPHXmS4vEvaXKWWuoCDUmh6Jr8fj79Tv6HfOanEIbwRLgnQFhjAwviA5EC3hCcCYCq6gamEVsC1oB7LfoRt6iMYKEVvGtFQXfeNFKc7mXe2MjNVzl+mARR6lRU63Ipd4apFWodOx9w2FBL4D23tBSGXi3mhbG+OPPGVQTB+ITvg24dGN7vlN5JTxhc+dYAHZls4LzIxGr1k/B4iXcLbq50jfLNtd9i8OB2jD3KnW0dKstokG08Zby8uLbyCfX/tG46AgAA) **\]**

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
let min_baseline_t=datetime(2016-08-23 14:50);
let max_baseline_t=datetime(2016-08-23 14:58); // Leave a gap between the baseline and the spike to avoid the transition zone.
let splitime=(max_baseline_t+min_peak_t)/2.0;
demo_clustering1
| where (PreciseTimeStamp between(min_baseline_t..max_baseline_t)) or
        (PreciseTimeStamp between(min_peak_t..max_peak_t))
| extend AB=iff(PreciseTimeStamp > splitime, 'Anomaly', 'Baseline')
| evaluate diffpatterns(AB, 'Anomaly', 'Baseline')
```

| SegmentID | CountA | CountB | Yüzta | Yüztb | Percentdıssb | Bölge | ScaleUnit | DeploymentId | İzleme noktası |
|-----------|--------|--------|----------|----------|---------------|--------|-----------|----------------------------------|------------|
| 0 | 639 | 21 | 65,74 | 1,7 | 64,04 | Eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |
| 1 | 167 | 544 | 17,18 | 44,16 | 26,97 | scUs |  |  |  |
| 2 | 92 | 356 | 9,47 | 28,9 | 19,43 |  |  |  | 10007007 |
| 3 | 90 | 336 | 9,26 | 27,27 | 18,01 |  |  |  | 10007006 |
| 4 | 82 | 318 | 8,44 | 25,81 | 17,38 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 5 | 55 | 252 | 5,66 | 20,45 | 14,8 | WEU | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |
| 6 | 57 | 204 | 5,86 | 16,56 | 10,69 |  |  |  |  |

En baskın segment, `autocluster`tarafından ayıklanmış olan kesimdir, iki dakikalık anormal penceredeki kapsamı da% 65,74 ' dir. Ancak sekiz dakikalık taban çizgisi penceresindeki kapsamı yalnızca% 1,7 ' dir. Fark% 64,04 ' dir. Bu fark, anormal ani artış ile ilgili gibi görünüyor. Bu varsayımını, özgün grafiği aşağıdaki sorguda görüldüğü gibi, bu sorunlu kesime ait kayıtlara bölerek bu soruna karşı doğrulayabilirsiniz:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WRsWrDMBCG9zzF4cmGGuJUjh2Ktw7tUkLTzuEsnRNRnRQkuSQlD185yRTo0EWIO913/J8MRWBttxE6iC5INOhzRey20owhktd2V8EZwsiMXv/Q9Dpfe5I60Idm2kTkQ1E8AczMxMLjf1h4/IN1PzY7Ax0jWQWBdomvhyF/p512FroOMsIxA0zdTdpKn1bHSzmMzbX8TAfjTkw2vqpLp69VpYQaatEogXOBsqrbtl5WDake6yabXWjkv7WkFxeuPGqG5VzWqhQrIUqx6B/L1WKB6aBViy01imT2ANnau94QT9c35xlNVqQAjF9UhpSHAtiRO+lGG/MCUoZ7CTB4x7ePie5mNbk4QDVn6E+ThUT0SQh5iGlM7tHHX4WFgLHOAQAA) **\]**

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| extend seg = iff(Region == "eau" and ScaleUnit == "su7" and DeploymentId == "b5d1d4df547d4a04ac15885617edba57"
and ServiceHost == "e7f60c5d-4944-42b3-922a-92e98a8e7dec", "Problem", "Normal")
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m by seg
| render timechart
```

![' Diffmodel ' segmenti zaman çizelgesi doğrulanıyor](media/machine-learning-clustering/validating-diffpattern-timechart.png)

Bu grafik, `diffpatterns` eklentisi kullanılarak keşfedilen, Salı günü Salı günü, bu belirli kesimden alınan özel durumlar nedeniyle görmemize olanak sağlar.

## <a name="summary"></a>Özet

Azure Veri Gezgini Machine Learning eklentileri birçok senaryo için yararlıdır. `autocluster` ve `basket`, denetimli öğrenme algoritmasını uygular ve kullanımı kolaydır. `Diffpatterns` denetimli öğrenme algoritmasını uygular ve daha karmaşık olsa da, RCA için fark eden kesimleri ayıklamada daha güçlüdür.

Bu eklentiler, geçici senaryolarda etkileşimli olarak ve otomatik olarak neredeyse gerçek zamanlı izleme hizmetlerinde kullanılır. Azure Veri Gezgini, zaman serisi anomali algılamanın ardından, gerekli performans standartlarını karşılamak üzere en iyi duruma getirilmiş bir tanılama işlemi gelir.
