---
title: Genel Bakış | Microsoft Azure haritaları
description: Microsoft Azure haritalarının Hizmetleri ve özellikleri hakkında bilgi edinin ve bunları uygulamalarınızda nasıl kullanacağınızı öğrenin.
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/15/2020
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b375ea8fdebd876bdc7ecf157a4cebd5e614dfd0
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443758"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?

Azure Maps, Web ve mobil uygulamalara coğrafi bağlam sağlamak için yeni eşleme verilerini kullanan Jeo-uzamsal hizmetler topluluğudur. Azure haritalar şunları sağlar:

* Haritaları birden çok stilde ve uydu Imagery 'de işlemek için REST API 'Leri.
* Dünyanın dört bir yanındaki adresleri, yerleri ve ilgi çekici noktaları bulmak için Hizmetleri arayın.
* Çeşitli yönlendirme seçenekleri; noktadan noktaya, MultiPoint, MultiPoint iyileştirmesi, ısovaone, ticari araç, trafik Etkileyenleri ve matris yönlendirme gibi.
* Trafik bilgisi gerektiren uygulamalar için trafik akışı görünümü ve olaylar görünümü.
* Ortak geçiş lojistik istemek, yönlendirmeleri gerçek zamanlı olarak planlamak ve alternatif taşıma modları için bilgi istemek üzere Mobility hizmeti.
* Saat dilimi ve coğrafi konum hizmetleri ve bir konumu saat dilimlerine dönüştürme.
* Azure 'da barındırılan konum bilgileriyle bölge sınırlaması hizmeti ve eşleme veri depolaması. 
* Jeo-uzamsal analiz aracılığıyla konum zeka. 

Ayrıca, Azure Maps Hizmetleri Web SDK 'Sı veya Android SDK ile kullanılabilir. Bu araçlar, geliştiricilerin konum bilgilerini Azure çözümleriyle tümleştiren çözümleri hızla geliştirmesini ve ölçeklendirmesine yardımcı olur. 

Ücretsiz bir [Azure Maps hesabına](https://azure.microsoft.com/services/azure-maps/) kaydolabilir ve geliştirmeye başlayabilirsiniz.

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Harita denetimleri

### <a name="web-sdk"></a>Web SDK

Azure Haritalar Web SDK 'Sı, etkileşimli haritaları kendi içerikleriyle ve Imagery ile özelleştirmenizi sağlar. Bu etkileşimli eşlemeyi Web veya mobil uygulamalarınız için kullanabilirsiniz. Harita denetimi WebGL kullanımını sağlar, böylece büyük veri kümelerini yüksek performanslı olarak işleyebilirsiniz. JavaScript veya TypeScript kullanarak SDK ile geliştirin.

![Popülasyon değişikliğinin örnek Haritası](media/about-azure-maps/intro_web_map_control.png)

### <a name="android-sdk"></a>Android SDK

Mobil eşleme uygulamaları oluşturmak için Azure Haritalar Android SDK kullanın. 

![Bir mobil cihazda örnek eşleme](media/about-azure-maps/android_sdk.png)

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure Maps, Azure uygulamalarınıza coğrafi içerik sağlayabilen aşağıdaki dokuz hizmetten oluşur.

### <a name="data-service"></a>Veri Hizmeti

Haritalar için veriler zorunludur. Uzamsal işlemler veya görüntü kompozisyonu ile kullanmak üzere Jeo uzamsal verileri karşıya yüklemek ve depolamak için veri hizmetini kullanın.  Müşteri verilerini Azure Maps hizmetine yaklaştırma gecikme süresini azaltır, üretkenliği artırabilir ve uygulamalarınızda yeni senaryolar oluşturur. Bu hizmetle ilgili ayrıntılar için bkz. [Data Service API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobility hizmeti

Azure haritalar Mobility hizmeti gerçek zamanlı Seyahat planlamasına izin verir. Olası en iyi yol seçeneklerini döndürür ve farklı türde seyahat modları sağlar. Metro (şehir) alanlarında bu modlar, yürüyen, biking ve genel aktarım içerebilir. Aktarım programı, satır geometrisi, durak listeleri, zamanlanmış varış, gerçek zamanlı varış ve hizmet uyarıları isteyebilirsiniz.

Hizmet ayrıca bir konum etrafında belirli nesne türlerini aramaya izin verir. Kullanıcılar, bir konum etrafında paylaşılan bisiklet, ayrıntılı veya otomobiller arayabilir. Kullanıcılar, en yakın dock 'ta kullanılabilir Bisiklet sayısını isteyebilir ve ilgili araçlar için de grup için arama yapabilir. Kullanıcılar, daha sonra Araçlar ve geçerli yakıt düzeyi kullanılabilirliği gibi ayrıntıları bulabilir.

Hizmet hakkında daha fazla bilgi edinmek için bkz. [Mobility API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>İşleme hizmeti

Işleme hizmeti, geliştiricilerin eşleme özellikleriyle web uygulamaları ve mobil uygulamalar oluşturmalarına yardımcı olur. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Render Service 'ten bir harita örneği](media/about-azure-maps/intro_map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Daha fazla ayrıntı için [işleme API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/render)okuyun.

### <a name="route-service"></a>Yönlendirme hizmeti

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Hizmet, geliştiricilerin otomobil, kamyon, Bisiklet veya yürüme gibi çeşitli seyahat modlarında yönleri hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli malzeme taşıma gibi girdileri de dikkate alır.

![Yol hizmetinden bir harita örneği](media/about-azure-maps/intro_route.png)

Rota hizmeti, gelişmiş özelliklerin bir önizlemesini sağlar, örneğin: 

* Birden çok yol isteği toplu işleme.
* Bir çıkış ve hedef kümesi arasındaki seyahat süresi ve mesafenin matrisleri.
* Kullanıcıların zaman veya yakıt gereksinimlerine göre geçebileceği rotaları veya uzaklıkları bulma. 

Yönlendirme özellikleri hakkında ayrıntılı bilgi için [route API belgelerini](https://docs.microsoft.com/rest/api/maps/route)okuyun.

### <a name="search-service"></a>Arama hizmeti

Arama hizmeti, geliştiricilerin adresleri, yerleri, iş listelerini ada veya kategoriye göre ve diğer coğrafi bilgileri aramasına yardımcı olur. Arama hizmeti, [kacode](https://en.wikipedia.org/wiki/Reverse_geocoding) adreslerini ve çapraz Streets 'leri, Latitudes ve Longitudes temelinde ters kaydedebilir.

![Haritada arama örneği](media/about-azure-maps/intro_search.png)

Arama hizmeti Ayrıca, gibi gelişmiş özellikler de sağlar:

* Bir rota üzerinde arama yapın.
* Daha geniş bir alan içinde arama yapın.
* Arama istekleri grubunu toplu olarak oluşturma.
* Konum noktası yerine daha büyük alanı arayın. 

Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkında daha fazla bilgi için [Arama API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/search)okuyun.

### <a name="spatial-operations-service"></a>Uzamsal Işlemler hizmeti

Azure haritalar uzamsal Işlemler hizmeti, konum bilgilerini alır. BT bilgilerini hızlı bir şekilde analiz ederek, müşterileri zaman ve alanda oluşan devam eden olayların bilgilendirmesine yardımcı olur. Neredeyse gerçek zamanlı analizler ve olayların modellenmesi sağlar. 

Hizmet, müşterilerin konum zekasını ortak Jeo uzamsal matematik hesaplamaları kitaplığıyla geliştirmesini sağlar. Yaygın hesaplamalar, en yakın nokta, büyük daire uzaklığı ve arabellekleri içerir. Hizmet ve çeşitli özellikler hakkında daha fazla bilgi edinmek için, [uzamsal işlemler API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/spatial)okuyun.

### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat dilimi hizmeti geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini sorgulamanızı sağlar. Enlem ve boylam çiftlerini ya da [ıANA kimliğini](https://www.iana.org/)kullanabilirsiniz. Saat dilimi hizmeti ayrıca şunları sağlar:

* Microsoft Windows saat dilimi kimliklerini ıANA saat dilimlerine dönüştürme.
* UTC 'ye saat dilimi kayması getiriliyor.
* Seçili saat diliminde geçerli saati alma. 

Saat dilimi hizmetine bir sorgu için tipik bir JSON yanıtı aşağıdaki örneğe benzer şekilde görünür:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Bu hizmetle ilgili ayrıntılar için, [saat dilimi API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/timezone)okuyun.

### <a name="traffic-service"></a>Trafik hizmeti

Trafik hizmeti, geliştiricilerin trafik bilgileri gerektiren Web uygulamaları veya mobil uygulamalar için kullanabileceği bir Web hizmetleri paketidir. Hizmet, iki tür veri sağlar:

* Trafik akışı: ağdaki tüm anahtar yolların gerçek zamanlı gözlemlenen hızları ve seyahat süreleri.
* Trafik olayları: trafik sıkışıklıklarının 'nin ve yol ağı çevresindeki olayların güncel görünümü.

![Trafik bilgilerine sahip bir harita örneği](media/about-azure-maps/intro_traffic.png)

Daha fazla bilgi için bkz. [trafik API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location-service"></a>IP-konum hizmeti

IP adresi için alınan iki harfli ülke kodunu önizlemek için IP konumu hizmetini kullanın. Bu hizmet, coğrafi konuma göre özelleştirilmiş uygulama içeriği sağlayarak kullanıcı deneyimini geliştirmenize yardımcı olabilir.

REST API IP ile konum hizmeti hakkında daha fazla bilgi için [Azure Maps coğrafi konum API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/geolocation)okuyun.

## <a name="programming-model"></a>Programlama modeli

Azure haritalar, taşınabilirlik için geliştirilmiştir ve platformlar arası uygulamalar geliştirmenize yardımcı olabilir. Dil belirsiz bir programlama modeli kullanır ve [REST API 'ler](https://docs.microsoft.com/rest/api/maps/)aracılığıyla JSON çıkışını destekler.

Ayrıca, Azure Maps basit bir programlama modeliyle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-control) sunar. Geliştirme, hem Web uygulamaları hem de mobil uygulamalar için hızlı ve kolaydır.

## <a name="usage"></a>Kullanım

Azure haritalar hizmetlerine erişmek, [Azure Portal](https://portal.azure.com) ve Azure Maps hesabı oluşturmaya göre yapılır.

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınız sizin için önceden oluşturulmuş iki anahtar ile birlikte gelir, her iki anahtar kullanın. Uygulamanızda bu konum yeteneklerini tümleştirmeye başlayın ve Azure Maps hizmetlerine istek yapın.

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure Haritalar API 'Leri Şu anda tüm ülkelerde ve bölgelerde mevcuttur:

* Çin
* Güney Kore

Geçerli IP adresinizin konumunun desteklenen bir ülkede olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar 'ı gösteren örnek bir uygulama deneyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Web uygulaması oluşturma](quick-demo-map-app.md)

Azure haritalar 'da güncel kalın: 

> [!div class="nextstepaction"]
> [Azure haritalar blogu](https://azure.microsoft.com/blog/topics/azure-maps/)
