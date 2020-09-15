---
title: Microsoft Azure Eşlemlerde Azure Maps Yönlendirme Hizmeti için en iyi yöntemler
description: Microsoft Azure haritalardan Yönlendirme Hizmeti kullanarak her bir yolu yönlendirmeyi öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/02/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 1c108c79cafb591dced6f6be0dd5c1b353ddac45
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90086411"
---
# <a name="best-practices-for-azure-maps-route-service"></a>Azure Maps Route hizmeti için en iyi yöntemler

Azure haritalar 'daki yol yönleri ve yol Matrisi API 'Leri [yönlendirme hizmeti](https://docs.microsoft.com/rest/api/maps/route) , istenen her bir rota için tahmini varış zamanlarını (Etas) hesaplamak üzere kullanılabilir. Yönlendirme API 'Leri, gerçek zamanlı trafik bilgileri ve geçmiş trafik verileri gibi faktörleri, haftanın istenen günü ve günün saati için tipik yol hızları gibi düşünün. API 'Ler, zaman veya mesafeden başlayarak, tek seferde birden çok hedefe veya en iyi duruma getirilmiş sıraya göre sunulan en kısa veya en hızlı yolları döndürür. Kullanıcılar ayrıca, waltlar, bıyıva ve kamyonlar gibi ticari araçlar için özel yollar ve Ayrıntılar isteyebilir. Bu makalede, Azure haritalar [yönlendirme hizmeti](https://docs.microsoft.com/rest/api/maps/route)çağırmak için en iyi yöntemleri paylaşacağız ve nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Yol yönleri API 'Leri ve matris yönlendirme API 'SI arasında seçim yapın
> * Gerçek zamanlı ve geçmiş trafik verilerine göre geçmiş ve tahmin edilen seyahat sürelerini isteyin
> * Yolun tamamı ve yolun her bir BAI için zaman ve uzaklık gibi yol ayrıntılarını iste
> * Kamyon gibi ticari bir araç için istek rotası
> * Trafik bilgilerini, sıkışıklıklarının ve ücretli bilgiler gibi bir yol üzerinde isteyin
> * Bir veya daha fazla durdurulduğunda (waypoints) oluşan bir rota isteyin
> * Her bir durdurmayı (Waypoint) ziyaret etmek için en iyi sırayı elde etmek üzere bir veya daha fazla durak yolunu en iyi hale getirin
> * Destekleyici noktaları kullanarak alternatif yolları iyileştirin. Örneğin, elektrik araç doldurma istasyonunu geçiren alternatif yollar sunun.
> * Azure Maps web SDK ile [yönlendirme hizmeti](https://docs.microsoft.com/rest/api/maps/route) kullanma

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).

Yönlendirme Hizmeti kapsamı hakkında daha fazla bilgi için bkz. [yönlendirme kapsamı](routing-coverage.md).

Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.postman.com/downloads/) kullanır, ancak HERHANGI bir API geliştirme ortamı seçebilirsiniz.

## <a name="choose-between-route-directions-and-matrix-routing"></a>Rota yönleri ve matris yönlendirme arasında seçim yapın

Yol yönleri API 'Leri, seyahat süresi ve bir yol yolunun koordinatları dahil olmak üzere yönergeler döndürür. Yol Matrisi API 'SI, kaynak ve hedef konumlar tarafından tanımlanan bir rota kümesi için seyahat süresini ve uzaklıkları hesaplamanızı sağlar. Her verilen kaynak için, matris API 'SI, bu kaynaktan verilen her hedefe yönlendirme maliyetini (seyahat süresi ve mesafe) hesaplar. Bu API 'lerin hepsi, istenen kalkış saati, varış zamanları ve araç türü gibi parametreleri belirtmenize olanak tanır. Bunlar, en iyi yolları döndürecek şekilde gerçek zamanlı veya tahmine dayalı trafik verileri kullanır.

Senaryonuz şu şekilde olursa yol yönleri API 'Lerini çağırmayı düşünün:

* Teslimat masanızla ilgili kesin alma süreleri almak için, iki veya daha fazla bilinen konum arasında en kısa veya en hızlı yol talep isteyin.
* Harita içindeki yolları görselleştirmek için yol geometrisi dahil ayrıntılı yol kılavuzu isteyin
* Müşteri konumlarından oluşan bir liste verildiğinde, her müşteri konumunu ziyaret edip kaynağa geri dönmek için olası en kısa yolu hesaplayın. Bu senaryo genellikle seyahat problemine sorunu olarak bilinir. Tek bir istekte en fazla 150 geçiş noktası (durduran) geçirebilirsiniz.
* Yalnızca tek bir API çağrısını kullanarak, toplu Iş API 'sine rota yönlerine toplu sorgular gönderin.

Senaryonuz şu şekilde olursa matris yönlendirme API 'sini çağırmayı düşünün:

* Bir çıkış ve hedef kümesi arasındaki seyahat süresini veya mesafeyi hesaplayın. Örneğin, 12 sürücünüz vardır ve restorana ait yiyecek teslimatını çekmek için kullanılabilir en yakın sürücüyü bulmanız gerekir.
* Olası rotaları gerçek seyahat mesafesini veya saatine göre sıralayın. Matris API 'SI, her bir kaynak ve hedef birleşimi için yalnızca seyahat sürelerini ve uzaklıkları döndürür.
* Seyahat zamanına veya uzaklıklara göre küme verileri. Örneğin, şirketinizde 50 çalışanı varsa, ofisinizden 20 dakikalık sürücü süresi içinde yaşayan tüm çalışanları bulabilirsiniz.

Yol yönlerinin ve matris API 'Lerinin bazı özelliklerini göstermek için bir karşılaştırma aşağıda verilmiştir:

| Azure Haritalar API 'SI | İstekteki en fazla sorgu sayısı | Alanlardan kaçının | Kamyonu ve elektrik araç yönlendirmesi | Waypoints ve seyahat Salesman iyileştirmesi | Destekleyici noktaları |
| :--------------: |  :--------------: |  :--------------: | :--------------: | :--------------: | :--------------: |
| Rota yönlerini al | 1 | | X | X | |
| Rota gönderme yönleri | 1 | X | X | X | X |
| Rota yönlendirme sonrası Batch | 700 | | X | X | |
| Rota sonrası matrisi | 700 | | X | | |

Elektrik araç yönlendirme özellikleri hakkında daha fazla bilgi edinmek için bkz. [Python ile Azure Notebooks kullanarak elektrik taşıtlara](tutorial-ev-routing.md)yönlendirme hakkında öğreticimize bakın.

## <a name="request-historic-and-real-time-data"></a>Geçmiş ve gerçek zamanlı veriler iste

Varsayılan olarak, rota hizmeti seyahat modunun bir otomobil olduğunu varsayar ve ayrılma zamanı şu anda olur. Yol hesaplama isteği aksini belirtmedikçe gerçek zamanlı trafik koşullarına bağlı olarak yol döndürür. ' Sol dönüşler arasında 4:00 PM ile 6:00 PM arasında izin verilmez ' gibi sabit zamana bağımlı trafik kısıtlamalarına göre yakalanacaktır ve yönlendirme altyapısı tarafından kabul edilir. Özel olarak geçerli canlı trafiği yok sayan bir yol istemediğiniz için, yol kapanışları gibi yol kapanışları göz önünde bulundurulacaktır. Geçerli trafiği yok saymak için `traffic` `false` API isteğiniz içinde olarak ayarlayın.

Route Calculation **Seyahattimeınseconds** değeri trafik nedeniyle gecikme süresini içerir. Bu, geçerli ve geçmiş seyahat saati verilerinden yararlanarak oluşturulur. bu zaman, ayrılma saati şimdi olarak ayarlanır. Ayrılma zamanı gelecekte ayarlandıysa, API 'Ler, geçmiş verilere göre tahmini seyahat süresi döndürür.

İsteğinize **computeTravelTimeFor = All** parametresini eklerseniz, yanıttaki Summary öğesi geçmiş trafik koşulları dahil olmak üzere aşağıdaki ek alanlara sahip olacaktır:

| Öğe | Açıklama|
| :--- | :--- |
| noTrafficTravelTimeInSeconds | Trafik koşullarından dolayı yol üzerinde gecikme yok olarak hesaplanan tahmini seyahat süresi (örneğin sıkışıklık nedeniyle) |
| historicTrafficTravelTimeInSeconds | Zamana bağlı geçmiş trafik verileri kullanılarak hesaplanan tahmini seyahat süresi |
| liveTrafficIncidentsTravelTimeInSeconds | Gerçek zamanlı hız verileri kullanılarak hesaplanan tahmini seyahat süresi |

Sonraki bölümlerde, bahsedilen parametreleri kullanarak yol API 'Lerine nasıl çağrı yapılacağı gösterilmektedir.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki ilk örnekte, yazma sırasında kalkış saatinin daha sonra bir şekilde ayarlanmış olması gerekir.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=51.368752,-0.118332:51.385426,-0.128929&travelMode=car&traffic=true&departAt=2025-03-29T08:00:20&computeTravelTimeFor=all
```

Yanıt, aşağıdaki gibi bir Özet öğesi içerir. Kalkış saati gelecek şekilde ayarlandığından, **trafficDelayInSeconds** değeri sıfırdır. **Seyahattimeınseconds** değeri zamana bağlı geçmiş trafik verileri kullanılarak hesaplanır. Bu nedenle, bu durumda, **Seyahattimeınseconds** değeri **historicTrafficTravelTimeInSeconds** değerine eşittir.

```json
"summary": {
    "lengthInMeters": 2131,
    "travelTimeInSeconds": 248,
    "trafficDelayInSeconds": 0,
    "departureTime": "2025-03-29T08:00:20Z",
    "arrivalTime": "2025-03-29T08:04:28Z",
    "noTrafficTravelTimeInSeconds": 225,
    "historicTrafficTravelTimeInSeconds": 248,
    "liveTrafficIncidentsTravelTimeInSeconds": 248
},
```

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki ikinci örnekte, gerçek zamanlı bir yönlendirme isteği sunuyoruz ve bu süre şu anda ayrıldık. Varsayılan değer olduğundan URL 'de açıkça belirtilmiyor.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=47.6422356,-122.1389797:47.6641142,-122.3011268&travelMode=car&traffic=true&computeTravelTimeFor=all
```

Yanıt aşağıda gösterildiği gibi bir Özet içerir. Congesler nedeniyle, **trafficDelaysInSeconds** değeri sıfırdan büyük. Ayrıca, **historicTrafficTravelTimeInSeconds**' den büyük.

```json
"summary": {
    "lengthInMeters": 16637, 
    "travelTimeInSeconds": 2905, 
    "trafficDelayInSeconds": 1604, 
    "departureTime": "2020-02-28T01:00:20+00:00",
    "arrivalTime": "2020-02-28T01:48:45+00:00", 
    "noTrafficTravelTimeInSeconds": 872, 
    "historicTrafficTravelTimeInSeconds": 1976, 
    "liveTrafficIncidentsTravelTimeInSeconds": 2905 
},
```

## <a name="request-route-and-leg-details"></a>İstek yolu ve bacak ayrıntıları

Varsayılan olarak, yönlendirme hizmeti bir koordinat dizisi döndürür. Yanıt, adlı bir listede yolu oluşturan koordinatları içerir `points` . Yol yanıtı, yolun başından ve tahmini geçen sürenin uzaklığını da içerir. Bu değerler, tüm yolun ortalama hızını hesaplamak için kullanılabilir.

Aşağıdaki görüntüde `points` öğesi gösterilmektedir.

![Points öğesi](media/how-to-use-best-practices-for-routing/points-list-is-hidden-img.png)

`point`Yolun koordinatlarının listesini görmek için öğesini genişletin:

![Genişletilmiş noktaları öğesi](media/how-to-use-best-practices-for-routing/points-list-img.png)

Yol yönleri API 'Leri, **Komutctionstype** parametresini belirterek kullanılabilecek farklı yönergelerin biçimlerini destekler. Kolay bilgisayar işlemeye yönelik yönergeleri biçimlendirmek için, **Komutctionstype = Coded**kullanın. Yönergeleri kullanıcı için metin olarak göstermek için **Komutctionstype = etiketli** ' i kullanın. Ayrıca, yönergelerin bazı öğelerinin işaretlendiği ve yönergedeki özel biçimlendirmeyle birlikte sunulan yönergeler metin olarak biçimlendirilebilir. Daha fazla bilgi için [desteklenen yönerge türleri listesine](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routeinstructionstype)bakın.

Yönergeler istendiğinde, yanıt adlı yeni bir öğesi döndürür `guidance` . `guidance`Öğesi iki bilgi parçasını tutar: açma yönü ve özetlenen yönergeler.

![Yönerge türü](media/how-to-use-best-practices-for-routing/instructions-type-img.png)

`instructions`Öğesi seyahat için geri dönüş yönlerini tutar ve `instructionGroups` özetlenmiş yönergeleri vardır. Her yönerge Özeti, birden çok yol kapsayan seyahat segmentini kapsar. API 'Ler, bir yolun bölümlerinin ayrıntılarını döndürebilir. Örneğin, bir trafik sıkıştı koordinat aralığı veya trafiğin geçerli hızı.

![Açma yönergelerine göre aç](media/how-to-use-best-practices-for-routing/instructions-turn-by-turn-img.png)

![Özetlenen yönergeler](media/how-to-use-best-practices-for-routing/instructions-summary-img.png)

## <a name="request-a-route-for-a-commercial-vehicle"></a>Ticari araç için rota isteme

Azure haritalar yönlendirme API 'Leri, ticari structuralks yönlendirmeyi kapsayan ticari araç yönlendirmeyi destekler. API 'Ler belirtilen sınırları göz önünde bulundurun. Örneğin, araç için yükseklik ve ağırlık ve eğer araç, tehlikeli Cargo 'yu taşırsaldır. Örneğin, bir araç flaşmaya çalışıyorsanız, yönlendirme altyapısı, yöresel alanlara yakın bazı tünellerden kaçınır.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki örnek istek, bir ticari kamyonun yolunu sorgular. Kamyonu, sınıf 1 tehlikeli atık malzemesini taşıma.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass1&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Rota API 'SI, kamyonun boyutlarına ve tehlikeli çöp kutusu ile uyumlu yönler döndürür. Öğesini genişleterek yol yönergelerini okuyabilirsiniz `guidance` .

![Sınıf 1 hazisatık ile kamyonu](media/how-to-use-best-practices-for-routing/truck-with-hazwaste-img.png)

### <a name="sample-query"></a>Örnek sorgu

Yukarıdaki sorgudaki US hazı sınıfını değiştirmek, bu değişikliğe uyum sağlamak için farklı bir yol oluşmasına neden olur.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass9&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Aşağıdaki yanıt, sınıf 1 tehlikeli bir malzemeden daha az tehlikeli olan sınıf 9 tehlikeli bir malzeme taşıyan bir kamyon içindir. `guidance`Yönleri okumak için öğesini genişlettiğinizde, yönlerinin aynı olmadığına dikkat edin. Kamyonun Sınıf 1 tehlikeli malzemesini taşıyan daha fazla rota yönergesi bulunur.



![Class 9 hazisatık ile kamyonu](media/how-to-use-best-practices-for-routing/truck-with-hazwaste9-img.png)



## <a name="request-traffic-information-along-a-route"></a>Yol üzerinde trafik bilgilerini isteme

Azure haritalar yol yönü API 'Leri ile, geliştiriciler istekteki parametreyi ekleyerek her bir bölüm türü için Ayrıntılar talep edebilir `sectionType` . Örneğin, her trafik sıkışıklığı kesimine yönelik hız bilgilerini isteyebilirsiniz. İstediğiniz çeşitli ayrıntılar hakkında bilgi edinmek için [sectionType anahtarının değer listesine](https://docs.microsoft.com/rest/api/maps/route/getroutedirections#sectiontype) bakın.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu öğesini olarak ayarlar `sectionType` `traffic` . Seattle 'dan San Diego trafik bilgilerini içeren bölümleri ister.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&sectionType=traffic&query=47.6062,-122.3321:32.7157,-117.1611
```

Yanıt, verilen koordinatlar boyunca trafik için uygun olan bölümleri içerir.

![Trafik bölümleri](media/how-to-use-best-practices-for-routing/traffic-section-type-img.png)

Bu seçenek, aşağıdaki görüntüde olduğu gibi, Haritayı işlerken bölümleri renklendirmek için kullanılabilir: 

![Haritada işlenen renkli bölümler](media/how-to-use-best-practices-for-routing/show-traffic-sections-img.png)

## <a name="calculate-and-optimize-a-multi-stop-route"></a>Çoklu durduran bir yolu hesapla ve iyileştirme

Azure haritalar şu anda iki yol iyileştirmesi sağlar:

* Geçiş noktaları sırasını değiştirmeden, istenen yol türüne göre iyileştirmeler. [Desteklenen yol türlerini buradan](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routetype) bulabilirsiniz

* Her durdurmayı ziyaret etmek için en iyi sırayı elde etmek üzere waypoints sırasını değiştiren seyahat problemine iyileştirmesi

Çoklu durduran yönlendirme için, tek bir rota isteğinde en fazla 150 waypoints belirtilebilir. Başlangıç ve bitiş koordinat konumları, gidiş dönüş ile aynı olacak şekilde aynı olabilir. Ancak rota hesaplamasını yapmak için en az bir ek yol sağlamanız gerekir. Kaynak ve hedef koordinatları arasındaki içindeki sorguya waypoints eklenebilir.

Verilen waypoints 'i ziyaret etmek için en iyi sırayı iyileştirmek istiyorsanız **computeBestOrder = true değerini**belirtmeniz gerekir. Bu senaryo, seyahat problemine iyileştirme sorunu olarak da bilinir.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu, parametresi olarak ayarlanan altı waypoints için yol ister `computeBestOrder` `false` . Ayrıca, parametresi için varsayılan değerdir `computeBestOrder` .

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=false&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Yanıt, 140.851 ölçüm olacak yol uzunluğunu açıklar ve bu yolu gezecek 9.991 saniye sürer.

![İyileştirilmemiş yanıt](media/how-to-use-best-practices-for-routing/non-optimized-response-img.png)

Aşağıdaki görüntüde Bu sorgudan elde edilen yol gösterilmektedir. Bu yol, olası bir yoldur. Zamana veya mesafeye göre en iyi yol değildir.

![İyileştirilmemiş görüntü](media/how-to-use-best-practices-for-routing/non-optimized-image-img.png)



Bu yol güzergah noktası sırası: 0, 1, 2, 3, 4, 5 ve 6.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu, yukarıdaki örnekteki gibi aynı altı waypoints için yolu ister. Bu kez, `computeBestOrder` parametresi olarak ayarlanır `true` (seyahat problemine iyileştirmesi).

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=true&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Yanıt, 91.814 ölçüm olacak yol uzunluğunu açıklar ve bu yolu gezecek 7.797 saniye sürer. API en iyileştirilmiş yolu döndürdüğünden, seyahat mesafesi ve seyahat süresinin ikisi de burada düşüktür.

![İyileştirilmiş yanıt](media/how-to-use-best-practices-for-routing/optimized-response-img.png)

Aşağıdaki görüntüde Bu sorgudan elde edilen yol gösterilmektedir.

![İyileştirilmiş görüntü](media/how-to-use-best-practices-for-routing/optimized-image-img.png)

En iyi yol şu Yön noktası sırasına sahiptir: 0, 5, 1, 2, 4, 3 ve 6.

>[!TIP]
> Yönlendirme hizmetinden en iyileştirilmiş güzergah noktası sıra bilgileri bir dizin kümesi sağlar. Bunlar, kaynağı ve hedef dizinleri hariç tutar. Kaynak için bu değerleri 1 ile artırmanız gerekir. Ardından, tüm sıralı güzergah noktası listesini almak için hedefi sonuna ekleyin.

## <a name="calculate-and-bias-alternative-routes-using-supporting-points"></a>Destekleyici noktaları kullanarak alternatif rotalar hesaplama ve sapma

Bir başvuru yolu için sıfır veya daha fazla alternatif yol hesaplamak üzere bir yolu yeniden oluşturmak istediğiniz durumlara sahip olabilirsiniz. Örneğin, perakende deponuzu yönlendiren müşterilere alternatif yollar göstermek isteyebilirsiniz. Bu durumda, destekleyici noktaları kullanarak bir konumu sapgetirmeniz gerekir. Bir konumu sapma adımları aşağıda verilmiştir:

1. Bir yolu olduğu gibi hesapla ve yol yanıtından yolu al
2. Yol yolunda istenen konumları bulmak için yol yolunu kullanın. Örneğin, Azure Maps for [Interest API 'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) kullanabilir veya kendi verilerinizi veritabanınızda sorgulayabilirsiniz.  
3. Yolun başlangıcından uzaklığına göre konumları sıralama
4. Bu konumları, [yol yönlerini gönder API](https://docs.microsoft.com/rest/api/maps/route/postroutedirections)'sine yeni bir rota isteğinde destekleme noktaları olarak ekleyin. Destekleyici noktaları hakkında daha fazla bilgi edinmek için bkz. [yol yönlerini gönderme API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#supportingpoints). 

[Yol yönlerini gönder API](https://docs.microsoft.com/rest/api/maps/route/postroutedirections)'sini çağırırken, en düşük sapma süresini veya uzaklık kısıtlamalarını destekleme noktalarıyla birlikte ayarlayabilirsiniz. Alternatif yollar sunmak istiyorsanız, bu parametreleri kullanın, ancak seyahat süresini de sınırlandırmak istersiniz. Bu kısıtlamalar kullanıldığında, alternatif yollar, kaynak noktasındaki başvuru yolunu verilen zaman veya mesafe için izler. Diğer bir deyişle, diğer yollar verilen kısıtlamalara göre başvuru rotasına göre ayrılmış.

Aşağıdaki görüntüde, zaman ve mesafe için belirtilen Sapma limitleriyle alternatif yolların oluşturulmasına yönelik bir örnek verilmiştir.

![Alternatif yollar](media/how-to-use-best-practices-for-routing/alternative-routes-img.png)

## <a name="use-the-routing-service-in-a-web-app"></a>Yönlendirme hizmetini bir Web uygulamasında kullanma

Azure Haritalar Web SDK 'Sı bir [hizmet modülü](https://docs.microsoft.com/javascript/api/azure-maps-rest/)sağlar. Bu modül, JavaScript veya TypeScript kullanarak Web veya Node.js uygulamalarında Azure Maps REST API 'Lerini kullanmayı kolaylaştıran bir yardımcı kitaplıktır. Hizmet modülü, haritada döndürülen yolları işlemek için kullanılabilir. Modül, hangi API 'nin GET ve POST istekleri ile kullanılacağını otomatik olarak belirler.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için lütfen bkz.

> [!div class="nextstepaction"]
> [Azure haritalar yönlendirme hizmeti](https://docs.microsoft.com/rest/api/maps/route)

> [!div class="nextstepaction"]
> [Hizmet modülünü kullanma](https://docs.microsoft.com/azure/azure-maps/how-to-use-services-module)

> [!div class="nextstepaction"]
> [Haritada rota göster](https://docs.microsoft.com/azure/azure-maps/map-route)

> [!div class="nextstepaction"]
> [Azure haritalar NPM paketi](https://www.npmjs.com/package/azure-maps-rest  )
