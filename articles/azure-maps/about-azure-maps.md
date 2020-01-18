---
title: Genel Bakış | Microsoft Azure haritaları
description: Microsoft Azure haritalarının Hizmetleri ve özellikleri hakkında bilgi edinin ve bunları uygulamalarınızda nasıl kullanacağınızı öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 2909dbebc7531be0e45b321cc31b599b0f63ee4a
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76262580"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?

Azure Maps, Web ve mobil uygulamalara coğrafi bağlam sağlamak için yeni eşleme verilerini kullanan Jeo-uzamsal hizmetler topluluğudur. Azure haritalar şunları sağlar:

* Haritaları birden çok stilde ve uydu Imagery 'de işlemek için REST API 'Leri.
* Dünyanın dört bir yanındaki adres, yer ve ilgi noktalarını arar.
* Farklı yönlendirme türleri: noktadan noktaya, MultiPoint, MultiPoint iyileştirmesi, ısovaone, ticari araç, trafik Etkileyenleri ve matris yönlendirme
* Trafik akışı ve olaylar görünümü.
* Ortak aktarım ve farklı taşıma modları (Bisiklet payı, scooter paylaşma ve otomobil paylaşma gibi) istemek ve gerçek zamanlı olarak planlama yapmak için Mobility Hizmetleri. 
* Coğrafi konum aracılığıyla Kullanıcı konumu oluşturma ve konumu saat dilimlerine dönüştürme. 
* Azure 'da barındırılan konum bilgileriyle bölge sınırlaması ve harita veri depolama hizmetleri. 
* Jeo-uzamsal analiz aracılığıyla konum zeka. 

Ayrıca, Azure Maps Hizmetleri Web SDK 'Sı veya Android SDK ile kullanılabilir. Bu araçlar, geliştiricilerin konum bilgilerini Azure çözümleriyle tümleştiren çözümleri hızla geliştirmesini ve ölçeklendirmesine yardımcı olur. 

Ücretsiz bir [Azure Maps hesabına](https://azure.microsoft.com/services/azure-maps/) kaydolabilir ve geliştirmeye başlayabilirsiniz.

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Harita denetimleri

### <a name="web-sdk"></a>Web SDK

Azure Haritalar Web SDK 'Sı, etkileşimli haritaları, Web veya mobil uygulamalarınız için kendi içeriklerinizi ve Imagery ile özelleştirmenizi sağlar. Bu denetim WebGL kullanımını sağlar, böylece büyük veri kümelerini yüksek performanslı olarak işleyebilirsiniz. JavaScript veya TypeScript kullanarak SDK ile geliştirin.

![Popülasyon değişikliğinin örnek Haritası](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Android SDK

Mobil eşleme uygulamaları oluşturmak için Azure Haritalar Android SDK kullanın. 

![Bir mobil cihazda örnek eşleme](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure Maps, Azure uygulamalarınıza coğrafi içerik sağlayabilen aşağıdaki dokuz hizmetten oluşur.

### <a name="data-service"></a>Data Service

Haritalar için veriler zorunludur. Uzamsal işlemler veya görüntü kompozisyonu ile kullanmak üzere Jeo uzamsal verileri karşıya yüklemek ve depolamak için veri hizmetini kullanın.  Müşteri verilerini Azure Maps hizmetine yaklaştırma gecikme süresini azaltır, üretkenliği artırabilir ve uygulamalarınızda yeni senaryolar oluşturur. Bu hizmetle ilgili ayrıntılar için bkz. [Data Service API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobility hizmeti

Azure haritalar Mobility hizmeti gerçek zamanlı Seyahat planlamasına izin verir. Olası en iyi yol seçeneklerini döndürür ve farklı türde seyahat modları sağlar. Metro (şehir) alanlarında bu modlar, yürüyen, biking ve genel aktarım içerebilir. Aktarım programı, satır geometrisi, durak listeleri, zamanlanmış ve gerçek zamanlı varış ve hizmet uyarıları isteyebilirsiniz.

Hizmet Ayrıca, paylaşılan bisiklet, Scooters veya bir konum etrafındaki otomobiller gibi belirli nesne türlerini aramaya olanak tanır. Kullanıcılar, en yakın dock 'ta kullanılabilir Bisiklet sayısını isteyebilir ve ilgili araçlar için de grup için arama yapabilir. Ayrıca, kullanıcılar gelecekteki araçlar ve geçerli yakıt düzeyi kullanılabilirliği gibi ayrıntıları bulabilir.

Hizmet hakkında daha fazla bilgi edinmek için bkz. [Mobility API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>İşleme hizmeti

Işleme hizmeti, geliştiricilerin eşleme etrafında Web uygulamaları ve mobil uygulamalar oluşturmalarına yardımcı olur. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Render Service 'ten bir harita örneği](media/about-azure-maps/Introduction_Map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Daha fazla ayrıntı için [işleme API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/render)okuyun.

### <a name="route-service"></a>Yönlendirme hizmeti

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi çeşitli seyahat modları arasında yol tariflerini hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma gibi girdileri de dikkate alabilir.

![Yol hizmetinden bir harita örneği](media/about-azure-maps/Introduction_Route.png)

Rota hizmeti, gelişmiş özelliklerin bir önizlemesini sağlar, örneğin: 

* Birden çok yol isteği toplu işleme.
* Bir çıkış ve hedef kümesi arasındaki seyahat süresi ve mesafenin matrisleri.
* Kullanıcıların zaman veya yakıt gereksinimlerine göre geçebileceği rotaları veya uzaklıkları bulma. 

Yönlendirme özellikleri hakkında ayrıntılı bilgi için [route API belgelerini](https://docs.microsoft.com/rest/api/maps/route)okuyun.

### <a name="search-service"></a>Arama hizmeti

Arama hizmeti, geliştiricilerin adresleri, yerleri, iş listelerini ada veya kategoriye göre ve diğer coğrafi bilgileri aramasına yardımcı olur. Arama hizmeti, [kacode](https://en.wikipedia.org/wiki/Reverse_geocoding) adreslerini ve çapraz Streets 'leri, Latitudes ve Longitudes temelinde ters kaydedebilir.

![Haritada arama örneği](media/about-azure-maps/Introduction_Search.png)

Arama hizmeti Ayrıca, gibi gelişmiş özellikler de sağlar:

* Bir rota üzerinde arama yapın.
* Daha geniş bir alan içinde arama yapın.
* Arama istekleri grubunu toplu olarak oluşturma.
* Konum noktası yerine daha büyük alanı arayın. 

Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkında daha fazla bilgi için [Arama API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/search)okuyun.

### <a name="spatial-operations-service"></a>Uzamsal Işlemler hizmeti

Azure haritalar uzamsal Işlemler hizmeti, konum bilgilerini alır ve müşterileri, zaman ve alanda oluşan devam eden olayların bilgilendirmesine yardımcı olmak için hızlı bir şekilde analiz eder. Neredeyse gerçek zamanlı analizler ve olayların modellenmesi sağlar. 

Hizmet, müşterilerin konum zekasını ortak Jeo uzamsal matematik hesaplamaları kitaplığıyla geliştirmesini sağlar. Yaygın hesaplamalar, en yakın nokta, büyük daire uzaklığı ve arabellekleri içerir. Hizmet ve çeşitli özellikler hakkında daha fazla bilgi edinmek için, [uzamsal işlemler API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/spatial)okuyun.

### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat dilimi hizmeti geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini sorgulamanızı sağlar. Enlem ve boylam çiftlerini ya da [ıANA kimliğini](https://www.iana.org/)kullanabilirsiniz. Saat dilimi hizmeti ayrıca şunları sağlar:

* Microsoft Windows saat dilimi kimliklerini ıANA saat dilimlerine dönüştürme.
* UTC 'ye saat dilimi kayması getiriliyor.
* Geçerli saati bir saat diliminde alma. 

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

![Trafik bilgilerine sahip bir harita örneği](media/about-azure-maps/Introduction_Traffic.png)

Daha fazla bilgi için bkz. [trafik API 'si belgeleri](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location-service"></a>IP-konum hizmeti

IP adresi için alınan iki harfli ülke kodunu önizlemek için IP konumu hizmetini kullanın. Bu hizmet, coğrafi konuma göre özelleştirilmiş uygulama içeriği sağlayarak kullanıcı deneyimini geliştirmenize yardımcı olabilir.

REST API IP ile konum hizmeti hakkında daha fazla bilgi için [Azure Maps coğrafi konum API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/geolocation)okuyun.

## <a name="programming-model"></a>Programlama modeli

Azure haritalar, taşınabilirlik için geliştirilmiştir ve platformlar arası uygulamalar geliştirmenize yardımcı olabilir. Dil belirsiz bir programlama modeli kullanır ve [REST API 'ler](https://docs.microsoft.com/rest/api/maps/)aracılığıyla JSON çıkışını destekler.

Ayrıca, Azure Maps basit bir programlama modeliyle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-control) sunar. Geliştirme, hem Web uygulamaları hem de mobil uygulamalar için hızlı ve kolaydır.

## <a name="usage"></a>Kullanım

Azure haritalar hizmetlerine erişmek, [Azure Portal](https://portal.azure.com) ve Azure Maps hesabı oluşturmaya göre yapılır.

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınız sizin için önceden oluşturulmuş iki anahtar ile birlikte gelir. Bu konum yeteneklerini, anahtarları kullanarak ve Azure Maps hizmetine bir istek yapmak üzere uygulamanızda tümleştirmeye başlayın.

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
