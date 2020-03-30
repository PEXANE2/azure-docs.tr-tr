---
title: Genel Bakış | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar'daki hizmetler ve özellikler ve bunları uygulamalarınızda nasıl kullanacağınız hakkında bilgi edinin.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fdac12350db785e6194cd3d057f4d2adfefa5969
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80335743"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?

Azure Haritalar, web ve mobil uygulamalara coğrafi bağlam sağlamak için yeni haritalama verileri kullanan bir coğrafi hizmetler topluluğudur. Azure Haritalar şunları sağlar:

* REST API'ler haritaları birden fazla stilde ve uydu görüntülerinde işlemek için.''
* Dünya çapında adresleri, yerleri ve ilgi çekici noktaları bulmak için arama hizmetleri.
* Çeşitli yönlendirme seçenekleri; noktadan noktaya, çok noktalı, çok noktalı optimizasyon, isochrone, ticari araç, trafik etkilenmiş ve matris yönlendirme gibi.
* Trafik bilgisi gerektiren uygulamalar için trafik akışı görünümü ve olaylar görünümü.
* Toplu taşıma lojistiğini talep etmek, rotaları gerçek zamanlı planlamak ve alternatif ulaşım modları için bilgi istemek için mobilite hizmeti.
* Saat dilimi ve coğrafi konum hizmetleri ve bir konumu saat dilimlerine dönüştürme.
* Azure'da barındırılan konum bilgileriyle hizmet ve eşleme veri depolama. 
* Coğrafi analiz yoluyla konum istihbaratı. 

Ayrıca, Azure Haritalar hizmetleri Web SDK veya Android SDK üzerinden de kullanılabilir. Bu araçlar, geliştiricilerin konum bilgilerini Azure çözümlerine entegre eden çözümleri hızla geliştirmelerine ve ölçeklendirmelerine yardımcı olur. 

Ücretsiz bir [Azure Haritalar hesabına](https://azure.microsoft.com/services/azure-maps/) kaydolabilir ve geliştirmeye başlayabilirsiniz.

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Eşleme denetimleri

### <a name="web-sdk"></a>Web SDK

Azure Haritalar Web SDK, etkileşimli haritaları kendi içeriğinizle ve görsellerinizle özelleştirmenize olanak tanır. Bu etkileşimli haritayı hem web hem de mobil uygulamalarınız için kullanabilirsiniz. Harita denetimi WebGL'den yararlanarak büyük veri kümelerini yüksek performansla işleyebilirsiniz. JavaScript veya TypeScript kullanarak SDK ile geliştirin.

![Nüfus değişimi örneği haritası](media/about-azure-maps/intro_web_map_control.png)

### <a name="android-sdk"></a>Android SDK

Mobil harita uygulamaları oluşturmak için Azure Haritalar Android SDK'yı kullanın. 

![Mobil cihazdaki harita örnekleri](media/about-azure-maps/android_sdk.png)

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure Haritalar, Azure uygulamalarınız için coğrafi bağlam sağlayabilecek aşağıdaki dokuz hizmetten oluşur.

### <a name="data-service"></a>Veri Hizmeti

Veriler haritalar için zorunludur. Uzamsal işlemler veya görüntü kompozisyonu ile kullanılmak üzere jeouzamsal verileri yüklemek ve depolamak için Veri Hizmeti'ni kullanın.  Müşteri verilerinin Azure Haritalar hizmetine yaklanması gecikme süresini azaltır, üretkenliği artırır ve uygulamalarınızda yeni senaryolar oluşturur. Bu hizmetle ilgili ayrıntılar için [Veri Hizmeti API belgelerine](https://docs.microsoft.com/rest/api/maps/data)bakın.

### <a name="mobility-service"></a>Mobility hizmeti

Azure Haritalar Mobilite hizmeti, gerçek zamanlı seyahat planlamasına olanak tanır. Mümkün olan en iyi rota seçeneklerini döndürür ve farklı türde seyahat modları sağlar. Metro (şehir) alanları için bu modlar yürümeyi, bisiklete binmeyi ve toplu taşımayı içerebilir. Transit güzergahı, hat geometrisi, durak listeleri, zamanlanmış varışlar, gerçek zamanlı varışlar ve servis uyarıları isteyebilirsiniz.

Hizmet ayrıca, bir konum etrafında belirli nesne türleri için arama sağlar. Kullanıcılar bir konum etrafında paylaşılan bisikletleri, scooter'ları veya arabaları arayabilir. Kullanıcılar en yakın rıhtımdaki mevcut bisiklet lerin sayısını isteyebilir ve sürüş paylaşımı için kullanılabilir araçları arayabilir. Ayrıca, kullanıcılar araçların gelecekteki kullanılabilirliği ve mevcut yakıt seviyesi gibi ayrıntıları bulabilirler.

Hizmet hakkında daha fazla bilgi edinmek için [Mobilite API belgelerine](https://docs.microsoft.com/rest/api/maps/mobility)bakın.

### <a name="render-service"></a>İşleme hizmeti

Render hizmeti, geliştiricilerin haritalama özelliklerine sahip web ve mobil uygulamalar oluşturmasına yardımcı olur. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Render hizmetinden bir harita örneği](media/about-azure-maps/intro_map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Daha fazla ayrıntı için [Render API belgelerini](https://docs.microsoft.com/rest/api/maps/render)okuyun.

### <a name="route-service"></a>Yönlendirme hizmeti

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Bu hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi bir dizi seyahat modu boyunca yol tariflerini hesaplamalarına olanak tanır. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşımacılığı gibi girdileri de dikkate alır.

![Rota hizmetinden bir harita örneği](media/about-azure-maps/intro_route.png)

Rota hizmeti, aşağıdakiler gibi gelişmiş özelliklerin önizlemesini sunar: 

* Birden çok rota isteklerinin toplu işleme.
* Bir dizi köken ve destinasyon arasındaki seyahat süresi ve mesafenin matrisleri.
* Kullanıcıların zaman veya yakıt gereksinimlerine göre seyahat edebileceği rotaları veya mesafeleri bulmak. 

Yönlendirme özellikleri hakkında ayrıntılı bilgi için [Rota API belgelerini](https://docs.microsoft.com/rest/api/maps/route)okuyun.

### <a name="search-service"></a>Arama hizmeti

Arama hizmeti, geliştiricilerin adresleri, yerleri, işletme girişlerini ada veya kategoriye göre ve diğer coğrafi bilgileri aramalarına yardımcı olur. Arama [hizmeti, enlem](https://en.wikipedia.org/wiki/Reverse_geocoding) lere ve boylamlara göre coğrafi kod adreslerini ve çapraz sokakları tersine çevirebilir.

![Haritadaki arama örneği](media/about-azure-maps/intro_search.png)

Arama hizmeti de aşağıdakiler gibi gelişmiş özellikler sağlar:

* Bir güzergah boyunca arama yapın.
* Daha geniş bir alanda arama yapın.
* Bir arama isteği grubunu toplu olarak toplu olarak yapın.
* Konum noktası yerine daha büyük bir alan arayın. 

Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkında daha fazla bilgi için [Arama API belgelerini](https://docs.microsoft.com/rest/api/maps/search)okuyun.

### <a name="spatial-operations-service"></a>Mekansal Operasyonlar hizmeti

Azure Haritalar Uzamsal İşlemler hizmeti konum bilgilerini alır. Müşterileri zaman ve mekanda devam eden olaylar hakkında bilgilendirmeye yardımcı olmak için konum bilgilerini hızla analiz eder. Neredeyse gerçek zamanlı analiz ve olayların tahmine dayalı modellemesağlar. 

Bu hizmet, müşterilerin ortak jeouzamsal matematiksel hesaplamalardan oluşan bir kitaplıkla konum zekalarını geliştirmelerine olanak tanır. Ortak hesaplamalar en yakın nokta, büyük daire mesafesi ve arabellekleri içerir. Hizmet ve çeşitli özellikler hakkında daha fazla bilgi edinmek için [Mekansal İşlemler API belgelerini](https://docs.microsoft.com/rest/api/maps/spatial)okuyun.

### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat Dilimi hizmeti, geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini sorgulamanızı sağlar. Enlem ve boylam çiftleri veya [IANA kimliği](https://www.iana.org/)kullanabilirsiniz. Saat Dilimi hizmeti ayrıca aşağıdakileri sağlar:

* Microsoft Windows saat dilimi iD'lerini IANA saat dilimlerine dönüştürme.
* UTC'ye bir saat dilimi mahsulü alma.
* Seçilen bir saat diliminde geçerli saati alma. 

Saat Dilimi hizmetine bir sorgu için tipik bir JSON yanıtı aşağıdaki örnek gibi görünür:

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

Bu hizmetle ilgili ayrıntılar için [Saat Dilimi API belgelerini](https://docs.microsoft.com/rest/api/maps/timezone)okuyun.

### <a name="traffic-service"></a>Trafik hizmeti

Trafik hizmeti, geliştiricilerin trafik bilgileri gerektiren web veya mobil uygulamalar için kullanabileceği bir web hizmetleri paketidir. Hizmet, iki tür veri sağlar:

* Trafik akışı: Ağdaki tüm önemli yollar için gerçek zamanlı gözlemlenen hızlar ve seyahat süreleri.
* Trafik olayları: Trafik sıkışıklıklarının ve yol ağı etrafındaki olayların güncel görünümü.

![Trafik bilgilerini içeren bir harita örneği](media/about-azure-maps/intro_traffic.png)

Daha fazla bilgi için [Trafik API belgelerine](https://docs.microsoft.com/rest/api/maps/traffic)bakın.

### <a name="ip-to-location-service"></a>IP'den Konuma servis

Bir IP adresi için alınan iki harfli ülke kodunu önizlemek için IP to Location hizmetini kullanın. Bu hizmet, coğrafi konuma göre özelleştirilmiş uygulama içeriği sağlayarak kullanıcı deneyimini geliştirmenize yardımcı olabilir.

REST API'deki IP'den Konuma hizmet hakkında daha fazla bilgi için [Azure Haritalar Coğrafi Konum API belgelerini](https://docs.microsoft.com/rest/api/maps/geolocation)okuyun.

## <a name="programming-model"></a>Programlama modeli

Azure Haritalar mobilite için oluşturulmuştür ve platformlar arası uygulamalar geliştirmenize yardımcı olabilir. Bu dil agnostik bir programlama modeli kullanır ve [REST API'lar](https://docs.microsoft.com/rest/api/maps/)aracılığıyla JSON çıkış destekler.

Ayrıca, Azure Haritalar basit bir programlama modeli yle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-control) sunar. Geliştirme hem web hem de mobil uygulamalar için hızlı ve kolaydır.

## <a name="usage"></a>Kullanım

Azure Haritalar hizmetlerine erişmek, [Azure portalına](https://portal.azure.com) gidip bir Azure Haritalar hesabı oluşturma meselesidir.

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınızda sizin için oluşturulmuş iki anahtar var, her iki anahtarı da kullanın. Bu konum özelliklerini uygulamanıza entegre etmeye başlayın ve Azure Haritalar hizmetlerine istekte bulunun.

Not - Azure Haritalar, işlevsellik amaçlarıyla eşleme amacıyla müşteri tarafından sağlanan adres/konum sorgularını ("Sorgular") üçüncü taraf TomTom ile paylaşır. Sorgular TomTom ile paylaşıldığında herhangi bir müşteriye veya son kullanıcıya bağlı değildir ve kişileri tanımlamak için kullanılamaz. Microsoft şu anda Çevrimiçi Hizmetler Alt Yüklenici Listesine TomTom ekleme sürecindedir. Moovit ve AccuWeather ile entegrasyon içeren Hareketlilik ve Hava Durumu Hizmetlerinin şu anda [PREVIEW'de](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olduğunu unutmayın.

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure Haritalar API'leri şu anda aşağıdakiler dışında tüm ülke ve bölgelerde kullanılabilir:

* Çin
* Güney Kore

Geçerli IP adresinizin konumunun desteklenen bir ülkede olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalarını sergileyen örnek bir uygulamayı deneyin:

> [!div class="nextstepaction"]
> [Quickstart: Bir web uygulaması oluşturma](quick-demo-map-app.md)

Azure Haritalar'da güncel kalın: 

> [!div class="nextstepaction"]
> [Azure Haritalar günlüğü](https://azure.microsoft.com/blog/topics/azure-maps/)
