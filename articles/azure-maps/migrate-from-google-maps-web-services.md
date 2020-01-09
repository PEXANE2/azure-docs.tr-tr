---
title: Web hizmetlerini Google Maps 'tan geçirme | Microsoft Docs
description: Web hizmetlerini Google Maps 'tan Microsoft Azure Maps 'a geçirmeye yönelik bir öğretici.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 8fc3270d15f77576ff353a110d8a3a9a17c7144d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480027"
---
# <a name="migrate-web-service-from-google-maps"></a>Web hizmetini Google Maps 'tan geçirme

Hem Azure hem de Google Maps, REST Web Hizmetleri aracılığıyla uzamsal API 'lere erişim sağlar. Bu platformların API arabirimleri benzer işlevler gerçekleştirir, ancak farklı adlandırma kuralları ve yanıt nesneleri kullanır.

Aşağıdaki tabloda, listelenen Google Maps hizmeti API 'Lerinde benzer işlevler sağlayan Azure Maps hizmeti API 'Leri sağlanmaktadır.

| Google Maps hizmeti API 'SI | Azure haritalar hizmeti API 'SI                                                                      |
|-------------------------|---------------------------------------------------------------------------------------------|
| Yol tarifleri              | [Yolu](https://docs.microsoft.com/rest/api/maps/route)                               |
| Mesafe Matrisi         | [Yol matrisi](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) |
| Coğrafi kodlama               | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Konum arama           | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Otomatik tamamlamayı yerleştir      | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Statik eşleme              | [İşleme](https://docs.microsoft.com/rest/api/maps/render/getmapimage)                 |
| Saat Dilimi               | [Saat Dilimi](https://docs.microsoft.com/rest/api/maps/timezone)                        |

Aşağıdaki hizmet API 'Leri Şu anda Azure haritalar 'da kullanılabilir değil:

- Nedeniyle
- Coğrafi Konum
- Ayrıntıları ve fotoğrafları yerleştir. Azure haritalar arama API 'sinde telefon numaraları ve Web sitesi URL 'SI mevcuttur.
- Harita URL 'Leri
- Yollar – hız sınırı verileri Azure Maps 'taki Route ve ters coğrafi kodlama API 'Leri aracılığıyla sağlanır.
- Statik cadde görünümü

Azure haritalar, ilgi çekici olabilecek birkaç ek REST Web hizmetine sahiptir:

- [Uzamsal işlemler](https://docs.microsoft.com/rest/api/maps/spatial): bir hizmete bölge sınırlaması gibi karmaşık uzamsal hesaplamalar ve işlemler için yük boşaltma.
- [Trafik](https://docs.microsoft.com/rest/api/maps/traffic): gerçek zamanlı trafik akışına ve olay verilerine erişin.

## <a name="geocoding-addresses"></a>Coğrafi kodlama adresleri

Coğrafi kodlama, bir adresi ("1 Microsoft Way, Redmond, WA" gibi) bir koordine (Boylam:-122,1298, Latitude: 47,64005 gibi) dönüştürme işlemidir. Koordinatlar daha sonra bir haritada bir işaretçiyi konumlandırmak veya Haritayı ortalamak için kullanılır.

Azure Maps, coğrafi kodlama adresleri için çeşitli yöntemler sağlar:

- [**Serbest biçimli adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress): tek bir adres dizesi belirtin ("1 Microsoft Way, Redmond, WA" gibi) ve isteği hemen işleyin. Tek tek adreslere hızla coğrafi kod eklemeniz gerekiyorsa bu önerilir.
- [**Yapılandırılmış adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressstructured): sokak adı, şehir, ülke ve posta kodu gibi tek bir adresin parçalarını belirtin ve isteği hemen işleyin. Tek tek adreslerin hızla coğrafi olarak ve verilerin zaten tek tek adres bölümlerine ayrıştırılabilmeniz durumunda bu önerilir.
- [**Toplu iş adresi coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatchpreview): en fazla 10.000 adresi içeren ve bunların bir süre içinde işlenmesini içeren bir istek oluşturun. Tüm adresler, sunucuda paralel olarak coğrafi olarak kodlanmıştır ve tamamlandığında tam sonuç kümesi indirilecektir. Bu, büyük veri kümelerine coğrafi kodlama için önerilir.
- [**Benzer arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlamasını ilgi çekici arama noktasıyla birleştirir. Bu API, bir adres, yer, yer işareti, ilgi alanı veya ilgi alanı kategorisi olabilecek bir serbest biçimli dize alır ve isteği hemen işleyebilir. Bu API, kullanıcıların aynı metin kutusundan adres veya ilgi çekici noktaları araybilecekleri uygulamalar için önerilir.
- [**Benzer toplu işlem araması**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): en fazla 10.000 adres, yer, yer işareti veya ilgi alanı içeren ve bunların bir süre içinde işlenmesini içeren bir istek oluşturun. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilecektir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `address`                   | `query`                            |
| `bounds`                    | `topLeft` ve `btmRight`           |
| `components`                | `streetNumber`<br/>`streetName`<br/>`crossStreet`<br/>`postalCode`<br/>`municipality`-şehir/şehir<br/>`municipalitySubdivision` – komşuları, alt/süper şehir<br/>`countrySubdivision` eyalet veya il<br/>`countrySecondarySubdivision` İlçesi<br/>`countryTertiarySubdivision`-District<br/>`countryCode`-iki harfli ülke kodu |
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `region`                    | `countrySet`                       |

Arama hizmetinin nasıl kullanılacağına ilişkin bir örnek [burada](how-to-search-for-address.md)belgelenmiştir. [Arama belgeleri için en iyi uygulamaları](how-to-use-best-practices-for-search.md) gözden geçirdiğinizden emin olun.

> [!TIP]
> Serbest biçimli adres coğrafi kodlama ve benzer arama API 'Leri, istek URL 'sine `&amp;typeahead=true` eklenerek otomatik tamamlama modunda kullanılabilir. Bu durum, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu ve tahmine dayalı moda gideceklerini söyler.

## <a name="reverse-geocode-a-coordinate"></a>Bir koordinatı tersine çevrilmiş coğrafi kod

Ters coğrafi kodlama, coğrafi koordinatları (Boylam:-122,1298, Latitude: 47,64005 gibi) yaklaşık adrese ("1 Microsoft Way, Redmond, WA" gibi) dönüştürme işlemidir.

Azure haritalar, birkaç ters coğrafi kodlama yöntemi sağlar:

- [**Adres ters coğrafi bölge**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse): yaklaşık adresini almak ve isteği hemen işlemek için tek bir coğrafi koordinat belirtin.
- [**Çapraz cadde ters Geocoder**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet): yakın çapraz cadde bilgilerini almak için tek bir coğrafi koordinat belirtin (örneğin, & 1. ana) ve isteği hemen işleyin.
- [**Batch adresi ters coğrafi makinesi**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatchpreview): en fazla 10.000 koordinat içeren bir istek oluşturun ve bunların bir süre içinde işlenmesini sağlayabilirsiniz. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilecektir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi   | Karşılaştırılabilir Azure Maps API parametresi   |
|-----------------------------|---------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `latlng`                    | `query`  |
| `location_type`             | *Yok*     |
| `result_type`               | `entityType`    |

[Arama belgeleri için en iyi uygulamaları](how-to-use-best-practices-for-search.md) gözden geçirdiğinizden emin olun.

Azure haritalar ters coğrafi kodlama API 'SI, Google Maps 'ta, uygulamanızı geçirirken tümleştirilebilen yararlı olabilecek bazı ek özelliklere sahiptir:

- Hız sınırı verilerini alın.
- Yol kullanım bilgilerini alma: yerel yol, arterial, sınırlı erişim, rampa, vb.
- Koordinat, koordinat tarafında yer alır.

## <a name="search-for-points-of-interest"></a>İlgi noktası arama

İlgi alanı arama API 'sini kullanarak Google Maps 'ta ilgi çekici veriler aranabilir. Bu API, ilgilendiğiniz noktaları aramak için üç farklı yol sağlar:

- **Metinden yer bulun:** Adını, adresini veya telefon numarasını temel alarak bir ilgi alanı arar.
- **Yakın arama**: bir konumun belirli bir uzaklıkta yer alan ilgi alanlarını arar.
- **Metin arama:** İlgi noktası ve konum bilgisi içeren serbest biçimli bir metin kullanarak yerleri arar. Örneğin, "New York 'ta pizza" veya "ana St yakınında" Restoranlar ".

Azure Maps, ilgi noktaları için çeşitli arama API 'Leri sağlar:

- [**POI arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi): ada göre ilgi alanları için arama yapın. Örneğin, "Starbuları".
- [**POI kategorisi arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory): kategoriye göre ilgi alanları için arama yapın. Örneğin, "Restoran".
- [**Yakın arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby): bir konumun belirli bir uzaklıkta yer alan ilgi alanlarını arar.
- [**Benzer arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlamasını ilgi çekici arama noktasıyla birleştirir. Bu API, bir adres, yer, yer işareti, ilgi alanı veya ilgi alanı kategorisi olabilecek bir serbest biçimli dize alır ve isteği hemen işleyebilir. Bu API, kullanıcıların aynı metin kutusundan adres veya ilgi çekici noktaları araybilecekleri uygulamalar için önerilir.
- [**Geometri dahilinde ara**](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry): belirtilen geometri (çokgen) içinde olan ilgi alanlarını arayın.
- [**Yol boyunca ara**](https://docs.microsoft.com/rest/api/maps/search/postsearchalongroute): belirtilen yol yolu boyunca ilgi alanları için arama yapın.
- [**Benzer toplu işlem araması**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): en fazla 10.000 adres, yer, yer işareti veya ilgi alanı içeren ve bunların bir süre içinde işlenmesini içeren bir istek oluşturun. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilecektir.

Azure haritalar 'ın, Google Maps metin arama API 'sine yönelik karşılaştırılabilir bir API 'SI yok.

> [!TIP]
> POı araması, POı kategorisi araması ve benzer arama API 'Leri, istek URL 'sine `&amp;typeahead=true` eklenerek otomatik tamamlama modunda kullanılabilir. Bu durum, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu ve tahmine dayalı moda gideceklerini söyler.

[Arama belgeleri için en iyi uygulamaları](how-to-use-best-practices-for-search.md) gözden geçirdiğinizden emin olun.

### <a name="find-place-from-text"></a>Metinden yer bulun

Ad veya adrese göre ilgi alanlarını aramak için Azure Maps [POI araması](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) ve [benzer arama](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API 'leri kullanılabilir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi |
|---------------------------|-------------------------------------|
| `fields`                  | *Yok*                               |
| `input`                   | `query`                             |
| `inputtype`               | *Yok*                               |
| `key`                     | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `locationbias`            | `lat`, `lon` ve `radius`<br/>`topLeft` ve `btmRight`<br/>`countrySet`  |

### <a name="nearby-search"></a>Yakındaki arama

Yakın ilgi alanları, [yakındaki arama](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) API 'Si kullanılarak Azure haritalar 'da alınabilir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `keyword`                   | `categorySet` ve `brandSet`        |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `location`                  | `lat` ve `lon`                     |
| `maxprice`                  | *Yok*                               |
| `minprice`                  | *Yok*                               |
| `name`                      | `categorySet` ve `brandSet`        |
| `opennow`                   | *Yok*                               |
| `pagetoken`                 | `ofs` ve `limit`                   |
| `radius`                    | `radius`                            |
| `rankby`                    | *Yok*                               |
| `type`                      | `categorySet –` bkz. [desteklenen arama kategorileri](supported-search-categories.md) belgeleri.   |

## <a name="calculate-routes-and-directions"></a>Rotaları ve yönleri hesapla

Azure haritalar, yolları ve yönergeleri hesaplamak için kullanılabilir. Azure haritalar, Google Maps yönlendirme hizmetiyle aynı işlevselliklerin çoğuna sahiptir, örneğin:

- varış ve ayrılma süreleri.
- gerçek zamanlı ve tahmine dayalı trafik rotaları.
- farklı ulaşım modları; itici, yürüyen, Bicycling.

> [!NOTE]
> Azure haritalar tüm waypoints 'in koordinatlarını gerektirir. Önce adreslerin coğrafi kodlanmış olması gerekir.

Azure haritalar yönlendirme hizmeti, yolları hesaplamak için aşağıdaki API 'Leri sağlar:

- [**Rotayı hesapla**](https://docs.microsoft.com/rest/api/maps/route/getroutedirections): bir rotayı hesaplayın ve isteğin hemen işlenmesini sağlayabilirsiniz. Bu API hem GET hem POST isteklerini destekler. Çok sayıda waypoints belirtildiğinde veya URL isteğinin çok uzun olmadığından ve sorunlara yol açmamasına yetecek kadar fazla yol seçeneği kullanılırken POST istekleri önerilir.
- [**Batch rotası**](https://docs.microsoft.com/rest/api/maps/route/postroutedirectionsbatchpreview): en fazla 1.000 yol isteği içeren bir istek oluşturun ve bunların bir süre içinde işlenmesini isteyin. Tüm veriler sunucuda paralel olarak işlenir ve tamamlandığında tam sonuç kümesi indirilecektir.
- [**Mobility Hizmetleri**](https://docs.microsoft.com/rest/api/maps/mobility): genel aktarım kullanarak rotaları ve yönergeleri hesaplayın.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi    | Karşılaştırılabilir Azure Maps API parametresi  |
|------------------------------|--------------------------------------|
| `alternatives`                 | `maxAlternatives`                  |
| `arrival_time`                | `arriveAt`                          |
| `avoid`                        | `avoid`                            |
| `departure_time`              | `departAt`                          |
| `destination`                  | `query`: `"lat0,lon0:lat1,lon1…."` biçimde koordinatları  |
| `key`                          | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                     | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.   |
| `mode`                         | `travelMode`                       |
| `optimize`                     | `computeBestOrder`                 |
| `origin`                       | `query`                            |
| `region`                       | *Yok* – bu, coğrafi kodlama ile ilgili bir özelliktir. Azure Haritalar Coğrafi Kodlama API 'sini kullanırken *Countryset* parametresini kullanın.  |
| `traffic_model`               | *Yok* – *trafik parametresi yalnızca trafik parametresiyle* birlikte kullanılması gerekiyorsa belirtilebilir. |
| `transit_mode`                | [Mobility Hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `transit_routing_preference` | [Mobility Hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `units`                        | *Yok* – Azure Maps yalnızca ölçüm sistemini kullanır.  |
| `waypoints`                    | `query`                            |

> [!TIP]
> Varsayılan olarak, Azure Maps Route API 'SI yalnızca bir Özet (uzaklık ve saatler) ve yol yolunun koordinatlarını döndürür. Geri alma yönergelerini almak için `instructionsType` parametresini kullanın. `routeRepresentation` parametresi Özet ve yol yolunu filtrelemek için kullanılabilir.

Azure haritalar yönlendirme API 'SI, Google Maps 'ta, uygulamanızı geçirirken tümleştirilebilen yararlı olabilecek birçok ek özelliğe sahiptir:

- Rota türü desteği: en kısa, en hızlı, trilini ve en çok yakıt etkin.
- Ek seyahat modları için destek: Bus, motocycle, Taxi, kamyon ve Van.
- 150 waypoints desteği.
- Tek bir istekte birden çok seyahat süresi hesaplama; geçmiş trafik, canlı trafik, trafik yok.
- Ek yol türlerinden kaçının: Carpool yol, geri alınamaz yollar, zaten kullanılan yollar.
- Kaçınmak için özel alan belirtin.
- Yolun Ascend ile olan yükseltmesini sınırlayın.
- Altyapı belirtimi tabanlı yönlendirme. Kalan yakıt/ücret ve altyapı belirtimlerini temel alarak, combustion veya elektrik taşıtlar için rotaları hesaplayın.
- Ticari araç yönlendirme parametresi desteği; araç boyutları, ağırlığı, Axler sayısı ve kargo türü.
- Maksimum araç hızını belirtin.

Buna ek olarak, Azure haritalar 'daki yönlendirme hizmeti Ayrıca, belirli bir süre veya yakıt/ücret miktarı dahilinde bir kaynak noktasından herhangi bir yöne doğru bir şekilde bir alan kaplayan bir çokgen oluşturan, [yönlendirilebilir aralıkları hesaplamayı](https://docs.microsoft.com/rest/api/maps/route/getrouterange)da destekler.

## <a name="retrieve-a-map-image"></a>Harita görüntüsünü al

Azure Maps, çakışan verilerle statik harita görüntülerini işlemek için bir API sağlar. Azure haritalar [harita görüntü işleme](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile) API 'Si, Google Maps 'taki statik harita API 'siyle karşılaştırılabilir.

> [!NOTE]
> Azure haritalar, "boylam, Latitude" biçiminde koordinat, tüm işaretleyici ve yol konumlarının "enlem, Boylam" biçimini kullanması gerekir. Önce adreslerin coğrafi kodlanmış olması gerekir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `center`                    | `center`                           |
| `format`                    | `format`: URL yolunun bir parçası olarak belirtilir. Şu anda yalnızca PNG destekleniyor. |
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `maptype`                   | `layer` ve `style` – [desteklenen harita stilleri](supported-map-styles.md) belgelerine bakın. |
| `markers`                   | `pins`                             |
| `path`                      | `path`                             |
| `region`                    | *Yok* – bu, coğrafi kodlama ile ilgili bir özelliktir. Azure Maps coğrafi kodlama API 'sini kullanırken `countrySet` parametresini kullanın.  |
| `scale`                     | *Yok*                              |
| `size`                      | `width` ve `height` – boyutu 8192x8192 olabilir. |
| `style`                     | *Yok*                              |
| `visible`                   | *Yok*                              |
| `zoom`                      | `zoom`                             |

> [!NOTE]
> Azure haritalar, Google Maps 'ta kullanılan harita kutucukları boyutunun iki katı olan kutucukları olan bir kutucuk sistemi kullanır. Bu nedenle, Azure haritalar 'daki yakınlaştırma düzeyi değeri, Google Maps ile karşılaştırıldığında Azure Maps 'ta bir yakınlaştırma düzeyi daha yakından görünecektir. Bunun için telafi eden isteklerindeki yakınlaştırma düzeyini düşürün.

Daha fazla bilgi için [harita görüntü Işleme API 'Sindeki nasıl yapılır kılavuzuna](how-to-render-custom-data.md)bakın.

Azure haritalar işleme hizmeti, statik bir harita görüntüsü oluşturmaya ek olarak tarama (PNG) ve vektör biçimindeki harita kutucuklarına doğrudan erişme özelliği de sağlar:

- [**Harita kutucuğu**](https://docs.microsoft.com/rest/api/maps/render/getmaptile): temel haritalar (yollar, sınırlar, arka plan) için raster (png) ve vektör kutucukları alma.
- [**Harita Imagery kutucuğu**](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile): havadan ve uydu görüntüsü al kutucukları alın.

> [!TIP]
> Birçok Google harita, etkileşimli eşleme deneyimlerinden, bir maliyet tasarrufu yöntemi olarak birkaç yıl önce olan statik harita görüntülerine yapılan uygulamalardan eşleşir. Azure haritalar 'da, harita kutucuk yüklerini temel alan, Web SDK 'sında etkileşimli harita denetimini kullanmak genellikle çok daha uygun maliyetli bir maliyettir. Azure haritalar 'daki harita kutucukları büyüktür ve genellikle aynı harita görünümünü statik bir harita olarak yeniden oluşturmak için birkaç kez ve eşleme kutucukları tarayıcı tarafından otomatik olarak önbelleğe alınır. Bu nedenle, etkileşimli harita denetimi genellikle bir statik harita görünümü çoğaltma yaparken bir işlemin bir bölümünü oluşturur. Yatay kaydırma ve yakınlaştırma daha fazla kutucuk yükler, ancak isterseniz harita denetiminde bu davranışı devre dışı bırakmak için seçenekler vardır. Etkileşimli harita denetimi, statik eşleme hizmetlerinden çok daha fazla görselleştirme seçeneği de sağlar.

### <a name="marker-url-parameter-format-comparison"></a>İşaret URL 'SI parametre biçimi karşılaştırması

**Önce: Google Maps**

Google Maps işaretçileri ' nde, URL 'deki `markers` parametresi kullanılarak bir statik harita görüntüsüne eklenebilir. `markers` parametresi, aşağıda gösterildiği gibi, bu stille birlikte haritada oluşturulacak konumların bir stilini ve bir listesini alır:

```
&markers=markerStyles|markerLocation1|markerLocation2|...
```

Ek stiller, URL 'ye farklı bir stille ve konum kümesiyle ek `markers` parametreler eklenerek kullanılabilir.

İşaret konumları "enlem, Boylam" biçimiyle belirtilir.

Google Maps 'daki işaret stilleri `optionName:value`biçim ile eklenir ve bu "optionName1: değer1\|optionName2: değer2" gibi kanal (\|) karakterleriyle ayrılmış birden çok stil vardır. Seçenek adları ve değerleri, iki nokta üst üste (:)) ayrılır. Aşağıdaki stil seçenek adları, Google Maps 'daki stil işaretçileri için kullanılabilir:

- `color`: varsayılan işaret simgesinin rengi. 24 bit onaltılı renk (`0xrrggbb`) veya aşağıdaki değerlerden biri olabilir; `black`, `brown`, `green`, `purple`, `yellow`, `blue`, `gray`, `orange`, `red`, `white`.
- `label`: simgenin üstünde görüntülenecek tek bir büyük harf alfasayısal karakter.
- `size`-işaretin boyutu. `tiny`, `mid`veya `small`olabilir.

Özel simgeler, aşağıdaki stil seçenek adları kullanılarak Google Maps 'ta de kullanılabilir:

- `anchor` – simge resminin koordinatya nasıl hizalanacağını belirtir. Bir piksel (x, y) değeri veya aşağıdaki değerlerden biri olabilir; `top`, `bottom`, `left`, `right`, `center`, `topleft`, `topright`, `bottomleft`veya `bottomright`.
- `icon`: simge resmine işaret eden bir URL.

Örneğin, Google Maps 'ta haritaya, koordinatlara (Boylam:-110, Latitude: 45) aşağıdaki URL parametresi ile kırmızı ve orta ölçekli bir işaret eklenebilir:

```
&markers=color:red|size:mid|45,-110
```

<center>

![Google Maps işaretleyici](media/migrate-google-maps-web-services/google-maps-marker.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar işaretçileri, URL 'de `pins` parametresi belirtilerek statik bir harita görüntüsüne de eklenebilir. Google Maps gibi, bu parametrede bir stil ve konumların listesi belirlenebilir ve farklı stillerdeki işaretçileri desteklemek için `pins` parametresi birden çok kez belirtilebilir.

```
&pins=iconType|pinStyles||pinLocation1|pinLocation2|...
```

Ek stiller, URL 'ye farklı bir stille ve konum kümesiyle ek `pins` parametreler eklenerek kullanılabilir.

Azure haritalar, PIN konumlarına geldiğinde, "enlem, Boylam" biçimini kullanarak koordinatların "Boylam Enlem" biçiminde olmasını gerektirir. Ayrıca, Azure Maps 'ta boylam ve enlem bir virgülle ayırarak bir boşluk olduğunu unutmayın.

`iconType` değeri, oluşturulacak PIN türünü belirtir ve aşağıdaki değerlere sahip olabilir:

- `default` – varsayılan pin simgesi.
- `none` – hiçbir simge gösterilmez, yalnızca Etiketler işlenir.
- `custom` – kullanılacak özel bir simge belirtir. Simge resmine işaret eden bir URL, PIN konum bilgileri sonrasında `pins` parametresinin sonuna eklenebilir.
- `{udid}`: Azure Maps veri depolama platformunda depolanan bir simgenin benzersiz veri KIMLIĞI (UDıD).

Azure haritalar 'daki PIN stilleri, bu `iconType|optionName1Value1|optionName2Value2`gibi kanal (\|) karakterleriyle ayırarak birden çok stil `optionNameValue`biçim ile eklenir. Seçenek adları ve değerleri ayrılmadığını not edin. Aşağıdaki stil seçenek adları, Azure haritalar 'daki stil işaretçileri için kullanılabilir:

- `al` – işaretin opaklığını (Alpha) belirtir. 0 ile 1 arasında bir sayı olabilir.
- `an` – pin bağlayıcısını belirtir. "X y" biçiminde belirtilen X ve y piksel değerleri.
- `co` – PIN rengi. 24 bit onaltılık renk olmalıdır: `FFFFFF``000000`.
- `la` – etiket bağlayıcısını belirtir. "X y" biçiminde belirtilen X ve y piksel değerleri.
- `lc`: etiketin rengi. Bir 24-, onaltılık renk olmalıdır: `FFFFFF``000000`.
- `ls`: etiketin piksel cinsinden boyutu. 0 ' dan büyük bir sayı olabilir.
- `ro`: simgeyi döndürmek için derece cinsinden bir değer. -360 ile 360 arasında bir sayı olabilir.
- `sc`: pin simgesi için bir ölçek değeri. 0 ' dan büyük bir sayı olabilir.

Etiket değerleri, konumlar listesindeki tüm işaretçiler için geçerli olan tek bir etiket değerine sahip olmak yerine her PIN konumu için belirtilir. Etiket değeri birden fazla karakterden oluşan bir dize olabilir ve bir stil veya konum değeri olarak yanlış alınacağından emin olmak için tek tırnak işaretleriyle sarmalanabilir.

Örneğin, Azure Maps 'ta, "Space Iğne" etiketiyle kırmızı (`FF0000`) varsayılan simge ekleme (15 50) koordinatlarındaki simgenin (Boylam:-122,349300, Latitude: 47,620180) aşağıdaki URL parametresiyle yapılabilir:

```
&pins=default|coFF0000|la15 50||'Space Needle' -122.349300 47.620180
```

<center>

![Azure haritaları işaretçisi](media/migrate-google-maps-web-services/azure-maps-marker.png)</center>

Aşağıdaki örnek, ' 1 ', ' 2 ' ve ' 3 ' Etiket değerleriyle üç PIN ekler:

```
&pins=default||'1'-122 45|'2'-119.5 43.2|'3'-121.67 47.12
```

<center>

![Azure haritalarını çoklu işaretleyici](media/migrate-google-maps-web-services/azure-maps-multiple-markers.png)</center>

### <a name="path-url-parameter-format-comparison"></a>Yol URL parametre biçimi karşılaştırması

**Önce: Google Maps**

Google Maps 'ta çizgiler ve çokgenler, URL 'deki `path` parametresi kullanılarak bir statik harita görüntüsüne eklenebilir. `path` parametresi, aşağıda gösterildiği gibi, bu stille birlikte haritada oluşturulacak konumların bir stilini ve bir listesini alır:

```
&path=pathStyles|pathLocation1|pathLocation2|...
```

Ek stiller, URL 'ye farklı bir stille ve konum kümesiyle ek `path` parametreler eklenerek kullanılabilir.

Google Maps 'teki yol konumları `latitude1,longitude1|latitude2,longitude2|…`biçimiyle belirtilir. Yollar, noktaların adreslerini kodlanabilir veya içerebilir.

Google Maps 'taki yol stilleri, bu `optionName1:value1|optionName2:value2`gibi kanal (\|) karakterleriyle ayrılmış birden çok stil ile `optionName:value`biçim ile eklenir. Seçenek adları ve değerleri, iki nokta üst üste (:)) ayrılır. Aşağıdaki stil seçenek adları, Google Maps içindeki yollara stil eklemek için kullanılabilir:

- `color`: yolun veya Çokgen anahattının rengi. 24 bit onaltılı renk (`0xrrggbb`), 32 bitlik bir onaltılık renk (`0xrrggbbbaa`) veya aşağıdaki değerlerden biri olabilir. siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `fillColor`: yol alanını (çokgen) dolduracak olan renk. 24 bit onaltılı renk (`0xrrggbb`), 32 bitlik bir onaltılık renk (`0xrrggbbbaa`) veya aşağıdaki değerlerden biri olabilir. siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `geodesic` – yolun, dünya eğriliği takip eden bir çizgi olması gerekip gerekmediğini gösterir.
- `weight`: yolun piksel cinsinden kalınlığı.

Örneğin, Google Maps 'ta, %50 opaklık ve 4 piksellik bir çizgi, şu URL parametresi ile Koordinatlar (Boylam:-110, Enlem: 45 ve Boylam:-100, Latitude: 50) arasında haritaya eklenebilir:

```
&path=color:0xFF000088|weight:4|45,-110|50,-100
```

<center>

![Google Maps çoklu çizgi](media/migrate-google-maps-web-services/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, URL 'de `path` parametresi belirtilerek bir statik harita görüntüsüne da da eklenebilir. Google Maps gibi, bu parametrede bir stil ve konumların listesi belirlenebilir ve farklı stillerle birden çok daire, çizgi ve çokgen işlemek için `path` parametresi birden çok kez belirtilebilir.

```
&path=pathStyles||pathLocation1|pathLocation2|...
```

Azure haritalar, yol konumlarına geldiğinde, "enlem, Boylam" biçimini kullanarak koordinatların "Boylam Enlem" biçiminde olmasını gerektirir. Ayrıca, Azure Maps 'ta boylam ve enlem bir virgülle ayırarak bir boşluk olduğunu unutmayın. Azure Maps, noktalara yönelik kodlanmış yollar veya adresler desteklemez. Daha büyük veri kümeleri, [burada](how-to-render-custom-data.md#get-data-from-azure-maps-data-storage)belgelendiği gibi Azure Maps VERI depolama API 'Sine bir coğrafi JSON olarak yüklenebilir.

Azure haritalar 'daki yol stilleri, bu `optionName1Value1|optionName2Value2`gibi kanal (\|) karakterleriyle ayırarak birden çok stil `optionNameValue`biçim ile eklenir. Seçenek adları ve değerleri ayrılmadığını not edin. Aşağıdaki stil seçenek adları, Azure haritalar 'daki yollara stil eklemek için kullanılabilir:

- `fa`-poligonları işlerken kullanılan Fill Color geçirgenliği (Alpha). 0 ile 1 arasında bir sayı olabilir.
- `fc`-bir çokgenin alanını işlemek için kullanılan Fill Color.
- `la`: çizgiler ve çokgenler ana hattı işlenirken kullanılan çizgi rengi geçirgenliği (Alpha). 0 ile 1 arasında bir sayı olabilir.
- `lc`: çizgiler ve çokgenlerin anahattını işlemek için kullanılan çizgi rengi.
- `lw`: çizginin piksel cinsinden genişliği.
- `ra`: ölçümlerde bir daire yarıçapı belirtir.

Örneğin, Azure Maps 'ta, %50 opaklık ve 4 piksellik bir çizgi, şu URL parametresi ile Koordinatlar (Boylam:-110, Enlem: 45 ve Boylam:-100, Latitude: 50) arasında haritaya eklenebilir:

```
&path=lcFF0000|la.5|lw4||-110 45|-100 50
```

<center>

![Azure Maps çoklu çizgi](media/migrate-google-maps-web-services/azure-maps-polyline.png)</center>

## <a name="calculate-a-distance-matrix"></a>Uzaklık matrisini hesaplama

Azure Maps, bir konum kümesi arasında uzaklık matrisi olarak seyahat sürelerini ve uzaklıkları hesaplamak için bir API sağlar. Azure haritalar Mesafe Matrisi API 'SI, Google Haritalar 'daki uzaklık Matrisi API 'siyle karşılaştırılabilir;

- [**Yol matrisi**](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview): zaman uyumsuz bir çıkış ve hedef kümesi için seyahat sürelerini ve uzaklıkları hesaplar. İstek başına en fazla 700 hücre desteklenir (hedef sayısı ile çarpılan çıkış sayısı). Bu kısıtlama göz önünde bulundurularak, olası matris boyutlarına örnek olarak şunlar verilebilir: 700x1, 50x10, 10x10, 28x25, 10x70.

> [!NOTE]
> Uzaklık Matrisi API 'sine yönelik bir istek yalnızca istek gövdesinde kaynak ve hedef bilgileri olan bir POST isteği kullanılarak yapılabilir. Ayrıca, Azure Maps tüm kaynakları ve hedefleri koordine etmek gerektirir. Önce adreslerin coğrafi kodlanmış olması gerekir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi      | Karşılaştırılabilir Azure Maps API parametresi  |
|--------------------------------|--------------------------------------|
| `arrivial_time`                | `arriveAt`                           |
| `avoid`                        | `avoid`                              |
| `depature_time`                | `departAt`                           |
| `destinations`                 | `destination` – istek SONRASı gövdesinin coğrafi JSON olarak belirleyin. |
| `key`                          | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                     | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `mode`                         | `travelMode`                         |
| `origins`                      | `origins` – istek SONRASı gövdesinin coğrafi JSON olarak belirleyin.  |
| `region`                       | *Yok* – bu, coğrafi kodlama ile ilgili bir özelliktir. Azure Maps coğrafi kodlama API 'sini kullanırken `countrySet` parametresini kullanın. |
| `traffic_model`                | *Yok* – yalnızca trafik verilerinin `traffic` parametresiyle kullanılması gerekiyorsa belirtilebilir. |
| `transit_mode`                 | *N/A* -geçiş tabanlı uzaklık matrisleri Şu anda desteklenmiyor.  |
| `transit_routing_preference`   | *N/A* -geçiş tabanlı uzaklık matrisleri Şu anda desteklenmiyor.  |
| `units`                        | *Yok* – Azure Maps yalnızca ölçüm sistemini kullanır. |

> [!TIP]
> Azure haritalar yönlendirme API 'sinde (kamyon yönlendirme, altyapı belirtimleri, kaçınma...) sunulan tüm gelişmiş yönlendirme seçenekleri, Azure haritalar Mesafe Matrisi API 'sinde desteklenir.

## <a name="get-a-time-zone"></a>Saat dilimi al

Azure Maps, bir koordinat içindeki zaman dilimini almak için bir API sağlar. Azure haritalar Saat dilimi API 'SI, Google Maps 'taki saat dilimi API 'siyle karşılaştırılabilir:

- [**Koordine eden saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebycoordinates): bir koordinat belirtin ve bulunduğu saat dilimiyle ilgili ayrıntıları alın.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi   |
|---------------------------|---------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri.       |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.    |
| `location`                  | `query`             |
| `timestamp`                 | `timeStamp`         |

Buna ek olarak, Azure Maps Platformu, saat dilimi adları ve kimlikleri ile Dönüştürmelere yardımcı olmak için çeşitli ek saat dilimi API 'Leri de sağlar:

- [**Kimliğe göre saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebyid): belirtilen IANA saat dilimi kimliği için geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini döndürür.
- [**Bölge enum IANA**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumiana): bir IANA saat dilimi kimliklerinin tam listesini döndürür. IANA hizmetine yapılan güncelleştirmeler sisteme bir gün içinde yansıtılır.
- [**Zaman dilimi numaralandırması pencereleri**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumwindows): Windows saat dilimi kimliklerinin tam listesini döndürür.
- [**Saat dılımı IANA sürümü**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneianaversion): Azure haritalar tarafından kullanılan geçerli IANA sürüm numarasını döndürür.
- [**IANA Için saat dilimi pencereleri**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonewindowstoiana): geçerli bir Windows saat dilimi kimliği verilen ılgılı bır IANA kimliğini döndürür. Tek bir Windows KIMLIĞI için birden çok ıANA kimliği döndürülebilir.

## <a name="client-libraries"></a>İstemci kitaplıkları

Azure Maps, istemci kitaplıklarını aşağıdaki programlama dilleri için sağlar:

- JavaScript, TypeScript, Node. js – [belgeler](how-to-use-services-module.md) \| [NPM paketi](https://www.npmjs.com/package/azure-maps-rest)

Diğer programlama dilleri için açık kaynak istemci kitaplıkları:

- .NET Standard 2,0 – [GitHub projesi](https://github.com/perfahlen/AzureMapsRestServices) \| [NuGet paketi](https://www.nuget.org/packages/AzureMapsRestToolkit/)

## <a name="additional-resources"></a>Ek kaynaklar

Azure haritalar REST hizmetlerindeki bazı ek belgeler ve kaynaklar aşağıda verilmiştir.

- [Arama için en iyi uygulamalar](how-to-use-best-practices-for-search.md)
- [Adres arayın](how-to-search-for-address.md)
- [Azure haritalar REST hizmeti API başvuru belgeleri](https://docs.microsoft.com/rest/api/maps/)

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar REST hizmetleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmaya yönelik en iyi uygulamalar](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Hizmetler modülünü kullanma (Web SDK)](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)