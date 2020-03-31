---
title: Azure Ön Kapı - önbelleğe alma | Microsoft Dokümanlar
description: Bu makale, Azure Ön Kapı'nın arka uçlarınızın durumunu nasıl izlediğini anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: d4fed878e2c0b1430e963f43743fd772493d3270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471753"
---
# <a name="caching-with-azure-front-door"></a>Azure Ön Kapı ile Önbelleğe Alma
Aşağıdaki belge önbelleğe alma etkin yönlendirme kuralları ile Ön Kapı için davranış belirtir. Ön Kapı modern bir İçerik Dağıtım Ağı (CDN) ve böylece dinamik site ivme ve yük dengeleme ile birlikte, aynı zamanda diğer CDN gibi önbelleğe alma davranışları destekler.

## <a name="delivery-of-large-files"></a>Büyük dosyaların teslimi
Azure Ön Kapı, dosya boyutunda kapak olmayan büyük dosyalar sunar. Ön Kapı nesne parçalama denilen bir teknik kullanır. Büyük dosya isteği gönderildiğinde Front Door dosyayı arka uçtan küçük parçalar halinde alır. Front Door ortamı tam veya bayt aralığı belirtilen bir dosya isteği aldıktan sonra dosyayı 8 MB'lık parçalar halinde arka uçtan ister.

</br>Yığın Ön Kapı ortamına geldikten sonra önbelleğe alınarak hemen kullanıcıya sunulur. Ön Kapı sonra paralel olarak bir sonraki parçayı önceden getirir. Bu ön alma, içeriğin kullanıcıdan bir yığın önde kalmasını sağlar ve bu da gecikme süresini azaltır. Bu işlem, tüm dosya indirilene (istenirse), tüm bayt aralıkları kullanılabilir olana (istenirse) veya istemci bağlantıyı sonlandırıncaya kadar devam eder.

</br>Bayt aralığı isteği hakkında daha fazla bilgi için [RFC 7233'ü](https://web.archive.org/web/20171009165003/http://www.rfc-base.org/rfc-7233.html)okuyun.
Ön Kapı, alınan tüm parçaları önbelleğe almaz ve böylece tüm dosyanın Ön Kapı önbelleğinde önbelleğe alınması gerekmez. Dosya veya bayt aralıkları için sonraki istekler önbellekten sunulur. Tüm parçalar önbelleğe alınmamışsa, ön alma arka uçtan parçalar istemek için kullanılır. Bu optimizasyon, arka uçtaki bayt aralığı isteklerini destekleme yeteneğine dayanır; Arka uç bayt aralığı isteklerini desteklemiyorsa, bu optimizasyon etkili değildir.

## <a name="file-compression"></a>Dosya sıkıştırma
Ön Kapı, içeriği dinamik olarak sıkıştırarak müşterilerinize daha küçük ve daha hızlı bir yanıt verebilir. Tüm dosyalar sıkıştırma için uygundur. Ancak, bir dosya sıkıştırma listesi için uygun bir MIME türünde olmalıdır. Şu anda, Ön Kapı bu listenin değiştirilmesine izin vermez. Geçerli liste:</br>
- "uygulama/eot"
- "uygulama/yazı tipi"
- "uygulama/yazı tipi-sfnt"
- "uygulama/javascript"
- "application/json"
- "uygulama/opentype"
- "uygulama/otf"
- "uygulama/pkcs7-pandomim"
- "uygulama/truetype"
- "uygulama/ttf",
- "uygulama/vnd.ms-fontobject"
- "uygulama/xhtml+xml"
- "uygulama/xml"
- "uygulama/xml+rss"
- "uygulama/x-font-opentype"
- "uygulama/x-font-truetype"
- "uygulama/x-font-ttf"
- "uygulama/x-httpd-cgi"
- "uygulama/x-mpegurl"
- "uygulama/x-opentype"
- "uygulama/x-otf"
- "uygulama/x-perl"
- "uygulama/x-ttf"
- "uygulama/x-javascript"
- "yazı tipi/eot"
- "yazı tipi/ttf"
- "yazı tipi/otf"
- "yazı tipi/opentype"
- "görüntü/svg+xml"
- "metin/css"
- "metin/csv"
- "metin/html"
- "metin/javascript"
- "metin/js", "metin/düz"
- "metin/zengin metin"
- "metin/sekme ayrılmış değerler"
- "metin/xml"
- "metin/x-komut dosyası"
- "metin/x-bileşeni"
- "metin/x-java-kaynak"

Ayrıca, dosya da boyutu, dahil 1 KB ve 8 MB arasında olmalıdır.

Bu profiller aşağıdaki sıkıştırma kodlamalarını destekler:
- [Gzip (GNU zip)](https://en.wikipedia.org/wiki/Gzip)
- [Brotli](https://en.wikipedia.org/wiki/Brotli)

Bir istek gzip ve Brotli sıkıştırmayı destekliyorsa, Brotli sıkıştırma önceliklidir.</br>
Bir varlık için bir istek sıkıştırma belirtir ve bir önbellek özledim istek sonuçları, Ön Kapı doğrudan POP sunucusunda varlığın sıkıştırma gerçekleştirir. Daha sonra sıkıştırılmış dosya önbellekten servis edilir. Elde edilen öğe bir aktarım kodlaması ile döndürülür: chunked.

## <a name="query-string-behavior"></a>Dize davranışını sorgula
Ön Kapı ile, sorgu dizesi içeren bir web isteği için dosyaların önbelleğe nasıl önbelleğe alınır denetleyebilirsiniz. Sorgu dizesi olan bir web isteğinde, sorgu dizesi, soru işaretinden (?) sonra oluşan isteğin bu bölümüdür. Sorgu dizesi, alan adı ve değerinin eşit ler işaretiyle (=) ayrıldığı bir veya daha fazla anahtar değer çifti içerebilir. Her anahtar değeri çifti bir ampersand (&) ile ayrılır. Örneğin, `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Bir isteğin sorgu dizesinde birden fazla anahtar değeri çifti varsa, siparişleri önemli değildir.
- **Sorgu dizelerini yoksay**: Varsayılan mod. Bu modda, Ön Kapı ilk istekte istekte bulunan sorgu dizelerini arka uca geçirir ve varlığı önbelleğe getirir. Ön Kapı ortamından sunulan kıymet için sonraki tüm istekler önbelleğe alınan kıymetin süresi dolana kadar sorgu dizelerini yoksa.

- **Her benzersiz URL'yi önbelleğe alma**: Bu modda, sorgu dizesi de dahil olmak üzere benzersiz bir URL'ye sahip her istek, kendi önbelleğiyle benzersiz bir varlık olarak kabul edilir. Örneğin, istek için `www.example.ashx?q=test1` arka uçtan gelen yanıt Ön Kapı ortamında önbelleğe alınmış ve aynı sorgu dizesiyle sonraki önbellekler için döndürülür. Bir istek, `www.example.ashx?q=test2` kendi zaman-to-live ayarı ile ayrı bir varlık olarak önbelleğe alınr.

## <a name="cache-purge"></a>Önbellek temizleme
Ön Kapı, varlığın süresi (TTL) süresi dolana kadar varlıkları önbelleğe alacaktır. Varlığın TTL'si sona erdikten sonra, bir istemci kıymeti istediğinde, Ön Kapı ortamı istemci isteğine hizmet etmek ve önbelleği yenilemek için varlığın yeni güncelleştirilmiş bir kopyasını alır.
</br>Kullanıcılarınızın varlıklarınızın her zaman en son kopyasını aldığından emin olmak için en iyi yöntem, varlıklarınızı her güncelleştirme için sürüm altına almak ve bunları yeni URL'ler olarak yayımlamaktır. Ön Kapı, bir sonraki istemci istekleri için yeni varlıkları hemen geri alacaktır. Bazen önbelleğe alınan içeriği tüm kenar düğümlerinden temizlemek ve hepsini yeni güncelleştirilmiş varlıkları almaya zorlamak isteyebilirsiniz. Bunun nedeni, web uygulamanızdaki güncelleştirmeler veya yanlış bilgiler içeren varlıkları hızla güncelleştirmek olabilir.

</br>Kenar düğümlerinden temizlemek istediğiniz varlıkları seçin. Tüm varlıkları temizlemek istiyorsanız, Tüm Onay Kutusunu Temizleme'yi tıklatın. Aksi takdirde, temizlemek istediğiniz her varlığın yolunu Yol metin kutusuna yazın. Aşağıdaki biçimler yolda desteklenir.
1. **Tek yol temizleme**: Varlığın tam yolunu (protokol ve etki alanı olmadan) belirterek, dosya uzantısı, örneğin ,/pictures/strasbourg.png;
2. **Joker karakter temizleme**: Yıldız\*işareti ( ) joker karakter olarak kullanılabilir. Tüm klasörleri, alt klasörleri ve dosyaları bir bitiş\* noktası altında / yol içinde temizleme veya belirli bir klasör altında tüm\*alt klasörleri ve\*dosyaları tasfiye klasörü tarafından izlenen belirterek / , örneğin, / resimler / .
3. **Kök etki alanı temizleme**: Yoldaki "/" ile bitiş noktasının kökünü temizle.

Ön Kapı'daki önbellek tasfiyeleri büyük/küçük harf duyarsızdır. Ayrıca, bunlar sorgu dizesi agnostik, bir URL temizleme anlamına gelen tüm sorgu dize varyasyonları tasfiye edecek. 

## <a name="cache-expiration"></a>Önbellek süresi
Bir öğenin önbelleğimizde ne kadar süre yle depolanacağını belirlemek için aşağıdaki üstbilgi sırası kullanılır:</br>
1. Önbellek Kontrolü: s-maxage=\<saniye>
2. Önbellek Kontrolü: max-age=\<saniye>
3. Sona eriyor: \<http-tarih>

Yanıtın Önbellek Denetimi gibi önbelleğe alınamayacağını belirten Önbellek Denetimi yanıtı üstbilgisi: özel, Önbellek Denetimi: önbellek yok ve Önbellek Denetimi: mağaza yok. Ancak, aynı URL için bir POP'ta birden fazla istek varsa, yanıtı paylaşabilirler. Önbellek Denetimi yoksa varsayılan davranış AFD'nin kaynağı X miktarı için önbelleğe alması dır, X 1 ila 3 gün arasında rasgele seçilir.

## <a name="request-headers"></a>İstek üst bilgileri

Önbelleğe alma kullanırken aşağıdaki istek üstbilgisi arka uca iletilmeyecektir.
- İçerik Uzunluğu
- Aktarım-Kodlama

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
