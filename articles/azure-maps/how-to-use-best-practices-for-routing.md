---
title: Azure Haritalar Rota Hizmeti için en iyi uygulamalar | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar'daki Route Service'i kullanarak nasıl verimli bir şekilde yönlendirilenleri öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/11/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 85ce29d088b8fbd110988db67776d89346215e5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335412"
---
# <a name="best-practices-for-azure-maps-route-service"></a>Azure Haritalar Rotası hizmeti için en iyi uygulamalar

Azure Haritalar [Rota Hizmeti'ndeki](https://docs.microsoft.com/rest/api/maps/route) Rota Yönleri ve Rota Matrisi API'leri, istenen her rota için tahmini varış saatlerini (ETA) hesaplamak için kullanılabilir. Rota API'leri, haftanın istenen günü ve günün saatindeki tipik yol hızları gibi gerçek zamanlı trafik bilgileri ve geçmiş trafik verileri gibi faktörleri dikkate alır. API'ler, zamana veya mesafeye bağlı olarak, aynı anda veya en iyi duruma getirilmiş sırayla birden fazla hedefe kullanılabilen en kısa veya en hızlı rotaları döndürer. Kullanıcılar ayrıca aylaklar, bisikletçiler ve kamyonlar gibi ticari araçlar için özel rotalar ve ayrıntılar talep edebilirler. Bu makalede, Azure Haritalar [Rota Hizmeti](https://docs.microsoft.com/rest/api/maps/route)olarak adlandırmak için en iyi uygulamaları paylaşacağız ve nasıl yapılacağını öğreneceksiniz:

* Rota Yol Tarifi API'leri ve Matris Yönlendirme API'si arasında seçim yapın
* Gerçek zamanlı ve geçmiş trafik verilerine dayalı olarak tarihi ve tahmin edilmiş seyahat süreleri isteyin
* Rotanın tamamı ve rotanın her bacağı için zaman ve mesafe gibi rota ayrıntılarını isteyin
* Ticari bir araç için istek güzergahı, kamyon gibi
* Sıkışıklıklar ve geçiş ücreti bilgileri gibi bir rota boyunca trafik bilgileri isteme
* Bir veya daha fazla duraktan (yol işaretlerinden) oluşan bir rota isteme
* Her durağı (ara nokta) ziyaret etmek için en iyi siparişi almak için bir veya daha fazla duraktan oluşan bir rotayı optimize etme
* Destek noktalarını kullanarak alternatif rotaları optimize edin. Örneğin, bir elektrikli araç şarj istasyonu geçmek alternatif yollar sunuyoruz.
* Azure Haritalar Web SDK ile [Rota Hizmetini](https://docs.microsoft.com/rest/api/maps/route) Kullanma

## <a name="prerequisites"></a>Ön koşullar

Azure Haritalar API'larına arama yapmak için bir Azure Haritalar hesabına ve bir anahtara ihtiyacınız var. Daha fazla bilgi için [bkz.](quick-demo-map-app.md#create-an-account-with-azure-maps) [Get a primary key](quick-demo-map-app.md#get-the-primary-key-for-your-account) Birincil anahtar, birincil abonelik anahtarı veya abonelik anahtarı olarak da bilinir.

Azure Haritalar'da kimlik doğrulama hakkında bilgi için azure [haritalarda kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın. Ve Route Service kapsamı hakkında daha fazla bilgi için, [Yönlendirme Kapsamı](routing-coverage.md)bakın.

Bu makalede, REST aramaları oluşturmak için [Postacı uygulamasını](https://www.postman.com/downloads/) kullanır, ancak herhangi bir API geliştirme ortamı seçebilirsiniz.

## <a name="choose-between-route-directions-and-matrix-routing"></a>Rota Yönleri ve Matris Yönlendirme arasında seçim yapın

Route Directions API'leri, seyahat süresi ve bir rota yolunun koordinatlarını içeren talimatları iade eder. Route Matrix API, kaynak ve hedef konumlara göre tanımlanan bir dizi rotanın seyahat süresini ve mesafelerini hesaplamanızı sağlar. Verilen her kaynak için, Matrix API, bu kaynaktan her hedefe yönlendirmenin maliyetini (seyahat süresi ve mesafesi) hesaplar. Tüm bu API'ler, istenen kalkış saati, varış saatleri ve araç tipi gibi araç veya kamyon gibi parametreleri belirtmenize olanak sağlar. En uygun rotaları döndürmek için hepsi buna göre gerçek zamanlı veya tahmine dayalı trafik verilerini kullanır.

Senaryonuz şu şekildeyse Rota Yönergeleri API'lerini aramayı düşünün:

* Teslimat araçlarınız için kesin varış saatlerini almak için, iki veya daha fazla bilinen konum arasındaki en kısa veya en hızlı sürüş rotasını isteyin.
* Rotaları haritaüzerinde görselleştirmek için rota geometrisi de dahil olmak üzere ayrıntılı rota kılavuzu isteyin
* Müşteri konumlarının listesi göz önüne alındığında, her müşteri konumunu ziyaret etmek ve menşeine dönmek için mümkün olan en kısa rotayı hesaplayın. Bu senaryo genellikle seyahat eden satıcı sorunu olarak bilinir. Tek bir istekte en fazla 150 geçiş noktasını (durak) geçebilirsiniz.
* Tek bir API çağrısı nı kullanarak Rota Yol Önergesi Toplu Ek Api'sine toplu sorgular gönderin.

Senaryonuz şu larsa Matrix Yönlendirme API'sini aramayı düşünün:

* Bir dizi kaynak ve hedef arasındaki seyahat süresini veya mesafeyi hesaplayın. Örneğin, 12 sürücü var ve restorandan gıda teslimat almak için en yakın kullanılabilir sürücü bulmanız gerekir.
* Potansiyel rotaları gerçek seyahat mesafelerine veya zamanlarına göre sıralayın. Matrix API, her bir başlangıç ve hedef kombinasyonu için yalnızca seyahat sürelerini ve mesafeleri döndürür.
* Verileri seyahat süresine veya uzaklıklarına göre kümeleme. Örneğin, şirketinizin 50 çalışanı vardır, ofisinizden 20 dakikalık Drive Süresi içinde yaşayan tüm çalışanları bulun.

Rota Yönleri ve Matris API'lerinin bazı özelliklerini göstermek için karşılaştırma:

| Azure Haritalar API'si | İstekteki maksimum sorgu sayısı | Alanlardan kaçının | Kamyon ve elektrikli araç yönlendirme | yol işaretleri ve Seyahat Satış Elemanı optimizasyonu | Destek noktaları |
| :--------------: |  :--------------: |  :--------------: | :--------------: | :--------------: | :--------------: |
| Rota Yol Tarifleri alın | 1 | | X | X | |
| Rota Yol Tarifi Gönder | 1 | X | X | X | X |
| Rota Yol Tarifi Toplu İşlemi | 700 | | X | X | |
| Rota Sonrası Matrisi | 700 | | X | | |

Elektrikli araç yönlendirme özellikleri hakkında daha fazla bilgi edinmek için, Python ile Azure Dizüstü Bilgisayar kullanarak [elektrikli araçların nasıl yönlendirilenecek](tutorial-ev-routing.md)lerine ilişkin öğreticimize bakın.

## <a name="request-historic-and-real-time-data"></a>Geçmiş ve gerçek zamanlı veri isteme

Varsayılan olarak, Rota hizmeti seyahat modunun bir araba olduğunu ve kalkış saatinin şimdi olduğunu varsayar. Rota hesaplama isteği aksini belirtmediği sürece rotayı gerçek zamanlı trafik koşullarına göre döndürür. 'Sol dönüşlere 16:00 ile 18:00 arasında izin verilmez' gibi sabit zamana bağlı trafik kısıtlamaları yakalanır ve yönlendirme motoru tarafından dikkate alınır. Yol çalışmaları gibi yol kapatmaları, geçerli canlı trafiği yok sayan bir rota özellikle talep etmedikçe dikkate alınacaktır. Geçerli trafiği yoksaymak `traffic` `false` için API isteğinizde ayarlanır.

Rota **hesaplaması travelInSeconds** değeri trafik nedeniyle gecikmeyi içerir. Kalkış saati şimdi ayarlandığında, mevcut ve tarihi seyahat süresi verilerinden yararlanılarak oluşturulur. Kalkış saatiniz gelecekte ayarlanmışsa, API'ler geçmiş verilere dayalı olarak öngörülen seyahat sürelerini iade e-dönüşe geri döner.

İsteğinize **işlemTravelTimeFor=tüm** parametreyi eklerseniz, yanıttaki özet öğesi geçmiş trafik koşulları da dahil olmak üzere aşağıdaki ek alanlara sahip olacaktır:

| Öğe | Açıklama|
| :--- | :--- |
| noTrafficTravelTimeInSeconds | Tahmini seyahat süresi, örneğin trafik koşulları nedeniyle güzergahta gecikme yokmuş gibi hesaplanır. |
| tarihiTrafficTravelTimeInSeconds | Zamana bağlı geçmiş trafik verileri kullanılarak hesaplanan tahmini seyahat süresi |
| liveTrafficIncidentsTravelTimeInSeconds | Gerçek zamanlı hız verileri kullanılarak hesaplanan tahmini seyahat süresi |

Sonraki bölümler, tartışılan parametreleri kullanarak Rota API'lerine nasıl çağrı yapılacağını gösterir.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki ilk örnekte kalkış saati, yazma sırasında geleceğe ayarlanır.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=51.368752,-0.118332:51.385426,-0.128929&travelMode=car&traffic=true&departAt=2025-03-29T08:00:20&computeTravelTimeFor=all
```

Yanıt, aşağıdaki gibi bir özet öğesi içerir. Kalkış saati geleceğe ayarlandığı için **trafficDelayInSeconds** değeri sıfırdır. **travelTimeInSeconds** değeri zamana bağlı geçmiş trafik verileri kullanılarak hesaplanır. Yani, bu durumda, **travelTimeInSeconds** değeri **tarihselTrafficTravelTimeInSeconds** değeri eşittir.

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

Aşağıdaki ikinci örnekte, kalkış saatinin şimdi olduğu gerçek zamanlı yönlendirme isteğimiz var. Varsayılan değer olduğu için URL'de açıkça belirtilmez.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=47.6422356,-122.1389797:47.6641142,-122.3011268&travelMode=car&traffic=true&computeTravelTimeFor=all
```

Yanıt, aşağıda gösterildiği gibi bir özet içerir. Tıkanıklıklar nedeniyle **trafficDelaysInSeconds** değeri sıfırdan büyüktür. Ayrıca **tarihiTrafficTravelTimeInSeconds**daha büyük.

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

## <a name="request-route-and-leg-details"></a>Rota ve bacak ayrıntıları isteyin

Varsayılan olarak, Rota hizmeti bir dizi koordinat döndürecektir. Yanıt, adlı `points`bir listedeki yolu oluşturan koordinatları içerecektir. Rota yanıtı, rotanın başlangıcından itibaren mesafeyi ve tahmini geçen süreyi de içerir. Bu değerler, tüm rotanın ortalama hızını hesaplamak için kullanılabilir.

Aşağıdaki resim öğeyi `points` gösterir.

<center>

![puan listesi](media/how-to-use-best-practices-for-routing/points-list-is-hidden-img.png)

</center>

Yolun `point` koordinatlistesini görmek için öğeyi genişletin:

<center>

![puan listesi](media/how-to-use-best-practices-for-routing/points-list-img.png)

</center>

Route Directions API'leri, **yönergeleriTip** parametresini belirterek kullanılabilecek farklı yönerge biçimlerini destekler. Kolay bilgisayar işleme yönergelerini biçimlendirmek için **instructionsType=coded kullanın.** **Talimatları kullanType=kullanıcı** için metin olarak yönergeleri görüntülemek için etiketlendi. Ayrıca, yönergeler bazı öğeleri işaretlenmiş metin olarak biçimlendirilebilir ve yönerge özel biçimlendirme ile sunulur. Daha fazla bilgi için [desteklenen yönerge türlerinin listesine](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routeinstructionstype)bakın.

Yönergeler istendiğinde, yanıt adı verilen `guidance`yeni bir öğeyi döndürür. Öğe `guidance` iki bilgi parçasını tutar: turn-by-turn yönleri ve özetlenmiş yönergeler.

<center>

![Talimatlar türü](media/how-to-use-best-practices-for-routing/instructions-type-img.png)

</center>

Öğe, `instructions` yolculuk için turn-by-turn yönergeleri `instructionGroups` tutar ve özetlenmiş yönergeleri vardır. Her talimat özeti, birden çok yolu kapsayabilecek bir gezi bölümünü kapsar. API'ler rotanın bölümleriiçin ayrıntıları döndürebilir. örneğin, bir trafik sıkışıklığı veya trafik mevcut hızı koordinat aralığı.

<center>

![Dönüş talimatları](media/how-to-use-best-practices-for-routing/instructions-turn-by-turn-img.png)

</center>

<center>

![Özetli Talimatlar](media/how-to-use-best-practices-for-routing/instructions-summary-img.png)

</center>

## <a name="request-a-route-for-a-commercial-vehicle"></a>Ticari bir araç için rota isteme

Azure Haritalar Yönlendirme API'leri, ticari kamyon yönlendirmeyi kapsayan ticari araç yönlendirmeyi destekler. API'ler belirtilen sınırları dikkate alır. Örneğin, aracın boy ve ağırlığı ve araç tehlikeli yük taşıyorsa. Örneğin, bir araç yanıcı taşıyorsa, yönlendirme motoru yerleşim bölgelerine yakın belirli tünellerden kaçınır.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki örnek istek, ticari bir kamyon için bir rotayı sorgular. Kamyon sınıf 1 tehlikeli atık madde taşıyor.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass1&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Route API, kamyonun ve tehlikeli atıkların boyutlarını barındıran yönler döndürür. Öğeyi genişleterek rota yönergelerini `guidance` okuyabilirsiniz.

<center>

![Sınıf 1 hazwaste ile kamyon](media/how-to-use-best-practices-for-routing/truck-with-hazwaste-img.png)

</center>

### <a name="sample-query"></a>Örnek sorgu

Yukarıdaki sorgudan ABD Hazmat Sınıfı'nın değiştirilmesi, bu değişikliği karşılamak için farklı bir rota ya da başka bir yol açacaktır.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass9&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Aşağıdaki yanıt, sınıf 9 tehlikeli madde taşıyan ve sınıf 1 tehlikeli maddeden daha az tehlikeli olan bir kamyon içindir. Yönergeleri okumak `guidance` için öğeyi genişletdiğinizde, yol tariflerinin aynı olmadığını fark esiniz. Sınıf 1 tehlikeli madde taşıyan kamyon için daha fazla güzergah talimatları vardır.

<center>

![9 sınıfı hazwaste ile kamyon](media/how-to-use-best-practices-for-routing/truck-with-hazwaste9-img.png)

</center>

## <a name="request-traffic-information-along-a-route"></a>Rota boyunca trafik bilgileri isteme

Azure Haritalar Rota Yönü API'leri ile geliştiriciler, isteğe `sectionType` parametre ekleyerek her bir bölüm türü için ayrıntı isteyebilir. Örneğin, her trafik sıkışıklığı segmenti için hız bilgilerini isteyebilirsiniz. Talep edebilirsiniz çeşitli ayrıntılar hakkında bilgi edinmek [için bölümType anahtarı için değerler listesine](https://docs.microsoft.com/rest/api/maps/route/getroutedirections#sectiontype) bakın.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu `sectionType` için `traffic`ayarlar. Seattle'dan San Diego'ya kadar trafik bilgilerini içeren bölümleri talep etmektedir.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&sectionType=traffic&query=47.6062,-122.3321:32.7157,-117.1611
```

Yanıt, verilen koordinatlar boyunca trafiğe uygun bölümleri içerir.

<center>

![trafik bölümleri](media/how-to-use-best-practices-for-routing/traffic-section-type-img.png)

</center>

Bu seçenek, aşağıdaki resimde olduğu gibi, haritayı oluştururken bölümleri renklendirmek için kullanılabilir: 

<center>

![trafik bölümleri](media/how-to-use-best-practices-for-routing/show-traffic-sections-img.png)

</center>

## <a name="calculate-and-optimize-a-multi-stop-route"></a>Çok duraklı rotayı hesaplama ve optimize edin

Azure Haritalar şu anda iki rota optimizasyonu biçimi sağlar:

* Yol işaretlerinin sırasını değiştirmeden, istenen rota türüne göre optimizasyonlar. Desteklenen rota [türlerini burada](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routetype) bulabilirsiniz

* Her durağı ziyaret etmek için en iyi siparişi elde etmek için yol işaretlerinin sırasını değiştiren seyahat eden satıcı optimizasyonu

Çok duraklı yönlendirme için, tek bir rota isteğinde en fazla 150 yol noktası belirtilebilir. Başlangıç ve bitiş koordinat konumları, gidiş-dönüş te olduğu gibi aynı olabilir. Ancak rota hesaplaması yapmak için en az bir ek yol noktası sağlamanız gerekir. Yol işaretleri, kaynak ve hedef koordinatları arasındaki sorguya eklenebilir.

Verilen ara noktaları ziyaret etmek için en iyi siparişi optimize etmek istiyorsanız, **computeBestOrder=true**belirtmeniz gerekir. Bu senaryo, seyahat eden satıcı optimizasyon sorunu olarak da bilinir.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu, `computeBestOrder` parametre olarak ayarlanmış altı yol noktası `false`için yol istiyor. Ayrıca `computeBestOrder` parametre için varsayılan değerdir.

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=false&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Yanıt, yol uzunluğunun 140.851 metre olduğunu ve bu yoldan seyahat in9.991 saniye süreceğini açıklar.

<center>

![Optimize edilmeyen yanıt](media/how-to-use-best-practices-for-routing/non-optimized-response-img.png)

</center>

Aşağıdaki resimde bu sorgudan kaynaklanan yolu gösterin. Bu yol olası bir yoldur. Zamana ya da mesafeye göre en uygun yol değil.

<center>

![Optimize edilmeyen görüntü](media/how-to-use-best-practices-for-routing/non-optimized-image-img.png)

</center>

Bu rota yol işaret noktası sırası: 0, 1, 2, 3, 4, 5 ve 6.

### <a name="sample-query"></a>Örnek sorgu

Aşağıdaki sorgu, yukarıdaki örnekte olduğu gibi aynı altı yol noktası için yol ister. Bu kez, `computeBestOrder` parametre `true` (seyahat satıcı optimizasyonu) ayarlanır.

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=true&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Yanıt, yol uzunluğunun 91.814 metre olduğunu ve bu yoldan seyahat in7.797 saniye süreceğini açıklar. API optimize edilmiş rotayı iade ettiği için seyahat mesafesi ve seyahat süresi burada daha düşüktür.

<center>

![Optimize edilmeyen yanıt](media/how-to-use-best-practices-for-routing/optimized-response-img.png)

</center>

Aşağıdaki resimde bu sorgudan kaynaklanan yolu gösterin.

<center>

![Optimize edilmeyen görüntü](media/how-to-use-best-practices-for-routing/optimized-image-img.png)

</center>

En uygun rota aşağıdaki ara nokta sırasına sahiptir: 0, 5, 1, 2, 4, 3 ve 6.

>[!TIP]
> Yönlendirme hizmetinden en iyi duruma getirilmiş ara nokta sipariş bilgileri bir dizi endeks sağlar. Bunlar, kaynak ve hedef endeksleri hariç tutar. Kaynağını hesaba katmak için bu değerleri 1'e kadar artımgerekir. Ardından, sipariş edilen yol işaret noktasının tamamını almak için hedefinizi sonuna ekleyin.

## <a name="calculate-and-bias-alternative-routes-using-supporting-points"></a>Destek noktalarını kullanarak alternatif rotaları hesaplama ve sapma

Bir başvuru rotası için sıfır veya daha fazla alternatif rotayı hesaplamak için rotayı yeniden oluşturmak istediğiniz durumlar olabilir. Örneğin, müşterilere perakende mağazanızdan geçen alternatif rotalar göstermek isteyebilirsiniz. Bu durumda, destekleyici noktaları kullanarak bir konumu saptamanız gerekir. Bir konumu yalıtmak için gereken adımlar şunlardır:

1. Bir rotayı olduğu gibi hesaplayın ve rota yanıtından yolu alın
2. Rota yolu boyunca veya yakınında istenilen konumları bulmak için rota yolunu kullanın. Örneğin, Azure Haritalar [İlgi Alanı API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) kullanabilir veya veritabanınızda kendi verilerinizi sorgulayabilirsiniz.  
3. Rotanın başlangıcından gelen uzaklık durumuna göre konumları sipariş edin
4. Bu konumları, Yeni bir rota isteğinde destek noktaları olarak [Post Route Yol Tarifi'ne](https://docs.microsoft.com/rest/api/maps/route/postroutedirections)ekleyin. Destek noktaları hakkında daha fazla bilgi edinmek [için, Yol Sonrası Yol Tarifi API belgelerine](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#supportingpoints)bakın. 

[Posta Yolu Yol Tarifi'ni](https://docs.microsoft.com/rest/api/maps/route/postroutedirections)ararken, destekleyici noktaların yanı sıra minimum sapma süresini veya mesafe kısıtlamalarını ayarlayabilirsiniz. Alternatif rotalar sunmak istiyorsanız, ancak seyahat süresini de sınırlamak istiyorsanız bu parametreleri kullanın. Bu kısıtlamalar kullanıldığında, alternatif rotalar verilen süre veya mesafe için başlangıç noktasından başvuru rotasını izler. Başka bir deyişle, diğer yollar verilen kısıtlamalar adedine göre başvuru rotasından ayrılır.

Aşağıdaki resimde, zaman ve mesafe için belirtilen sapma sınırlarıile alternatif rotalar oluşturma örneği verilmiştir.

<center>

![Alternatif rotalar](media/how-to-use-best-practices-for-routing/alternative-routes-img.png)

</center>

## <a name="use-the-routing-service-in-a-web-app"></a>Yönlendirme hizmetini bir web uygulamasında kullanma

Azure Haritalar Web SDK bir [Hizmet modülü](https://docs.microsoft.com/javascript/api/azure-maps-rest/?view=azure-maps-typescript-latest)sağlar. Bu modül, JavaScript veya TypeScript kullanarak Web veya Node.js uygulamalarında Azure Haritalar REST API'lerinin kullanımını kolaylaştıran bir yardımcı kitaplıktır. Servis modülü, döndürülen yolları harita üzerinde işlemek için kullanılabilir. Modül, GET ve POST istekleriyle hangi API'nin kullanılacağını otomatik olarak belirler.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için lütfen bkz:

> [!div class="nextstepaction"]
> [Azure Haritalar Rota hizmeti](https://docs.microsoft.com/rest/api/maps/route)

> [!div class="nextstepaction"]
> [Hizmet modülü nasıl kullanılır?](https://docs.microsoft.com/azure/azure-maps/how-to-use-services-module)

> [!div class="nextstepaction"]
> [Rotayı haritada göster](https://docs.microsoft.com/azure/azure-maps/map-route)

> [!div class="nextstepaction"]
> [Azure Haritalar NPM Paketi](https://www.npmjs.com/package/azure-maps-rest  )
