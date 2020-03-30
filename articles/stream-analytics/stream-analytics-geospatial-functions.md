---
title: Azure Akışı Analizi coğrafi işlevlerine giriş
description: Bu makalede, Azure Akış Analizi işlerinde kullanılan coğrafi işlevler açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: f47f34b60c858bb9a0feafd25176e4a811046630
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426236"
---
# <a name="introduction-to-stream-analytics-geospatial-functions"></a>Akış Analizi jeouzamsal fonksiyonlara giriş

Azure Akış Analitiği'ndeki coğrafi işlevler, coğrafi verilerin akışı nda gerçek zamanlı analizsağlar. Sadece birkaç kod satırıyla, karmaşık senaryolar için üretim sınıfı çözümü geliştirebilirsiniz. 

Jeouzamsal işlevlerden yararlanabilecek senaryolara örnek olarak şunlar verilebilir:

* Sürüş paylaşımı
* Filo yönetimi
* Varlık izleme
* Geo-eskrim
* Hücre siteleri arasında telefon takibi

Akış Analitik Sorgu Dili yedi yerleşik jeouzamsal işlevleri vardır: **CreateLineString**, **CreatePoint**, **CreatePolygon**, **ST_DISTANCE**, **ST_OVERLAPS**, **ST_INTERSECTS**, ve **ST_WITHIN**.

## <a name="createlinestring"></a>CreateLineString

İşlev `CreateLineString` noktaları kabul eder ve harita üzerinde bir çizgi olarak çizilebilen bir GeoJSON LineString döndürür. LineString oluşturmak için en az iki noktanız olmalıdır. LineString noktaları sırayla bağlanır.

Aşağıdaki sorgu, `CreateLineString` üç nokta kullanarak bir LineString oluşturmak için kullanır. İlk nokta giriş verilerinin akışından oluşturulurken, diğer ikisi el ile oluşturulur.

```SQL 
SELECT  
     CreateLineString(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5))  
FROM input  
```  

### <a name="input-example"></a>Giriş örneği  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıktı örneği  

 {"type" : "LineString", "koordinatlar" : [ [-10.2, 3.0], [10.0, 10.0], [10.5, 10.5] ]}

 {"type" : "LineString", "koordinatlar" : [ [20.2321, -87.33], [10.0, 10.0], [10.5, 10.5] ]}

Daha fazla bilgi edinmek için CreateLineString başvuruadresini ziyaret [edin.](https://docs.microsoft.com/stream-analytics-query/createlinestring)

## <a name="createpoint"></a>CreatePoint

İşlev `CreatePoint` enlem ve boylam kabul eder ve harita üzerinde çizilebilen bir GeoJSON noktası döndürür. Enlemleriniz ve boylamlarınız bir **float** veri tipi olmalıdır.

Aşağıdaki örnek sorgu, giriş verilerinin akışından enlemleri ve boylamları kullanarak bir nokta oluşturmak için kullanılır. `CreatePoint`

```SQL 
SELECT  
     CreatePoint(input.latitude, input.longitude)  
FROM input 
```  

### <a name="input-example"></a>Giriş örneği  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıktı örneği
  
 {"type" : "Nokta", "koordinatlar" : [-10.2, 3.0]}  
  
 {"type" : "Nokta", "koordinatlar" : [20.2321, -87.33]}  

Daha fazla bilgi edinmek için CreatePoint başvuruadresini ziyaret [edin.](https://docs.microsoft.com/stream-analytics-query/createpoint)

## <a name="createpolygon"></a>CreatePolygon

İşlev `CreatePolygon` noktaları kabul eder ve bir GeoJSON çokgen kaydı döndürür. Noktaların sırası sağ halka yönünü veya saat yönünü nisacını izlemelidir. Bir noktadan diğerine, ilan edildikleri sırayla yürüdüğünü hayal et. Çokgenin merkezi tüm zaman boyunca solunda olurdu.

Aşağıdaki örnek sorgu `CreatePolygon` üç noktadan bir çokgen oluşturmak için kullanır. İlk iki nokta el ile oluşturulur ve son nokta giriş verilerinden oluşturulur.

```SQL 
SELECT  
     CreatePolygon(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5), CreatePoint(input.latitude, input.longitude))  
FROM input  
```  

### <a name="input-example"></a>Giriş örneği  
  
|Enlem|Boylam|  
|--------------|---------------|  
|3,0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Çıktı örneği  

 {"type" : "Çokgen", "koordinatlar" : [[ [[-10.2, 3.0], [10.0, 10.0], [10.5, 10.5], [-10.2, 3.0] ]]]
 
 {"type" : "Çokgen", "koordinatlar" : [[ [20.2321, -87.33], [10.0, 10.0], [10.5, 10.5], [20.2321, -87.33] ]]

Daha fazla bilgi için [CreatePolygon](https://docs.microsoft.com/stream-analytics-query/createpolygon) referansını ziyaret edin.


## <a name="st_distance"></a>ST_DISTANCE
Fonksiyon `ST_DISTANCE` metre iki nokta arasındaki mesafeyi döndürür. 

Aşağıdaki sorgu, `ST_DISTANCE` bir benzin istasyonu araçtan 10 km'den daha az olduğunda bir olay oluşturmak için kullanır.

```SQL
SELECT Cars.Location, Station.Location 
FROM Cars c  
JOIN Station s ON ST_DISTANCE(c.Location, s.Location) < 10 * 1000
```

Daha fazla bilgi için [ST_DISTANCE](https://docs.microsoft.com/stream-analytics-query/st-distance) başvuruyu ziyaret edin.

## <a name="st_overlaps"></a>ST_OVERLAPS
İşlev `ST_OVERLAPS` iki çokgenkarşılaştırır. Çokgenler çakışıyorsa, işlev 1 döndürür. Çokgenler çakışmazsa işlev 0 döndürür. 

Aşağıdaki sorgu, `ST_OVERLAPS` bir bina olası bir sel bölgesi içindeyken olay oluşturmak için kullanır.

```SQL
SELECT Building.Polygon, Building.Polygon 
FROM Building b 
JOIN Flooding f ON ST_OVERLAPS(b.Polygon, b.Polygon) 
```

Aşağıdaki örnek sorgu, bir fırtına bir arabaya doğru ilerlerken bir olay oluşturur.

```SQL
SELECT Cars.Location, Storm.Course
FROM Cars c, Storm s
JOIN Storm s ON ST_OVERLAPS(c.Location, s.Course)
```

Daha fazla bilgi için [ST_OVERLAPS](https://docs.microsoft.com/stream-analytics-query/st-overlaps) başvuruyu ziyaret edin.

## <a name="st_intersects"></a>ST_INTERSECTS
İşlev `ST_INTERSECTS` iki LineString karşılaştırır. LineString kesişiyorsa, işlev 1 döndürür. LineString kesişmezse işlev 0 döndürür.

Aşağıdaki örnek sorgu, döşeli bir yolun toprak bir yolla kesip kesişmeyiştiğini belirlemek için kullanır. `ST_INTERSECTS`

```SQL 
SELECT  
     ST_INTERSECTS(input.pavedRoad, input.dirtRoad)  
FROM input  
```  

### <a name="input-example"></a>Giriş örneği  
  
|datacenterAlan|stormArea|  
|--------------------|---------------|  
|{"type":"LineString", "koordinatlar": [ [-10.0, 0.0], [0.0, 0.0], [10.0, 0.0] ]}|{"type":"LineString", "koordinatlar": [ [0.0, 10.0], [0.0, 0.0], [0.0, -10.0] ]}|  
|{"type":"LineString", "koordinatlar": [ [-10.0, 0.0], [0.0, 0.0], [10.0, 0.0] ]}|{"type":"LineString", "koordinatlar": [ [-10.0, 10.0], [0.0, 10.0], [10.0, 10.0] ]}|  
  
### <a name="output-example"></a>Çıktı örneği  

 1  
  
 0  

Daha fazla bilgi için [ST_INTERSECTS](https://docs.microsoft.com/stream-analytics-query/st-intersects) başvuruyu ziyaret edin.

## <a name="st_within"></a>ST_WITHIN
İşlev, `ST_WITHIN` bir noktanın veya çokgenin çokgen içinde olup olmadığını belirler. Çokgen nokta veya çokgen içeriyorsa, işlev 1 döndürecektir. Nokta veya çokgen bildirilen çokgeniçinde bulunmazsa işlev 0 döndürecektir.

Aşağıdaki örnek sorgu, teslimat hedef noktasının verilen ambar çokgenin içinde olup olmadığını belirlemek için kullanır. `ST_WITHIN`

```SQL 
SELECT  
     ST_WITHIN(input.deliveryDestination, input.warehouse)  
FROM input 
```  

### <a name="input-example"></a>Giriş örneği  
  
|deliveryDestination|Ambar|  
|-------------------------|---------------|  
|{"type":"Point", "koordinatlar": [76.6, 10.1]}|{"type":"Polygon", "koordinatlar": [ [0.0, 0.0], [10.0, 0.0], [10.0, 10.0], [0.0, 10.0], [0.0, 0.0] ]}|  
|{"type":"Point", "koordinatlar": [15.0, 15.0]}|{"type":"Polygon", "koordinatlar": [ [10.0, 10.0], [20.0, 10.0], [20.0, 20.0], [10.0, 20.0], [10.0, 10.0] ]}|  
  
### <a name="output-example"></a>Çıktı örneği  

 0  
  
 1  

Daha fazla bilgi edinmek için [ST_WITHIN](https://docs.microsoft.com/stream-analytics-query/st-within) başvuruyu ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
