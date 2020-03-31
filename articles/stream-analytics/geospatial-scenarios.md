---
title: Azure Akış Analitiği ile geofencing ve jeouzamsal toplama
description: Bu makalede, geofencing ve jeouzamsal toplama için Azure Akış Analizi'nin nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 5a3aa3786469c3df37b53cb82bdd396871689297
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443634"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Azure Akış Analitiği ile geofencing ve jeouzamsal toplama senaryoları

Yerleşik coğrafi işlevlerle, filo yönetimi, sürüş paylaşımı, bağlı arabalar ve varlık izleme gibi senaryolar için uygulamalar oluşturmak için Azure Akış Analizi'ni kullanabilirsiniz.

## <a name="geofencing"></a>Geofencing

Azure Akış Analizi, bulutta ve IoT Edge çalışma zamanında düşük gecikme süresi gerçek zamanlı geofencing hesaplamalarını destekler.

### <a name="geofencing-scenario"></a>Geofencing senaryo

Bir üretim şirketinin binalarındaki varlıkları izlemesi gerekir. Her cihazı GPS ile donatettiler ve bir cihaz belirli bir alanı terk ederse bildirim almak istiyorlar.

Bu örnekte kullanılan referans verileri, binaların ve binaların her birinde izin verilen aygıtların geofence bilgilerini kullanır. Başvuru verilerinin statik veya yavaş değişebileceğini unutmayın. Statik başvuru verileri bu senaryo için kullanılır. Bir veri akışı sürekli olarak aygıt kimliğini ve geçerli konumunu yayır.

### <a name="define-geofences-in-reference-data"></a>Referans verilerdeki geofences tanımlama

GeoJSON nesnesi kullanılarak geofence tanımlanabilir. Uyumluluk sürüm 1.2 ve üzeri olan işleriçin, geofences de Bilinen Metin (WKT) olarak `NVARCHAR(MAX)`tanımlanabilir. WKT, uzamsal verileri metin biçiminde temsil etmek için kullanılan bir Açık Jeouzamsal Konsorsiyum (OGC) standardıdır.

Yerleşik jeouzamsal işlevler, bir elementin belirli bir jeoçit çokgenin içinde veya dışında olup olmadığını öğrenmek için tanımlanmış geofences'i kullanabilir.

Aşağıdaki tablo, Azure blob depolama veya Azure SQL tablosunda depolanabilecek coğrafi çit başvuru verilerine bir örnektir. Her site bir jeouzamsal çokgen tarafından temsil edilir ve her cihaz izin verilen bir site kimliği ile ilişkilidir.

|Siteıd|SiteAdı|Geofence|İzin VerilenDeviceID|
|------|--------|--------|---------------|
|1|"Redmond Binası 41"|"POLYGON(-122.1337357922017 47,63782998329432,-122,13373042778369 47,637634793257305,-122,122,133467577130022476662626262662662662-122.13346757713022466632626626626626626626666666,-122,122,13346757771302224667713022666,67767571302266666,-122.122.13346757771302247998329322666,-122.1334675777130247663262626262662-122.133467577713 2022530954,-122.13348902897235 47,6375080808066,-122.133617777500506 47,6375082806806,-122.1336124108780880880808080808 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432))"|"B"|
|2|"Redmond Binası 40"|"POLYGON(-122.1336154507967 47,636745947009,-122.13361008637867 47,636483015064555,-122.133492069918201 4776666994709,-122.1366100866667 9400347675,-122.1334974336004 47,636363729275773,-122.13372810357532 47,636363729753,-122,13373346799355 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009))"|"A"|
|3|"Redmond Binası 22"|"POLYGON((-122.13611660248233 47,63758544698554,-122.13635263687564 47,6374083293,-122.1362238908443434363,673,67352636363 3603619712,-122.136223890844293 47,637176999101473,-122,13581616195072664,636927766,-122,122,1355962539344444444444444444444444444444444444444444444444444444444444444 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554))"|"C"|

### <a name="generate-alerts-with-geofence"></a>Geofence ile uyarılar oluşturma

Aygıtlar, kimlikleri ve konumlarını her dakika `DeviceStreamInput`. Aşağıdaki tablo bir giriş akışıdır.

|Deviceıd|Geoposition|
|--------|-----------|
|"A"|"POINT(-122.13292341559497 47,636318374032726)"|
|"B"|"POINT(-122.1333847555553 47,63743531308874)"|
|"C"|"POINT(-122.13354001095752 47,63627622505007)"|

Coğrafi çit başvuru verileriyle aygıt akışını birleştiren ve aygıt izin verilen bir binanın dışında her olduğunda bir uyarı oluşturan bir sorgu yazabilirsiniz.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

Aşağıdaki resim geofences temsil eder. Aygıtların akış veri girişine uygun olarak nerede olduğunu görebilirsiniz.

![Bina geofences](./media/geospatial-scenarios/building-geofences.png)

"C" aygıtı, başvuru verilerine göre izin verilmeyen bina kimliği 2'nin içinde yer alır. Bu cihaz bina Kimliği 3 içinde yer almalıdır. Bu işi çalıştırmak, bu özel ihlal için bir uyarı oluşturur.

### <a name="site-with-multiple-allowed-devices"></a>Birden çok izin verilen aygıta sahip site

Bir site birden çok cihaza izin veriyorsa, `AllowedDeviceID` bir dizi aygıt kimliği tanımlanabilir ve `WHERE` akış aygıtı kimliğinin bu listedeki herhangi bir aygıt kimliğiyle eşleşip eşleşmediğini doğrulamak için yan tümcede kullanıcı tanımlı bir işlev kullanılabilir. Daha fazla bilgi için, bulut işleri için [Javascript UDF](stream-analytics-javascript-user-defined-functions.md) öğreticisini ve kenar işleri için [C# UDF](stream-analytics-edge-csharp-udf.md) öğreticisini görüntüleyin.

## <a name="geospatial-aggregation"></a>Jeouzamsal toplama

Azure Akış Analizi, bulutta ve IoT Edge çalışma zamanında düşük gecikme süresi gerçek zamanlı coğrafi toplamayı destekler.

### <a name="geospatial-aggregation-scenario"></a>Jeouzamsal toplama senaryosu

Bir taksi şirketi, şu anda daha yüksek talep le karşı karşıya olan şehirlerin alanlarına doğru binmek isteyen taksi şoförlerine rehberlik etmek için gerçek zamanlı bir uygulama oluşturmak istiyor.

Şirket, referans verisi olarak şehrin mantıksal bölgelerini depolar. Her bölge regionID, RegionName ve Geofence tarafından tanımlanır.

### <a name="define-the-geofences"></a>Geofences tanımlayın

Aşağıdaki tablo, Azure blob depolama veya Azure SQL tablosunda depolanabilecek coğrafi çit başvuru verilerine bir örnektir. Her bölge, veri akışından gelen isteklerle ilişkilendirmek için kullanılan bir jeouzamsal çokgen tarafından temsil edilir.

Bu çokgenler sadece referans içindir ve gerçek şehir mantıksal veya fiziksel ayrımları temsil etmez.

|RegionID|Regionname|Geofence|
|--------|----------|--------|
|1|"SoHo"|"POLYGON((-74.00279525078275 40,72833625216264,-74.005474597 9765 40,721929158663244,-74,00125029839018 40,71893680218 -73,73,9957785919999998 40,72521409075776,-73,997237137039 40.72557184584898,-74.00279525078275 40,72833625216264) )"|
|2|"Çin Mahallesi"|"POLYGON(-73.99712367114876 40,712815822671333,-73.9901070123656881907936,-73.99023575839851 40,71288822672267222612001265200120012665 359088633,-73.98976368961189 40,71554823078944,-73,99551434573982 40,717337246783735,-73,994806242559999 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166,-73.99748845157478 40.71405192594819,-73.99712367114876 40.71281582267133))"|
|3|"Tribeca"|"POLYGON((-74.01091641815208 40,72583120006777,-74.013384050444578 40,71436586362705,-74.0137059155277740,7158312085740,71581208 177702123415,-74.00862044723533 40,711308107057235,-74.0019471120628 40,7194238654018,-74.01091641818152088 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Zaman penceresinde verileri toplama

Aşağıdaki tabloda "sürmek" akış verileri yer alıyor.

|UserID|Konumdan Gelen|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|"A"|"PUAN(-74.00726861389182 40,71610611981975)"|"POINT(-73.986150959917779 40,703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"NOKTA(-74.00249841021645 40,723827238895666)"|"NOKTA(-74.01160699942085 40,71378884930115)"|"2019-03-12T07:01:00Z"|
|"C"|"POINT(-73.99680120565864 40,716439898624024)"|"POINT(-73.98289663412544 40,72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"PUAN(-74.00741090068288 40,71615626755086)"|"POINT(-73.97999843120539 40,73477895807408)"|"2019-03-12T07:03:00Z"|

Aşağıdaki sorgu, coğrafi çit başvuru verileriyle aygıt akışına katılır ve her dakika 15 dakikalık bir zaman penceresinde bölge başına istek sayısını hesaplar.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Bu sorgu, şehirdeki her bölge tarafından son 15 dakika için her dakika istek sayısı çıkar. Bu bilgiler Power BI panosu tarafından kolayca görüntülenebilir veya Azure işlevleri gibi hizmetlerle tümleştirme yoluyla SMS metin mesajı olarak tüm sürücülere yayınlanabilir.

Aşağıdaki resimde, Power BI panosuna sorguçıktısı gösteriş. 

![Power BI panosunda sonuç çıktısı](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Akış Analizi jeouzamsal fonksiyonlara giriş](stream-analytics-geospatial-functions.md)
* [Coğrafi Fonksiyonlar (Azure Akışı Analitiği)](https://docs.microsoft.com/stream-analytics-query/geospatial-functions)
