---
title: "Öğretici: Web hizmetlerini Google Haritalar'dan geçirin | Microsoft Azure Haritaları"
description: Web hizmetlerinin Google Haritalar'dan Microsoft Azure Haritalar'a nasıl geçirilir?
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: d2f25f2b786686b8af9bad4ea8ce3c8aea9b589f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371470"
---
# <a name="migrate-web-service-from-google-maps"></a>Web hizmetini Google Haritalar'dan geçirin

Hem Azure hem de Google Haritalar, REST web hizmetleri aracılığıyla mekansal API'lere erişim sağlar. Bu platformların API arabirimleri benzer işlevler gerçekleştirir. Ancak, her biri farklı adlandırma kuralları ve yanıt nesneleri kullanır.

Tablo, listelenen Google Haritalar hizmet API'lerine benzer bir işlevsellik olan Azure Haritalar hizmet API'lerini gösterir.

| Google Haritalar hizmet API'si | Azure Haritalar hizmet API'si                                                                      |
|-------------------------|---------------------------------------------------------------------------------------------|
| Yol tarifleri              | [Rota](https://docs.microsoft.com/rest/api/maps/route)                                     |
| Mesafe Matrisi         | [Rota Matrisi](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview)       |
| Coğrafi kodlama               | [Arama](https://docs.microsoft.com/rest/api/maps/search)                                   |
| Yerler Arama           | [Arama](https://docs.microsoft.com/rest/api/maps/search)                                   |
| Otomatik Tamamlama yı yerleştir      | [Arama](https://docs.microsoft.com/rest/api/maps/search)                                   |
| Yola Yapış            | Bkz. [Rotaları ve yönleri Hesapla](#calculate-routes-and-directions) bölümü.            |
| Hız Limitleri            | Bkz. Bir koordinat bölümünü [tersine çevirgeocode.](#reverse-geocode-a-coordinate)                  |
| Statik Harita              | [İşleme](https://docs.microsoft.com/rest/api/maps/render/getmapimage)                       |
| Saat Dilimi               | [Saat Dilimi](https://docs.microsoft.com/rest/api/maps/timezone)                              |

Aşağıdaki hizmet API'leri şu anda Azure Haritalar'da kullanılamıyor:

- Yükseklik
- Coğrafi Konum
- Yerler ayrıntıları ve fotoğraflar - Telefon numaraları ve web sitesi URL'si Azure Haritalar arama API'sinde mevcuttur.
- Harita URL'leri
- En Yakın Yollar - Bu [burada](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Basic%20snap%20to%20road%20logic
)gösterildiği gibi Web SDK kullanılarak elde edilebilir , ancak şu anda bir hizmet olarak mevcut değildir.
- Statik sokak görünümü

Azure Haritalar'da ilgi nizi çekebilir birkaç ek REST web hizmeti vardır:

- [Uzamsal işlemler](https://docs.microsoft.com/rest/api/maps/spatial): Karmaşık uzamsal hesaplamaları ve geofencing gibi işlemleri bir hizmete boşaltın.
- [Trafik](https://docs.microsoft.com/rest/api/maps/traffic): Gerçek zamanlı trafik akışına ve olay verilerine erişin.

## <a name="geocoding-addresses"></a>Coğrafi kodlama adresleri

Coğrafi kodlama, bir adresi koordinata dönüştürme işlemidir. Örneğin, "1 Microsoft yolu, Redmond, WA" boylam dönüştürür: -122.1298, enlem: 47.64005. Daha sonra, Koordinatlar, bir işaretçiyi haritaüzerinde konumlandırma gibi farklı türde amaçlar için kullanılabilir.

Azure Haritalar, coğrafi kodlama adresleri için çeşitli yöntemler sağlar:

- [**Serbest biçimli adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress): Tek bir adres dizesi belirtin ve isteği hemen işle. "1 Microsoft yolu, Redmond, WA" tek bir adres dize örneğidir. Bu API, tek tek adresleri hızla coğrafi kodlamanız gerekiyorsa önerilir.
- [**Yapılandırılmış adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressstructured): Sokak adı, şehir, ülke ve posta kodu gibi tek bir adresin bölümlerini belirtin ve isteği hemen işleme alın. Bu API, tek tek adresleri hızlı bir şekilde coğrafi olarak kodlamanız gerekiyorsa ve veriler zaten tek tek adres bölümlerine ayrıştırılırsa önerilir.
- [**Toplu adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatchpreview): 10.000 adrese kadar bir istek oluşturun ve bunları belirli bir süre içinde işletin. Tüm adresler sunucuda paralel olarak coğrafi olarak kodlanır ve tamamlandığında tam sonuç kümesi indirilebilir. Bu, büyük veri kümelerini coğrafi kodlama için önerilir.
- [**Bulanık arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlama ile ilgi noktası aramanoktasını birleştirir. Bu API serbest biçimli bir dize alır. Bu dize bir adres, yer, simge sel, ilgi noktası veya ilgi alanı kategorisi olabilir. Bu API isteği gerçek zamanlı ya yılönce işleme. Bu API, kullanıcıların adresleri veya aynı metin kutusunda ilgi çekici noktaları aradığı uygulamalar için önerilir.
- [**Bulanık toplu arama**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): 10.000 adres, yer, yer veya ilgi alanı içeren bir istek oluşturun ve bunları belirli bir süre boyunca işleyin. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilebilir.

Aşağıdaki tablo, Google Haritalar API parametrelerini Azure Haritalar'daki karşılaştırılabilir API parametreleriyle karşılaştırmıştır.

| Google Haritalar API parametresi | Karşılaştırılabilir Azure Haritalar API parametresi  |
|---------------------------|--------------------------------------|
| `address`                   | `query`                            |
| `bounds`                    | `topLeft` ve `btmRight`           |
| `components`                | `streetNumber`<br/>`streetName`<br/>`crossStreet`<br/>`postalCode`<br/>`municipality`- şehir / şehir<br/>`municipalitySubdivision`– mahalle, alt / süper şehir<br/>`countrySubdivision`- eyalet veya il<br/>`countrySecondarySubdivision`- ilçe<br/>`countryTertiarySubdivision`- ilçe<br/>`countryCode`- iki harfli ülke kodu |
| `key`                       | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                  | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `region`                    | `countrySet`                       |

Arama hizmetinin nasıl kullanılacağına bir örnek [burada](how-to-search-for-address.md)belgelenmiştir. Arama için [en iyi uygulamaları](how-to-use-best-practices-for-search.md)gözden geçirmek için emin olun.

> [!TIP]
> Serbest biçimli adres coğrafi kodlama ve bulanık arama API'leri, `&typeahead=true` istek URL'sine eklenerek otomatik tamamlama modunda kullanılabilir. Bu, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu ve aramanın tahmin moduna geçeceğini söyler.

## <a name="reverse-geocode-a-coordinate"></a>Bir koordinatı tersine çevirgeokodu

Ters coğrafi kodlama, coğrafi koordinatları yaklaşık bir adrese dönüştürme işlemidir. "Boylam: -122.1298, enlem: 47.64005" ile koordinatları "1 Microsoft yolu, Redmond, WA" dönüştürmek.

Azure Haritalar birkaç ters coğrafi kodlama yöntemi sağlar:

- [**Adres ters geocoder**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse): Bu koordinata karşılık gelen yaklaşık adresi almak için tek bir coğrafi koordinat belirtin. İsteği gerçek zamanlı olarak işler.
- [**Çapraz sokak ters geocoder**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet): Yakındaki çapraz sokak bilgilerini almak ve isteği hemen işlemek için tek bir coğrafi koordinat belirtin. Örneğin, aşağıdaki çapraz sokaklar 1 St Ave ve Main St alabilirsiniz.
- [**Toplu adres ters geocoder**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatchpreview): 10.000 koordinatı içeren bir istek oluşturun ve bunları bir süre boyunca işleyin. Tüm veriler sunucuda paralel olarak işlenir. İstek tamamlandığında, tüm sonuç kümesini indirebilirsiniz.

Bu tablo, Google Haritalar API parametrelerine Azure Haritalar'daki karşılaştırılabilir API parametreleriyle çapraz başvurularda bulunuyor.

| Google Haritalar API parametresi   | Karşılaştırılabilir Azure Haritalar API parametresi   |
|-----------------------------|---------------------------------------|
| `key`                       | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                  | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `latlng`                    | `query`  |
| `location_type`             | *Yok*     |
| `result_type`               | `entityType`    |

Arama için en iyi uygulamaları gözden [geçirin.](how-to-use-best-practices-for-search.md)

Azure Haritalar ters coğrafi kodlama API'si, Google Haritalar'da bulunmayan bazı ek özelliklere sahiptir. Bu özellikler, uygulamanızı geçirerken uygulamanızla tümleştirmek için yararlı olabilir:

- Hız sınırı verilerini alma
- Yol kullanım bilgilerini alın: yerel yol, arteriyel, sınırlı erişim, rampa ve benzeri
- Koordinatın bulunduğu caddenin yan tarafını alma

## <a name="search-for-points-of-interest"></a>İlgi noktası arama

İlgi çekici veriler, Yerler Arama API'si kullanılarak Google Haritalar'da aranabilir. Bu API, ilgi çekici noktaları aramak için üç farklı yol sağlar:

- **Metinden yer bulma:** Adını, adresini veya telefon numarasını temel alan bir ilgi noktası arar.
- **Yakındaki Arama**: Bir konuma belirli bir mesafede ki ilgi alanlarını arar.
- **Metin Arama:** İlgi alanı ve konum bilgilerini içeren serbest biçimli bir metin kullanarak yerleri arar. Örneğin, "New York'ta pizza" veya "ana st yakın restoranlar".

Azure Haritalar ilgi çekici noktalar için çeşitli arama API'leri sağlar:

- [**POI arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi): Ada göre ilgi noktaları arayın. Örneğin, "Starbucks".
- [**POI kategori arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory): Kategoriye göre ilgi alanları nın noktalarını arayın. Örneğin, "restoran".
- [**Yakındaki arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby): Bir konuma belirli bir mesafede ki ilgi alanlarını arar.
- [**Bulanık arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlama ile ilgi noktası aramanoktasını birleştirir. Bu API, adres, yer, simge sel, ilgi noktası veya ilgi alanı kategorisi olabilecek serbest biçimli bir dize alır. İsteği gerçek zamanlı olarak işler. Bu API, kullanıcıların adresleri veya aynı metin kutusunda ilgi çekici noktaları aradığı uygulamalar için önerilir.
- [**Geometri içinde ara**](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry): Belirli bir geometri içinde yer alan ilgi alanlarını arayın. Örneğin, çokgen içinde ilgi çekici bir nokta arayın.
- [**Yol boyunca arama**](https://docs.microsoft.com/rest/api/maps/search/postsearchalongroute): Belirli bir rota yolu boyunca olan ilgi alanlarını arayın.
- [**Bulanık toplu arama**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): 10.000 adres, yer, yer veya ilgi alanı içeren bir istek oluşturun. İsteği bir süre içinde işledi. Tüm veriler sunucuda paralel olarak işlenir. İstek işleme tamamlandığında, tüm sonuç kümesini indirebilirsiniz.

Şu anda Azure Haritalar'da Google Haritalar'daki Metin Arama API'si ile karşılaştırılabilir bir API yoktur.

> [!TIP]
> İçN araması, İçN kategori araması ve bulanık arama API'leri `&typeahead=true` istek URL'sine eklenerek otomatik tamamlama modunda kullanılabilir. Bu, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu söyler. API, aramayı tahmin modunda yürütecektir.

Arama belgeleri [için en iyi uygulamaları](how-to-use-best-practices-for-search.md) gözden geçirin.

### <a name="find-place-from-text"></a>Metinden yer bulma

İlgi alanlarını ada veya adrese göre aramak için Azure Haritalar [İçN aramasını](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) ve [Bulanık aramasını](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanın.

Tablo, Google Haritalar API parametrelerine karşılaştırılabilir Azure Haritalar API parametreleri ile çapraz başvurular.

| Google Haritalar API parametresi | Karşılaştırılabilir Azure Haritalar API parametresi |
|---------------------------|-------------------------------------|
| `fields`                  | *Yok*                               |
| `input`                   | `query`                             |
| `inputtype`               | *Yok*                               |
| `key`                     | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `locationbias`            | `lat`ve `lon``radius`<br/>`topLeft` ve `btmRight`<br/>`countrySet`  |

### <a name="nearby-search"></a>Yakındaki arama

Azure Haritalar'da yakındaki ilgi alanlarını almak için [Yakındaki arama](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) API'sini kullanın.

Tablo, Karşılaştırılabilir Azure Haritalar API parametreleri ile Google Maps API parametrelerini gösterir.

| Google Haritalar API parametresi | Karşılaştırılabilir Azure Haritalar API parametresi  |
|---------------------------|--------------------------------------|
| `key`                       | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `keyword`                   | `categorySet` ve `brandSet`        |
| `language`                  | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `location`                  | `lat` ve `lon`                     |
| `maxprice`                  | *Yok*                               |
| `minprice`                  | *Yok*                               |
| `name`                      | `categorySet` ve `brandSet`        |
| `opennow`                   | *Yok*                               |
| `pagetoken`                 | `ofs` ve `limit`                   |
| `radius`                    | `radius`                            |
| `rankby`                    | *Yok*                               |
| `type`                      | `categorySet –`[Desteklenen arama kategorileri](supported-search-categories.md) belgelerine bakın.   |

## <a name="calculate-routes-and-directions"></a>Rotaları ve yönleri hesaplama

Azure Haritalar'ı kullanarak rotaları ve yol tariflerini hesaplayın. Azure Haritalar, Google Haritalar yönlendirme hizmetiyle aynı işlevlere sahiptir( örneğin:

- Varış ve kalkış saatleri.
- Gerçek zamanlı ve tahmine dayalı trafik rotaları.
- Farklı ulaşım şekilleri. Araba sürmek, yürümek, bisiklet sürmek gibi.

> [!NOTE]
> Azure Haritalar tüm yol işaretlerinin koordinat olmasını gerektirir. Önce adresler coğrafi kodlanmalıdır.

Azure Haritalar yönlendirme hizmeti, rotahesaplamaiçin aşağıdaki API'leri sağlar:

- [**Rotayı Hesapla**](https://docs.microsoft.com/rest/api/maps/route/getroutedirections): Rotayı hesaplayın ve isteği hemen işletin. Bu API hem GET hem de POST isteklerini destekler. POSTA istekleri, çok sayıda yol noktası belirtirken veya URL isteğinin çok uzun olmamasını ve sorunlara neden olmasını sağlamak için çok sayıda yol seçeneği kullanırken önerilir. Azure Haritalar'daki POST Route Direction, binlerce destek [noktası](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#supportingpoints) nı alan ve bunları aralarında mantıksal bir rota yolunu yeniden oluşturmak için kullanacak (yola yapış) bir seçeneğe sahiptir. 
- [**Toplu işlem rotası**](https://docs.microsoft.com/rest/api/maps/route/postroutedirectionsbatchpreview): 1.000'e kadar rota isteği içeren bir istek oluşturun ve bunları belirli bir süre içinde işletin. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilebilir.
- [**Mobilite hizmetleri**](https://docs.microsoft.com/rest/api/maps/mobility): Toplu taşıma araçlarını kullanarak güzergahları ve yol tariflerini hesaplayın.

Tablo, Google Haritalar API parametrelerine Azure Haritalar'daki karşılaştırılabilir API parametreleriyle çapraz başvurularda bulunuyor.

| Google Haritalar API parametresi    | Karşılaştırılabilir Azure Haritalar API parametresi  |
|------------------------------|--------------------------------------|
| `alternatives`                 | `maxAlternatives`                  |
| `arrival_time`                | `arriveAt`                          |
| `avoid`                        | `avoid`                            |
| `departure_time`              | `departAt`                          |
| `destination`                  | `query`– formattaki koordinatlar`"lat0,lon0:lat1,lon1…."`  |
| `key`                          | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                     | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.   |
| `mode`                         | `travelMode`                       |
| `optimize`                     | `computeBestOrder`                 |
| `origin`                       | `query`                            |
| `region`                       | *N/A* – Bu özellik coğrafi kodlama ile ilgilidir. Azure Haritalar coğrafi kodlama API'sini kullanırken *countrySet* parametresini kullanın.  |
| `traffic_model`               | *N/A* – Yalnızca trafik verisinin *trafik* parametresi ile kullanIlip kullanılmayabileceğini belirtebilirsiniz. |
| `transit_mode`                | [Mobilite hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `transit_routing_preference` | [Mobilite hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `units`                        | *N/A* – Azure Haritalar yalnızca metrik sistemi kullanır.  |
| `waypoints`                    | `query`                            |

> [!TIP]
> Varsayılan olarak, Azure Haritalar rota API yalnızca bir özet döndürür. Mesafeyi, saatleri ve rota yolunun koordinatlarını döndürür. Sırayla `instructionsType` yönergeleri ni almak için parametreyi kullanın. Ayrıca, özet `routeRepresentation` ve rota yolunu filtrelemek için parametreyi kullanın.

Azure Haritalar yönlendirme API'si, Google Haritalar'da bulunmayan ek özelliklere sahiptir. Uygulamanızı geçirdiğinizde, bu özellikleri kullanmayı düşünün, bu özellikleri yararlı bulabilirsiniz.

- Rota türü için destek: en kısa, en hızlı, trilling ve en fazla yakıt verimli.
- Ek seyahat modları için destek: otobüs, motosiklet, taksi, kamyon ve minibüs.
- 150 yol noktası için destek.
- Tek bir istekte birden çok seyahat kez hesaplayın; tarihi trafik, canlı trafik, trafik yok.
- Ek yol tiplerinden kaçının: otopark yolları, asfaltsız yollar, zaten kullanılmış yollar.
- Kaçınmak için özel alanlar belirtin.
- Rotanın yükselebileceği yüksekliği sınırlayın.
- Motor özelliklerine göre rota. Motor özelliklerine ve kalan yakıt veya şarja göre yanma veya elektrikli araçlar için rotaları hesaplayın.
- Ticari araç rota parametrelerini destekleyin. Araç boyutları, ağırlık, aks sayısı ve kargo tipi gibi.
- Maksimum araç hızını belirtin.

Buna ek olarak, Azure Haritalar'daki rota [hizmeti, routable aralıklarının hesaplanmasını](https://docs.microsoft.com/rest/api/maps/route/getrouterange)destekler. Routable aralıkları hesaplanması da isochrones olarak bilinir. Bir başlangıç noktasından herhangi bir yöne seyahat edilebilir bir alanı kapsayan bir çokgen oluşturmayı gerektirir. Tüm zaman veya yakıt veya şarj miktarı belirli bir miktar altında.

Belgeleri yönlendirme için en iyi uygulamaları gözden [geçirin.](how-to-use-best-practices-for-routing.md)

## <a name="retrieve-a-map-image"></a>Harita görüntüsü alma

Azure Haritalar, statik harita görüntülerini veri üzerine yığıldıktan işlemek için bir API sağlar. Azure Haritalar'daki [Harita görüntüsü oluşturma](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile) API'si, Google Haritalar'daki statik harita API'si ile karşılaştırılabilir.

> [!NOTE]
> Azure Haritalar, "boylam,enlem" biçiminde koordinatlar için merkez, tüm işaretçi ve yol konumlarının gerekli olmasını gerektirir. Oysa, Google Haritalar "enlem, boylam" biçimini kullanır. Adreslerin önce coğrafi kodlanmış olması gerekir.

Tablo, Google Haritalar API parametrelerine Azure Haritalar'daki karşılaştırılabilir API parametreleriyle çapraz başvurularda bulunuyor.

| Google Haritalar API parametresi | Karşılaştırılabilir Azure Haritalar API parametresi  |
|---------------------------|--------------------------------------|
| `center`                    | `center`                           |
| `format`                    | `format`– URL yolunun bir parçası olarak belirtilir. Şu anda sadece PNG desteklenmiştir. |
| `key`                       | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                  | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `maptype`                   | `layer`ve `style` – [Bkz. Desteklenen harita stilleri](supported-map-styles.md) belgeleri. |
| `markers`                   | `pins`                             |
| `path`                      | `path`                             |
| `region`                    | *N/A* – Bu, coğrafi kodlama ile ilgili bir özelliktir. Azure `countrySet` Haritalar coğrafi kodlama API'sini kullanırken parametreyi kullanın.  |
| `scale`                     | *Yok*                              |
| `size`                      | `width`ve `height` – boyutu 8192x8192 kadar olabilir. |
| `style`                     | *Yok*                              |
| `visible`                   | *Yok*                              |
| `zoom`                      | `zoom`                             |

> [!NOTE]
> Azure Haritalar döşeme sisteminde, kutucuklar Google Haritalar'da kullanılan harita kutucuklarının iki katı büyüklüğündedir. Bu nedenle, Azure Haritalar'daki yakınlaştırma düzeyi değeri, Azure Haritalar'da Google Haritalar'a kıyasla bir yakınlaştırma düzeyi görünür. Bu farkı telafi etmek için, geçiş yaptığınız isteklerde yakınlaştırma düzeyini azat edin.

Daha fazla bilgi [için, harita görüntüsündeki Nasıl YapIlen kılavuzuna bakın.](how-to-render-custom-data.md)

Azure Haritalar işleme hizmeti, statik bir harita görüntüsü oluşturabilme özelliğinin yanı sıra, raster (PNG) ve vektör biçiminde harita kutucuklarına doğrudan erişme olanağı sağlar:

- [**Harita döşemesi**](https://docs.microsoft.com/rest/api/maps/render/getmaptile): Temel haritalar (yollar, sınırlar, arka plan) için raster (PNG) ve vektör karoları alın.
- [**Harita görüntüleri kutucuğu**](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile): Havadan ve uydu görüntü kutucuklarını alın.

> [!TIP]
> Birkaç yıl önce etkileşimli harita deneyimlerinden statik harita görüntülerine geçtiği birçok Google Haritalar uygulaması. Bu bir maliyet tasarrufu yöntemi olarak yapıldı. Azure Haritalar'da, Web SDK'daki etkileşimli harita denetimini kullanmak genellikle daha uygun maliyetlidir. Etkileşimli harita kontrol ücretleri, döşeme yüklerinin sayısını temel alar. Azure Haritalar'daki harita kutucukları büyüktür. Genellikle, statik bir harita yla aynı harita görünümünü yeniden oluşturmak için yalnızca birkaç döşeme gerekiyor. Harita döşemeleri tarayıcı tarafından otomatik olarak önbelleğe alınır. Bu nedenle, etkileşimli harita denetimi genellikle statik bir harita görünümünü çoğaltırken işlemin bir kısmını oluşturur. Kaydırma ve yakınlaştırma daha fazla döşeme yükler; ancak, bu davranışı devre dışı kılabilir için harita denetiminde seçenekler vardır. Etkileşimli harita denetimi de statik harita hizmetleri çok daha fazla görselleştirme seçenekleri sağlar.

### <a name="marker-url-parameter-format-comparison"></a>Marker URL parametre biçimi karşılaştırması

**Önce: Google Haritalar**

URL'deki parametreyi `markers` kullanarak işaretçiler ekleyin. `markers` Parametre, haritada işlenecek konumların bir stilini ve listesini aşağıda gösterildiği gibi bu stilde alır:

```
&markers=markerStyles|markerLocation1|markerLocation2|...
```

Ek stiller eklemek `markers` için, parametreleri URL'ye farklı bir stil ve konum kümesiyle kullanın.

"Enlem, boylam" biçiminde işaret konumlarını belirtin.

Bu "optionName1:value1 `optionName:value` \|\|optionName2:value2" gibi birden çok stilin pipe ( ) karakterleri yle ayrılmış biçimiyle işaretleyici stilleri ekleyin. Seçenek adları ve değerleri bir üst üste (:) ile ayrılır unutmayın. Google Haritalar'daki işaretçileri stillemek için stil seçeneğinin aşağıdaki adlarını kullanın:

- `color`– Varsayılan işaretsimgesinin rengi. 24 bit hex rengi (`0xrrggbb`) veya aşağıdaki değerlerden biri olabilir; `black`, `brown`, `green`, `purple`, `yellow`, `blue`, `gray`, `orange`, `red`, `white`.
- `label`– Simgenin üstünde görüntülenecek tek bir büyük alfasayısal karakter.
- `size`- İşaretleyicinin boyutu. Olabilir `tiny`, `mid`veya `small`.

Google Haritalar'daki Özel simgeler için aşağıdaki stil seçenekleri adlarını kullanın:

- `anchor`– Simge görüntüsünün koordinatla nasıl hizalanır olduğunu belirtir. Bir piksel (x,y) değeri veya aşağıdaki değerlerden biri olabilir; `top`, `bottom` `left`, `right` `center`, `topleft` `topright`, `bottomleft`, `bottomright`, , veya .
- `icon`– Simge resmine işaret eden bir URL.

Örneğin, boylamda haritaya kırmızı, orta ölçekli bir işaretçi ekleyelim: -110, enlem: 45:

```
&markers=color:red|size:mid|45,-110
```

<center>

![Google Haritalar işareti](media/migrate-google-maps-web-services/google-maps-marker.png)</center>

**Sonra: Azure Haritalar**

URL'deki parametreyi `pins` belirterek statik harita görüntüsüne işaretçiler ekleyin. Google Haritalar gibi, parametredeki konumların bir stilini ve listesini belirtin. `pins` Parametre, farklı stilleri olan işaretçileri desteklemek için birden çok kez belirtilebilir.

```
&pins=iconType|pinStyles||pinLocation1|pinLocation2|...
```

Ek stilleri kullanmak için, URL'ye farklı bir stil ve konum kümesiyle ek `pins` parametreler ekleyin.

Azure Haritalar'da pin konumunun "boylam enlemi" biçiminde olması gerekir. Google Haritalar "enlem, boylam" biçimini kullanır. Virgül değil, boşluk, Azure Haritalar biçiminde boylam ve enlem ayırır.

Oluşturulacak `iconType` pin türünü belirtir. Aşağıdaki değerlere sahip olabilir:

- `default`– Varsayılan pin simgesi.
- `none`– Hiçbir simge görüntülenmez, yalnızca etiketler işlenir.
- `custom`– Özel bir simge nin kullanılacağını belirtir. Simge resmine işaret eden bir URL, pin `pins` konum bilgilerinden sonra parametrenin sonuna eklenebilir.
- `{udid}`– Azure Haritalar Veri Depolama platformunda depolanan bir simge için Benzersiz Veri Kimliği (UDID).

`optionNameValue` Biçimle birlikte pin stilleri ekleyin. Birden çok stili pipe\|( ) karakterleri ile ayırın. Örneğin: `iconType|optionName1Value1|optionName2Value2`. Seçenek adları ve değerleri ayrılmaz. Stil işaretçileri için aşağıdaki stil seçeneği adlarını kullanın:

- `al`– İşaretleyicinin opaklığını (alfa) belirtir. 0 ile 1 arasında bir sayı seçin.
- `an`– Pim çapasını belirtir. X ve y piksel değerlerini "x y" biçiminde belirtin.
- `co`– Pimin rengi. 24 bit hex rengi `000000` belirtin: için `FFFFFF`.
- `la`– Etiket çapasını belirtir. X ve y piksel değerlerini "x y" biçiminde belirtin.
- `lc`– Etiketin rengi. 24 bit hex rengi `000000` belirtin: için `FFFFFF`.
- `ls`– Piksel olarak etiketboyutu. 0'dan büyük bir sayı seçin.
- `ro`– Simgeyi döndürmek için derece değeri. -360 ve 360 arasında bir sayı seçin.
- `sc`– Pin simgesi için bir ölçek değeri. 0'dan büyük bir sayı seçin.

Her pin konumu için etiket değerlerini belirtin. Bu yaklaşım, konumlar listesindeki tüm işaretçilere tek bir etiket değeri uygulamaktan daha etkilidir. Etiket değeri birden çok karakterden oluşan bir dize olabilir. Stil veya konum değeri olarak yanlış olmadığından emin olmak için dizeyi tek tırnak işaretleriyle sarın.

Aşağıda (15 50) konumlandırılmış "Space Needle" etiketiyle kırmızı (`FF0000`) varsayılan simgesi ekleyelim. Simge boylam: -122.349300, enlem: 47.620180:

```
&pins=default|coFF0000|la15 50||'Space Needle' -122.349300 47.620180
```

<center>

![Azure Haritalar işaretçisi](media/migrate-google-maps-web-services/azure-maps-marker.png)</center>

'1', '2' ve '3' etiket değerlerini içeren üç pin ekleyin:

```
&pins=default||'1'-122 45|'2'-119.5 43.2|'3'-121.67 47.12
```

<center>

![Azure Haritalar birden çok işaretçi](media/migrate-google-maps-web-services/azure-maps-multiple-markers.png)</center>

### <a name="path-url-parameter-format-comparison"></a>Yol URL parametre biçimi karşılaştırması

**Önce: Google Haritalar**

URL'deki parametreyi `path` kullanarak statik harita görüntüsüne satır ve çokgen ekleyin. `path` Parametre, aşağıda gösterildiği gibi, haritada işlenecek yerlerin bir stili ni ve listesini alır:

```
&path=pathStyles|pathLocation1|pathLocation2|...
```

URL'ye farklı `path` bir stil ve konum kümesiyle ek parametreler ekleyerek ek stiller kullanın.

Yol konumları `latitude1,longitude1|latitude2,longitude2|…` biçimiyle belirtilir. Yollar kodlanabilir veya puan adresleri içerebilir.

`optionName:value` Biçimi ile yol stilleri ekleyin, boru (\|) karakterleri tarafından birden çok stilleri ayırın. Ve, bir iki nokta üst üste (:) ile ayrı seçenek adları ve değerleri. Bunun gibi: `optionName1:value1|optionName2:value2`. Aşağıdaki stil seçeneği adları, Google Haritalar'daki yolları stillemek için kullanılabilir:

- `color`– Yolun rengi veya çokgen anahat. 24 bit hex rengi (`0xrrggbb`), 32-bit hex`0xrrggbbbaa`renk ( ) veya aşağıdaki değerlerden biri olabilir: siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `fillColor`– Yol alanını (çokgen) dolduracak renk. 24 bit hex rengi (`0xrrggbb`), 32-bit hex`0xrrggbbbaa`renk ( ) veya aşağıdaki değerlerden biri olabilir: siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `geodesic`– Yolun yeryüzünün eğriliğini takip eden bir çizgi olup olmadığını gösterir.
- `weight`– Yol çizgisinin piksel kalınlığında.

URL parametresinde koordinatlar arasındaki haritaya kırmızı çizgi opaklığı ve piksel kalınlığı ekleyin. Aşağıdaki örnekte, çizgi %50 donukluk ve dört piksel kalınlığındadır. Koordinatlar boylam: -110, enlem: 45 ve boylam: -100, enlem: 50.

```
&path=color:0xFF000088|weight:4|45,-110|50,-100
```

<center>

![Google Haritalar polyline](media/migrate-google-maps-web-services/google-maps-polyline.png)</center>

**Sonra: Azure Haritalar**

URL'deki parametreyi belirterek statik harita resmine `path` çizgiler ve çokgenler ekleyin. Google Haritalar gibi, bu parametredeki bir stil ve konum listesi belirtin. Farklı `path` stillesahip birden çoklu daireleri, çizgileri ve çokgenleri işlemek için parametreyi birden çok kez belirtin.

```
&path=pathStyles||pathLocation1|pathLocation2|...
```

Yol konumları söz konusu olduğunda, Azure Haritalar koordinatların "boylam enlemi" biçiminde olmasını gerektirir. Google Haritalar "enlem, boylam" biçimini kullanır. Virgül değil, boşluk, Azure Haritalar biçiminde boylam ve enlem ayırır. Azure Haritalar, puan için kodlanmış yolları veya adresleri desteklemez. [Burada](how-to-render-custom-data.md#get-data-from-azure-maps-data-storage)belgelendiği gibi Azure Haritalar Veri Depolama API'sine GeoJSON dosyası olarak daha büyük veri kümelerini yükleyin.

`optionNameValue` Biçimle yol stilleri ekleyin. Bu gibi, boru\|( ) karakterleri, ayrı birden çok stilleri `optionName1Value1|optionName2Value2`. Seçenek adları ve değerleri ayrılmaz. Azure Haritalar'da yolları stiloluşturmak için aşağıdaki stil seçeneği adlarını kullanın:

- `fa`- Çokgenleri işlerken kullanılan dolgu rengi opaklığı (alfa). 0 ile 1 arasında bir sayı seçin.
- `fc`- Çokgenin alanını işlemek için kullanılan dolgu rengi.
- `la`– Çizgileri ve çokgenlerin anahatlarını işlerken kullanılan çizgi rengi opaklığı (alfa). 0 ile 1 arasında bir sayı seçin.
- `lc`– Çizgileri işlemek için kullanılan çizgi rengi ve çokgenlerin anahatları.
- `lw`– Piksel olarak çizgigenişliği.
- `ra`– Bir daire yarıçapını metre olarak belirtir.

URL parametresinde koordinatlar arasına kırmızı çizgi opaklığı ve piksel kalınlığı ekleyin. Aşağıdaki örnekte, çizgi %50 donukluk ve dört piksel kalınlığındadır. Koordinatlar aşağıdaki değerlere sahiptir: boylam: -110, enlem: 45 ve boylam: -100, enlem: 50.

```
&path=lcFF0000|la.5|lw4||-110 45|-100 50
```

<center>

![Azure Haritalar polyline](media/migrate-google-maps-web-services/azure-maps-polyline.png)</center>

## <a name="calculate-a-distance-matrix"></a>Uzaklık matrisini hesaplama

Azure Haritalar, mesafe matrisi API'sini sağlar. Bu API'yi, bir mesafe matrisi ile bir konum kümesi arasındaki seyahat sürelerini ve mesafeleri hesaplamak için kullanın. Google Haritalar'daki mesafe matris API'si ile karşılaştırılabilir.

- [**Rota matrisi**](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview): Bir dizi kaynak ve varış noktası için seyahat sürelerini ve mesafeleri eşzamanlı olarak hesaplar. İstek başına 700'e kadar hücreyi destekler. Bu, hedef sayısıyla çarpılarak kaynak sayısıdır. Bu kısıtlama göz önünde bulundurularak, olası matris boyutlarıörnekleri şunlardır: 700x1, 50x10, 10x10, 28x25, 10x70.

> [!NOTE]
> Mesafe matris api için bir istek sadece istek gövdesinde kaynak ve hedef bilgileri ile bir POST isteği kullanılarak yapılabilir. Ayrıca, Azure Haritalar tüm kökenlerin ve hedeflerin eşgüdümlü olmasını gerektirir. Adreslerin önce coğrafi kodlanmış olması gerekir.

Bu tablo, Google Haritalar API parametrelerine karşılaştırılabilir Azure Haritalar API parametreleri ile çapraz başvurular.

| Google Haritalar API parametresi      | Karşılaştırılabilir Azure Haritalar API parametresi  |
|--------------------------------|--------------------------------------|
| `arrivial_time`                | `arriveAt`                           |
| `avoid`                        | `avoid`                              |
| `depature_time`                | `departAt`                           |
| `destinations`                 | `destination`- GeoJSON olarak POST istek gövdesinde belirtin. |
| `key`                          | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın. |
| `language`                     | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.  |
| `mode`                         | `travelMode`                         |
| `origins`                      | `origins`- GeoJSON olarak POST istek gövdesinde belirtin.  |
| `region`                       | *N/A* – Bu özellik coğrafi kodlama ile ilgilidir. Azure `countrySet` Haritalar coğrafi kodlama API'sini kullanırken parametreyi kullanın. |
| `traffic_model`                | *N/A* – Yalnızca trafik verilerinin `traffic` parametreile birlikte kullanIlip kullanılmayabileceğini belirtebilirsiniz. |
| `transit_mode`                 | *N/A* - Transit tabanlı mesafe matrisleri şu anda desteklenmez.  |
| `transit_routing_preference`   | *N/A* - Transit tabanlı mesafe matrisleri şu anda desteklenmez.  |
| `units`                        | *N/A* – Azure Haritalar yalnızca metrik sistemi kullanır. |

> [!TIP]
> Azure Haritalar yönlendirme API'sinde bulunan tüm gelişmiş yönlendirme seçenekleri Azure Haritalar mesafe matris apisinde desteklenir. Gelişmiş yönlendirme seçenekleri şunlardır: kamyon yönlendirme, motor özellikleri, ve benzeri.

Belgeleri yönlendirme için en iyi uygulamaları gözden [geçirin.](how-to-use-best-practices-for-routing.md)

## <a name="get-a-time-zone"></a>Saat dilimi alma

Azure Haritalar, bir koordinatın saat dilimini almak için bir API sağlar. Azure Haritalar saat dilimi API'si, Google Haritalar'daki saat dilimi API'si ile karşılaştırılabilir:

- [**Koordinat ile saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebycoordinates): Koordinat belirtin ve koordinatın saat dilimi ayrıntılarını alın.

Bu tablo, Google Haritalar API parametrelerine Azure Haritalar'daki karşılaştırılabilir API parametreleriyle çapraz başvurularda bulunuyor.

| Google Haritalar API parametresi | Karşılaştırılabilir Azure Haritalar API parametresi   |
|---------------------------|---------------------------------------|
| `key`                       | `subscription-key`– Azure Haritalar belgelerinde [kimlik doğrulamaya](azure-maps-authentication.md) da bakın.       |
| `language`                  | `language`– [Desteklenen diller](supported-languages.md) belgelerine bakın.    |
| `location`                  | `query`             |
| `timestamp`                 | `timeStamp`         |

Bu API'ye ek olarak, Azure Haritalar bir dizi saat dilimi API'si sağlar. Bu API'ler saati saat diliminin adlarına veya dlerine göre dönüştürür:

- [**ID'ye göre saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebyid): Belirtilen IANA saat dilimi kimliği için geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini verir.
- [**Saat dilimi Enum IANA**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumiana): IANA saat dilimi tam listesini verir. IANA hizmetindeki güncellemeler bir gün içinde sisteme yansıtılır.
- [**Saat dilimi Enum Windows**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumwindows): Windows Saat Dilimi tam listesini verir.
- [**Saat dilimi IANA sürümü**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneianaversion): Azure Haritalar tarafından kullanılan geçerli IANA sürüm numarasını verir.
- [**Windows'dan IANA'ya Saat Dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonewindowstoiana): Geçerli bir Windows Saat Dilimi Kimliği verilen karşılık gelen bir IANA kimliğini verir. Tek bir Windows Kimliği için birden çok IANA kimliği döndürülebilir.

## <a name="client-libraries"></a>İstemci kitaplıkları

Azure Haritalar, aşağıdaki programlama dilleri için istemci kitaplıkları sağlar:

- JavaScript, TypeScript, Node.js – [belgeler](how-to-use-services-module.md) \| [NPM paketi](https://www.npmjs.com/package/azure-maps-rest)

Bu Açık kaynak istemci kitaplıkları diğer programlama dilleri içindir:

- .NET Standard 2.0 – [GitHub projesi](https://github.com/perfahlen/AzureMapsRestServices) \| [NuGet paketi](https://www.nuget.org/packages/AzureMapsRestToolkit/)

## <a name="additional-resources"></a>Ek kaynaklar

Aşağıda Azure Haritalar REST hizmetleri için ek belgeler ve kaynaklar ve bilgiler veremetleri yer alır.

- [Arama için en iyi yöntemler](how-to-use-best-practices-for-search.md)
- [Adres arama](how-to-search-for-address.md)
- [Yönlendirme için en iyi uygulamalar](how-to-use-best-practices-for-routing.md)
- [Azure Haritalar REST Hizmet API başvuru belgeleri](https://docs.microsoft.com/rest/api/maps/)

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar REST hizmetleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmak için en iyi uygulamalar](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Yönlendirme hizmetini kullanmak için en iyi uygulamalar](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Hizmetler modülü nasıl kullanılır (Web SDK)](how-to-use-best-practices-for-routing.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
